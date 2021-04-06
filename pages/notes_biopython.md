# Biopython Notes

# Install

## Install/remove with pip
Installing Biopython
```
pip3 install biopython
pip3 install --upgrade biopython
pip3 uninstall biopython
```

## Install from source
First setup your repository, what do you want to install? the master or the branch? checkout accordingly and install. Most of the time I install from master and run test against the files I updated on the branch, when they are minor changes that is fine; when is functional code, bugs or adding functionallity, is better to install from the branch.

On ~/Projects/biopython$
```
sudo python3 setup.py build
python3 setup.py test
sudo python3 setup.py install
```
## Black format
```
# Turn black code style off
# fmt: off

# This comment stops black style adding a blank line here, which causes flake8 D202.

# fmt: on
```
## Test
You don't need to install to test, but it needs to be built before you test.
```
biopython$ python3 setup.py build
```

run all test including online test, doctest.
```
biopython$ python3 setup.py test

Which is the same as 
biopython/Tests$ python3 run_tests.py
```
Run individual test:
```
biopython/Tests$ python3 run_tests.py test_file1.py test_file2.py
```
To run the docstring test use:
```
biopython/Tests$ python3 run_tests.py doctest
```
To skip online test:
```
biopython/Tests$ python3 run_tests.py --offline
```
Run doctest in the tutorials, within Test directory
```
biopython/Tests$ python3 run_tests.py test_Tutorial.py
more examples here https://github.com/biopython/biopython/issues/531
```
### Clear built and re-built
when you update the code and need to re-test, remove the previous built, re-built then test.

deleting the previous build directory
```
~/Projects/biopython$ sudo rm -R ~/Projects/biopython/build
And remove all *.pyc files
~/Projects/biopython$ find . -name "*.pyc" -exec rm -rf {} \;
```
Re-built
```
# On ~/Projects/biopython$
sudo python3 setup.py build
python3 setup.py test
```

### Checking where Bio defaults to
within Python3, out of the source code,
```
import sys; print(sys.version)
import platform; print(platform.python_implementation()); print(platform.platform())
import Bio; print(Bio.__version__)
```
## Setting up your development environment

### Install the pre commit hooks (pep. black)
```
$ cd your-local-biopython-repositoty-branch
# (check correct venv)
$ pip3 install pre-commit

# Activate pre-commit for biopython
$ pre-commit install
```
### For commits to Documentation
```
$ pip3 install restructuredtext_lint
$ rst-lint --level warning *.rst
```
More information here: https://github.com/biopython/biopython/blob/master/CONTRIBUTING.rst

### Create a branch for your work/commits
Later on when your work is completed, you will create a Pull Request from the web interface against this branch.

```
In your /cloned/fork-repo
git checkout -b issue_1
```

## Setting up your repo

### Create a Fork from biopython repo

Use your web interface on Github to create a fork of https://github.com/biopython/biopython.git, this creates your own repository of the biopython project, takes a snapshot of it and adds it to your repositories, you will need to keep this updated, see below instructions on how to update your fork.

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

### pre-commit and commit
#### Checks from cli
black style
```
/usr/local/bin/black --diff ~/PythonProjects/biopython/Bio/new_files
```
doc8 check
```
doc8 NEWS.rst 
```

flake8
```
python3 -m flake8 PythonProjects/biopython/TBio/new_files
```
Options for flake8
```
~/PythonProjects/biopython$ python3 -m flake8 --version
python3 -m flake8 PythonProjects/biopython/Tests
flake8 --select E123,W503 path/to/code/
flake8 --ignore E24,W504 path/to/code/
python3 -m flake8 --ignore E225,E226,E231,E211,E111,E113,E112,E261,E128,E402,E227,W604,W504,E702,W503 NEWS.rst
```

# Other
Index of ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase3/
Index of ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/proteomics_mapping/

ISC002 implicitly concatenated string 
A502 prefer assertIsNone() instead of comparing to None
