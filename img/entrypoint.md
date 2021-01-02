# ENTRYPOINT vs CMD


Rappel: Dockerfile du service Vote


```
FROM python:3.7-alpine

# set the application directory
WORKDIR /app

# copy source code
ADD . /app

# install dependencies
RUN pip install -r requirements.txt

# Define our command to be run when launching the container
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Tableau de correspondance `ENTRYPOINT` vs. `CMD`:

[Voir la documentation Docker](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)

---

## Exercice: définir ENTRYPOINT et CMD

Définir `ENTRYPOINT` et `CMD` afin d'avoir une commande lancée par Docker dans le container répondant à divers contraintes.

### Exemple

```
# Le container sera lancé avec la commande
#   gunicorn app:app -b 0.0.0.0:80
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Pour tester:

```
# Update vote Dockerfile and build
docker-compose build vote

# Run vote container
# COMMAND and ARG will override default COMMAND (CMD)
docker run -d --name test_entrypoint --rm vote:local [COMMAND] [ARG...]
# or
docker-compose run vote [COMMAND] [ARG...]


# Check running processus
docker top test_entrypoint -o pid,command
# or
docker-compose top vote -o pid,command
# Output like
#   PID   COMMAND
#   7767  /usr/local/bin/python /usr/local/bin/gunicorn app:app -b 0.0.0.0:80

# stop and remove container
docker stop test_entrypoint
# or
docker-compose down vote
```

### Cas 1

La commande lancée au démarrage par défaut doit être:

```
gunicorn app:app -b 0.0.0.0:80
```

Il doit être possible d'overrider les options de `gunicorn` définies par défaut (i.e. overrider l'usage de `-b 0.0.0.0:80`)

Résultat attendu:

```
#
# Sans argument supplémentaire
#
docker-compose run vote
#
# gunicorn app:app -b 0.0.0.0:80
#

#
# Arguments: --log-level DEBUG
#
docker-compose run vote --log-level DEBUG
#
# gunicorn app:app --log-level DEBUG
#
```

Cette configuration peut-être utilisée pour fournir une image lançant notre serveur Vote en permettant à l'utilisateur final de passer à `gunicorn` des options différentes (comme un port différent ou un niveau de debug plus élévé)

## Cas 2

La commande lancée au démarrage par défaut doit être:
```
gunicorn app:app -b 0.0.0.0:80
```

Il doit être possible de passer des options supplémentaires à `gunicorn` tout en **conservant l'usage de `-b 0.0.0.0:80` par défaut**

Résultat attendu:

```
#
# Sans argument supplémentaire
#
docker-compose run vote
#
# gunicorn app:app -b 0.0.0.0:80
#

#
# Arguments: --log-level DEBUG
#
docker-compose run vote --log-level DEBUG
#
# gunicorn app:app -b 0.0.0.0:80 --log-level DEBUG
#
```

Cette configuration peut-être utilisée pour fournir une image lançant notre serveur Vote en permettant à l'utilisateur final de passer à `gunicorn` des options différentes (comme niveau de debug plus élévé) tout en semi-forçant l'utilisation de certaines options (dans notre cas, l'utilisation du port 80)

## Cas 3

La commande lancée au démarrage par défaut doit être:

```
gunicorn app:app -b 0.0.0.0:80
```

Passer un argument au run du container doit overrider intégralement la commande lancée par le container par celle en paramètre.

Résultat attendu:

```
#
# Sans argument supplémentaire
#
docker-compose run vote
#
# gunicorn app:app -b 0.0.0.0:80
#

#
# Arguments: --log-level DEBUG
#
docker-compose run vote sh
#
# Lance une session shell interactive
#
```

Cette configuration peut-être utilisée pour fournir une image lançant notre serveur Vote en permettant à l'utilisateur final de lancer un binaire ou une commande différent au lancement du container.
