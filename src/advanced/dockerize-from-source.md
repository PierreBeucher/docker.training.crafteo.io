# Dockerizer une application depuis le code source

Considérons le contexte suivant:

_Vous êtes un opérateur travaillant avec une équipe de développeur. L'équipe de dev viens de vous livrer le code source d'une application web que vous devez déployer sous Docker._

_L'application est codé en [NodeJS](https://nodejs.org/en/) et a pour but d'afficher un message à partir d'un fichier de configuration. Le code source de l'application se trouve à l'emplacement suivant: [NodeJS app](https://github.com/PierreBeucher/example-voting-app/tree/master/resources/nodejs-sample)_

Les développeurs vous donnent les instructions suivantes:

- L'application peut se lancer avec NodeJS 15+
- Pour fonctionner, elle va charger au démarrage le fichier de configuration `./config.yaml` qui doit lui être mis à disposition. Les devs vous fournissent le fichier de config testé en développement:
  ```yaml
  # Message to print when app is accessed via a web browser
  message: "Test message from dev"

  # Host and port on which to bind server
  hostname: 127.0.0.1
  port: 8080
  ```
- Instructions d'utilisation de l'application:
  ```sh
  # Install Node dependencies
  npm install

  # Run node app
  node app.js

  # App should not respond to web request
  # For example, localhost:8080
  ```

Vous devez _dockeriser_ l'application:

- Ecrire un `Dockerfile` permettant de builder l'application
- Ecrire un `docker-compose.yml` permettant de lancer l'application:
  - Assurez-vous de monter un fichier `config.yaml` dans le container
- Lancer l'application et vérifier qu'elle soit joignable