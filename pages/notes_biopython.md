# Biopython Notes

## Create a branch for your work/commits
Later on when your work is completed, you will create a Pull Request from the web interface against this branch.

```
In your /cloned/fork-repo
git checkout -b issue_1
```

## Setting up your repo

### Create a Fork from biopython repo

Use your web interface to create a fork of https://github.com/biopython/biopython.git

### Clone your fork and setup the upstream
```
~/Projects$ git clone https://github.com/<your-user>/biopython.git
~/Projects/biopython$ git remote add upstream https://github.com/biopython/biopython.git
```
### check origin is your fork, not the main repo
```
~/Projects/biopython$ git remote -v
origin	https://github.com/<your-user>/biopython.git (fetch)
origin	https://github.com/<your-user>/biopython.git (push)
upstream	https://github.com/biopython/biopython.git (fetch)
upstream	https://github.com/biopython/biopython.git (push)
```
### Check your git is setup user and email
```
git config --list
```
if not setup it up before any commits

### update your local master and fork
your need your github token ready for this.
```
git pull upstream master
git push origin master
```
In your web interface check that your fork copy **This branch is even with biopython:master.** with that you are ready to create your new branches for contributions.
