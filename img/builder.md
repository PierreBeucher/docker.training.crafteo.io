# Instruction de build Dockerfile avancées

De nombreuses instructions de build existent pour Dockerfile.

La documentation officielle [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) référence l'ensemble des instructions disponibles

Rappel: le service Vote peut être construit avec un Dockerfile tel que:

```
FROM python:2.7-alpine

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

## Exercice

Modifier le Dockerfile et donné précédemment et re-builder le service Vote en le taggant `vote-service:builder` afin de tester les changements.

Rappel:

```
# builder l'image vote
docker build -t vote-service:builder vote

# lancer l'image vote
docker run -d --name builder1 vote-service:builder 

# lancer une session shell dans le container
# sera utile pour tester vos manipulations!
docker exec -it builder1 bash 
```

Conseil: nommez vos container builder1, builder2, etc. au fur et à mesure de vos lancements puis faire un cleanup à la fin de l'exercice (cela vous permettra de ne pas perdre de temps avec des erreurs du type *"container name already in use"*

---

Définir la variable d'environnement `HOME=/app` par défaut dans l'image

- utiliser la commande `env` dans le container pour tester l'existence de la variable

---

Configurer l'image pour que l'utilisateur `crafteo` soit utilisé pour lancer le processus principal

- Attention: il n'est pas possible de binder un port <1024 avec un utilisateur non-root avec Linux, il sera nécéssaire d'utiliser un port plus élevé, 8080 par exemple. 
- utiliser la commande `docker top` depuis votre machine ou `ps -ef` dans le container pour voir l'ensemble des processus et leur utilisateur

---

Ajouter un healthcheck permettant de vérifier le fonctionnement de l'image avec `curl localhost:80`. Ce healthcheck permettra de vérifier que l'image est bien active si une réponse est renvoyé lors d'un appel à `localhost:80`

- il peut-être nécéssaire d'installer `curl` ou d'utiliser une image de base ou il l'est déjà

--- 

Ajouter un argument passable au build de l'image permettant de spécifier la version de l'image Python de base à utiliser. Utiliser la version `2.7-alpine` par défaut.  

Par exemple, il devra être possible de spécifier:

```
# utiliser Python 3.5
docker build -t vote-service:builder --build-arg PYTHON_VERSION=3.5-alpine vote

# utiliser Python 2.7
docker build -t vote-service:builder --build-arg PYTHON_VERSION=2.7-alpine vote
```  
