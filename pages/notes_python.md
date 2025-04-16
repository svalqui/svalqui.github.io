Collection of commands from https://stackoverflow.com
https://stackoverflow.com/questions/20101834/pip-install-from-git-repo-branch

# pip/Modules

## pip install

```
pip install git+https://github.com/prj/prj.git@branch-name

# Install to a directory
pip install git+https://github.com/prj/prj.git@branch-name -t /target-dir
```

## pip update
$ python3 -m pip install --upgrade pip

## Modules installed

`$ pip freeze` or from python:
```
>>> help("modules")
```
which version
```
$ pip show <module>
```
## upgrade a module
```
sudo pip3 install <package-name> --upgrade
```
## downgrade a module or install a particular version
```
pip install --force-reinstall -v "my_module==0.0.2"
```

## show which version are available
```
pip index versions python-novaclient
```
## remove pip module
```
pip uninstall <pkg-name>
```

# Flake8

best way to run it

```
python3 -m flake8 scripts setup.py
```

# virtual environments
```
$. .venv/bin/activate
# or
$ source venv/bin/activate
# Deactivate 
$ deactivate
```
Create
```
# create venv on current directory
python3 -m venv venv
# or
python3 -m venv /path/to/new/virtual/environment
```
Create a venv with a different version of python, you need the version already installed on your system
```
# https://stackoverflow.com/questions/1534210/use-different-python-version-with-virtualenv
$ virtualenv --python="/usr/bin/python3.12" ".venv"

```
Deactivate
```
$ deactivate
```
## virtualenv
Install
sudo pip3 install virtualenv

Create
virtualenv -p python3 my-env

Activate
source my-env/bin/activate

## find venv packages
```
find / -d -name "dist-packages" 2>/dev/null
```

# Create a deb package from you project
```
python3 setup.py --command-packages=stdeb.command install_deb
# python3 setup.py --command-packages=stdeb.command bdist_deb???
```
