# Guide de configuration Supabase pour Mon Cerveau Augmenté

## 1) Créer le projet Supabase
- Ouvrir `https://supabase.com` et créer un compte (gratuit)
- Créer un nouveau projet (Free). Retenir:
  - L’URL du projet (`https://YOUR_PROJECT_ID.supabase.co`)
  - La `anon key` (Project Settings > API)

## 2) Créer les tables SQL
Dans SQL Editor, exécute les requêtes suivantes:

```sql
create table if not exists tasks (
  id bigint primary key,
  title text not null,
  quadrant text not null check (quadrant in ('untriaged','do','plan','delegate','delete')),
  status text not null check (status in ('todo','done')),
  "order" int not null default 0
);

create table if not exists settings (
  key text primary key,
  value jsonb not null
);
```

Activer RLS et ajouter des policies publiques si usage personnel (sans Auth):
```sql
alter table tasks enable row level security;
alter table settings enable row level security;

create policy "public_read_tasks" on tasks for select using (true);
create policy "public_write_tasks" on tasks for insert with check (true);
create policy "public_update_tasks" on tasks for update using (true);
create policy "public_delete_tasks" on tasks for delete using (true);

create policy "public_read_settings" on settings for select using (true);
create policy "public_write_settings" on settings for insert with check (true);
create policy "public_update_settings" on settings for update using (true);
create policy "public_delete_settings" on settings for delete using (true);
```

Notes:
- Pour un usage multi-utilisateur sécurisé, active l’Auth et stocke `user_id` sur les lignes, puis filtre par `auth.uid()` dans les policies.

## 3) Activer le temps réel
- Project Settings > Database > Realtime > Tables
- Activer `public.tasks` et `public.settings` pour le Realtime

## 4) Configurer l’app
Dans `v0.html`:
- Remplacer:
```js
const supabaseUrl = 'https://YOUR_PROJECT_ID.supabase.co';
const supabaseAnonKey = 'YOUR_ANON_KEY';
```
par tes valeurs (Settings > API). Le flag `cloudEnabled` passera automatiquement à `true`.

## 5) Vérifier l’intégration
- Ouvrir `v0.html` dans le navigateur
- Ajouter quelques tâches
- Vérifier dans Supabase > Table Editor que les lignes apparaissent dans `tasks`
- Changer l’ordre et le statut, puis vérifier les mises à jour en base
- Sélectionner le Top 3 et vérifier une ligne `settings` avec `key = 'top3Tasks'`

## 6) Realtime
- Ouvre deux onglets du navigateur sur `v0.html`
- Modifie une tâche dans l’un; observe la mise à jour automatique dans l’autre

## 7) Dépannage
- Rien ne se synchronise: vérifier `supabaseUrl`, `supabaseAnonKey`, Realtime activé sur les tables
- Erreurs RLS: en usage perso, policies ci-dessus doivent autoriser lectures/écritures
- Ordre de tri: la colonne `order` est gérée par l’app; en cas d’incohérence, un glisser-déposer réindexe

## 8) Aller plus loin (optionnel)
- Auth utilisateur: ajouter `user_id uuid default auth.uid()` sur les tables et policies par utilisateur
- Index: ajouter index sur `(quadrant, "order")` pour de gros volumes
- Backups: activer des sauvegardes automatiques dans le plan payant si besoin 