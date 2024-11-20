# Bridge networking

Quelques exercices sur Bridge networking avec Docker.

# Exercices

Lancer la stack `docker-compose.yml`. Par défaut, un réseau est créé et attaché à chaque container.


- Identifier le réseau `bridge` créé et utilisé par la stack par défaut
  - Utiliser la CLI Docker: `docker network --help` 
- Identifier l'ensemble des réseaux actuellement existant et le Driver utilisé par chacun
- Inspecter le réseau utilisé par la stack Compose et:
  - Trouver la liste des containers associés au réseau
  - Identifier le subnet utilisé par le réseau

---

Quid de l'isolation des réseaux Bridge? Par défaut, les containers sur un même réseau sont joignables par leur nom (i.e. le container `vote` est joignable via le hostname `vote`). Docker effectue une résolution DNS interne.

Isolons les containers de notre stack:
- Configurer les réseaux `vote-net` et `result-net`
- Isoler `vote`, `redis`, `worker` dans le réseau `vote-net`
- Isoler `worker`, `db` et `result` dans le réseau `result-net`
- _Note: le container `worker` sera dans 2 réseaux: `vote-net` et `result-net`_

Seul `worker` pourra communiquer avec chaque container. `vote` / `redis` et `db` / `result` seront mutuellement isolés dans leurs réseaux respectifs. 


Vérifier la *non-connectivité* entre le container `vote` et  `result`
- Lancer une session shell sur `vote`  et essayer de joindre `result`
    ```sh
    docker exec -it vote sh

    # Need curl and/or ping ?
    # Use 'apk add curl iputils-ping' 
    # or 'apt update && apt install -y curl iputils-ping'
    
    # Try to connect
    $ curl result

    # Check DNS resolution
    $ getent hosts result
    $ getent hosts worker
    ```

---

Docker peut aussi connecter et déconnecter des containers d'un réseau à la volée (sans besoin de redémarrer ou recréer un container).

- Connecter le container `vote` manuellement au réseau `result-net`
- Vérifier à nouveau la connectivité
- Déconnecter le container `vote` de `result-net`

---

Configuration réseau customisée avec Docker Compose

- Ajouter un réseau bridge `my-bridge` dans `docker-compose.yml` et configurer chaque container pour utiliser ce réseau
- Modifier la configuration de `my-bridge` pour:
  - Forcer l'utilisation du driver réseau `bridge`
  - Affecter un nom spécifique `named-bridge`
  - Utiliser le subnet `172.42.0.0/16` 
- Appliquer les configurations et inspecter le réseau `named-bridge` pour vérifier les configurations 

---

Il est aussi possible d'utiliser un réseau déjà existant par ailleurs:

- Créer un réseau Bridge nommé `my-external-network`
  - `docker network --help`
- Configurer les services pour utiliser ce réseau déjà existant et appliquer les configurations 
