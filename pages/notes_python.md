Collection of commands from https://stackoverflow.com

# Modules

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
# pip update
$ python3 -m pip install --upgrade pip

# virtualenv
Install
sudo pip3 install virtualenv

Create
virtualenv -p python3 my-env

Activate
source my-env/bin/activate
my-env/bin/activate

# Create a deb package from you project
```
python3 setup.py --command-packages=stdeb.command install_deb
# python3 setup.py --command-packages=stdeb.command bdist_deb???
```
