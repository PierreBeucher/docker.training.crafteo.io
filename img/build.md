# Voting App: Build Python image

L'appplication Voting App dispose de plusieurs services:

- **Worker**: récupère les votes et les stockes en base de donnée
- **Vote**: application web permettant de voter
- **Result** : permet d'afficher les résultats

Objectif de l'exercice: écrire un Dockerfile pour l'application Vote.

## Exercice - Dockerfile pour l'application Vote

Cloner le repository Vote avec la commande:

```
git clone -b img-build https://gitlab.com/crafteo/training/example-voting-app
```

Le code de l'application Vote se trouve dans `vote/`:
- `app.py` est le fichier applicatif permettant de lancer l'application
- `requirements.txt` contiens les dépendences de l'application
 
L'application doit être buildée et runnée selon les contraintes suivantes:

- Utiliser Python 2.7
- L'ensemble du code source de l'application (fichiers dans `/vote`) doit être copié dans l'image Docker
- L'image Docker doit contenir l'ensemble des dépendences du projet. Pour intaller les dépendences, utiliser la commande 
   ```
   pip install -r requirements.txt
   ```
- La commande pour lancer l'application est:
   ```
   gunicorn app:app -b 0.0.0.0:80
   ```
- L'application expose le port 80 une fois lancée, mais doit être accessible depuis le port 8080 de la machine

Exercices:

- Ecrire un fichier `vote/Dockerfile` permettant de builder Vote
- Builder votre image Vote et la tagger `training.crafteo.io/vote:build`
- Lancer les composants Worker, Result, Redis et DB avec `run.sh`
- Lancer le composant Vote en exposant le port 80 sur 5000 en affectant le container au réseau `example-voting-app_default`

