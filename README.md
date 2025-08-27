# Manuel de lancement – Mon Cerveau Augmenté

## 🌐 Disponible en ligne
**Application déployée :** https://aurlro.github.io/productivity-project/

## Aperçu
Application web (HTML unique) pour capturer, trier et prioriser vos tâches (Matrice d'Eisenhower + Top 3 du jour), avec minuteur de 120s (effet Zeigarnik), drag-and-drop, et synchronisation optionnelle via Supabase.

## 1) Utiliser l'application
### Option A - En ligne (recommandée)
- Ouvrir : https://aurlro.github.io/productivity-project/
- Aucune installation requise

### Option B - Local (sans cloud)
- Prérequis: un navigateur moderne (Chrome, Edge, Safari, Firefox)
- Étapes:
  1. Ouvrir le fichier `index.html` en double-cliquant (ou via "Ouvrir avec…" votre navigateur)
  2. Ajouter des tâches via le champ en haut
  3. Glisser-déposer les tâches entre colonnes (À Trier, Do/Plan/Déléguer/Éliminer)
  4. Sélectionner vos 3 priorités (bouton "Choisir mes 3 victoires") puis utiliser le minuteur 120s
- Persistance: vos données sont enregistrées automatiquement dans `localStorage` (navigateur)

## 2) Activer la synchronisation Supabase (optionnel)
- Suivre le guide `SUPABASE_GUIDE.md` (déjà inclus dans le projet):
  - Créer le projet Supabase (gratuit)
  - Créer les tables `tasks` et `settings` + activer RLS et Realtime
  - Récupérer `supabaseUrl` et `supabaseAnonKey`
- Dans `index.html`, remplacer les placeholders:
```js
const supabaseUrl = 'https://YOUR_PROJECT_ID.supabase.co';
const supabaseAnonKey = 'YOUR_ANON_KEY';
```
- Ouvrir `index.html` et vérifier:
  - Les tâches s’affichent depuis la base
  - Les modifications (ajout, ordre, statut, suppression) se synchronisent
  - Le Top 3 est stocké dans `settings` (clé `top3Tasks`) et se met à jour en temps réel

## 3) Fonctionnalités principales
- Saisie rapide: tapez une tâche et pressez Entrée
- Matrice d’Eisenhower: organisez via drag-and-drop
- Top 3 du jour: sélection, affichage, re-sélection à tout moment
- Minuteur Zeigarnik 120s: démarrer/arrêter, notification sonore + toast à la fin
- Suppression de tâche: bouton corbeille dans chaque carte
- Icônes: Lucide (chargées automatiquement)

## 4) Raccourcis et astuces
- Entrée: ajoute la tâche courante
- Re-choisir vos priorités: bouton “Re-choisir” dans la section Top 3
- Une seule session de minuteur à la fois (prévention incluse)

## 5) Dépannage
- Rien ne s’affiche: ouvrir `v0.html` directement dans le navigateur (pas besoin de serveur)
- Icônes manquantes: vérifier votre connexion internet (CDN). Rafraîchir la page
- Supabase ne se synchronise pas:
  - Vérifier `supabaseUrl` et `supabaseAnonKey`
  - Activer Realtime sur `public.tasks` et `public.settings`
  - RLS: en usage solo, policies “public_*” du guide doivent être actives
- Ordre incohérent: faites un glisser-déposer pour forcer la réindexation

## 6) Sécurité et multi-utilisateur (optionnel)
- Pour un usage à plusieurs, activer Auth (Supabase) et associer un `user_id` aux tâches, puis restreindre les policies par `auth.uid()` (voir suggestions en fin de `SUPABASE_GUIDE.md`)

## 7) Structure du projet
- `index.html`: application complète (UI + logique + intégrations)
- `SUPABASE_GUIDE.md`: guide de configuration Supabase
- `README.md`: ce manuel de lancement

Bon usage et belles victoires du jour ! 