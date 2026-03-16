## Ultimate Foot Quiz – Europe & NBA

Application de quiz sportive optimisée pour mobile, avec deux catégories :

- **⚽ Football Européen** (`questionsFoot`)
- **🏀 NBA Legends** (`questionsBasket`)

Le jeu est en **mode sombre** avec une UI moderne (design basée sur `design.json / Quizio UI Kit`), un système de **vies (3 coeurs)**, un **shuffle complet des questions et des options** et un mode **Premium** déverrouillable via Instagram.

---

### Fonctionnalités principales

- **Sélection du terrain**
  - Écran d’accueil `sport-select-screen` : choix entre *Football Européen* et *NBA Legends*.
  - Le choix est mémorisé dans `localStorage` (`ufq_mode`).

- **Deux banques de questions**
  - `questionsFoot` : base de nombreuses questions sur le football européen (LDC, Euros, clubs, années 90/2000, bloc expert, etc.).
  - `questionsBasket` : base de nombreuses questions NBA (MVP, records, drafts, busts, franchises, nationalités, années 90/2000/2010, niveau moyen à très difficile).
  - **Aucune ancienne question n’est supprimée**, les nouveaux items sont ajoutés à la suite.

- **Logique de jeu**
  - **3 vies** (3 coeurs) par partie.
  - **+10 points** par bonne réponse.
  - **Perte d’une vie** à chaque mauvaise réponse ou timeout.
  - Quand `lives <= 0` → **Game Over**.
  - Score, vies et Premium sont persistés dans `localStorage` :
    - `ufq_score`
    - `ufq_lives`
    - `ufq_is_premium`
    - `ufq_mode`

- **Système de vies (coeurs)**
  - Affichage dynamique via `updateLivesDisplay()` :
    - Coeur normal = vie disponible.
    - Coeur **grisé + réduit** (`.lost`) = vie perdue.
    - Quand il ne reste **qu’une vie**, le dernier coeur **pulse en orange** (`.lives.low`) pour signaler le danger.

- **Mélange (shuffle)**
  - Fonction **`shuffleArray(array)`** (Fisher–Yates) pour :
    - Mélanger **l’ordre des questions** du mode sélectionné.
    - Mélanger **les options de réponse** à l’intérieur de chaque question.
  - À chaque **choix de mode** (Foot / Basket) et **à chaque nouvelle partie**, un nouvel ordre est généré.
  - La bonne réponse n’est jamais toujours en “A” : elle est identifiée par `dataset.correct="true"` sur le bouton, indépendamment de la position.

- **UX & UI**
  - Mode sombre, fond en dégradé **noir → vert forêt** avec touches violettes/orange.
  - Carte de question sombre, arrondie, avec ombres douces.
  - Barre de progression, badges de score/vies/question.
  - Timer par question (25 s) avec affichage lisible.
  - Toasts pour feedback (“Bonne réponse…”, “Mauvaise réponse…”, “Temps écoulé…”).

- **Mode Premium via Instagram**
  - Bouton “S’abonner pour vies INFINIES”.
  - Ouverture du compte Instagram : `https://www.instagram.com/emma_lm_4`.
  - Flow de vérification (timer + retour d’onglet) qui active `ufq_is_premium = true`.
  - Premium est **partagé** entre Foot & NBA.

- **Paiement (optionnel)**
  - Intégration Flutterwave inline (`makePayment()`) pour acheter des vies supplémentaires.
  - Le bouton visuel “Acheter 3 vies” a été **supprimé de l’UI**, mais la logique reste dans le script si besoin de la réactiver.

---

### Structure des fichiers

- `index.html`  
  Contient :
  - Le markup complet de l’application (sélection de mode, écrans de jeu, Game Over, toasts).
  - Les styles inline (mode sombre, layout mobile-first).
  - Le script JS (banques de questions, logique de jeu, shuffle, timer, vies, Premium, paiement).

- `neon-ui.html` / `neon-ui.css`  
  Maquette séparée qui illustre un écran de quiz **neon dark mode** inspiré d’un design type Dribbble (compte à rebours circulaire, boutons de réponses avec états correct/incorrect, bouton Next orange).  
  **Cette maquette est purement visuelle** et ne modifie pas le jeu principal.

- `design.json`  
  Spécification du design system utilisé (Quizio UI Kit), couleurs, typographie, rayons, ombres, etc.  
  `index.html` suit ces tokens adaptés en mode sombre.

---

### Lancer le projet en local

1. Cloner le dépôt ou copier les fichiers dans un dossier (par ex. `c:\Emma\Business\jeu`).
2. Ouvrir `index.html` dans un navigateur récent (Chrome, Edge, Firefox).
3. Optionnel : utiliser une extension de type **Live Server** pour recharger automatiquement lors des modifications.

---

### Notes de développement

- Le projet ne dépend d’aucun bundler : **HTML + CSS + JS** pur.
- Le code est pensé **mobile-first** (viewport 480px, layout centré).
- Le système de stockage `localStorage` peut être réinitialisé en vidant les données du site dans les DevTools si nécessaire.
