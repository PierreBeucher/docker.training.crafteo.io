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

Inspecter l'image Vote créé précedemment, trouver l'emplacement sur le disque des fichiers de l'image

---

Tagger votre image Vote `voting-app:cli`  

---

Afficher l'historique de build de l'image `voting-app:cli`

---

Import/export d'image:

- Exporter l'image `voting-app:cli` comme archive
    - Bien lire la doc de la commande avant ;)
    - Cette action peut prendre quelques secondes... 
- Supprimer l'image Vote de votre système local
    - L'image ne doit plus apparaitre avec `docker images`
    - Si besoin, il sera possible de la re-builder from scratch
- Re-importer l'image Vote depuis l'archive créée précédemment
    - `docker images` doit affiche l'image

---

Lancer un container basé sur `voting-app:cli` et ouvrez une session bash dans le container (`docker exec ...`) pour modifier le contenu du fichier `/app/app.py` afin de modifier les options *Cat/Dog* pour *Windows/Mac* (les variables `options_a|b`) Redémarer le container pour constater les changements. 

Une fois effectué, créer une nouvelle image à partir du container modifié directement sans passer par `docker build`, tagger cette image `voting-app:from-container` puis démarrer un container à partir de celle-ci et tester. 

