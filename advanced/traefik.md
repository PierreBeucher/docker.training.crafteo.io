# HTTPS & Reverse Proxying avec Traefik

Ajoutons des configurations TLS (HTTPS) et un reverse proxy (Traefik). 

- Explore le contenu de `resources/traefik.yml`
- Modifier les configurations TLS pour utiliser `vote.<you>.training.crafteo.io`
- Lancer la stack avec les overrides Traefik:
    ```sh
    # make traefik
    docker compose -f docker-compose.yml -f resources/traefik.yml up -d
    ```

`vote` et `result` sont maintenant exposées via:
- `https://vote.<you>.training.crafteo.io`
- `https://result.<you>.training.crafteo.io`

_Note: le certificat TLS ne sera pas reconnu par votre navigateur, ils sont fournis par `(STAGING) Let's Encrypt`, une version de test des certificats [Let's Encrypt](https://letsencrypt.org/)_