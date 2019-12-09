---
title: Docker commands
date: '2019-11-05 11:35'
template: "post"
draft: false
slug: "/posts/docker-commands/"
category: "Docker"
tags:
  - "docker"
description:
socialImage: "/media/image-2.jpg"
---
## Info commands
- `docker info`

## Images command
- `docker images` - List all Downloaded Docker Container Images
- `docker search {imageName}` - Search Docker Images in its Registry Server
- `docker pull {imageName}` - download docker container images from its registry server or repository.
- `docker push [OPTIONS] NAME[:TAG]` - push the image to the repository
  - you need to be logged in the docker hub. use the below command to login to docker hub.
```
docker login --username=yourhubusername --email=youremail@company.com
```
- `docker rmi {imageName}` - Delete / Remove Docker Container image
- `docker build -t {imageName:tag} {path_to_docker_file}` - build docker image from docker file
  - `-t {imageName:tag}` - -t option is used to set tag name of docker image

## Container commands
- `docker run -p 8080:80 --name my_nginx {nginx}` - Create a new container and start it.
  - `-p 8080:80` is telling Docker to map your localhost port 8080 to Docker container's port 80.
  - `--name my_nginx` - name the container to uniquely identify the container.
  - `{nginx}` - image name
  - The NGINX container is attached to your command line. So if you exit the command line, the container will die. You can start the NGINX container with the **_detach ('-d') option_**, so it can keep running even if you exit the command line.
  - `docker run -p 8080:80 -d nginx`
  - `docker attach {containerName}` - to ssh into the container.
- `docker ps` - List running Docker Containers
  - `docker ps -a` - all docker containers
- `docker stop {containerName}` - to stop a container.
- `docker kill {containerName}` - to kill a container.
- `docker start {containerName} - to start a container.
- `docker rm {containerName or containerId}` - to remove a container.
- `docker logs {container_name_or_container_id}` - Fetching Logs from the Container
  - `docker logs -f {container_name_or_container_id}` - To fetch the live logs
