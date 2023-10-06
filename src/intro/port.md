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

Lancer 2 containers basés sur `httpd:alpine` exposant leur port `80` sur deux ports différents (tel que `8081` et `8082`) et vérifier le fonctionnement

- l'option `-d` peut être utile pour lancer le container en background
- curl `localhost:8081` et `localhost:8082` permettront de tester avec l'un et l'autre
- possible aussi de tester avec votre navigateur web 

---

Lancer 2 containers `httpd:alpine` utilisant le **réseau hôte** directement et tester

- l'interface réseau de la machine sera utilisé directement, attention aux conflits de ports!
