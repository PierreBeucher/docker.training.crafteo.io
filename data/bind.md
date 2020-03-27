# Bind Mount avancé

Les exercices utiliseront Docker Compose avec le fichier `docker-compose.yml` suivant:

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

Lancer la stack avec:

```
docker-compose up -d
```

# Exercices

*Afin de conserver les données PostgreSQL sur la machine locale même si le container est supprimé et pour faciliter le processus de backup, vous cherchez une solution pour que les données de la BDD soient persistées. Un **Bind Mount** vous parait une bonne solution.*  

- Configurer le service `db` pour monter le dossier local `/home/ubuntu/db-data` à l'emplacement `/var/lib/postgresql/data` 
- Redémarrer le container de la stack pour prendre en compte les configurations
- Observer le contenu du dossier `/home/ubuntu/db-data`

---

*Vous devez configurer plus finement la base de donnée via un fichier de configuration `postgresql.conf` fourni par votre administrateur système et qui doit être utilisé par la BDD:*

```
listen_addresses = '*'
temp_file_limit = 1000
```

- Trouver l'emplacement auquel le fichier doit être monté dans le container `db` basé sur l'image `postgres`
- Monter le fichier de configuration via un Bind Mount avec les contraintes:
  - le fichier de configuration doit être monté en **read-only** (lecture seule)
  - Définir l'option *Bind Propagation* à `rprivate`
  

