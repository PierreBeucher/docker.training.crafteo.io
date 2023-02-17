# Build avec Docker Compose

`docker compose build` permet de lancer des builds d'images Docker en spécifiant le dossier de contexte, le Dockerfile, etc. de façon similaire à `docker build`.

## Exercices  

Les `Dockerfile` des services `result` et `worker` sont présents dans des sous-dossiers correspondants à leurs noms (i.e. `result/Dockerfile`) mais le build des images n'est pas managé par Docker Compose avec la configuration actuelle.

Modifier la configuration des services `result` et `worker` dans `docker-compose.yml` pour indiquer l'emplacement de build à Docker Compose.

