# CLI Docker: images

```
# list images
docker images # mind the plural!
docker image ls

# S.O.S
docker image -h

# pull an image
docker images pull redis:alpine
docker pull redis:alpine
```

## Exercices

Inspecter l'image `vote` de votre stack Example Voting App et trouver l'emplacement sur le disque des fichiers de l'image.

---

Tagger l'image `vote` de votre stack en `vote:newtag` et configurer votre stack pour l'utiliser  

---

Afficher et comparer l'historique de build de l'image `vote` de base et `vote:newtag`

---

Import/export d'image

Une image Docker n'est rien d'autre qu'une archive contenant des fichiers. Exportons notre image sous forme d'archive avant de la ré-importer comme image Docker.

- Exporter l'image `vote:newtag` comme archive
    - Trouver la commande adapée via `docker image --help`
    - Cette action peut prendre quelques secondes...
- Supprimer l'image Vote de votre système local
    - L'image ne doit plus apparaitre avec `docker images`
    - Si besoin, il sera possible de la re-builder from scratch
- Re-importer l'image Vote depuis l'archive créée précédemment
    - `docker images` doit affiche l'image

---

Lancer un container basé sur `vote:newtag` et ouvrez une session bash dans le container (`docker exec ...`) pour modifier le contenu du fichier `/app/app.py` afin de modifier les options *Cat/Dog* pour *Windows/Mac* (les variables `options_a|b`) Redémarer le container pour constater les changements.

Il est possible de créer une image Docker directement à partir d'un container (sans passer par `docker build`). Ce principe est équivalent à créer une image de VM via un snapshot de VM existante afin de lancer des clones de la VM d'origine.

Créer une nouvelle image à partir du container modifié précédemment **sans passer par `docker build`**, tagger cette image `voting-app:from-container` puis démarrer un container à partir de celle-ci et tester.
