# Installation de Docker Compose

Docker Compose est distribué sous forme d'un binaire pré-compilé à installer directement. Sous Linux, l'installation est très simple:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# test installation
docker-compose --version
```

Les instructions d'installation sont disponibles sur la [page Docker Compose du site officiel de Docker](https://docs.docker.com/compose/install/)