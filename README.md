<div align="center">

# Noliae IA

### L’espace conversationnel Noliae, en Nolc MVC

[![CI](https://github.com/Noliae-France/Noliae-IA/actions/workflows/ci.yml/badge.svg)](https://github.com/Noliae-France/Noliae-IA/actions/workflows/ci.yml)
[![Container](https://github.com/Noliae-France/Noliae-IA/actions/workflows/container.yml/badge.svg)](https://github.com/Noliae-France/Noliae-IA/actions/workflows/container.yml)
[![Runtime](https://img.shields.io/badge/runtime-Nolc-FF4D2E)](https://github.com/Noliae-France/nolc)

</div>

Noliae IA est l’interface conversationnelle de **`ia.noliae.com`**. Elle est
construite avec Nolc et des vues `.nhtml` rendues côté serveur : pas de Node.js,
pas de framework frontend et pas de copie des connecteurs IA dans le navigateur.

## Expérience produit

Le produit reprend la charte Pulse de Noliae : topbar sombre, identité waveform,
navigation latérale dense, typographie mono pour les repères et Vermillon pour
les actions et états actifs. Le rendu est responsive et conserve une vraie
structure applicative, pas une landing page.

## Architecture MVC

```text
main.nol              Contrôleurs, routage et boucle HTTP Nolc
views/chat.nhtml      Vue source de la conversation
views/chat.nol        Vue transpilée utilisée par le binaire
static/noliae.css     Design system Noliae Pulse
vendor/nolc/lib/      Runtime web Nolc versionné pour les builds
```

| Route | Rôle |
|---|---|
| `GET /` | Espace de discussion et choix du modèle |
| `POST /discussion` | Contrôleur de message, point de raccordement au Core |
| `GET /api/health` | Probe d’exécution |

Le contrôleur discussion est le point d’intégration vers
[NolCore-API](https://github.com/Noliae-France/NolCore-API), qui garde les
sessions et délègue ensuite à [NolCore-IA](https://github.com/Noliae-France/NolCore-IA).
Les tokens Claude, ChatGPT, Mistral et Gemini ne passent jamais dans cette UI.

Le catalogue de modèles est fourni par `GET /v1/ia/models` via le Core ; l’UI
ne déclare aucun modèle ni tarif fictif. Les conversations et quotas restent
des responsabilités du Core et de PostgreSQL.

## Développement

```sh
nolc nhtml views/chat.nhtml
nolc check main.nol
nolc build main.nol -o noliae-ia-web --lien ssl --lien crypto
NOLIAE_PORT=8080 ./noliae-ia-web
```

## Livraison

```sh
docker build -t noliae-ia-web .
docker run --rm -p 8080:8080 noliae-ia-web
kubectl apply -f deploy/k8s.yaml
```

Le manifeste Kubernetes déploie deux réplicas, un Service et l’Ingress
`ia.noliae.com`. Le DNS, le TLS et le proxy vers NolCore sont configurés dans
l’infrastructure de production qui héberge le domaine.

## Navigation multi-domaines

Les destinations Search, Account, Login et Register sont calculées depuis le
hostname actif. Une préproduction `ia.beta.noliae.com` reste ainsi dans
`*.beta.noliae.com` sans lien codé en dur vers la production.

## CI/CD lié au Core

La CI compile les `.nhtml`, construit l’image native et smoke-teste la route de
santé. L’image publiée est `ghcr.io/noliae-france/noliae-ia-web:main`.
Le workflow d’intégration du dépôt [NolCore](https://github.com/Noliae-France/NolCore)
clone et démarre les deux interfaces pour éviter toute dérive du contrat web.

## Licence

Distribué sous [licence MIT](LICENSE).
