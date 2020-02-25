# ENTRYPOINT vs CMD


Rappel: Dockerfile du service Vote


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


## Exercice 1: définir ENTRYPOINT et CMD

Définir `ENTRYPOINT` et `CMD` selon divers contraintes dans votre Dockerfile pour builder des variantes de l'image `voting-app`

### Cas 1

La commande lancée au démarrage par défaut doit être:
```
gunicorn app:app -b 0.0.0.0:80 
```

Il doit être possible de passer des arguments supplémentaires au lancement du container qui seront passés comme **options de `gunicorn`** en **overridant les options par défaut** 

Résultat attendu:

```
# lance la commande:
# 
#   gunicorn app:app -b 0.0.0.0:80
# 
docker run voting-app 

# lance la commande: 
#
#   gunicorn app:app --loglevel DEBUG
#
# passer un argument à docker run écrase les arguments par défaut
# app:app et -b 0.0.0.0:80
docker run voting-app app:app --log-level DEBUG
```
 
## Cas 2

La commande lancée au démarrage par défaut doit être:
```
gunicorn app:app -b 0.0.0.0:80 
```

Il doit être possible de passer des arguments supplémentaires au lancement du container qui seront passés comme **options de `gunicorn`** en **conservant les options par défaut** 

Résultat attendu:

```
# lance la commande:
# 
#   gunicorn app:app -b 0.0.0.0:80
#  
docker run voting-app 

# lance la commande:
# 
#   gunicorn app:app -b 0.0.0.0:80 --loglevel DEBUG
#   
docker run voting-app --loglevel DEBUG
```

## Cas 3

La commande lancée au démarrage par défaut doit être:
```
gunicorn app:app -b 0.0.0.0:80 
```

Passer un argument au run du container doit overrider intégralement la commande lancée par le container par celle en paramètre. 

Résultat attendu:

```
# lance la commande:
# 
#   gunicorn app:app -b 0.0.0.0:80
#  
docker run voting-app 

# lance la commande:
# 
#   gunicorn --version
#   
docker run voting-app gunicorn --version
```

## Exercice 2

Ecrire un Dockerfile pour Vote ayant comme ENTRYPOINT `gunicorn` et comme CMD `app:app -b 0.0.0.0:80` et builder l'image `vote-service:entrypoint`

Utiliser `docker run` pour lancer directement une session bash ou shell avec `vote-service:entrypoint`. 

L'objectif est de lancer une session shell l'image sans que l'application ne soit en train de fonctionner, il ne faut donc pas faire un `docker run -d ...` suivi d'un `docker run -it ...`.    

Ce type de situation est fréquent pour debugger le contenu d'une image sans pour autant lancer le processus principal (notamment pour debugger un bug de démarrage du processus principal!)