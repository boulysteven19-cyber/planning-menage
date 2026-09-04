# Planning de ménage

Tableau de bord interactif pour gérer le planning de ménage d'un foyer :
tâches quotidiennes, hebdomadaires, mensuelles/trimestrielles, plus une vue
calendrier. Chaque tâche (y compris celles fournies par défaut) peut être
ajoutée, modifiée ou supprimée.

## Structure du projet

```
src/
  components/     Composants UI réutilisables (bouton, carte, formulaire, etc.)
  pages/          Une page par onglet (Accueil, Quotidien, Hebdo, Mensuel, Calendrier)
  hooks/          Logique d'état réutilisable (listes de tâches, cases cochées)
  utils/          Fonctions pures (dates, identifiants)
  data/           Données de départ (les tâches pré-remplies)
  theme/          Palette de couleurs et constantes partagées
  App.jsx         Assemble les pages et les hooks
  main.jsx        Point d'entrée React
public/
  manifest.json   Rend l'app installable sur iPhone ("Ajouter à l'écran d'accueil")
  icon-192.png / icon-512.png   Icônes de l'app
```

Aucun fichier ne dépasse ~150 lignes : chaque composant a une seule
responsabilité, ce qui rend le code facile à relire et à faire évoluer.

## Lancer le projet en local

Prérequis : [Node.js](https://nodejs.org) 18 ou plus récent.

```bash
npm install
npm run dev
```

Le site est alors disponible sur `http://localhost:5173`.

## Qualité de code

```bash
npm run lint     # ESLint : règles React, hooks et accessibilité (jsx-a11y)
npm run build    # Build de production — doit compiler sans erreur
```

Les deux commandes doivent se terminer sans erreur avant tout envoi en
production.

## État actuel : stockage en mémoire

Pour l'instant, les données (tâches, cases cochées, dernières dates de
réalisation) vivent uniquement dans la mémoire du navigateur : elles sont
perdues à la fermeture de l'onglet. C'est volontaire à ce stade — l'app
est fonctionnelle et testable, mais pas encore persistante.

**Prochaine étape recommandée** : brancher [Firebase](https://firebase.google.com)
(Firestore pour les données, Firebase Auth pour restreindre l'accès,
Firebase Cloud Messaging pour les notifications push) — la même stack
gratuite que celle déjà utilisée sur d'autres projets.

## Mettre le projet sur GitHub (dépôt privé)

1. Crée un dépôt sur [github.com/new](https://github.com/new), en cochant
   **Private**. Ne coche aucune case d'initialisation (pas de README, pas
   de `.gitignore`) puisque le projet en a déjà.
2. Dans ce dossier :

```bash
git init
git add .
git commit -m "Version initiale du planning de ménage"
git branch -M main
git remote add origin https://github.com/<ton-compte>/<nom-du-depot>.git
git push -u origin main
```

3. Pour donner accès à quelqu'un d'autre sur ce dépôt privé : sur GitHub,
   **Settings → Collaborators → Add people**.

## Déployer gratuitement (rendre l'app accessible par une URL)

Avec [Firebase Hosting](https://firebase.google.com/docs/hosting) (gratuit,
HTTPS automatique — nécessaire pour l'installation sur iPhone) :

```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # dossier public : "dist", app monopage : "Yes"
npm run build
firebase deploy
```

Une fois déployée, l'URL fournie s'installe sur iPhone via
**Safari → Partager → Sur l'écran d'accueil**.
