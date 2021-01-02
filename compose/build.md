# Build avec Docker Compose

`docker-compose` permet de lancer des builds d'images Docker en spécifiant le dossier de contexte, le Dockerfile, etc. de façon similaire à `docker`.

## Exercices  

Les `Dockerfile` des services `vote`, `result` et `worker` est présent dans des sous-dossiers correspondants à leurs noms (i.e. `vote/Dockerfile` pour `vote`) mais le build des images n'est pas managé par Docker Compose.

Configurer le build des services `vote`, `result` et `worker` dans `docker-compose.yml` tel que:

- Le contexte de build doit être le sous-dossier correspondant à chaque service (i.e. dossier `vote/` pour le service vote)
- Le service `worker` doit utiliser le Dockerfile `worker/Dockerfile.j`
