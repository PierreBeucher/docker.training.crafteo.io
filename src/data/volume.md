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

## Exercices

*Besoin: vous souhaitez gérer les données Postgres via un volume plutôt qu'un bind mount afin de limiter l'accès aux données depuis la machine locale et faciliter les procédures de backup*


Modifier `docker-compose.yml` pour:

- configurer un volume `psqsl-data` et
- monter ce volume à l'emplacement `/var/lib/postgresql/data` du service `db`
- redémarrer le service `db` pour prendre en compte les modifications

Une fois effectué:

- Lister les volumes existants et trouver le volume créé par Docker Compose
- Trouver l'emplacement des données du volume sur le disque local

Cleanup: supprimer la stack Docker Compose et les volumes utilisés par Postgres 