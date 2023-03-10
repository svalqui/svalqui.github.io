Prerequisites
```
apt install -y vim
apt install -y devscripts
apt-get install -y dh-python
apt-get install -y python3-distutils-extra
apt install -y python3-pip
pip3 install pybuilder
```
Directory structure
```
├── my-prj
│   ├── debian
│   │   ├── changelog
│   │   ├── compat
│   │   ├── control
│   │   ├── copyright
│   │   ├── rules
```
Create a standard log file `dch --create`
```
# cat debian/changelog 
my-prj (1.0.0) UNRELEASED; urgency=low

  * Initial release. (Closes: #XXXXXX)

 -- root <root@vm.local>  Thu, 02 Mar 2023 04:42:42 +0000
```
echo "7" > debian/compat # might not be necesary
```
# cat debian/compat 
7
```
Create debian control file
reference
https://www.debian.org/doc/debian-policy/ch-controlfields.html
```
# cat debian/control 
Source: my-prj
Section: devel
Priority: optional
Maintainer: my@email.org
Standards-Version: 1.0.0
Build-Depends:  python3,       # Dependencies to build the package on the building host
 python3-distutils-extra,
 dh-python

Package: my-prj
Section: devel
Priority: optional
Architecture: all
Depends: pymodule(>= 1.0), pymodule, pymodule(>= 0.5.0) # Dependencies on the installing host, apt install ./my.deb should install, shall be accesible via pip
Description: my-prj-shrt-des
 long-des

```
Rules for python package
```
# cat debian/rules 
#!/usr/bin/make -f
%:
	dh $@ --with python3 --buildsystem=pybuild

```
Built it

```
# fakeroot dpkg-buildpackage -b
```
