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

*Besoin: afin de conserver les données PostgreSQL sur la machine locale même si le container est supprimé et pour faciliter le processus de backup, vous cherchez une solution pour que les données de la BDD soient persistées. Un **Bind Mount** vous parait une bonne solution.*  

- Configurer le service `db` pour monter le dossier local `/home/ubuntu/db-data` à l'emplacement `/var/lib/postgresql/data` 
- Redémarrer le container de la stack pour prendre en compte les configurations
- Observer le contenu du dossier `/home/ubuntu/db-data`

---

*Besoin: vous devez configurer plus finement la base de donnée via un fichier de configuration `postgresql.conf` fourni par votre administrateur système et qui doit être utilisé par la BDD:*

```
listen_addresses = '*'
temp_file_limit = 1000
```

- Trouver l'emplacement auquel le fichier doit être monté dans le container `db` basé sur l'image `postgres`
- Monter le fichier de configuration via un Bind Mount avec les contraintes:
  - le fichier de configuration doit être monté en **read-only** (lecture seule)
  - Définir l'option *Bind Propagation* à `rprivate`
- Appliquer les modifications **sans redémarrer le container**, à la place **envoyer un signal `SIGHUP` au container pour reloader la configuration

*De nombreux applicatifs utilisent `SIGHUP` pour recharger une configuration sans avoir à faire un redémarrage complet, cette feature n'est pas spécifique à Docker mais néanmoins très pratique.* 

---

*Besoin: vous souhaitez configurer une procedure de backup des données via un au contrainer `postgres` qui fera un dump régulier de la BDD*


Configurer une tâche *cron* qui lancera toutes les heures un container tel que:

- Utilise la même image que le service `db`
- Est supprimé automatiquement lorsque le container s'arrête
- Monte les données de la BDD via un bind mount
- Monte un dossier spécifique permettant de persister les backups 
- Lance une commande de backup, par exemple:
    ```
    pg_dumpall -c -U postgres > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql
    ```

*Il est possible de monter le même dossier/fichier sur plusieurs containers, pratique dans divers situations comme le backup de données*