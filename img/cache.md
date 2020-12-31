# Build cache

Docker utilise le **cache** lors d'un build si les instructions du Dockerfile le permettent.

Le cache peut-être optimisé de diverses façons, notamment pour éviter de relancer des instructions longues ou consommatrices de ressources en jouant sur le contexte et les instructions du Dockerfile.

Typiquement, un changement d'instruction dans un Dockerfile invalidera le cache à partir de l'étape ayant été modifiée. Pour un set d'étape identique, Docker tentera de ré-utiliser le cache. La plupart du temps le cache est invalidé car une instruction `ADD` ou `COPY` copie un ou plusieurs fichiers dont le contenu est différent du précédent build. Docker n'invalidera pas le cache si le contenu des fichiers copiés dans l'image est identique (via un calcul d'un hash à partir des fichiers à copier)

Voir la [documentation Docker sur la ré-utilisation](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache) du cache pour plus de détails.

## Exercice

Le service Vote peut être construit avec un Dockerfile tel que:

```
FROM python:3.7-alpine

# set the application directory
WORKDIR /app

# copy all context files into /app
# if anything changes in context files,
# cache is invalidated, causing subsequent instructions to be run again
ADD . /app

# install dependencies
# may take a long time, and can be optimized
RUN pip install -r requirements.txt

# Define our command to be run when launching the container
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Cependant un problème surviens: dès qu'un fichier du code source de l'application est modifié, le cache est invalidé par `ADD . /app` et l'instruction `RUN` pour installer les dépendences est de nouveau lancée, ce qui peu prendre du temps.

Cette situation est problématique lorsque des builds d'image Docker sont réalisés fréquemment. Par exemple, une application ayant de nombreuses dépendences peut subir des builds de 20+ min à cause d'une mauvaise utilisation du cache. Il est possible de réduire ce temps à quelques secondes en optimisant le Dockerfile pour une meilleure utilisation du cache.

Dans notre cas, un changement de code source ne devrait pas obliger notre build à relancer la commande `pip install -r requirements.txt` qui dépends uniquement sur le fichier `requirements.txt`. Nous allons optimiser notre Dockerfile et son utilisation du cache pour ne lancer `pip install -r requirements.txt` que si le fichier `requirements.txt` a été modifié depuis le dernier build:

- Builder l'image Vote selon le Dockerfile donné précédemment
- Modifier le fichier `vote/app.py` (ajouter une ligne vide en fin de fichier est suffisant) et relancer le build: constater que le cache est invalidé à l'étape `ADD`
- Optimiser l'utilisation du cache afin de ne ré-éxécuter l'instruction `RUN pip install...` que si le fichier `requirements.txt` a été modifié.
