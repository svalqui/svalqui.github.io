Linux Notes

# System
Version of linux
```
cat /etc/os-release
hostnamectl
lsb_release -a
```
# Managing files
Removing a directory
```
$rm -r mydir
```

## find
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
## tail
```
shows the last 20 lines
tail -n 20 <filename>

Keeps showing you additions to the file, as it gets updated
tail -f <my-file>

Keeps showing you additions to the file that match grep, as it gets updated
tail -f my_log.log | grep look_for_this
```
## file permissions
```
# change owner
$ chown sergio .ssh/known_hosts
# change group
$ chown :sergio .ssh/known_hosts
# or change both 
$ chown sergio:sergio .ssh/known_hosts
```
# Disk
How much you user uses:
```
du -hd 1 | sort -h

All in /
sudo du -smx /* | sort -n
sudo du -hsx /* | sort -rh | head -n 40

Top biggest
find / -printf '%s %p\n'| sort -nr | head -10

```
Show Disks:
```
sudo fdisk -l
````
Show Partitions:
````
sudo lsblk
or
fdisk -l /dev/sda
````
Fix disk with errors, corrupted files
```
fsck -yf /dev/<your-disk-partition-here>
```
List the scsi device
```
# lsscsi
 or
 # cat /proc/scsi/scsi 
```
List Physical Volumes
```
 # pvdisplay
```
List Logical Volumes
```
 # lvdisplay
```
List disk space
```
 # df -h
```

## Clear Space
```
dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge
```


# Shares


## Cifs mount

```
edit and add on /etc/fstab

//Share   /mnt/share-on-local cifs credentials=/etc/.credentials,fsc,ro,file_mode=0664,dir_mode=0775 0 0

.credentials
username=username
password=pass
domain=my-domain.rog
```
## NFS mount

edit and add on /etc/fstab
```
my-nfs.org.au:/<nfs-share-vol> /mnt/my-nfs/<local-name> nfs vers=3,fsc 0 0
```
or via /etc/auto.local
```
<local-dir>	-fstype=nfs,nfsvers=3,exec	my-nfs.org.au:/<nfs-share-vol>
```
## ssh mount
```
sshfs#root@sharing_svr.org.au:/mnt/shared/home /mnt/local_home fuse allow_other,default_permissions,IdentityFile=/root/.ssh/details 0 0
```

Test
```
sshfs root@svr1.your.domain.org:<directory/source> /mnt/test/
or 
sshfs -oIdentityFile=/root/.ssh/ssh-key-4svr1 root@jsrv1.org.au:/mnt/my-dir /mnt/svr1-mydir
```
## list smb shares
```
smbclient -L myserver.mydomain.org.au -U username@mydomain.org.au
```

# Home in another partition
1. from your spare space create a new partition ext4 for your home directories
```
fdisk -l # list your disks/partitions
gdisk /dev/sdx # Option n (new), 1 (partition 1), w (write)
sudo mkfs.ext4 /dev/sdx1 # or sudo mkfs -t ext4 /dev/sdx1, formats the partition

```
2. mount the new partition
```
mount -t auto /dev/sbd1 /mt/sdb1
```
3. take note of the uuid of the new partition
```
blkid
```
4. copy your current /home to the new partition
```
sudo rsync -aXS /home/. /mnt/home/.
```
5. edit /etc/fstab to auto mount the new partition, use the uuid of the new partition
```
UUID=uuid-of-new-partition     /home     ext4     nodev,nosuid     0     2
```
6. rename your current home as old, and create a new home directory, called /home, to mount the home on the new partition
```
cd /
sudo mv /home /home_old
sudo mkdir /home
```
7. reboot
8. check home is working on the new partition and delete the old home directory

# Processes
## Dealing with zombies
```
Find Zombies
ps aux | egrep "Z|defunct"

Using the PID of the defunct process find the parent PID
ps -o ppid= -p <PID of the defunct here>
 
Double check which is the parent PID
ps -e | grep <parent PID here>

Send the SIGCHLD signal to the parent process.
kill -s SIGCHLD <parent PID>

or try killing the parent process
kill -SIGKILL 7636
```
## Processes of a user
```
top -U <UserName>
ps -u <UserName>
ps -ef | grep <UserName>
```

# Memory
## Memory issues
```
# egrep "of memory" /var/log/kern
```
## Memory use
```
free -mh
```

# sudo
from a user, become root
```
sudo bash
```
Who is sudo
```
grep '^sudo:.*$' /etc/group | cut -d: -f4
```

# Logs
```
ls -l /var/log/
/var/log/auth.log
/var/log/sssd/*.log
```
## Logs apache
```
/var/log/apache2/error.log
/var/log/apache2/access.log
```
## Users last login
```
last
```
## Check Logs for system issues
```
dmesg | tail

dmesg | tail -f /var/log/syslog

sudo find /var/log -type f -mtime -1 -exec tail -Fn0 {} +
https://www.hpe.com/us/en/insights/articles/the-first-5-things-to-do-when-your-linux-server-keels-over-1705.html
```


# journalctl
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
16.04 session
```
sudo journalctl -t gnome-session
```
Logs before last reboot
```
journalctl -b -1
```
# Authentication
```
last <username>
```

## Authentication failures
```
grep "Failed password" /var/log/auth.log
```
```
egrep "Failed|failure" /var/log/auth.log
```
```
grep "Failed password" /var/log/auth.log | awk '{print $11}' | uniq -c | sort -nr
```
```
journalctl _SYSTEMD_UNIT=ssh.service | egrep "Failed|Failure"
```
```
lastb -a | more
```
```
last <username>
```
Bad login attempts
```
last -f /var/log/btmp
```

## Authentication succes
```
egrep "New session" /var/log/auth.log
egrep "session opened for user" /var/log/auth.log
egrep "session opened for user|session closed for user|Removed session" /var/log/auth.log
```

# network
/etc/network/interfaces

restart network
```
sudo service network-manager restart
```
ping
```
ping server.org.au | while read pong; do echo "$(date): $pong"; done
```
## nmap
````
Nmap:
sudo nmap -v -sS -A -T4 <hostname>

Check if a given port is open
sudo nmap -p <port-number> <ip-add> 

No probes
sudo nmap -v -sS -A -T4 -Pn <ip-add>

````
## ports listening
```
sudo lsof -nP -iTCP -sTCP:LISTEN
```
## ports open
```
sudo apt install net-tools
$ netstat -tapn
```
or
```
sudo lsof -nP -iTCP -sTCP:ESTABLISHED
```
## DNS
```
systemd-resolve --status

```
Check DNS resolvers, this is a dynamic file do not change it directly, it would recreate when restarting system. changes via resolvconf, which doesn't install by default.
```
cat /etc/resolv.conf
```

## network monitor
```
ethtool <interface> 
```
$ sudo iptraf

ip link show

lspci | grep Ethernet

ifconfig -a | grep Link
 
netstat -i | column -t
 
ip route | column -t
 
netstat -r
 
### show errors

ifconfig -a

# SSH
 
Adding a public key to your computer so you can connect to a remote server; you need the remote server public key (Remote_Servers.pub)
```
$ chmod 0600 .ssh/Remote_Servers.pub
$ ssh-add Remote_Servers.pub
``` 
use a public key
```
ssh -i .ssh\remote_server.pub <user>@mydomain.com.au
```
terminate a locked/frozen session
```
Enter ~ .
```
## ssh file copy scp
[COMMAND] [OPTIONAL ARGUMENTS] [SOURCE] [DESTINATION]

Copy a file
```
scp my-file <username>@<host-name>:/home/
```
Copy recursively
```
scp -r * <username>@<host-name>:/home/
```
# Repositories
list
```
grep ^[^#] /etc/apt/sources.list /etc/apt/sources.list.d/*
```

# Packages 

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
install a package including dependencies
```
sudo apt-get install gnome-terminal
```
search cache packages
```
apt-cache search pkg_name
```
show the package an its dependencies
```
sudo apt-cache show <package-name>
```
remove a package
```
sudo apt-get --purge remove gnome-terminal
``` 
remove rc , broken packages
```
sudo apt-get remove --purge <packge-name>
```
source of package
```
dpkg -s <package>
```
which repo comes from
```
apt-cache showpkg <package>
``` 
extract a deb file
```
ar vx file.deb
tar xvf control.tar.xz
tar data.tar.xz
```

# Software

## VirtualBox
Install
```
sudo apt-get install virtualbox
```
Remove
```
sudo apt-get remove virtualbox
```
Upgrade, there is no upgrade, remove and install the newest version
```
$ sudo apt-get remove virtualbox
$ sudo dpkg -i virtualbox-6.1_6.1.26-145957~Ubuntu~eoan_amd64.deb
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
```
$ VBoxManage list runningvms
```
power off a vm
```
VBoxManage controlvm "<vm-name-here>" poweroff --type headless
```
## fail2ban

### Fail2ban log 
```
/var/log/fail2ban.log
```
### Jails list
```
fail2ban-client status
```
### IPs in jail
```
fail2ban-client status <JailName>
```
### unban ip
```
fail2ban-client set <jail_name> unbanip <ip_address>
```
## Puppet

Check puppet client has finished applying catalogue
```
journalctl -u puppet -f  # Shall show "Applied catalog"
```
test puppet agent
```
puppet agent --test
```
If is running
```
sudo cat `sudo puppet agent --configprint agent_disabled_lockfile`
```
Run Puppet agent
```
/opt/puppetlabs/bin/puppet agent -t 
```
Enable Puppet agent
```
echo "START=yes" > /etc/default/puppet 
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true 
/opt/puppetlabs/bin/puppet resource service pxp-agent ensure=running enable=true 
```
# Environmental Variables
```
#printenv
https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps

#set
export VARIABLE=value

# PATH
~$ printenv PATH
or
echo $PATH

# Add a path
# to the end
export PATH=$PATH:/path/to/add
# to the begining
export PATH=/path/to/add:$PATH
```
# Shutdown
## force shutdown
```
echo 1 > /proc/sys/kernel/sysrq
echo b > /proc/sysrq-trigger
```

# Files edit, content
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

# Show line number
:set number

```
### show line numbers
```
$ vi .vimrc
# on the file append
set number
```
## diff
```
diff file1 file2
diff -y file1 file2
```

# Unclasified

repair xorg
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg

 ```
 
# Desktops 

Check there is at least one desktop installed on the server
```
systemctl status <desktop-package-name>
```
## gnome

is the default, on x sessions some apps do not work, such term, you might one to keep it because looks fine on the console, but better to install an additional one.
```
systemctl status gdm
# if not started "gdm.service: Unit cannot be reloaded because it is inactive"
#systemctl start gdm
```
Install
```
sudo apt install tasksel
sudo tasksel install ubuntu-desktop 
systemctl start gdm
```
Install xterm, gnome terminal doesn't work well
```
sudo apt-get install -y xterm
```
Remove ubuntu-desktop
```
sudo apt purge ubuntu-desktop -y && sudo apt autoremove -y && sudo apt autoclean
sudo apt-get purge gnome*
```
## Mate
Seems reasonable but on x sessions might need to define the initial menus
```
sudo tasksel install ubuntu-mate-desktop
```
## lxde

Also looks reasonable looks good on x sessions
```
sudo apt install lxde
```
## Desktop notes
Which display you are using
```
systemctl status display-manager
```
Where the configurations sits:
depending on the desktop
```
/etc/lightdm/lightdm.conf
/etc/gdm3/custom.conf
/etc/sddm.conf.d/autologin.conf
```
Login configuration for lightdm
/etc/lightdm/lightdm.conf
```
[SeatDefaults]
# some times fixes to a user, then set manual login
greeter-show-manual-login=true
#enable auto login
autologin-user=<user_name>
autologin-user-timeout=0
```
# preferred pkgs

Python development packages
```
sudo apt-get install python3-dev
```
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
codecs
```
sudo apt-get install ubuntu-restricted-extras
```
cifs support
```
sudo apt install cifs-utils
```

#Minecraft
```
screen -S "Minecraft server"
java -Xmx1024M -Xms1024M -jar minecraft_server.1.17.1.jar nogui
```
