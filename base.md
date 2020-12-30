# Docker - basics


```
docker --help|-h
docker [COMMAND] --help
docker run --help
docker image -h

# Run a container
docker run [OPTIONS] IMAGE

# List existing containers
> docker ps [-a]

# Show container logs
> docker logs [OPTIONS] CONTAINER

# start, stop, restart containers
> docker start|stop|restart CONTNER
```

## Exercices

Lancer un container utilisant l'image **`httpd:alpine`** (image officielle Apache HTTP server) **en mode détaché (daemon)** et le **nommer `myapache`**

- `docker run` devra rendre la main et afficher un ID de container

---

Vérifier que `myapache` **existe et est actif**

- Le container doit être au status `Up`

---

Afficher les **10 dernières lignes** de logs de myapache et **suivre les changements**

---

**Arrêter** puis **redémarrer** le container `myapache`

- Plusieurs solutions possible utilisant le même set de commandes

---

**Supprimer** le container `myapache`

- Des actions préalables peuvent être requises
