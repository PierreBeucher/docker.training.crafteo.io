# Monitoring example: Prometheus + Grafana stack

Déployons une stack Prometheus + Grafana que nous pourrons utiliser pour obtenir des metrics sur nos containers (CPU, RAM, etc.) et configurer des alertes. 

Cloner le repository Git Prometheus:

```
git clone https://github.com/PierreBeucher/prometheus.git
```

Lancer la stack Docker Compose:

```sh
cd prometheus
docker-compose up -d
```

- L'interface Grafana est accessible via `http://localhost:3000`
- Explorer le contenu de `docker-compose.yml` pour y trouver les configurations des différents services