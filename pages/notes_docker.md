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




