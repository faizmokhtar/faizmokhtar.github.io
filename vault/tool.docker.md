---
id: e4789c61-e98a-44c0-99b6-e996b0aa4a48
title: Docker
desc: ''
updated: 1624458617989
created: 1618199690834
---

# Docker Cheatsheet

### To display Docker version & info
```bash
docker --version
docker version
docker info
```

### To execute Docker image
```bash
docker run $IMAGE_NAME
```

### To list all Docker images
```bash
docker image ls
```

### To list Docker containers
```bash
docker container ls # runnning
docker container ls --all # all, even one that's not running
docker container ls --aq # all in quiet mode
```

### To create image using current directory's Dockerfile
```bash
docker build -t $CONTAINER_NAME .
```

### To run $CONTAINER_NAME and map port 4000 to 80
```bash
docker run -p 4000:80 $CONTAINER_NAME
docker run -d -p 4000:80 $CONTAINER_NAME # Same thing, but in detached mode
```

### To display log entries
```bash
docker container logs [ID]
```

### To remove container
```bash
docker container rm --force [ID]
```

### To run interactive shell container
```bash
docker run -it [IMAGE] sh
```

### To copy files from host into container (container must be running)
- change will dissappear when container is stop and run again 
```bash
docker container cp [FILES] [DOCKER_ID]:[PATH_TO_COPY_TO]
```
### To explicitly pull an image from docker hub
```bash
docker image pull [IMAGE]
```
### To search docker image by name
```bash
docker images -f reference="[NAME]"
```

--- 

# What is `Dockerfile`?
- A script to package up an application
- It outputs docker image

- **FROM** - every image has to start from another image
- **ENV** - to set env variables.use key-value pair
- **WORKDIR** - creates a directory in container image filesystem and set it as current working directory
- **COPY** - copies files or directories from local filestem (host) into the container image
- **CMD** - specify command to run when container starts
- building an image
```bash
docker image build --tag [NAME] .
```
  - the `.` is the current directory. docker refers it as build context