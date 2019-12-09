---
title: Dockerfile parts
date: '2019-11-05 12:41'
template: "post"
draft: false
slug: "/posts/dockerfile-parts/"
category: "Docker"
tags:
  - "docker"
description:
socialImage: "/media/image-2.jpg"
---
## DOCKERFILE PARTS
- `FROM` - From Keyword specify the name of image that will be used as base image while building the Docker Image, Docker command will search this image from local image repository if it is not available in local repository then it will fetch from Registry server
- `ENV` - Environment variables
- `RUN` - Run commands/shell scripts, etc.
  - Commands mentioned after RUN keyword will be executed during creation of Docker image
- `EXPOSE` - Ports to expose
- `CMD` - Final command run when you launch a new container from image.
  - Commands mentioned after `CMD` keyword will executed when a container is launched from a docker image.
  - The primary purpose of `CMD` is to tell the container which command it should run when it is started. 
- `WORKDIR` - Sets working directory (also could use 'RUN cd /some/path')
- `COPY` # Copies files from host to container

Sample:
```
FROM node:12

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm ci 

# Bundle app source
COPY . .

EXPOSE 8080

CMD [ "npm", "run", "prod" ]
```

## About Docker Volume
* In order to understand what a Docker volume is, we first need to be clear about how the filesystem normally works in Docker. Docker images are stored as series of read-only layers. 
* When we start a container, Docker takes the read-only image and adds a read-write layer on top. If the running container modifies an existing file, the file is copied out of the underlying read-only layer and into the top-most read-write layer where the changes are applied. 
* The version in the read-write layer hides the underlying file, but does not destroy it -- it still exists in the underlying layer. 
* When a Docker container is deleted, relaunching the image will start a fresh container without any of the changes made in the previously running container -- those changes are lost. Docker calls this combination of read-only layers with a read-write layer on top a **_Union File System_**.
> volumes are directories (or files) that are outside of the default Union File System and exist as normal directories and files on the host filesystem.

There are 3 ways to mount the data in docker.
1. **Volume : ** **_Volumes_** are stored in a part of the host filesystem which is managed by Docker (`/var/lib/docker/volumes/` on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
   - When you create a volume, it is stored within a directory on the Docker host. When you mount the volume into a container, this directory is what is mounted into the container.
2. **Bind mounts :** **_Bind mounts_** may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.
3. **_tmpfs_** mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

### Volume
* Choose the -v or --mount flag to specify the volume.
  - **-v or --volume**: Consists of three fields, separated by colon characters (:). The fields must be in the correct order, and the meaning of each field is not immediately obvious.
    - In the case of named volumes, the first field is the name of the volume, and is unique on a given host machine. For anonymous volumes, the first field is omitted.
    - The second field is the path where the file or directory are mounted in the container.
    - The third field is optional, and is a comma-separated list of options, such as ro.
   - **--mount**: Consists of multiple key-value pairs, separated by commas and each consisting of a `<key>=<value>` tuple. The **_--mount_** syntax is more verbose than -v or --volume, but the order of the keys is not significant, and the value of the flag is easier to understand.
     - The type of the mount, which can be `bind`, `volume`, or `tmpfs`. 
     - The source of the mount. For named volumes, this is the name of the volume. For anonymous volumes, this field is omitted. May be specified as source or `src`.
     - The destination takes as its value the path where the file or directory is mounted in the container. May be specified as `destination`, `dst`, or `target`.
     - The `readonly` option, if present, causes the bind mount to be mounted into the container as read-only.
     - The `volume-opt` option, which can be specified more than once, takes a key-value pair consisting of the option name and its value.

Example:
```
$ docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest


$ docker run -d \
  --name devtest \
  -v myvol2:/app \
  nginx:latest
```
