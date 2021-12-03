# Self-hosted Docker Registry


Cette série d'exercice nous permettra de déployer notre propre registry Docker.

Le Registry Docker se déploie sous forme d'un container Docker, par exemple pour déployer une registry sur votre machine écoutant sur le port 8080:

```
docker run -d -p 8080:5000 --name registry registry:2
``` 

## Exercices

- Lancer une registry locale
- Builder et pusher les images `vote`, `service` et `worker` sur votre registry locale
  - Votre registry est accessible via `localhost:8080` ou `127.0.0.1:8080`, il faudra donc nommer vos images en conséquence! 
- Une fois pushée, supprimez vos images `vote`, `service` et `worker` locales et les pusher de la registry locale

Si votre machine est accessible via un nom de domaine, par exemple `registry.docker.crafteo.io`, elle sera accessible à cette adresse.

Par défaut un Docker daemon utilisera TLS/HTTPS et une registry non sécurisée ne pourra pas être utilisée. Pour tester une registry non sécurisée, ajouter au fichier `/etc/docker/daemon.json`:

```
{
  "insecure-registries" : ["you.training.crafteo.io:8080"]
}
```

Et redémarrer Docker:

```
sudo systemctl restart docker
```
  
Puis puller une image tel que:

```
docker pull pierre.training.crafteo.io:8080/crafteo-worker:2021-12
```

Pour plus de détails, voir [la documentation officielle Docker](https://docs.docker.com/registry/deploying/)