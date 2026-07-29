# Noliae IA

Interface MVC de `ia.noliae.com`, écrite entièrement en **Nolc** : routeur et
contrôleurs `.nol`, vues serveur `.nhtml`, assets CSS de la charte Noliae.

```sh
nolc nhtml views/chat.nhtml
nolc check main.nol
docker build -t noliae-ia-web .
docker run --rm -p 8080:8080 noliae-ia-web
```

Routes : `/`, `POST /discussion`, `GET /api/health`. La CI compile les vues et
le binaire Nolc, puis publie `ghcr.io/noliae-france/noliae-ia-web:main`.
Le raccordement API réel se fait via le NolCore derrière l’Ingress de
`ia.noliae.com`, afin de conserver une session first-party. Le manifeste
`deploy/k8s.yaml` livre le Deployment, Service et Ingress du sous-domaine.
