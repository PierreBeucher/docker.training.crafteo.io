# Container Layer data

Quelques exercices de manipulation des données d'un container

## Exercices

Lancer une session shell dans le container `vote` (`docker exec -it vote sh`) et:

- Créer un fichier `/test.txt` avec le contenu `test`
  ```
  echo test > /test.txt
  ```  
- Editer le fichier `app.py` et remplacer les options A et B **Cat** et **Dog** par **Windows** et **Mac**
    - installer un editeur avec `apk add nano` si besoin 
- Quitter et redémarrer le container
- Se connecter à `localhost:5000` et constater les changements

*Il est fréquent d'avoir à éditer des fichiers au sein d'un container dans des environnements de dev ou test, et d'y installer des utilitaires "on the fly" - c'est cependant déconseillé en production: les changements seraient perdus en cas de re-création du container et 
pourrait modifier le comportement du container.*

---

**Inspecter** le container `vote` et trouver l'emplacement des données du layer container sur le disque local. 

*Il est possible de modifier directement les données depuis le disque... mais c'est très fortement déconseillé et risque la corruption de l'installation Docker!*

---

Supprimer le container `vote` et le récréer puis:
- Lancer une session shell dans le container et vérifier si l'état des fichiers `/test.txt` et `app.py`
- Vérifier l'existence des données du container layer sur le disque