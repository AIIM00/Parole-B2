# Parole B2 — Du B1 vers le B2, un jour à la fois

Application autonome de préparation à la **production orale du DELF B2**. Interface et explications grammaticales en français, corrections **après** la conversation, 30 minutes par jour : 7 min de cartes, 15 min d'oral, 5 min de bilan, 3 min de leçon.

## Démarrer

Prérequis : **Node.js 20 ou plus récent**. Aucune installation de dépendances et aucun compte requis.

```bash
cd parole-b2
npm start
```

Ouvrir **http://localhost:3000** dans Chrome ou Edge. Sur iPhone, le navigateur ne prend pas toujours en charge l'API de reconnaissance vocale utilisée ; la saisie au clavier reste disponible. L'accès au microphone nécessite généralement HTTPS ou localhost. Pour un téléphone sur un réseau local, utilisez un hébergement HTTPS.

### Activer l'IA adaptative (facultatif)

Par défaut, l'application utilise des questions préparées et un bilan local limité, sans inventer de note ni d'erreurs. Pour activer **les questions adaptées à vos réponses et une analyse détaillée de votre expression**, configurez la variable d'environnement `OPENAI_API_KEY` **sur votre serveur** (pas dans le navigateur) :

macOS / Linux :
```bash
export OPENAI_API_KEY="votre_cle_api"
export OPENAI_MODEL="gpt-4o-mini"
npm start
```

Windows PowerShell :
```powershell
$env:OPENAI_API_KEY="votre_cle_api"
$env:OPENAI_MODEL="gpt-4o-mini"
npm start
```

**Remarque :** Le fichier `.env.example` documente les variables, mais le serveur sans dépendances ne lit pas automatiquement un fichier `.env`. Définissez des variables d'environnement dans votre terminal ou l'interface d'hébergement. Les appels API peuvent entraîner des frais auprès du fournisseur. La clé n'est jamais envoyée au navigateur.

## Fonctionnalités

- 30 sujets et parcours en quatre semaines pour passer de B1 à B2.
- 11 à 12 flashcards par sujet, définitions et exemples en français, audio vocal natif, cartes connues et à revoir.
- Conversation guidée avec Camille, reconnaissance vocale `fr-FR` quand le navigateur le permet, transcription modifiable, alternatives de saisie clavier.
- Modes monologue et examen blanc : préparation 30 min, exposé 5–7 min, débat 10–13 min.
- Corrections grammaticales **uniquement à la fin** de l'échange. Le bilan local hors IA ne reconnaît qu'une courte liste d'erreurs identifiables, sans prétendre évaluer tout le français.
- Leçons issues de vos erreurs, exercices et nouvelle prise de parole.
- Historique, progression, révision du vocabulaire, export et suppression des données locales.
- Pas d'authentification : données personnelles conservées dans le stockage du navigateur. Changer d'appareil, de navigateur, ou vider le stockage supprime cet historique sans synchronisation.

## Confidentialité et évaluation

Les transcriptions sont envoyées au fournisseur d'IA **uniquement** lorsque le serveur est configuré avec une clé. La reconnaissance vocale peut elle-même utiliser un service extérieur, selon le navigateur et le système. Aucun enregistrement audio n'est stocké par cette application. Il est impossible d'évaluer correctement la prononciation depuis une transcription : ce critère reste **Non évalué**. Une note /25 n'est pas calculée tant que des critères sont absents. Ce logiciel n'est pas affilié à France Éducation international.

Grille officielle : https://www.france-education-international.fr/document/grille-po-b2

## Architecture

- `index.html` et `styles.css` : interface responsive.
- `content.js` : programme 30 jours, expressions, exemples du diagnostic B1.
- `app.js` : parcours complet, navigation et stockage local, microphone, flashcards, exercices, statistiques.
- `server.mjs` : serveur Node sans dépendances et passerelle IA sécurisée. Routes `/api/status`, `/api/examiner`, `/api/evaluate`.

## Vérification

```bash
npm run check
```

Tester les boutons de carte, conversation avec saisie clavier, bilan, exercice et historique avant un usage prolongé. L'analyse IA nécessite une clé et un accès réseau au fournisseur ; elle n'est pas garantie sans eux.

## Hébergement sur Render (iPhone / Safari)

1. Créez un dépôt GitHub dédié `parole-b2`, sans inclure de secrets. Placez les fichiers de ce dossier à la racine du dépôt (les fichiers `server.mjs`, `app.js`, `index.html`, etc.).
2. Sur Render, créez un **Web Service** à partir de ce dépôt. Choisissez **Free**, région **Frankfurt** (ou la plus proche), branche `main`, runtime **Node**, build `npm run check`, démarrage `npm start`.
3. Ajoutez `APP_ACCESS_CODE` comme variable d'environnement avec un mot de passe long et unique. Lors de l'ouverture du site, Safari demandera un nom d'utilisateur et un mot de passe : utilisateur `parole`, mot de passe = `APP_ACCESS_CODE`. Ne commitez pas cette valeur.
4. Quand vous avez une clé API OpenAI, ajoutez-la dans Render > Service > Environment, variable `OPENAI_API_KEY`. Ne la placez pas sur GitHub. Si vous ne l'ajoutez pas, l'application reste accessible en mode questions préparées.
5. Render vous attribue une URL HTTPS `https://votre-service.onrender.com`, compatible avec l'accès au microphone par le navigateur lorsque ce dernier prend en charge la reconnaissance vocale. Dans Safari, choisissez Partager > **Sur l'écran d'accueil** pour une icône pratique.

**Protection de coûts :** avec `APP_ACCESS_CODE`, le site et les routes API sont protégés par authentification HTTP Basic. Les requêtes d'analyse IA sont également limitées par adresse IP. Un code d'accès solide et des limites de dépenses dans votre compte API restent essentiels.

**Offre gratuite :** Render peut arrêter le serveur après 15 minutes sans visite et le redémarrage peut prendre environ 1 minute. La progression est stockée **dans le navigateur de l'appareil** : celle de votre iPhone ne se synchronise pas avec celle de votre ordinateur.
