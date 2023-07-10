image:
the bare minimun of a bare OS

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
