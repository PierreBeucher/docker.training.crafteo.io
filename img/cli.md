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

Afficher l'historique de build de l'image Vote

---

Import/export d'image:

- Exporter l'image Vote comme archive
    - Bien lire la doc de la commande avant ;)
    - Cette action peut prendre quelques secondes... 
- Supprimer l'image Vote de votre système local
    - L'image ne doit plus apparaitre avec `docker images`
    - Si besoin, il sera possible de la re-builder from scratch
- Re-importer l'image Vote depuis l'archive créée précédemment
    - `docker images` doit affiche l'image

--- 

Trouver la différence entre `docker image load` et `docker image import`