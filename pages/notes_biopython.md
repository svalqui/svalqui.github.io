Biopython Notes

= Setting up your repo

== Create a Fork from biopython repo

Use your web interface to create a fork of https://github.com/biopython/biopython.git

== Clone your fork and setup the upstream
```
git clone https://github.com/svalqui/biopython.git
git remote add upstream https://github.com/biopython/biopython.git
```
== check origin is your fork, not the main repo
```
$ git remote -v
origin	https://github.com/biopython/biopython.git (fetch)
origin	https://github.com/biopython/biopython.git (push)
upstream	https://github.com/biopython/biopython.git (fetch)
upstream	https://github.com/biopython/biopython.git (push)
```
