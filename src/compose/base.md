# Docker Compose - basics

Nous utiliserons l'application Example Voting App pour l'exercice. Elle se trouve dans le dossier `~/example-voting-app`.

Le fichier `docker-compose.yml` décrit l'ensemble des **services** définissant notre **stack Docker Compose**:

- Vote: permet de voter pour Chien ou Chat
- Result: permet de voir les résultats des votes
- Worker: Traite les votes pour les insérer en base de donnée
- Redis: gère les votes en cours via un système de queue
- DB (Postgres): stocke les résultats

Nous utiliserons cette application pour illustrer nos exemples et exercices.

Documentation de référence:

- [Compose file specs](https://docs.docker.com/reference/compose-file/)
- [Docker Compose CLI](https://docs.docker.com/reference/cli/docker/compose/)
  - Par ex, `service` et les sous-éléments sont documentés dans [_Services top-level elements_](https://docs.docker.com/reference/compose-file/services/)

**🔖 Conseil: Ajoutez des liens à fos favoris, ils seront utiles !**

_Note: la CLI `docker-compose` standalone a été dépréciée en faveur de `docker compose`_

---

## Exercices

### Lancement d'une stack Docker Compose

- Utiliser la CLI `docker compose` pour lancer la stack en mode détachée
  ```
  docker compose --help
  ```

Les services Vote et Result expose une interface web accessible depuis la machine locale une fois lancés. Trouver dans le fichier `docker-compose.yml` les bindings de ports utilisés pour y accéder via votre navigateur.

---

### Configuration de services Docker Compose

Modifier le service Redis pour:
 - Utiliser l'image `redis:8.8.0`
 - Nommer le container `my_redis` au démarrage
 - Ajouter une variable d'environnement `SKIP_FIX_PERMS=1`
   - Cette variable change le comportement de Redis au démarrage pour du setup de permissions. 
   - Vérifier que le container a bien pris en compte ce changement. Plusieurs méthodes sont possibles
  
### Fichier `.env` et variables d'environnement

Il est possible d'interpoler des variables d'environnements dans notre fichier `docker-compose.yml` à l'aide de fichiers `.env`. Par exemple:

```
# .env example
POSTGRES_USER: "postgres"
POSTGRES_PASSWORD: "postgres"
```

Créer un fichier `.env` et modifier le service `db` pour passer les variables d'environnement depuis celui-ci. Plusieurs méthodes possibles:
- Interpolation de variable
- Configuration du service `db` pour utiliser directement un ou plusieurs fichiers `.env` et passer les variables dans le container

### Healthcheck & depends on

Faisons en sorte de démarrer le service Worker après nous êtes assuré que la base de donnée Postgres soit disponible:

Ajouter une instruction `healthcheck` au service `db`:
- Faire un bind mount du script `resources/healthchecks/postgres.sh` dans le container
- Configurer `healthcheck` pour lancer le script toutes les 5s afin de vérifier la "healthiness" du service
