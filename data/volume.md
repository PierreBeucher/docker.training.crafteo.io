# Docker Volumes

Exercices sur les **volumes** Docker. Quelques rappels: 

```
# as usual
docker volume --help
docker volume create --help

# create a volume named myvolume
docker volume create myvolume

# run a container and attach a volume
docker run -v myvolume:/data some_image
```

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
    image: registry.gitlab.com/crafteo/training/example-voting-app/result
    ports:
      - "5001:80"
      - "5858:5858"

  vote:
    container_name: vote
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker
```

## Exercices

*Besoin: vous souhaitez gérer les données Postgres via un volume plutôt qu'un bind mount afin de limiter l'accès aux données depuis la machine locale et faciliter les procédures de backup*


Modifier `docker-compose.yml` pour:

- configurer un volume `psqsl-data` et
- monter ce volume à l'emplacement `/var/lib/postgresql/data` du service `db`
- redémarrer le service `db` pour prendre en compte les modifications

Une fois effectué:

- Lister les volumes existants et trouver le volume créé par Docker Compose
- Trouver l'emplacement des données du volume sur le disque local

---

*Besoin: des données Redis sont présentes sur un volume existant et vous souhaitez les intégrer à votre stack Compose*

- créer un volume `redis-data` avec la CLI Docker
  - Pour l'exercice, on considérera que ce volume contiens nos données :)
- modifier `docker-compose.yml` pour:
  - configurer un volume `redis-data` **externe** 
  - monter ce volume à l'emplacement `/data`  
  - redémarrer le service `redis` pour prendre en compte les modifications 
  
---

Cleanup: supprimer la stack Docker Compose et les volumes utilisés par Redis et Postgre 