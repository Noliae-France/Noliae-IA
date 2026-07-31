# Journal des changements

## [Unreleased]

- Refonte du chat : historique, envoi/réception, blocs de code avec
  coloration syntaxique légère, indicateur de réflexion (3 points).
- Chaque conversation a sa propre URL (`/chat/:id`), `/chat/new` pour une
  nouvelle discussion, navigation précédent/suivant du navigateur prise en
  charge.
- Partage public en lecture seule d'une conversation (`/chat/:id?public=1`),
  consultable sans connexion.
- Sélecteur de modèle par pastilles de fournisseur (ChatGPT, Claude, Mistral,
  Gemini, Grok, DeepSeek), avec popup de choix du modèle précis.
- Pièce jointe : documents texte (contexte RAG) et désormais images
  (png/jpg/webp/gif), uploadées directement vers S3 via URL présignée et
  insérées en markdown dans le message.
- Vrai dark mode natif (sans librairie) : clair / sombre / OLED (noir pur),
  détection automatique puis mémorisation du choix, bascule dans la barre du
  haut.
- Vendoring de Bulma comme socle CSS, sous la charte Pulse existante.
- Mise en cache localStorage de l'historique des conversations et du
  catalogue de modèles pour éviter le clignotement au rechargement de page.

[Unreleased]: https://github.com/Noliae-France/Noliae-IA/commits/main
