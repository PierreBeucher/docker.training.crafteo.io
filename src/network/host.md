# Host networking

Configurer `docker-compose.yml` pour:

- Utiliser le network `host` pour chacun des services
- Lancer la stack et vérifier que chacun des services est `Up`
- Se connecter au service Vote et Worker pour vérifier leur fonctionnement
- Trouver l'adresse IP du container `worker` - si elle existe

---

- Identifier le réseau Docker utilisant le driver `host`
- Essayer de créer un autre réseau Docker utilisant le driver `host` 
