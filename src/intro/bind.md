# Docker - bind mount

```
# mount file (or folder) from ./some-data in container at /example/mydir
# short syntax: -v SOURCE:DEST
docker run -v ./some-data:/example/mydir -d httpd:alpine

# long syntax, equivalent of short syntax
# recommended by Docker but both work just as fine
docker run --mount type=bind,source=/tmp/myfile,target=/myfile -d httpd:alpine

# on oublie pas... 
docker run -h
```

## Exercices

Créer un dossier `apache/` avec un fichier `apache/index.html` contenant `"Hello Docker!"`
 
```
# Example
mkdir apache
echo 'Hello Docker!' > ./apache/index.html
```

Lancer un container `httpd:alpine` **montant le fichier `./apache/index.html` sur `htdocs/index.html` du container** (et exposant `8085:80`)

- le chemin complet de destination sera peut-être nécéssaire
- tester avec `curl localhost:8085` ou votre navigateur
- lancer une session shell dans le container et modifier le fichier `index.html`, re-tester `localhost:8085` et constater les changements
  ```
  # rappel: lancer une session shell
  docker exec -it mycontainer sh
  ```
- modifier le fichier `apache/index.html` directement depuis la machine locale, re-tester `localhost:8085` et constater les changements

---

Même exercice mais **monter le dossier complet `./apache/`** dans le container afin de pouvoir écraser l'`index.html` du container et obtenir le même résultat que précédemment
