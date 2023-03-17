# Logging with Docker example: ELK Stack 

Déployons une stack ELK (Logstash, Elasticearch, Kibana) que nous pourrons utiliser pour obtenir les logs de nos containers:

`resources/elk-stack.yml` contiens les configurations d'une stack ELK. La déployer avec:

```sh
# make elk
docker compose -f resources/elk-stack.yml up -d
```

- L'interface Kibana est accessible via `http://<host>:8082`
- Explorer le contenu de `resources/elk-stack.yml` pour y trouver les configurations des différents services

Une fois déployée, lancer une stack de containers avec le logging driver Docker adapté. Utiliser le fichier `resources/logging.yml`:

```sh
# make logging
docker compose -f docker-compose.yml -f resources/logging.yml up -d
```

Aller sur Kibana (`<host>:8082`) et:

- Cliquer sur _Discover_ (bouton avec la boussole)
- Vous serez redirigé sur la page _Create index pattern_. Entrez `logstash-*` et cliquer sur _Next step_
- Choisissez `@timestamp` comme _Time filter field_ et confirmer avec _Create index pattern_
- Cliquer à nouveau sur _Discover_ pour accéder aux logs 