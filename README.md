# Parole B2

Préparation quotidienne à la production orale du DELF B2 (progression B1 → B2).

## Déploiement Render

Le projet exécutable complet se trouve dans `Parole-B2-Render-Ready.zip`. Le fichier ZIP doit être ajouté **à la racine** de ce dépôt, sans le renommer.

Le build Render extrait le dossier `parole-b2/`, vérifie le JavaScript puis lance le serveur Node.js.

- **Runtime** : Node
- **Plan** : Free
- **Build** : `unzip -o Parole-B2-Render-Ready.zip && cd parole-b2 && npm run check`
- **Start** : `cd parole-b2 && npm start`
- **Région** : Frankfurt
- **Variable facultative** : `OPENAI_MODEL=gpt-4o-mini`
- **Secret** : `OPENAI_API_KEY` (ne jamais ajouter au dépôt)
- **Protection recommandée** : `APP_ACCESS_CODE` (mot de passe utilisateur `parole`)

La version sans clé utilise des questions prédéfinies et une évaluation limitée. Les transcriptions et progrès restent dans le stockage local du navigateur.
