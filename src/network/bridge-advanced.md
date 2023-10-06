# Bridge networking - advanced

Recherchons comment Docker intéragi avec le système Linux pour manager les réseaux et interfaces:

- Lancer la stack Compose 
- Trouver l'interface réseau Linux sous-jacente du réseau Docker utilisé par la stack
  - Les interfaces réseaux sont les *devices* Linux tels que `eth0`, `lo` (loop local), etc.
  - Les commandes `nmcli device status` ou `ifconfig` permettent d'afficher les interfaces réseaux
- Trouver l'entrée de la table de routage permettant de rediriger les packets réseaux vers cette interface réseau
  - Utiliser `ip route` ou `netstat -rn`

