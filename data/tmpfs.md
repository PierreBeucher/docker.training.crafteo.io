# Montage `tmpfs`

Exercices sur le montage de volume de type *tmpfs**

Les exercices utiliseront Docker Compose avec le fichier `docker-compose.yml` suivant:

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

  vote:
    container_name: vote
    image: crafteo/example-voting-app-vote
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: crafteo/example-voting-app-worker
```

## Exercices

*Contexte: les données Redis sont dans notre cas considérées comme éphémères et ne doivent pas être persistées sur le disque ou dans le writable layer du container. De plus, pour optimiser les performances les données Redis doivent être stockées en mémoire directement.*


Modifier `docker-compose.yml` pour:

- configurer un volume de type `tmpfs` sur le service `redis` à l'emplacement `/data`
- limiter la taille du volume à `500Mo`
- redémarrer le service pour prendre en compte les modifications

Lancer une session shell dans le container `redis` et:

- créer un fichier test 
  ```
  echo test > /data/test 
  ```
- redémarrer le container
- vérifier l'éxistence du fichier précédemment créé

