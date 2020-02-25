# Build cache

Docker utilise le **cache** lors d'un build si les instructions du Dockerfile le permettent. 

Le cache peut-être optimisé de diverses façons, notamment pour éviter de relancer des instructions longues ou consommatrices de ressources en jouant sur le contexte et les instructions du Dockerfile.
 
La plupart du temps le cache est invalidé car une instruction `ADD` ou `COPY` copie un ou plusieurs fichiers dont le contenu est différent du précédent build. Docker n'invalidera pas le cache si le contenu des fichiers copiés dans l'image est identique (via un calcul d'un hash à partir des fichiers à copier) 

## Exercice

Le service Vote peut être construit avec un Dockerfile tel que:

```
FROM python:2.7-alpine

# set the application directory
WORKDIR /app

# copy source code
ADD . /app

# install dependencies
RUN pip install -r requirements.txt

# Define our command to be run when launching the container
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Cependant un problème surviens: dès qu'un fichier du code source de l'application est modifié, le cache est invalidé par `ADD . /app` et l'instruction `RUN` pour installer les dépendences est de nouveau lancée, ce qui prends un certain temps.

Cette situation est problématique lors du dev de l'application ou les builds sont fréquents, et en tant que developpeur cela deviens frustrant d'attendre à chaque fois. Concrètement il est inutile de relancer `pip install` alors que seul le code source a été modifié mais pas les dépendences. 


- Builder l'image Vote selon le Dockerfile donné précédemment
- Modifier le fichier `vote/app.py` (ajouter une ligne vide en fin de fichier est suffisant) et relancer le build: constater que le cache est invalidé à l'étape `ADD`
- Trouver un moyean d'optimiser l'utilisation du cache afin de ne pas re-éxécuter l'instruction `RUN pip install` en cas de modification du code, mais seulement si le fichier `requirements.txt` a été modifié.