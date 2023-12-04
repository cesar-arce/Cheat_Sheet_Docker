# Working with Dockers (Dockerfile, Images, Containers, and Networks)

Docker provides the ability to package and run an application in a loosely isolated environment called a container.
The isolation and security allows you to run many containers simultaneously on a given host. 
Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. 
You can easily share containers while you work, and be sure that everyone you share with gets the same container that works in the same way.

***********************************************************
DOCKERS --> Dokerfile (in the root) same place as requirements.txt

<pre>
FROM python:3.8                      < image >
FROM python
FROM ubuntu:latest
                                       
RUN pip install -r requirements.txt   < command > ["< executable >", "< param 1 >", "< param 2 >"] 
RUN pip install request pandas others....
RUN apt update
RUN apt install python3 -y

WORKDIR /fastapi-app                 < path to working directory >
WORKDIR c:\\windows\
WORKDIR D:/Users/...

ADD main.py .                        < source > < destination > 
ADD test1.txt c:\temp\               ["< source >", "< destination >"] -> destination includes white space

COPY requirements.txt .               (Libraries)
COPY ./app ./app                      < source > < destination >  
COPY config* c:/temp/                 ["< source >", "< destination >"] -> destination includes white space
COPY main.py ./

CMD ["python", "./app/main.py"]       < command >  ["< executable >", "< param >"]
CMD ["python3", "./main.py"]
CMD ["c:\\Apache24\\bin\\httpd.exe", "-w"]
</pre>
--------------------------------------------------------
# RUN = pull + create + start

```
docker pull python:3.9
```
      
```
docker pull python:latest
```
      
```
docker create --name <container_name> python:3.9
```
      
```
docker start <container_name>
```
      
```
docker run -it --name <container_name> python:3.9 /bin/bash
```

or case MySQL
```
docker run -it --name <container_name> -e MYSQL_ROOT_PASSWORD=root -d mysql/mysql -server:5.7 mysql
```
      
```
docker exec -it <container_name> /bin/bash
```
      
```
docker container stop <container_name>
```

## Getting Help

Display Docker version with docker --version
```
docker --version
```
(-v = --version)
```
docker -v
```

Display Docker system info with docker info
```
docker info
```
Get help on Docker with docker --help
```
docker --help
```

Get help on Docker command usage with docker {command} --help
```
docker run --help
```

(-t = --tty, -i = --interactive, -d = --detashed, -e = --env, -p = --publish, -rm = --remove after, -a = --all)

## Building Images

Docker images are a lightweight, standalone, executable package of software that includes everything needed to run an application:

code, runtime, system tools, system libraries and settings.

List local images
```
docker images
```

```
docker image ls
```

Build an Image from a Dockerfile
```
docker build -t <image_name> .
```

```
docker run -it <image_name> /bin/bash
```

```
python3 print.py
```

Hello World!
```
cat print.py
```
print('Hello World!')
```
exit
```

```
docker run <image_name>
```
Hello World!

### Build Images

Build an image with docker build {path}
```
docker build .
```

Build an Image from a Dockerfile
```
docker build -t <image_name> .
```
To Update modifications run
```
docker run <image_name>
```

Build an Image from a Dockerfile without the cache
```
docker build -t <image_name> . –no-cache
```

Build an image from the Dockerfile in the current directory and tag the image
```
docker build -t myapp:1.0 .
```

Build a tagged image with docker build --tag {name:tag} {path}
```
docker build --tag myimage:2023-edition .
```

Build an image without using the cache docker build -no-cache {path}
```
docker build --no-cache .
```

## Image Management

List all local images with docker images
```
docker images
```

```
docker image ls
```

Show Docker disk usage with docker system df
```
docker system df
```

Show image creation steps from intermediate layers with docker history {image}
```
docker history alpine
```

```
docker history python
```

Save an image to a file with docker save --output {filename}
Usually combined with a compression tool like gzip
```
docker save julia | gzip > julia.tar.gz
```
Load an image from a file with docker load --input {filename}
```
docker load --input julia.tar.gz
```
Delete an image with docker rmi {image}
```
docker rmi <image_name>
```

```
docker rmi rocker/r-base
```
Delete an image from the local image store
```
docker rmi alpine:3.4
```
Remove all unused images
```
docker image prune
```

## Inspecting Containers

A container is a runtime instance of a docker image. A container will always run the same, regardless of the infrastructure.

Containers isolate software from its environment and ensure that it works uniformly despite differences for instance between development and staging.

To list currently running containers:
```
docker container ls
```

```
docker ps
```
List all docker containers (running and stopped):
```
docker ps --all
```

```
docker container ls --all
```

List all containers matching a conditions with docker ls --filter {key}={value}
```
docker container ls --filter 'name=red1'
```
Show container log output with docker logs --follow {container}
```
docker run --name bb busybox sh -c "$(echo date)"  --> Print current datetime
```

```
dockerlogs --follow bb   --> Print what bb container printed
```

Running docker stats on all running containers against a Linux daemon.
> docker stats [OPTIONS] [CONTAINER...]
```
docker stats
```

Running docker stats on container with name nginx and getting output in json format.
```
docker stats nginx --no-stream --format "{{ json . }}"
```

## Running Containers

Create and run a container from an image, with a custom name
```
docker run --name <container_name> <image_name>
```
Run a container with and publish a container’s port(s) to the host.
```
docker run -p <host_port>:<container_port> <image_name>
```
Run a container in the background
```
docker run -d <image_name>
```
Start or stop an existing container:
```
docker start|stop <container_name> (or <container-id>)
```
Remove a stopped container:
```
docker rm <container_name>
```
Open a shell inside a running container:
```
docker exec -it <container_name> sh
```
Fetch and follow the logs of a container:
```
docker logs -f <container_name>
```
To inspect a running container:
```
docker inspect <container_name> (or <container_id>)
```
View resource usage stats
```
docker container stats
```


Run a container with docker run {image}
```
docker run hello-world  --> Runs a test container to check your installation works
```
Run a container then use it to run a command with docker run {image} {command}
  --> Run Python & print text
```
docker run python python -c "print('Python in Docker')"
```
--> Run R & print a model
```
docker run rocker/r-base r -e "print(lm(disc`speed, cars))" 
```
Run a container interactively with docker run --interactive --tty
--> Run R interactively
```
docker run --interactive --tty rocker/r-base  
```
Run a container, and remove it once you've finished with docker run --rm
```
docker run --rm mysql --> Run MySQL, then clean up
```
Run an image in the background with docker run --detach
```
docker run --detach postgres
```
Run an image, assigning a name, with docker --name {name} run
```
docker run --name red1 redis --> Run redis, naming the container as red1
```
Run an image as a user with docker run --user {username}
```
docker run --user doctordocker mongo
```

```
docker run
```
- --rm remove container automatically after it exits
- -it connect the container to terminal
- --name web name the container
- -p 5000:80 expose port 5000 externally and map to port 80
- -v ~/dev:/code create a host mapped volume inside the container
- alpine:3.4 the image from which the container is instantiated
- /bin/sh the command to run inside the container

Stop a running container through SIGTERM
```
docker stop web
```
Stop a running container through SIGKILL
```
docker kill web
```
Delete all running and stopped containers 
```
docker rm -f $(docker ps -aq)
```
Create a new bash process inside the container and connect it to the terminal
```
docker exec -it web bash
```
Print the last 100 lines of a container’s logs
```
docker logs --tail 100 web
```

Stop one or more running containers
> docker stop [OPTIONS] CONTAINER [CONTAINER...]
```
docker stop <container_name>
```

## Creating a Network
```
docker network ls
```

```
docker network --help
```

```
docker network create my_app_net <network_name>
```

```
docker network inspect my_app_net
```

```
docker container run -d --name <container_name> --network <network_name> nginx
```

```
docker container run -d --name new_nginx --network my_app_net nginx
```

## Connect a container to a network started
(Connect "Id" Network of  "Driver": "bridge", 8facc8c8429a to a Container) 
```
docker network connect <network_name> <container_name> 
```

```
docker network connect my_app_net db
```

```
docker network inspect <network_name>
```

```
docker container inspect <container_name>
```
   (Disconnect a Container to a Network)
```
docker network disconnect <network_name> <container_name>
```

```
docker network disconnect bridge db
```

```
docker network disconnect bridge webhost
```

```
docker network disconnect bridge proxy
```

```
docker network ls ( active only )
```

```
docker network ls -a  (--all = active+stopped)
```

## Docker Hub

Docker Hub is a service provided by Docker for finding and sharing container images with your team. Learn more and find images at https://hub.docker.com

Login into Docker
```
docker login -u <username>
```
Publish an image to Docker Hub
```
docker push <username>/<image_name>
```
Search Hub for an image
```
docker search <image_name>
```
Pull an image from a Docker Hub
```
docker pull <image_name>
```
### After modify your python script build again
```
docker build -t python-imdb .
```

```
docker run -t -i python-imdb
```

### After modifying your python script build again
```
docker build -t python-fastapi .
```

```
docker run -p 8000:8000 python-fastapi
```

Docker Tutorial For Beginners - How To Containerize Python Applications
https://www.youtube.com/watch?v=bi0cKgmRuiA&t=695s


![Docker CheatSheet](./Docker_cheat_sheet.png)

### Download Docker Cheat Sheets

![Docker Cheat Sheet - 6 pages](./docker_cheat_sheet.pdf)

![Docker Swarm - 2 pages](./docker-swarm.pdf)


