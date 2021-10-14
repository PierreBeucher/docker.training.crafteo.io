# Docker - misc commands

## Exercices 

Puller l’image `httpd` taggée `2.4.41-alpine` et lancer un nouveau container en mode détaché sans le nommer

- `docker pull -h`, si jamais...
- penser à récupérer l'identifiant unique de votre container donné en sortie de `docker run`   

---
    
Renommer le container lancé précédemment en `myalpine` en utilisant son identifiant unique (pas son nom)

- l'occasion de découvrir une nouvelle commande?

---

Afficher l’ensemble des processus du container `myalpine`

- équivalent de Linux `top` ou `ps` pour un container Docker

---

**Inspecter** le container `myalpine` et trouver la **commande lancée au démarrage**

- il s'agit du processus qui sera lancé au démarrage du container

---

Obtenir les **statistiques**  d’usage ressources (CPU, RAM...) du container

- pratique pour débugger dans certain cas