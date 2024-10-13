# Docker - exposing container ports 

```
# expose host port 8080 on container port 80
docker run -d --name myport -p 8080:80 httpd:alpine

# try it out!
curl localhost:8080

# help may always be useful
docker run -h
```

## Exercices

Lancer 2 containers `httpd:alpine` exposant chacun leur port `80` sur des ports différents (tel que `8081` et `8082`) et vérifier le fonctionnement

- l'option `-d` peut être utile pour lancer le container en background
- curl `localhost:8081` et `localhost:8082` permettront de tester avec l'un et l'autre
- possible aussi de tester avec votre navigateur web 

---

Lancer un container `httpd:alpine` utilisant le **réseau hôte** directement et tester

- Apache se lancera directement sur le réseau de la machine hôte en se bindant au port `80`
- Si le port `80` de la machine hote est déjà utilisé par un autre processus il y aura un conflit de port
- Pour voir les process utilisant le port `80` sur la machine hote:
  ```sh
  sudo netstat -lnap | grep -e ":80\s"
  ```
