# Docker - Volumes

```
docker volume -h
# should be sufficient for this one! 
```

## Exercices

Créer un volume Docker nommé `httpd_vol` 

- cette étape est optionnelle pour la suite, mais il serait intéréssant de comprendre pourquoi

---

Lancer un container `httpd:alpine` nommé `containervol` montant le volume Docker `htdocs_vol` sur `/usr/local/apache2/htdocs`. Vérifier le contenu du dossier `htdocs` dans le container une fois démarré. 

- que se passe-t-il si le Volume n'existe pas au préalable?
- afficher les montages avec `df -h` ou `mount` donne des infos intéréssantes  

---

Modifier le contenu de `htodcs/index.html` dans le container `containervol` puis stoppez et supprimer le container `containervol`. Relancer un nouveau container nommé `containervol2` montant `httpd_vol`

- plutôt pratique pour backuper et manipuler des données lorsqu'une base de donnée type Mongo ou MySQL tourne en containerisée

---

Lancer un container basé sur `busybox` montant `httpd_vol` en parallèle 
de `containervol2` et faites des manipulations sur le fichier dans chacun des containers
