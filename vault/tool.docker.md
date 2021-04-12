---
id: e4789c61-e98a-44c0-99b6-e996b0aa4a48
title: Docker
desc: ''
updated: 1618203230594
created: 1618199690834
---

# Docker Cheatsheet

### To display Docker version & info
```
docker --version
docker version
docker info
```

### To execute Docker image
```
docker run $IMAGE_NAME
```

### To list all Docker images
```
docker image ls
```

### To list Docker containers
```
docker container ls # runnning
docker container ls --all # all, even one that's not running
docker container ls --aq # all in quiet mode
```

### To create image using current directory's Dockerfile
```
docker build -t $CONTAINER_NAME .
```

### To run $CONTAINER_NAME and map port 4000 to 80
```
docker run -p 4000:80 $CONTAINER_NAME
docker run -d -p 4000:80 $CONTAINER_NAME # Same thing, but in detached mode
```
