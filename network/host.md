# Host networking

Quelques exercices sur le networking Host avec Docker. Utilisent le fichier `docker-compose.yml` suivant:

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

  result:
    container_name: result
    image: registry.gitlab.com/crafteo/training/example-voting-app/result
    ports:
      - "5001:80"

  vote:
    container_name: vote
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker
```

# Exercice

Configurer `docker-compose.yml` pour:

- Utiliser le network `host` pour chacun des services
- Lancer la stack et vérifier que chacun des services est `Up`
- Se connecter au service Vote et Worker pour vérifier leur fonctionnement
- Trouver l'adresse IP du container `worker` - si elle existe

---

- Identifier le réseau Docker utilisant le driver `host`
- Essayer de créer un autre réseau Docker utilisant le driver `host` 
