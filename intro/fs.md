# Docker - File System

Les fichiers de nos containers et images ne sortent pas de n'importe ou! 

## Exercices

Lancer un container `httpd:alpine` nommé `filesystem` et y créer un fichier. Trouver l'emplacement des fichiers du container sur la machine hôte.

- `docker exec -it filesystem bash` avec `touch somefile` pour créer un fichier
- `docker exec filesystem sh -c 'touch somefile'` pour les pros ;)
- `docker -h` est toujous notre ami!

---

Trouver l'emplacement des fichiers de l'image `httpd:alpine` sur la machine hôte. 

- il est conseillé de ne toucher à ces fichiers qu'avec les yeux... 