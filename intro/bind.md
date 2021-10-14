# Docker - bind mount

```
# mount file (or folder) /tmp/myfile in container at /myfile
# short syntax: -v SOURCE:DEST
docker run -v /tmp/myfile:/myfile -d httpd:alpine

# long syntax, equivalent of short syntax
# recommended by Docker but both work just as fine
docker run --mount type=bind,source=/tmp/myfile,target=/myfile -d httpd:alpine

# on oublie pas... 
docker run -h
```

## Exercices

Créer un fichier `/tmp/index.html` avec contenant `"Hello Docker!"`
 
```
echo 'Hello Docker!' > /tmp/index.html
```

Lancer un container `httpd:alpine` **montant le fichier `/tmp/index.html` sur `htdocs/index.html`** (et exposant `8085:80`)

- le chemin complet de destination sera peut-être nécéssaire
- tester avec `curl localhost:8085` ou votre navigateur
- lancer une session shell dans le container et modifier le fichier `index.html`, re-tester `localhost:8085` et constater les changements
  ```
  # rappel: lancer une session shell
  docker exec -it mycontainer sh
  ```
- modifier le fichier `/tmp/index.html` directement depuis la machine locale, re-tester `localhost:8085` et constater les changements

---

Créer un dossier `/tmp/myhtdocs` et y copier le fichier `index.html`
```
mkdir /tmp/myhtdocs
cp /tmp/index.html /tmp/myhtdocs
```

**Monter le dossier complet** dans votre container afin de pouvoir écraser l'`index.html` du container et obtenir le même résultat que précédemment
