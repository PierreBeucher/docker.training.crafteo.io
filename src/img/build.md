# Voting App: Build Python image

L'appplication Voting App dispose de plusieurs services:

- **Worker**: récupère les votes et les stockes en base de donnée
- **Vote**: application web permettant de voter
- **Result** : permet d'afficher les résultats

Objectif de l'exercice: écrire un Dockerfile pour l'application Vote.

Lien utile: [Dockerfile reference](https://docs.docker.com/engine/reference/builder) - l'ensemble des instructions utilisables dans un Dockerfile

## Exercice - Dockerfile pour le service Vote

Le code du service Vote se trouve dans `vote/`:
- `app.py` est le fichier applicatif permettant de lancer l'application
- `requirements.txt` contiens les dépendences de l'application

Pour l'instant le service utilise une image Docker déjà buildée:

```
services:
  vote:
    image: crafteo/example-voting-app-vote
```

Nous allons faire en sorte de builder notre propre service Vote selon les contraintes suivantes:

- Utiliser **Python 3.9** ou plus récente 

  - Chercher sur [Docker Hub](https://hub.docker.com/) une image Python correspondante
- L'ensemble du code source du service (fichiers dans `/vote`) doit être **copiée dans l'image Docker**
- L'image Docker doit contenir l'ensemble du service. Pour **installer les dépendences**, utiliser la commande
   ```
   pip install -r requirements.txt
   ```
- La commande qui doit être lancée au démarrage du container est:
   ```
   gunicorn app:app -b 0.0.0.0:80
   ```
   et doit être **éxecutée depuis le dossier contenant l'ensemble du code source** du service (le service exposera le port 80 par défaut une fois lancé)

Exercices:

- Ecrire un fichier `vote/Dockerfile` permettant de builder l'image du service Vote
- Builder votre image Vote et la tagger `vote:local` (mettre à jour la `docker-compose.yml` en conséquence)
- Lancer la stack avec votre nouvelle image `vote`
