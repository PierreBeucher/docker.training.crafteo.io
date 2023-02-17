# Self-hosted Docker Registry


Cette série d'exercice nous permettra de déployer notre propre registry Docker.

Le Registry Docker se déploie sous forme d'un container Docker, par exemple pour déployer une registry sur votre machine écoutant sur le port 8080:

```
docker run -d -p 5010:5000 --name registry registry:2
``` 

Taggons une image pour la pusher sur notre registry locale:

```
docker pull ubuntu:latest
docker tag ubuntu localhost:5010/ubuntu
docker push localhost:5010/ubuntu
```

Pour plus de détails, voir [la documentation officielle Docker](https://docs.docker.com/registry/deploying/)