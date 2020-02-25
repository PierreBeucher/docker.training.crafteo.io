# Voting App: Build Python image

L'appplication Voting App dispose de plusieurs services:

- **Worker**: récupère les votes et les stockes en base de donnée
- **Vote**: application web permettant de voter
- **Result** : permet d'afficher les résultats

Objectif de l'exercice: écrire un Dockerfile pour l'application Vote.

## Exercice - Dockerfile pour le service Vote

Cloner le repository Voting App avec la commande:

```
git clone -b img-build https://gitlab.com/crafteo/training/example-voting-app
```

Le code du service Vote se trouve dans `vote/`:
- `app.py` est le fichier applicatif permettant de lancer l'application
- `requirements.txt` contiens les dépendences de l'application
 
Le service Vote doit être buildée et runnée selon les contraintes suivantes:

- Utiliser **Python 2.7**
- L'ensemble du code source du service (fichiers dans `/vote`) doit être **copié dans l'image Docker**
- L'image Docker doit contenir l'ensemble du service. Pour **intaller les dépendences**, utiliser la commande 
   ```
   pip install -r requirements.txt
   ```
- La commande pour lancer le service est:
   ```
   gunicorn app:app -b 0.0.0.0:80
   ```
   et doit être **éxecutée depuis le dossier contenant l'ensemble du code source** du service (le service exposera le port 80 par défaut une fois lancé)

Exercices:

- Ecrire un fichier `vote/Dockerfile` permettant de builder l'image du service Vote
- Builder votre image Vote et la tagger `training.crafteo.io/vote:build`
- Lancer les composants Worker, Result, Redis et DB avec `run.sh`
- Lancer le composant Vote en exposant le port 80 sur 5000 en **affectant le container au réseau `example-voting-app_default`**

