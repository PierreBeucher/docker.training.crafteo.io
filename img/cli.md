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

Puller et inspecter l'image `httpd:alpine`, trouver l'emplacement sur le disque des fichiers de l'image

---

Afficher l'historique de build de l'image `httpd:alpine`

---

Import/export d'image:

- Exporter l'image `httpd:alpine` comme archive
    - Bien lire la doc de la commande avant ;)
    - Cette action peut prendre quelques secondes... 
- Supprimer l'image `httpd:alpine` de votre système local
    - L'image ne doit plus apparaitre avec `docker images`
- Re-importer l'image `httpd:alpine` depuis l'archive créée précédemment
    - `docker images | grep httpd` doit trouver votre image

--- 

Trouver la différence entre `docker image load` et `docker image import`