# Docker: quelques bonnes pratiques!

Quelques exercices sur les bonnes pratiques avec Docker: limitation de ressources, healthcheck, logging...

Les exercices utiliserons le `docker-compose.yml` suivant:

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

Se documenter sur les méthodes de limitation de ressources pour les containers Docker puis modifier le `docker-compose.yml` pour limiter la consommation de chaque service:

- Maximum de 0.2 CPU et 256Mo RAM par service
- Réservation de 0.1 CPU 128Mo de RAM par service

*Note: Docker Compose v3 ignore par défaut les paramètres `deploy.resources` qui sont réservés à Docker Swarm. Pour les prendre en compte, utiliser `--compatibility` ou un fichier compose `version: "2"`*

---

Docker permet de configurer des drivers de Logging. Faire quelques recherches sur les configuration de logging possible avec Docker puis:

- Lancer un container `fluent/fluentd` écoutant montant le volume `/tmp/docker-logs:/fluentd/log`
   ```
   docker run -d --rm --name fluentd -v /tmp/docker-logs:/fluentd/log fluent/fluentd
   ```
- Configurer le service `db` pour utiliser le driver `fluentd` afin d'envoyer les logs vers le container `fluentd`
  - Utiliser `docker inspect` pour obtenir l'IP du container `fluentd`
  - Ne pas utiliser le nom du container directement qui ne sera pas reconnu 
- Lancer la stack ainsi configurée

Vérifier l'existence des logs dans `/tmp/docker-logs`



 