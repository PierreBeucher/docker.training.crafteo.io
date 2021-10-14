# Self-hosted Docker Registry


Cette série d'exercice nous permettra de déployer notre propre registry Docker.

Le Registry Docker se déploie sous forme d'un container Docker, par exemple pour déployer une registry sur votre machine écoutant sur le port 8080:

```
docker run -d -p 8080:5000 --name registry registry:2
``` 

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

- Lancer une registry locale
- Builder et pusher les images `vote`, `service` et `worker` sur votre registry locale
  - Votre registry est accessible via `localhost:8080` ou `127.0.0.1:8080`, il faudra donc nommer vos images en conséquence! 
- Une fois pushée, supprimez vos images `vote`, `service` et `worker` locales et les pusher de la registry locale

Si votre machine est accessible via un nom de domaine, par exemple `registry.docker.crafteo.io`, elle sera accessible à cette adresse. Le port par défaut est `443`. 
  
