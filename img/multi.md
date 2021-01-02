# Build multi-stage

Build multi-stage: permet de builder plusieurs images dans un même Dockerfile et réutiliser les données d'une image dans une autre.

Exemple:

```
#
# Multi-stage build example
#
FROM golang:1.7.3 AS build
WORKDIR /go/src/project/
[...]

# Copy source code
COPY app.go .

# Build command, generate binary at /go/src/project/app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

# Second part of the build in the same Dockerfile
FROM alpine:latest

RUN apk --no-cache add ca-certificates
WORKDIR /root/

# Re-use binary built from first image
COPY --from=build /go/src/project/app .

CMD ["./app"]
```

Il est possible de définir autant d'étapes de build que nécéssaire dans un même Dockerfile.

## Exercice

Le service Worker de l'application Voting App est implémenté en Java. Le Dockerfile correspondant est `worker/Dockerfile.j`.

Refactorer ce Dockerfile pour en faire un multi-build:

- La première étape de build devra compiler et générer le fichier `.jar` utilisé pour lancer l'application
  - `mvn package` génère un `.jar` dans `target/worker-jar-with-dependencies.jar`
- La seconde étape de build sera basée sur `openjdk:8-jre-alpine` et ne doit contenir que le `.jar` permettant de lancer l'application (pas de code source ou fichier temporaire de compilation)
  - Copier le `jar` depuis `/code/target/worker-jar-with-dependencies.jar` à la racine `/` et ré-utiliser le même `CMD`
