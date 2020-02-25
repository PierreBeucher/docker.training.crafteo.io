# Contexte de build

Lors de l'exercice prédédent, nous avons buildé une image pour le service Vote. Une solution possible de Dockerfile est:

```
FROM python:2.7-alpine

# set the application directory
WORKDIR /app

# copy source code
ADD . /app

# install dependencies
RUN pip install -r requirements.txt

# Define our command to be run when launching the container
CMD [ "gunicorn", "app:app", "-b", "0.0.0.0:80" ]
```

Problème: le dossier `vote` contiens de nombreux fichiers et dossiers qui ne sont pas utilisés par l'application, notamment dans `vote/dotnet` qui contiens le code source d'une implémentation en Dotnet du même service. Cependant ce code source a été copié dans l'image Docker:

- `docker build vote/` envoi l'intégralité du contenu de `vote/` au Daemon Docker
- Le Daemon utilise ce contexte comme base pour les instructions `ADD` et `COPY`
- `ADD . /app` copy l'ensemble du context dans l'image Docker  

Il est préférable d'éviter de copier dans une image Docker des éléments inutiles ou indésirables (souvent des images ou du code de test):
- l'image buildée sera plus volumineuse (plus longues à puller, plus difficile à stocker, etc.)
- le contexte envoyé au Daemon lors du build sera plus volumineux, et pourra même faire crasher le Daemon
- ces fichiers supplémentaires peuvent représenter un risque de sécurité si un attaquant prends le contrôle du container


Exercice: sans supprimer les fichiers indésirables du repository d'origine, faites en sorte que les fichiers dans `vote/dotnet` soit **ignorés lors de build**