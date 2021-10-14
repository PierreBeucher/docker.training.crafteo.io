# Docker Hub - Registry Docker officielle


Cette série d'exercice démontrera l'usage de Docker Hub, la registry officielle. Nous allons créer un compte, s'authentifier avec la CLI `docker` et pusher nos imagees buildées localement directement sur la registry.  

Les exercices utiliserons le `docker-compose.yml` suivant:

```
version: "3.7"

services:
  db:
    container_name: db
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"

  redis:
    container_name: redis
    image: redis:alpine

  result:
    container_name: result
    image: crafteo/example-voting-app-result
    ports:
      - "5001:80"
      - "5858:5858"
    build:
      context: result/

  vote:
    container_name: vote
    image: crafteo/example-voting-app-vote
    ports:
      - "5000:80"
    build:
      context: vote/

  worker:
    container_name: worker
    image: crafteo/example-voting-app-worker
    build:
      context: worker/
```

## Exercices

Exploration de la Registry Docker

- Aller sur [hub.docker.com](https://hub.docker.com/), site de la Registry Docker officielle
- Trouver l'image `httpd` (image officielle Apache2)
- Identifier les tags disponibles pour l'image `httpd`
- Trouver quelle version d'Apache est actuellement disponible avec l'image taggée `alpine` (i.e. la version complète comme `2.4.3` ou`2.1.6`)

---

Nous avons buildés nos images Example Voting App, nous allons maintenant pouvoir les pusher sur la Registry!

- Avec votre compte, créer un repository sur Docker Hub nommé `voting-app-vote` qui sera utilisé pour héberger l'image `vote`
- Modifier `docker-compose.yml` pour que l'image `vote`, `worker` et `result` buildée soit nommée selon votre registry
- Utiliser `docker-compose` pour pusher l'image `vote` dans votre domaine Docker Hub
  - il sera nécéssaire de s'authentifier sur la registry pour effectuer cette action
- Utiliser `docker-compose` pour pusher l'image `worker` dans votre domaine Docker Hub - pour lequel vous n'avez pas créé de repository pour l'instant
- Utiliser la CLI `docker` pour pusher l'image `result`
- Modifier `docker-compose.yml` pour tagger les images `1.0` et pusher toutes les images en une seule commande

---

Les images pushées sur la registry peuvent maintenant être utilisée publiquement. Il est aussi possible de les rendre privées, auquel cas il sera obligatoire de s'authentifier sur la registry avant de pouvoir les puller. 

- Essayer de puller les images construires et pushées depuis la registry de votre voisin
- Essayer de pusher une image construite par vous-même sur la registry de votre voisin