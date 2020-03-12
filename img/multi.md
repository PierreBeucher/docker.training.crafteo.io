# Build multi-stage

Build multi-stage: permet de builder plusieurs images dans un même Dockerfile et réutiliser les données d'une image dans une autre.

Exemple:

```
# First step of the build
# It will generate an image named "build" that can be re-used later 
FROM golang:1.7.3 AS build
WORKDIR /go/src/project/

# Copy source code
COPY app.go .

# Build command, generate binary at /go/src/project/app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

# Second part of the build
FROM alpine:latest  

RUN apk --no-cache add ca-certificates
WORKDIR /root/

# re-use binary from first image with --from=build
COPY --from=build /go/src/project/app .

CMD ["./app"] # Commande de run
```

Il est possible de définir autant d'étapes de build que nécéssaire dans un même Dockerfile.

## Exercice

Le service Worker de l'application Voting App est implémenté en Java. Le Dockerfile correspondant est: 

```
FROM maven:3.5-jdk-8-alpine

WORKDIR /code

# installl dependencies
COPY pom.xml /code/pom.xml
RUN ["mvn", "dependency:resolve"]

# add source and compile
# creates a jar file in target/worker-jar-with-dependencies.jar
COPY ["src/main", "/code/src/main"]
RUN ["mvn", "package"]

CMD ["java", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-jar", "/code/target/worker-jar-with-dependencies.jar"]
```  

Refactorer ce Dockerfiler pour en faire un multi-build:

- La première étape de build devra compiler et générer le fichier `.jar` utilisé pour lancer l'application
  - `mvn package` génère un `.jar` dans `target/worker-jar-with-dependencies.jar` 
- La seconde étape de build est basée l'image `openjdk:8-jre-alpine` et ne doit contenir que le `.jar` permettant de lancer l'application (pas de code source ou fichier temporaire de compilation) 