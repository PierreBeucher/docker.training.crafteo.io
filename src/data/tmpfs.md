# Montage `tmpfs`

Exercices sur le montage de volume de type *tmpfs**

## Exercices

*Contexte: les données Redis sont dans notre cas considérées comme éphémères et ne doivent pas être persistées sur le disque ou dans le writable layer du container. De plus, pour optimiser les performances les données Redis doivent être stockées en mémoire directement.*


Modifier `docker-compose.yml` pour:

- configurer un volume de type `tmpfs` sur le service `redis` à l'emplacement `/data`
- limiter la taille du volume à `500Mo`
- redémarrer le service pour prendre en compte les modifications

Lancer une session shell dans le container `redis` et:

- créer un fichier test 
  ```
  echo test > /data/test 
  ```
- redémarrer le container
- vérifier l'éxistence du fichier précédemment créé

