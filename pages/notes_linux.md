Linux Notes

## Managing files
Removing a directory
```
$rm -r mydir
```

### find
file containing
```
recursevely
grep -rnw '/path/to/somewhere/' -e 'pattern'

grep this_word /var/log/on_this_file

grep 'this_word\and_this_word' /var/log/on_this_file

In which line was found
grep -n this_word /var/log/on_this_file
```

file named
```
find /. -name 'toBeSearched.file' 2>/dev/null
find / -type f -name "*.txt"
```

list files mounted on nautilius - Connect to Server
```
ls -al /run/user/$UID/gvfs 
```
### tail
```
shows the last 20 lines
tail -n 20 <filename>

Keeps showing you additions to the file, as it gets updated
tail -f <my-file>

Keeps showing you additions to the file that match grep, as it gets updated
tail -f my_log.log | grep look_for_this
```

## Disk
How much you user uses:
```
du -hd 1 | sort -h

All in /
sudo du -smx /* | sort -n
sudo du -hsx /* | sort -rh | head -n 40

Top biggest
find / -printf '%s %p\n'| sort -nr | head -10

```

## sudo
from a user, become root
```
sudo bash
```
## Logs
```
ls -l /var/log/
/var/log/auth.log
/var/log/sssd/*.log
```

### Logs apache
```
/var/log/apache2/error.log
/var/log/apache2/access.log
```

## journalctl
```
The last 100
journalctl -n 100
The last in 1 hour
journalctl --since "1 hour ago"
```

Only messages related to a given user
```
~$ id <username>
~$ journalctl _UID=1388827084
```

Only kernel messages
```
sudo journalctl -k
```

By Unit
Only Networkmanager messages
```
sudo journalctl -u NetworkManager.service
```
Only lightdm messages
```
sudo journalctl -u lightdm
```
By Process
```
sudo journalctl -t systemd 
```
Extended messages
```
sudo journalctl -xe
```
Keep only last 2 days
```
journalctl --vacuum-time=2d
```
16.04
```
sudo journalctl -t gnome-session
```

## network
/etc/network/interfaces

restart network
```
sudo service network-manager restart
```

### nmap
````
Nmap:
sudo nmap -v -sS -A -T4 <hostname>

Check if a given port is open
sudo nmap -p <port-number> <ip-add> 
````

## VirtualBox
Install
```
sudo apt-get install virtualbox
```
Remove
```
sudo apt-get remove virtualbox
```

Issues with kernel reinstall dpkg
```
# remove current version from pkgs
# Download the latest version from their web site
sudo dpkg-reconfigure --priority low virtualbox-dkms 
sudo service virtualbox start
```

### VB cli

 list running vms
 $ VBoxManage list runningvms

 power off a vm
 VBoxManage controlvm "<vm-name-here>" poweroff --type headless
 
## SSH
Adding a public key to your computer so you can connect to a remote server; you need the remote server public key (Remote_Servers.pub)
```
$ chmod 0600 .ssh/Remote_Servers.pub
$ ssh-add Remote_Servers.pub
``` 
use a public key
```
ssh -i .ssh\remote_server.pub <user>@mydomain.com.au
```
## Packages 
Install a package
```
sudo dpkg -i DEB_PACKAGE
# sudo apt --fix-broken install ./filename.deb # to install dependencies
``` 
remove a Package
```
sudo dpkg -r PACKAGE_NAME
```
check packages installed
```
dpkg -l
dpkg -l | grep openssh-server
```
check package running
```
sudo service <service> status
sudo service ssh status
``` 
remove a package
```
sudo apt-get --purge remove gnome-terminal
``` 
install a package including dependencies
```
sudo apt-get install gnome-terminal
```
search cache packages
```
apt-cache search pkg_name
```
environmental Variables
```
#printenv
https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps
~$ printenv PATH
 
set
export VARIABLE=value
```

## Unclasified

repair xorg
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg

 ```
 
## vi
```
yy copy yank
p paste

/string 	search forward
?string 	search backward 
n 	move to next 
N 	move to next in opposite direction

^g line number

0 move to the begining of line
$ move to the end of line
:0<Return> or 1G  move to the first line
:$<Return> or G  move to the last line
```
## preferred pkgs

vpn Cisco
```
chmod 755 Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 
sudo Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 
```

Displaylink for the use of Docks
```
chmod 777 displaylink-driver-5.3.1.34.run 
sudo ./displaylink-driver-5.3.1.34.run 
```
ipa client to connect to institutional account using IPA Server
```
sudo apt-get install freeipa-client freeipa-admintools
```
git - software collaboration
```
sudo apt install git
```
ssh
```
sudo apt-get install openssh-server
```
ssh adding a local key copied from another computer (your previous), when using other computer's ssh_keys, you need to copy your key(id_rsa) to the new computer you will be using then add the key.
```
$ chmod 0600 .ssh/id_rsa
$ ssh-add id_rsa 
sudo service ssh restart
```
Accessing fileshares from files(Nautilius)
```
smb://<username>@server.dom.com.au/Users/<username>
```
Password DB 
```
apt-get install -y keepassxc
```
Accessing owncloud cloud storage
```
apt-get install -y owncloud-client
```
zoom - video conferencing
```
$ sudo apt --fix-broken install ./zoom_amd64.deb
```
fail2ban
```
apt install fail2ban -y
```
