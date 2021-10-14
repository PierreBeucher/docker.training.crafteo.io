# Bridge networking

Quelques exercices sur Bridge networking avec Docker. Utilisent le fichier `docker-compose.yml` suivant:

```
version: "3"

services:
  db:
    container_name: db
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"

  redis:
    container_name: redis
    image: redis:alpine

  result:
    container_name: result
    image: registry.gitlab.com/crafteo/training/example-voting-app/result
    ports:
      - "5001:80"

  vote:
    container_name: vote
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker
```

# Exercices

Lancer la stack `docker-compose.yml`. Par défaut, un réseau est créé et attaché à chaque container.


- Identifier le réseau `bridge` créé et utilisé par la stack par défaut
  - Utiliser la CLI Docker: `docker network --help` 
- Identifier l'ensemble des réseaux actuellement existant et le Driver utilisé par chacun
- Inspecter le réseau utilisé par la stack Compose et:
  - Trouver la liste des containers associés au réseau
  - Identifier le subnet utilisé par le réseau

---

Configuration réseau customisée avec Docker Compose

- Ajouter un réseau bridge `my-bridge` dans `docker-compose.yml` et configurer chaque container pour utiliser ce réseau, redémarrer la stack et vérifier l'existence du réseau
- Modifier la configuration de `my-bridge` pour:
  - Forcer l'utilisation du driver réseau `bridge`
  - Affecter un nom spécifique `named-bridge`
  - Utiliser le subnet `172.42.0.0/16` 
- Appliquer les configurations et inspecter le réseau `my-named-bridge` pour vérifier les configurations 

---

Il est aussi possible d'utiliser un réseau déjà existant par ailleurs:

- Créer un réseau Bridge nommé `my-external-network`
  - `docker network --help`
- Configurer les services pour utiliser ce réseau déjà existant et appliquer les configurations 

---

Quid de l'isolation des réseaux Bridge? Par défaut, les containers sur un même réseau sont joignables par leur nom (i.e. le container `vote` est joignable via le hostname `vote`). Docker effectue une résolution DNS interne.

- Vérifier la liaison entre le container `vote` et les autres containers de l'application
  - Lancer une session shell sur `vote`  et installer `dig`: 
  ```
  # You can use getent hosts
  # or dig (requires install)
  docker exec -it vote sh
   
   $ getent hosts worker
   $ getent hosts redis

   $ apt update && apt install dnsutils
   $ dig +short worker
   $ dig +short redis
  ``` 
- Créer un réseau `bridge-bis`
- Détacher les containers `result` et `worker` de leur réseau et les attacher au réseau `bridge-bis`
- Vérifier de nouveau la connectivité entre `vote` et les autres containers 
- Vérifier la connectivité entre `result`, `worker` et les autres containers

---

Cleanup:
- Détruire le réseau `bridge-bis`, `named-bridge`
- Détruire la stack Docker Compose
