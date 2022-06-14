# devops_docker_tp2

## Objectifs

l’objectif est de créé une api à partir du wrapper du 1er TP, lancer l’api sur docker et automatiser la publication les push sur DockerHub.


## GitHub repository

[Docker-Tp2](https://github.com/mustaphabenhalima/devops_docker_tp2)

## DockerHub

[Docker Hub](https://hub.docker.com/repository/docker/mustaben/efrei-devops-tp2)


### L’api en Python

l’api est baser sur le wrapper, on récupère la latitude et longitude avec des request flask pour permettre de les ajouter dans l’url. On lance l’api sur le [localhost](http://localhost) port:8081 

Le code est dans app.py

## Docker

pour faire marcher le wrapper on crée un environnement Python avec Docker. 

 

### Le Dockerfile

dockerfile

### Le fichier requirements.txt

Librairies nécessaire pour faire marcher le code python 

## Automatisation du push sur DockerHub

### Le workflow

GitHub actions permet d’automatiser le push de l’image sur dockerhub.

```yaml
name: Docker Image CI
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v2
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      -
        name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v3
        with:
          push: true
          tags: mustaben/efrei-devops-tp2
```

### Secrets

Configurer nos identifiants Dockerhub sur Github secrets.

```yaml
username: ${{ secrets.DOCKERHUB_USERNAME }}
password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Bonus

### handolint

```yaml
name: Docker Image CI
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v2
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      -
        name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: hadolint
        uses: reviewdog/action-hadolint@v1
        with:
          reporter: github-pr-review
      -
        name: Build and push
        uses: docker/build-push-action@v3
        with:
          push: true
          tags: mustaben/efrei-devops-tp2
```

### Sécurité

Toute les données sensibles sont hors image ou code .
