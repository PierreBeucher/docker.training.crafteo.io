# Optimization de build

Problèmes typiques de build: 
- temps de build (notamment download des dépendances)
- taille finale de l'image (temps de push/pull) 
- contenu de l'image: ne pas y copier des fichiers par accident (secret, token, etc.)

Documentation de référence:
- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices )

## Exercices

- Ajouter un fichier `.dockerignore` permettant de filtrer Dockerfile du contexte
- Changer les instructions de build pour n'invalider le cache de `pip install` qu'en cas de modification de `requirements.txt`
- Utiliser une image `alpine` pour réduire la taille finale de l'image