# Docker Compose CLI

De nombreuses commandes CLI `docker` ont leurs équivalents avec `docker-compose`.

Utiliser le fichier `docker-compose.ym` suivant:

```
version: "3"

services:
  db:
    container_name: db
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker

  result:
    container_name: result
    image: registry.gitlab.com/crafteo/training/example-voting-app/result
    ports:
      - "5001:80"
      - "5858:5858"

  redis:
    container_name: redis
    image: redis:alpine

  vote:
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:8080"
```

## Exercices 

Lancer la stack Compose en mode détaché

- tout comme `docker`, Docker Compose lance les containers en mode interactif par défaut

---

Lancer la stack puis modifier le fichier `docker-compose.yml` pour changer le port exposé de Result pour `5002`. Appliquer les changements à la stack avec une commande `docker-compose` 

--- 

Quelques manipulations:

- Arrêter, démarrer puis re-démarrer la stack
- Lister les images utilisées par la stack
- Lister les containers de la stack
- Afficher les logs de l'ensemble des containers de la stack
- Afficher les logs d'un container de la stack et suivre les changements
- Executer une session shell interactive dans le container `vote`

---

Supprimer l'ensemble des containers, volumes et réseaux de la stack avec une commande `docker-compose`

- il serait possible de passer par plusieurs commandes `docker`, mais `docker-compose` permet de le faire simplement 


--- 

Copier le fichier `docker-compose.yml` et nommer cette copie `docker-compose-bis.yml`. Lancer 2 stacks en parallèle, l'une utilisant `docker-compose.yml` et l'autre `docker-compose-bis.yml`

- plusieurs stacks peuvent coéxister en s'assurant qu'il n'y a pas de conflits de ports ou autre... 