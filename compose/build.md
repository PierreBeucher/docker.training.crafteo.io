# Build avec Docker Compose

Docker Compose permet de lancer le build d'images Docker en spécifiant le dossier de contexte, le Dockerfile, etc.

## Exercices  

A partir du Docker Compose suivant:

```
version: "3"

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
    
  worker:
    container_name: worker
    image: my-worker

  result:
    container_name: result
    image: my-result
    ports:
      - "5001:80"
      - "5858:5858"

  vote:
    container_name: vote
    image: my-vote
    ports:
      - "5000:80"
```

Configurer le build des services Vote, Result et Worker tel que:

- chaque service doit utiliser le cache de l'image `registry.gitlab.com/crafteo/training/example-voting-app/*` correspondante
  - `vote` doit utiliser le cache de `registry.gitlab.com/crafteo/training/example-voting-app/vote`
- le service Worker doit avoir comme argument de build `JAVA_VERSION=9-jre`
- chaque service doit avoir un label `app.service.name=[NOM DU SERVICE]`, i.e. le service Vote doit avoir un label `app.service.name=vote`