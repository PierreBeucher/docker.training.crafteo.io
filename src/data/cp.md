# Manipulation de données avec `docker cp`

Petit exercice de manipulation des données avec `docker cp`

```
docker cp --help

docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

## Exercice

*Contexte: vous souhaitez modifier les options de vote pour utiliser `Windows` vs. `Mac` à la place de `Cat` vs. `Dog` mais il est interdit de manipuler directement les données dans le container. Il faudra copier le fichier depuis le container, le modifier, puis le remettre en place.*

En utilisant `docker cp`:

- Copier le fichier `app.py` du container `vote` depuis le container sur la machine hôte
- Modifier les options de vote depuis la machine hôte
- Copier le fichier `app.py` modifié depuis la machine hôte vers le container en écrasant l'ancien fichier `app.py`
