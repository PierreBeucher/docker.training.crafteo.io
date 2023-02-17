# Bind Mount avancé

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

Lancer la stack avec:

```
docker compose up -d
```

# Exercices

*Besoin: afin de conserver les données PostgreSQL sur la machine locale même si le container est supprimé et pour faciliter le processus de backup, vous cherchez une solution pour que les données de la BDD soient persistées. Un **Bind Mount** vous parait une bonne solution.*  

- Configurer le service `db` pour monter le dossier local `/home/ubuntu/db-data` à l'emplacement `/var/lib/postgresql/data` 
- Redémarrer le container de la stack pour prendre en compte les configurations
- Observer le contenu du dossier `/home/ubuntu/db-data`

---

*Besoin: vous devez configurer plus finement la base de donnée via un fichier de configuration `postgresql.conf` fourni par votre administrateur système et qui doit être utilisé par la BDD:*

```
# Full content of postgresql.conf to be mounted in container
listen_addresses = '*'
temp_file_limit = 1000
```

- Trouver l'emplacement auquel le fichier doit être monté dans le container `db` basé sur l'image `postgres`
  - La documentation des images Docker disponible sur Docker Hub indique généralement ces use-cases typiques
  - Pour `postgres` il faudra passer un flag de configuration au binaire lancé au démarrage du container
- Monter le fichier de configuration via un Bind Mount avec les contraintes:
  - le fichier de configuration doit être monté en **read-only** (lecture seule)
  - Définir l'option *Bind Propagation* à `rprivate`
- Appliquer les modifications **sans redémarrer le container**, à la place *envoyer un signal `SIGHUP` au container pour reloader la configuration*

*De nombreux applicatifs utilisent `SIGHUP` pour recharger une configuration sans avoir à faire un redémarrage complet, cette feature n'est pas spécifique à Docker mais néanmoins très pratique.* 

---

*Besoin: vous souhaitez configurer une procedure de backup des données via un au container `postgres` qui fera un dump régulier de la BDD*


Lancer un container utilisant permettant d'effectuer un backup:

- utiliser la même image que le service `db`
- monter un dossier spécifique permettant de persister les backups (par ex. un bind mount `$PWD/backup:/backup`)
- supprimer automatiquement le container lorsqu'il s'arrêtera
- lancer directement la commande de backup, par exemple:
  ```
  PGPASSWORD=postgres pg_dumpall -h db -c -U postgres -w > /backup/my-backup.sql
  ```
    - Attention: pour résoudre le nom d'hôte `db` le container de backup doit se trouver sur le même réseau.

*Il est possible de monter le même dossier/fichier sur plusieurs containers, pratique dans divers situations comme le backup de données*

Bonus: configurer une tâche *cron* qui lancera toutes les heures notre backup. Pour créer une tâche cron:

```
# run cron editor for user
crontab -e

# specify cronjob to run every hour
0 * * * *   COMMAND 
```
