# Docker - build

```
# Build an image with a Dockerfile
docker build [OPTIONS] CONTEXT_PATH

# Use Dockerfile by default with context from current directory (.)
docker build .

# did you miss me?
docker build -h
```

---

```
# Example Dockerfile content to build a custom Apache image

# Image used as base to create our new image
FROM alpine:3.10

# Run some commands to install apache
RUN apk add apache2

# define commands which will be run on startup
CMD ["-DFOREGROUND"]
ENTRYPOINT ["httpd"]
```

## Exercices

Créer un dossier `mybuild` et y créer un fichier `Dockerfile` avec le contenu de l'exemple ci-dessus. **Builder** une image Docker à partir du Dockerfile et la **tagger `myapache:1.0`**

- votre image doit apparaitre via `docker images`

---

Lancer un container utilisant votre image `myapache` et exposer `8089:80`, vérifier le fonctionnement

- rappel: `curl localhost:8089` ou navigateur
- essayer de monter des volumes pour obtenir un fichier `index.html` customisé!
