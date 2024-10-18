Collection of commands from https://stackoverflow.com
# https://stackoverflow.com/questions/20101834/pip-install-from-git-repo-branch

# pip/Modules

## pip install

```
pip install git+https://github.com/prj/prj.git@branch-name
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
$ source venv/bin/activate
$ deactivate
```
Create
```
python3 -m venv /path/to/new/virtual/environment
# or create venv on current directory
python3 -m venv venv
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
