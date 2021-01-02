# Docker Compose - basics

Nous utiliserons l'application Example Voting App pour l'exercice, pour obtenir le code source:

```
git clone -b img-build https://github.com/PierreBeucher/example-voting-app.git
```

Le repository contiens un fichier `docker-compose.yml` avec l'ensemble des **services** composant notrer **stack Docker Compose**:

- Vote: permet de voter pour Chien ou Chat
- Result: permet de voir les résultats des votes
- Worker: Traite les votes pour les insérer en base de donnée
- Redis: gère les votes en cours via un système de queue
- DB (Postgres): stocke les résultats

Nous utiliserons cette application pour illustrer nos exemples et exercices.

Le fichier `docker-stack.yml` n'est pas utilisé pour le moment.

---

## Exercices

### Lancement d'une stack Docker Compose

- Utiliser la CLI `docker-compose` pour lancer la stack en mode détachée
  ```
  docker-compose --help
  ```

Les services Vote et Result expose une interface web accessible depuis la machine locale une fois lancés. Trouver dans le fichier `docker-compose.yml` les bindings de ports utilisés pour y accéder via votre navigateur.

---

### Configuration de services Docker Compose

- Modifier le service Redis pour:
 - Utiliser l'image `redis:6.0-alpine`
 - Nommer le container `my_redis` au démarrage
 - Ajouter une variable d'environnement `FOO=BAR` au runtime du container

### Fichier `.env` et variables d'environnement

Il est possible de référencer des variables d'environnements dans notre fichier `docker-compose.yml` et de spécifier un fichier `.env` contenant des variables d'environnements par défaut. Par exemple:

```
# .env example
DOTENV_POSTGRES_USER: "postgres"
DOTENV_POSTGRES_PASSWORD: "postgres"
```

Créer un fichier `.env` et modifier le service `db` pour utiliser les valeurs de ce fichier plutôt que des valeurs hardcodées.
