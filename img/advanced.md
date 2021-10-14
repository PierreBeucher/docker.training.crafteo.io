# Instruction de build Dockerfile avancées

De nombreuses instructions de build existent pour Dockerfile.

La documentation officielle [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) référence l'ensemble des instructions disponibles

Rappel: le service Vote peut être construit avec un Dockerfile tel que:

```
FROM python:3.7-alpine

# set the application directory
WORKDIR /app

ADD requirements.txt /app/requirements.txt

# install dependencies
RUN pip install -r requirements.txt

# copy source code
ADD . /app

# Define our command to be run when launching the container
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Rappel: commandes de build

```
# builder l'image vote
docker build -t vote:builder vote

# lancer l'image vote
docker run -d --name builder1 vote:builder

# lancer une session shell dans le container
# sera utile pour tester vos manipulations!
docker exec -it builder1 bash

# Supprimer un container et une image
docker stop builder1
docker rm builder1
docker rmi vote:builder
```

## Exercice

---

Configurer l'image pour que l'utilisateur `crafteo` soit utilisé pour lancer le processus principal

- Pour créer un utilisateur `crafteo` sous Linux Alpine:
  ```

  adduser -S crafteo
  ```

Il sera nécéssaire d'ajouter le chemin `/home/crafteo/.local/bin` au `PATH` du user `crafteo` pour éxecuter les binaires installés par Python.

- Ajouter une instruction au Dockerfile permettant de définir la variable `PATH` tel que:
  ```
  PATH=$PATH:/home/crafteo/.local/bin
  ```

Attention: il n'est pas possible de binder un port <1024 avec un utilisateur non-root avec Linux, il sera nécéssaire d'utiliser un port plus élevé. Modifier votre Dockerfile pour lancer l'application en utilisant le port `8080`.

---

Ajouter un healthcheck permettant de vérifier le fonctionnement de l'image avec `curl localhost:80`. Ce healthcheck permettra de vérifier que l'image est bien active si une réponse est renvoyée lors d'un appel à `localhost:80`

- Il peut-être nécéssaire d'installer `curl` ou d'utiliser une image de base ou il l'est déjà
- Pour installer `curl` sous Linux Alpine:
  ```
  apk add curl
  ```

Quel est le comportement d'un container Docker en cas de failure du healthcheck?

---

Ajouter un argument **utilisable au build de l'image Vote** permettant de spécifier la version de l'image Python de base à utiliser. Utiliser la version `3.7-alpine` par défaut.  

Par exemple, il devra être possible de lancer les commandes:

```
# utiliser Python 3.8
docker build -t vote:builder --build-arg PYTHON_VERSION=3.8-alpine vote

# utiliser Python 3.7 par défaut
docker build -t vote:builder vote
```  
