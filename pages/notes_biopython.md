Biopython Notes

# Setting up your repo

## Create a Fork from biopython repo

Use your web interface to create a fork of https://github.com/biopython/biopython.git

## Clone your fork and setup the upstream
```
~/Projects$ git clone https://github.com/svalqui/biopython.git
~/Projects/biopython$ git remote add upstream https://github.com/biopython/biopython.git
```
## check origin is your fork, not the main repo
```
~/Projects/biopython$ git remote -v
origin	https://github.com/svalqui/biopython.git (fetch)
origin	https://github.com/svalqui/biopython.git (push)
upstream	https://github.com/biopython/biopython.git (fetch)
upstream	https://github.com/biopython/biopython.git (push)
```
## Check your git is setup user and email
```
git config --list
```
if not setup it up before any commits

## update you local master and fork
your need your github token ready for this.
```
git pull upstream master
git push origin master
```
with that you are ready to create your new branches for contributions.
