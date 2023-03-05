```
├── my-prj
│   ├── debian
│   │   ├── changelog
│   │   ├── compat
│   │   ├── control
│   │   ├── copyright
│   │   ├── rules
```

```
# cat debian/changelog 
my-prj (1.0.0) UNRELEASED; urgency=low

  * Initial release. (Closes: #XXXXXX)

 -- root <root@vm.local>  Thu, 02 Mar 2023 04:42:42 +0000
```

```
# cat debian/compat 
9
```

```
# cat debian/control 
Source: my-prj
Section: devel
Priority: optional
Maintainer: my@email.org
Standards-Version: 1.0.0
Build-Depends:  python3,
 python3-distutils-extra,
 dh-python

Package: my-prj
Section: devel
Priority: optional
Architecture: all
# Depends: pymodule(>= 1.0), pymodule, pymodule(>= 0.5.0)
Description: my-prj-shrt-des
 long-des

```

```
# cat debian/rules 
#!/usr/bin/make -f
%:
	dh $@ --with python3 --buildsystem=pybuild

```


```
# fakeroot dpkg-buildpackage -b
```
