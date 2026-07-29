# Noliae IA

Interface conversationnelle de `ia.noliae.com`, écrite en HTML/CSS/JavaScript
sans Node.js. Elle applique la charte Pulse de Noliae et délègue toutes les
requêtes au [NolCore API](https://github.com/Noliae-France/NolCore-API).

```sh
docker build -t noliae-ia .
docker run --rm -p 8082:8080 -e NOLIAE_API_BASE=https://api.noliae.com noliae-ia
```

Configurez le proxy de `ia.noliae.com` vers le conteneur et exposez le Core à
une URL API compatible CORS/cookies. La CI construit et smoke-teste l’image
GHCR à chaque push.
