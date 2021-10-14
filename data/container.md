# Container Layer data

Quelques exercices de manipulation des données au sein d'un container

Les exercices utiliseront Docker Compose avec le fichier `docker-compose.yml` suivant:

```
version: "3.7"

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
      - "5858:5858"

  vote:
    container_name: vote
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:80"

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker
```

Lancer la stack avec:

```
docker-compose up -d
```

## Exercices

Lancer une session shell dans le container `vote` (`docker exec -it vote sh`) et:

- Créer un fichier `/test.txt` avec le contenu `test`
  ```
  echo test > /test.txt
  ```  
- Editer le fichier `app.py` et remplacer les options A et B **Cat** et **Dog** par **Windows** et **Mac**
    - installer un editeur avec `apk add nano` si besoin 
- Quitter et redémarrer le container
- Se connecter à `localhost:5000` et constater les changements

*Il est fréquent d'avoir à éditer des fichiers au sein d'un container dans des environnements de dev ou test, et d'y installer des utilitaires "on the fly" - c'est cependant déconseillé en production: les changements seraient perdus en cas de re-création du container et 
pourrait modifier le comportement du container.*

---

**Inspecter** le container `vote` et trouver l'emplacement des données du layer container sur le disque local. 

*Il est possible de modifier directement les données depuis le disque... mais c'est très fortement déconseillé et risque la corruption de l'installation Docker!*

---

Supprimer le container `vote` et le récréer puis:
- Lancer une session shell dans le container et vérifier si l'état des fichiers `/test.txt` et `app.py`
- Vérifier l'existence des données du container layer sur le disque