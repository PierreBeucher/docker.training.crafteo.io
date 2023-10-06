# Docker Compose CLI

La plupart des commandes `docker` ont leurs équivalents avec `docker compose`, mais leurs fonctionnalités sont adaptées à la gestion de stack multi-container et les fonctionnalités ne sont pas toujours équivalentes.

```
# Rappel
docker compose --help
```

## Exercices

Utiliser une commande permettant de **puller** l'ensemble des images de la stack en parallèle.

- Permet de gagner du temps lors du pull de nombreuses images

---

Lancer la stack Compose avec les options suivantes:

- Mode détachée (Tout comme `docker`, `docker compose` lance les containers en mode interactif par défaut)
- Forcer la récréation des containers déjà existants

---

Lancer la stack puis modifier le fichier `docker-compose.yml` pour changer le port exposé de `result` pour `5002`. Appliquer les changements à la stack avec une commande `docker compose`.

---

Quelques manipulations:

- Arrêter, démarrer puis re-démarrer la stack
- Lister les images utilisées par la stack
- Lister les containers de la stack
- Afficher les logs de l'ensemble des containers de la stack
- Afficher les logs d'un container de la stack et suivre les changements
- Executer une session shell interactive dans le container `vote`
- Arrêter et supprimer la stack, puis ne lancer que le service `vote`
- Arrêter et supprimer la stack

Ces commandes seraient possibles directement avec `docker` en y spécifiant les options requises. `docker compose` intéragi avec le Daemon Docker tout comme `docker`.

---

Plusieurs stacks peuvent coéxister en s'assurant qu'il n'y a pas de conflits de ports ou autre.

Copier le fichier `docker-compose.yml` et nommer cette copie `docker-compose-bis.yml` puis lancer 2 stacks en parallèle

- Celle utilisant `docker-compose.yml` doit être nommé `app`
- Celle utilisant `docker-compose-bis.yml` doit être nommée `app-bis`
- Attention aux conflits de nom de container et ports: le nom des containers doivent être uniques ainsi que les ports exposées sur la machine
