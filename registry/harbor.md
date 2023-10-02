# Harbor

Harbor est un outil de registry Docker. Nous allons créer un compte sur https://demo.goharbor.io pour expérimenter les fonctionnalités. 

## Exercices

Après le build locale nos images Example Voting App, pushons les sur une Registry pour les partager !

Aller sur [demo.goharbor.io](https://demo.goharbor.io) et créez-vous un compte (gratuit). 

- Créer un projet `example-voting-app`
- Pusher les images Example Voting App dans la registry `demo.goharbor.io`. Vous devrez:
  - Vous authentifier avec `docker login`
  - Modifier `docker-compose.yml` pour spécifier l'URL de Harbor demo pour chaque image
  - Rebuilder les images avec le tag pointant vers Harbor demo
  - Pusher vos images avec `docker compose push` ou `docker push`