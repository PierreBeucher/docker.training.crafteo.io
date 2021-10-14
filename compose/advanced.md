# Docker Compose - ateliers avancés

Les exercices se déroulent à partir du fichier `docker-compose.yml` suivant:

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
    image: crafteo/example-voting-app-result
    build:
      context: result/
    ports:
      - "5001:80"
      - "5858:5858"

  vote:
    container_name: vote
    image: crafteo/example-voting-app-vote
    build:
      context: vote/
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: crafteo/example-voting-app-worker
    build:
      context: worker/
      dockerfile: Dockerfile.j
      args:
        JAVA_VERSION: 9-jre
```

## Exercices

Configuration de Volumes:

- Ajouter un volume `db-data`
- Configurer le service `db` pour utiliser le volume `db-data` à l'emplacement `/var/lib/postgresql/data`
- Configurer le service `redis` pour monter le dossier local `/tmp/redis-data` à l'emplacement `/data` du container et relancer la stack


---

Configuration de Réseaux:

- Ajouter un réseau `app-network`
- Configurer l'ensemble des services pour que les containers soient associés au réseau `app-network` et relancer la stack

---

Docker Compose permet de définir un fichier de variable d'environnement global qui sera affecté à chaque container. 

- Trouver dans la documentation la fonctionnalité correspondante
- Remplacer les variables d'environnement du service `db` dans `docker-compose.yml` et par le fichier de variable d'environnement utilisé par Compose

--- 

Docker Compose permet d'utiliser plusieurs fichiers de configuration `.yml` pour étendre une configuration de base.

- Trouver dans la documentation la référence à ce méchanisme d'extention des configurations
- Créer un fichier `docker-compose.dev.yml` étendant `docker-compose.yml` tel que:
  - le service `vote` monte le code source `vote/` à l'emplacement `/app`
  - le service `vote` utilise la commande `python -m flask run -h 0.0.0.0 -p 80`
  - le service `vote` dispose des variables d'environnements:
    ```
    FLASK_APP=app.app
    FLASK_ENV=development
    ```
- Relancer la stack en combinant `docker-compose.yml` et `docker-compose.dev.yml`
- Renommer `docker-compose.dev.yml` de façon à pouvoir lancer la stack avec `docker-compose.yml` et le fichier d'override avec uniquement la commande `docker-compose up -d` sans aucune autre option

Cette configuration permet de monter le code source de l'application Vote (Python) directement dans le container et de relancer l'application dynamiquement au sein du container sans avoir à re-builder ou redémarrer le container.

Cette méthode utilise les fonctionnalités de Flask (serveur web Python) mais la plupart des frameworks permettent un setup similaire pour les environnement de développement.

Il est ainsi possible de définir une configuration de base Docker Compose avec des overrides selon différent contextes (dev, CI, production, etc.)

---

Docker Compose permet de définir des variables qui seront substitués aux variables d'environnement.

- Modifier `docker-compose.yml` pour définir la version du service `redis` pour qu'elle utilise la variable d'environnement `REDIS_VERSION` si possible, et `alpine` par défaut:
- Définir la variable d'environnement `REDIS_VERSION=5.0.7-alpine` et relancer la stack. Vérifier que le container `redis` utilise l'image `redis:5.0.7-alpine`

