# DockerNotes
Repository for Docker learning notes, resources, and code
# Installation
Follow the steps in https://docs.docker.com/engine/install/

Check installation: sudo docker version

# Basic Commands
`Run`: start a container (1) with a local image (2) pull an image from Docker Hub
- A Docker container stops when the app inside finishes execution
- Append a command: `docker run ubuntu sleep 5`

`ps`: lists the running container information

`stop`: stops a running container (with ID or Name)

`rm`: permanently removes a container

`image`: image commands such as list images, sizes, etc

`rmi`: removes images (be sure to remove all depedent containers before removing an image)

`pull`: downloads an image from a repository

`exec`: execute a command on a running container

detach & attach: running docker container in background/foreground
- `docker run -d [DOCKER-APP]`
- `docker attach [CONTAINER-ID]`

# Docker Run
`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

Example: `docker run redis:4.0`
- Port mapping: `-p [EXT PORT][INT PORT]`
- Volume mapping: `-v [EXT PATH]:[INT PATH]`

    Store data inside a container in host. The data will be preserved even after the container is destroyed.
    
    Example: `docker run -v /opt/datadir:/var/lib/mysql mysql`

- More container detail in JSON format: `docker inspect [CONTAINER ID/NAME]`
- Container logs: `docker log [CONTAINER ID/NAME]`: 
- Setting Environment variables: `-e VAR_NAME=VALUE`


# Creating Own Images with Dockerfiles
1. Write a docker file executing all the steps for setting up the dev environment
2. Build the docker image: `docker build -t [NAME:TAG] [CONTEXT PATH]`
3. Push the image to the Docker Hub: `docker push [IMAGE NAME]`

# Specifying the program to execute and parameters for Dockerfile
Both `ENTRYPOINT` and `CMD` give you a way to identify which executable should be run when a container is started from your image

`CMD`:
- has two formats (1) `CMD COMMAND PARAM1 PARAM2 ...` (2) `CMD ["COMMAND", "PARAM", ...]`
- The command and parameters can be overridden with `docker run`

`ENTRYPOINT`:
- parameters can be appended with `docker run`

Combining `CMD` and `ENTRYPOINT`
```
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"] # default parameter that can be overridden
```

# Docker Networks
Three default networks: bridge, none, host

Example: `docker run --network=wp-mysql-network -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --name webapp --link mysql-db:mysql-db -d kodekloud/simple-webapp-mysql`

# Docker Compose
Configuration of running multiple containers with YAML files

Setting up
1. Specify the container names and images in the .yml file
2. Run `docker-compose up`

# Container Orchestration: Docker Swarm
- Used for automatic load balancing and avoiding host failure on multiple servers.
- Popular container orchestration services: Docker Swarm, Kubernetes, MESOS
- Example: `docker service create --replicas=100 nodejs`
