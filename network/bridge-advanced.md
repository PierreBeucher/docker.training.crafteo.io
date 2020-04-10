# Bridge networking - advanced

Exercices avancés sur Bridge networking avec Docker. Utilisent le fichier `docker-compose.yml` suivant:

```
version: "3"

services:
  db:
    container_name: db
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    networks:
      - my-bridge

  redis:
    container_name: redis
    image: redis:alpine
    networks:
      - my-bridge

  result:
    container_name: result
    image: registry.gitlab.com/crafteo/training/example-voting-app/result
    ports:
      - "5001:80"
    networks:
      - my-bridge

  vote:
    container_name: vote
    image: registry.gitlab.com/crafteo/training/example-voting-app/vote
    ports:
      - "5000:80"
    networks:
      - my-bridge

  worker:
    container_name: worker
    image: registry.gitlab.com/crafteo/training/example-voting-app/worker
    networks:
      - my-bridge

networks:
  my-bridge:
    ipam:
      config:
        - subnet: 172.42.0.0/16
```

# Exercices

Recherchons comment Docker intéragi avec le système Linux pour manager les réseaux et interfaces:

- Lancer la stack Compose 
- Trouver l'interface réseau Linux sous-jacente du réseau Docker utilisé par la stack
  - Les interfaces réseaux sont les *devices* Linux tels que `eth0`, `lo` (loop local), etc.
  - Les commandes `nmcli device status` ou `ifconfig` permettent d'afficher les interfaces réseaux
- Trouver l'entrée de la table de routage permettant de rediriger les packets réseaux vers cette interface réseau
  - Utiliser `ip route` ou `netstat -rn`

