# General

# image:
the bare minimun of a OS

Check docker is running 
```
# systemctl status docker
```
list the containers
```
# docker container ls
```
check the details of the container
```
# docker inspect <container-id>
```
check docker processes
```
# docker ps -a

docker ps -l --format=$FORMAT
# the last one run
```
Show/List the images available
```
docker images
```
start a container
run an image to make a container
```
docker run --rm -ti ubuntu:latest bash

-ti: terminal interactive
```
Create an image 
```
# from a container id
docker commit <container-id>
# get the container id from docker ps -l --format=$FORMAT

or from a container
docker commit <existing-container-name> <new-container-tag>
```
docker tag , gives a name to an image
```
docker tag <commit-id> my-imange-name
```
# Running 
```
docker run --rm  ubuntu:latest bash
# --rm removes the container when you finish
```
attaching and detaching
```
docker run -d --ti ubuntu bash
#-d deattached, leave it run it in the backgroup
#or if you running on it Ctrl-p Ctrl-q detaches it

# find you detached container
docker ps

docker attach <name-of-detached-container>
```
connecting via another session
```
docker exec --ti <container-name> bash
```
## docker logs
```
docker logs <container-name>
```
## kill a container
```
docker kill <container-name>
# stops the container

docker rm <container-name>
# removes the container
```
# Networking
exposing ports
```
docker run --rm  -ti -p 1234567:1234567 -p 234567:234567 ubuntu:latest bash
# links port 1234567 on the container to the same port on the host
# links port 234567 on the container to the same port on the host
```
referer to the machine hosting the container
```
host.docker.internal
```
Show the port association
```
docker port <container-name>
```
Show networks
```
docker network ls
```

Create a network
```
docker network create <net-name>
```
Create a container in the new network
```
docker run --rm -ti --net net-name --name tserver1 ubuntu:20.04 bash
```
Put a container on a network
```
docker network connect net-name container-name
```
# Volumes
volume shared with the localhost
```
docker run --rm  -ti -v /user/myuser/example:/shared-folder ubuntu bash
```
create a volume to shared with other containers, only last while the containers are using it
```
docker run -ti -v /shared-data ubuntu bash
# create a container to use the same volume, give the name of the 1st container using the volume or any other that has used volume-from already
docker run -ti -v --volumes-from <container-did-1stshare-orotherusing it> ubuntu bash
```
# images
Find images
```
docker search ubuntu
```
# Docker Files
Create the file
FROM <image-name>
RUN <Run something>
CMD <Run something in the container>

Build the container 
docker built -t <container-name>

Run the container
docker run --rm <container-name>



FROM debian:sid
RUN apt-get -y update
RUN apt-get install nano
CMD ["/bin/nano", "/tmp/notes"]

FROM <image-to-use>
MAINTAINER name lastname email
RUN runs the command, executes, waits to finish and save the result
ADD adds local files
ENV set env variables
EXPOSE maps a port to the external host
VOLUME
WORKDIR
USER

