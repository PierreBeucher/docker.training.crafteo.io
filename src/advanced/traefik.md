# HTTPS & Reverse Proxying avec Traefik

Ajoutons des configurations TLS (HTTPS) et un reverse proxy (Traefik). 

- Explorer le contenu de `resources/traefik.yml`
- Créer un fichier `.env` (remplacer `<you>` par votre nom):
  ```
  VOTE_URL=vote.<you>.training.crafteo.io
  RESULT_URL=result.<you>.training.crafteo.io
  ```
- Lancer la stack avec les overrides Traefik:
    ```sh
    # make traefik
    docker compose -f docker-compose.yml -f resources/traefik.yml up -d
    ```

`vote` et `result` sont maintenant exposés via:
- `https://vote.<you>.training.crafteo.io`
- `https://result.<you>.training.crafteo.io`

_Note: le certificat TLS ne sera pas reconnu par votre navigateur, ils sont fournis par `(STAGING) Let's Encrypt`, une version de test des certificats [Let's Encrypt](https://letsencrypt.org/)_