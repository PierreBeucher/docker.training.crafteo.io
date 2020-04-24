# Configuration avancées du Docker Daemon

Suite à l'installation de Docker sur Linux:

- Docker est installé comme service Linux (`systemd`)
- La CLI `dockerd` est utilisé pour lancer le Docker daemon
- Il est possible d'overrider les configurations du daemon via des options CLI au lancement du service ou via un fichier `/etc/docker/daemon.json` lu par `dockerd` par défaut

Quelques infos et rappels:

- Le **Docker daemon** est aussi appelé **Docker host** ou **Docker server**
- Le Docker client (CLI `docker`) communique avec le client via API REST en utilisant la socket `/var/run/docker.sock` par défaut. Le client peut aussi contacter directement une adresse tel que `tcp://127.0.0.1:2375` ou `tcp://198.265.78.1:2376` en fonction des configurations du Daemon
- Memo de commandes `systemctl` utile pour manipuler les services Linux:
    ```
    # restart, stop, get status of docker
    sudo systemctl [restart|stop|status|...] docker
    
    # edit systemd file override
    # or edit file directly at /lib/systemd/system/docker.service
    sudo systemctl edit docker 
    
    # reload after edit
    sudo systemctl daemon-reload
    
    # see newest service logs
    sudo journalctl -u docker -r
    ```
  
  
# Exercices

Créer un fichier `etc/docker/daemon.json` et y configuer les options suivantes pour le Docker daemon:

- Activer le mode debug
- Configurer le daemon Docker pour écouter sur `127.0.0.1:2375`
- Configurer le port binding pour écouter sur `127.0.0.1` par défaut
  - sinon, un port binding avec `docker run -p 80:80 ...` écoutera sur `0.0.0.0:80` par défaut, ce qui peut représenter un risque de sécurité
- Ajouter le DNS `8.8.8.8` 

Votre Docker daemon écoute maintenant sur `127.0.0.1:2375` et n'utilisera plus la socket `/var/run/docker.sock`. La CLI Docker utilisant cette socket par défaut, des configurations supplémentaires seront requises en utilisant la CLI `docker`. 

Redémarrer le service Docker et tester: 

- Lancer un container exposant un port et vérifier que l'adresse d'écoute est bien `127.0.0.1`
- Au sein du container, vérifier que le DNS `8.8.8.8` est bien utilisé
- Afficher les logs du Daemon

---

Certains déploiements mettent à disposition un daemon Docker accessible à distance. (i.e. le daemon Docker est installé sur une machine différente du client Docker)  

Cela peut-être utile dans certaines situations, par exemple pour partager un daemon avec une équipe de développeur ou un système de CI et accéler le build d'image: le cache de build pourra être réutilisé facilement et ainsi réduire le temps de build (pratique dans certains cas ou un build sans cache dure 20+ min et avec cache seulement quelques secondes!)

Un tel déploiement demande de sécuriser les communications entre le client Docker et le daemon. Utilisant HTTP par défaut, il faut y configurer TLS pour passer en HTTPS. 

Configurer le daemon Docker pour:

- écouter sur `127.0.0.1:2376`
- Configurer l'authentification du serveur en TLS - un client pourra ainsi authentifier le serveur lors de son utilisation (similaire à la connection à un site web en HTTPS)
- (Optionnel) Configurer l'authentification du client en TLS - le client devra s'authentifier auprès du serveur avec un certificate pour pouvoir utiliser le daemon

Ce setup s'appelle une double-authentification TLS. Pour ce besoin il faut un certificat et une clé pour être utilisé par le Daemon (idem pour le client):

- Le client va "truster" un CA (certificate Authority) et le daemon fournira un certificat signé par ce CA pour s'authentifier (c'est exactement le même processus en se connectant à un site web en HTTPS)
- Le Daemon va "truster" un CA (généralement le même), et le client devra fournir un certificat signé par ce CA pour être autorisé par le daemon

 
Dans le cadre de l'exercice nous pourrons générer nos propre CA et certificats. Pour générer une clé et un certificat autosigné:

```
# generate CA cert and key
openssl genrsa -out ca.key 4096
openssl req  -nodes -new -x509  -keyout ca.key -out ca.cert

# Generate key for daemon
openssl genrsa -out daemon.key 4096

# Generate CSR for daemon
openssl req -new -sha256 -key daemon.key -out daemon.csr

# Generate cert for daemon signed by CA
openssl x509 -req -in daemon.csr -CA ca.cert -CAkey ca.key -CAcreateserial -out daemon.cert -days 500

# same for client: key, csr and cert
```

Points d'attention:
- Par convention, le port d'écoute de Docker est `2375` sans TLS et `2376` avec TLS. 
- Le client aura besoin de configuration supplémentaire pour activer TLS et vérifier l'authenticité du serveur
    