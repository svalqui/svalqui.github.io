Linux Notes
some of this comes from https://stackoverflow.com

# Sudo
from a user, become root
```
sudo bash
```
Check user has sudo rights
```
sudo -l -U  <username>  
```
Adding a sudo user
```
useradd <username>
passwd <username>
usermod -aG sudo <username>
```
List the sudo users
```
$ grep '^sudo:.*$' /etc/group | cut -d: -f4
```
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

file containing, use grep
```
recursevely - word
# grep -nrs 'word-to-look-for' /<directory-path>

recursevely - expression
grep -rnw '/path/to/somewhere/' -e 'pattern'

grep this_word /var/log/on_this_file

grep 'this_word\and_this_word' /var/log/on_this_file

In which line was found
grep -n this_word /var/log/on_this_file
```

file by name, use find
```
find . -name "*<str-here>*"
find /. -name 'toBeSearched.file' 2>/dev/null # send error to null
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
# change both 
$ chown sergio:sergio .ssh/known_hosts
# change recursively 
$ chown -R sergio:sergio /mnt/my-data
```
## tar 

### .tar.gz
Create
```
$tar -cvf  /home/my-tar-file.tar /home/dir-to-tar/.
```
Extraxt
```
tar xvzf my-file.tar.gz
```

## rsync

```
rsync -aAXvPh /media/usr/Data/bkp-laptop-scienceit/home/usr/Desktop/ /home/usr/Desktop/
```
dry run 
```
rsync -naAXvPh /source/* /dest/
```
### options
-a archive mode; equals -rlptgoD (no -H,-A,-X)
-A preserve acl
-X extended attributes, preserve
-v verbose
-P progress
-h human redable

## Symbolic links (shortcut file)

ln -s /full/path/of/existing/file.txt /home/my-link-name.txt

## Open files

Open fiels in a given directory
```
# lsof +D '/usr/local/'
```

## Files and directories are the same
https://askubuntu.com/questions/421712/comparing-the-contents-of-two-directories
```
find /dir1/ -type f -exec md5sum {} + | sort -k 2 > dir1.txt
find /dir2/ -type f -exec md5sum {} + | sort -k 2 > dir2.txt
diff -u dir1.txt dir2.txt
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
io statistics
```
iostat -x
```
Disk use and other siak stats and data
```
$ sudo smartctl -a /dev/nvme0n1p5
```

## Clear Space, root full
```
dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge
```
```
sudo apt-get clean
```
## Disk format and partitionspartition and format 
partion and format
```
fdisk -l # list your disks and partitions
lsblk -f # list the filesystem type
gdisk /dev/vdc # Option n (new), 1 (partition 1), w (write) # partition
sudo mkfs.ext4 /dev/sdx1 # or sudo mkfs -t ext4 /dev/sdx1, formats the partition
```
or format full disk
```
# mkfs.ext4 /dev/vdc
```
## mount new disk permanently
list the file system type
```
# lsblk -f
```
Format disk and/or partition
```
# mkfs.ext4 /dev/vdc
```
Add an entry to /etc/fstab
```
/dev/vdc       /mnt/disk ext4    defaults        0       0
```
mount
```
$ sudo mount -a
```
check mount works
```
$ df -h
```
## Home in another partition
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
6. test fstab mounts
```
mount -a
```
7.rename your current home as old, and create a new home directory, called /home, to mount the home on the new partition
```
cd /
sudo mv /home /home_old
sudo mkdir /home
```
8.reboot
9.check home is working on the new partition and delete the old home directory


# Shares

## Cifs mount

```
edit and add on /etc/fstab

//Share   /mnt/share-on-local cifs credentials=/etc/.credentials,fsc,ro,file_mode=0664,dir_mode=0775 0 0

.credentials
username=username
password=pass
domain=my-domain.rog


Mounting using current ticket/credentials
sudo mount -v -t cifs //srv1.org.au/sharedir/ /mnt -o user=$USER,cruid=$USER,sec=krb5

Mounting authenticating on mount
sudo mount -v -t cifs //srv1.org.au/sharedir/ /mnt -o username=my-user
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
sshfs#root@sharing_svr.org.au:/shared/dir /mnt/local_home fuse allow_other,default_permissions,IdentityFile=/root/.ssh/details 0 0
```

Test
```
sshfs root@svr1.your.domain.org:<directory/source> /mnt/test/
or 
sshfs -o allow_other,default_permissions -o IdentityFile=/root/.ssh/ssh-key-4svr1 root@jsrv1.org.au:/mnt/my-dir /mnt/svr1-mydir
```
## list smb shares
```
smbclient -L myserver.mydomain.org.au -U username@mydomain.org.au
```

## mediaflux
```
smbclient //mediaflux.yourdomain.net/your-proj -m SMB2 -W <AD-domain> -U <username>
```
```
# sudo apt install krb5-user keyutils
# id <username>
# kinit <username>@YOURDOMAIN.ORG
# sudo mount -t cifs -o cruid=$USER,sec=krb5,uid=$UID,gid=$(id -g),vers=2.0 //mediaflux.yourdomain.org/your-proj /mnt/testmount
``` 

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
Kill processes of a given user
```
# pkill -9 -u <username>
```
## run app as a user
```
# su -c "/usr/lib/bin/java" <username>
```
# Services

List services running
```
systemctl list-units --type=service
```
List services failed
```
systemctl list-units --state=failed
```
Stop start services
```
service sssd stop
service sssd start
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

last -w | 

Unique
last -w | sort -uk1,1

# grep -i <username> /var/log/auth.log
```
## Check Logs for system issues
```
dmesg | tail

dmesg | tail -f /var/log/syslog

sudo find /var/log -type f -mtime -1 -exec tail -Fn0 {} +
https://www.hpe.com/us/en/insights/articles/the-first-5-things-to-do-when-your-linux-server-keels-over-1705.html
```

# Multicommand

## pdsh
```
pdsh -l root -w svr[01-10],svr[22-31],svr218 'systemctl restart dhcp'
```

## clush
```
clush -bL -l root -w svr[211-317]-storage 'ls /dev/nvm* | sort'
```

-b clush waits for command completion and then displays gathered output results  
-w allows you to specify remote hosts by using ClusterShell NodeSet syntax  
-L disable header block and order output by nodes; additionally, when used in conjunction with -b/-B, it will enable "life gathering" of results by line mode, such as the next line is displayed as soon as possible  
-l USER, --user=USER  


# journalctl
```
The last 100
journalctl -n 100
The last in 1 hour
journalctl --since "1 hour ago"
The last day
journalctl --since "1 day ago"
Reverse order
journalctl -r
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
# sudo journalctl -t acvpnagent # Cisco Anyconect
```
Puppet related
```
journalctl -u puppet -f  # Shall show "Applied catalog"
```
Extended messages
```
sudo journalctl -xe
```
Keep only last 2 days
```
journalctl --vacuum-time=2d
```
Keep Only 500Mb in the log
```
sudo journalctl --vacuum-size=500M
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

## Authentication Configuration
/etc/nsswitch.conf

### PAM AD
Refer to /etc/ldap/ldap.conf and /etc/nsswitch.conf

Auto create homedir
/etc/pam.d/common-session:
Add this line at the bottom:
session required pam_mkhomedir.so umask=0022 skel=/etc/skel


### IPA
Refer to /etc/sssd/sssd.conf

### Kerberos
/etc/krb5.conf

### Samba
chack /etc/samba/smb.conf

### Winbind
Look that is enabled here:
```
/etc/pam.d/login
```
And also
```
/etc/security/access.conf
```
### Authentication rules on pam
```
grep '^auth' /etc/pam.d/*
```
## Authentication restrictions
Refer to cat /etc/security/access.conf 

## Log
refer to /var/log/auth.log to identify the type of authentication

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
## sssd related
The service
```
service sssd stop
service sssd start
```
The Logs
```
/var/log/sssd/*.log
```
### Check is working
```
systemctl status sssd-pam.socket
systemctl status sssd-pam.service
systemctl status sssd-pam-priv.socket
```
### Clearing sssd cache
```
systemctl stop sssd
sudo sss_cache -E  # or rm -rf /var/lib/sss/db/*
systemctl start sssd
```
### Fixing issues with sssd - reconfiguring
https://askubuntu.com/questions/1288626/ubuntu-20-10-sssd-system-security-services-daemon-failure

```
sudo cp /usr/lib/x86_64-linux-gnu/sssd/conf/sssd.conf /etc/sssd/.
sudo chmod 600 /etc/sssd/sssd.conf 
sudo systemctl enable sssd
sudo systemctl start sssd
# Then add your configuration
```
## some test
```
Check that you can see AD group membership
$ getent group groupname@domain.org

Check you can see user
$ getent passwd <username@domain.org>

Check user can join Ad Domin
$ sudo net ads join -U <username>
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
## nc
```
nc -zv <ip> <port>
```
## nmap
````
Nmap:
sudo nmap -v -sS -A -T4 <hostname>

Check if a given port is open
sudo nmap -p <port-number> <ip-add> 

No probes
sudo nmap -v -sS -A -T4 -Pn <ip-add>

Subnet check
nmap -sP 192.168.1.0/24 --scan-delay 1s

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
$ sudo iptraf
ip link show
lspci | grep Ethernet
ifconfig -a | grep Link
netstat -i | column -t
ip route | column -t
netstat -r
```
### show errors

ifconfig -a

## Network performance

### iperf3, between 2 hosts
- test the delfault port is open
```
# nmap -Pn -p 5201 x.x.x.x
```
- On one host run
```
# iperf3 -s
```
- On the other run
```
# iperf3 -c x.x.x.x
```
### transfering files to cifs

- Create a test file 
```
$ dd if=/dev/zero of=1g-test-file.tmp bs=1 count=0 seek=1G
```
- rsync the file and show stats
```
$ rsync -hP 1g-test-file.tmp  /mnt/file-test.tmp
```
# SSH
 
Adding a public key to your computer so you can connect to a remote server; you need the remote server public key (Remote_Servers.pub)
```
$ chmod 0600 .ssh/Remote_Servers.pub
$ ssh-add Remote_Servers.pub
``` 
Adding a public key to a server so the user can ssh to the server
```
# cat /tmp/id_rsa_user_pub >> ~USER/.ssh/authorized_keys
```
use a public key
```
ssh -i .ssh\remote_server.pub <user>@mydomain.com.au
```
terminate a locked/frozen session
```
Enter ~ .
```
debug  a connection
```
ssh -vvv root@my-server.org
```
## ssh file copy scp
[COMMAND] [OPTIONAL ARGUMENTS] [SOURCE] [DESTINATION]

Copy a file
```
scp my-file <username>@<host-name>:/home/

File from a server to local
scp root@svr.org.au:/root/data svr-data
```
Copy recursively
```
scp -r * <username>@<host-name>:/home/
```
Copy directory
```
scp -r  <username>@<host-name>:/path/directory /local/loc-dir
```
Copy with ssh-key
```
scp -i my-ssh-pub-key /home/file-to-cp.txt  root@<ip>:/home/
```
## ssh good to know
Show auth methods
```
$ ssh -o PreferredAuthentications=none localhost
```
## ssh server configuration
Allow X11
```
# cat /etc/ssh/ssh_config 
Host *
    ForwardAgent yes
    ForwardX11Trusted yes
    GSSAPIAuthentication no
    ServerAliveInterval 10
```
# wget
Download a file
```
wget https://my-server.org/my-file.45.171.01.tar
```
Download and name it somethinelse
```
wget -O my-new-name https://my-server.org/my-file.45.171.01.tar
```
Download it in a different directory
```
wget -P /mnt/my-files/ https://my-server.org/my-file.45.171.01.tar
```
Download in backgroud
```
wget -b https://my-server.org/my-file.45.171.01.tar
# check the status
tail -f wget-log
```

# curl


# Repositories
list
```
grep ^[^#] /etc/apt/sources.list /etc/apt/sources.list.d/*
```
Add a repo
```
# Sources are located in /etc/apt/sources.list.d
# there should be a files for you repo, my-cia.list containing all the repos for your cia
deb https://my-repo.org focal main
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
show the version available of the package
```
apt-cache showpkg <package-name>
```
show source of package
```
dpkg -s <package>
```
show which repo comes from
```
apt-cache showpkg <package>
``` 
which can be upgraded
```
apt list --upgradable
```
remove a package
```
sudo apt-get --purge remove gnome-terminal
``` 
remove rc , broken packages
```
sudo apt-get remove --purge <packge-name>
```
extract a deb file
```
ar vx file.deb
tar xvf control.tar.xz
tar data.tar.xz
```
all packages in a repo
```
# ls /var/lib/apt/lists/*_Packages
and open or grep  the corresponding repo file
```

# Software

## tmux
Create a new session
```
tmux new -s my-sess1
```
To detatach, on session
```
press Ctrl+B, and then D
```
To attach to a session
```
tmux attach-session -t my-sess1
```
To open a new session on terminal
```
tmux new -s my-sess2
```
To move to another session from a session
```
press Ctrl+B, and then S
```
https://linuxize.com/post/getting-started-with-tmux/
```
    Ctrl+b c Create a new window (with shell)
    Ctrl+b w Choose window from a list
    Ctrl+b 0 Switch to window 0 (by number )
    Ctrl+b , Rename the current window
    Ctrl+b % Split current pane horizontally into two panes
    Ctrl+b " Split current pane vertically into two panes
    Ctrl+b o Go to the next pane
    Ctrl+b ; Toggle between the current and previous pane
    Ctrl+b x Close the current pane
```
## VirtualBox
Install
```
sudo apt-get install virtualbox
```
Install the extension pack
```
sudo apt install virtualbox-ext-pack
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
fail2ban-client set sshd unbanip <ip_address>
```
### banned ssh ip
```
fail2ban-client status sshd
```
## Puppet
Puppet Client configuration
```
/etc/puppetlabs/puppet/puppet.conf
[main]
srv_domain = <puppet-server-here>

[agent]
environment = <Env-to-use>
```

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
Disable puppet agent
```
puppet agent -t --disable "Puppet Agent Down for Maintenance"
```
Run Puppet dry run
```
puppet agent -t --agent_disabled_lockfile=/none --no-use_srv_records --server r10k.org --environment <your-dev-here> --noop

```
Run Puppet from a r10k environment
```
puppet agent -t --agent_disabled_lockfile=/none --no-use_srv_records --server r10k.org --environment <your-dev-here>

```
Enable Puppet agent
```
/opt/puppetlabs/bin/puppet agent --enable
```
Start puppet agent service
```
echo "START=yes" > /etc/default/puppet 
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true 
/opt/puppetlabs/bin/puppet resource service pxp-agent ensure=running enable=true 
```
Clear pupppet agent on host
```
delete /etc/puppetlabs/puppet/ssl.
# run puppet agent to request a new cert
```
Puppet certs on ca server
```
# all certs
sudo puppetserver ca list --all

# needed signature
sudo puppetserver ca list

# sign cert
sudo puppetserver ca sign  --certname <hostname>
 
# delete cert
sudo puppetserver ca clean --certname
```
## zoom
remove old version
```
sudo dpkg -r zoom
```
install zoom
```
sudo apt --fix-broken install ./Downloads/zoom_amd64.deb
```
## Terminal

### is your scroll bar gone?
Try : ```tput rmcup```

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
# Bash Prompt
```
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]($ENV-VAR)\[\033[00m\]\$ '
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;33m\]($ENV-VAR)\[\033[00m\]\$ '
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;33m\]$OS_PROJECT_NAME\[\033[00m\]\$ '
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

## grep
```
# match to include System, Memory, Processor
| grep -i -e System -e Memory -e Processor
-e pattern
-i ignore case

# include x lines before and after match
| grep -A 10 -i -e product
| grep -A 10 -B 10 -i -e product
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

# configuring a yubikey
https://gist.github.com/artizirk/d09ce3570021b0f65469cb450bee5e29

# preferred pkgs

```
DEBIAN_FRONTEND=noninteractive apt-get install -y python3-dev git sysstat \
net-tools nmap openssh-server keepassxc fail2ban smartmontools cifs-utils \
nfs-common icedtea-netx
```
vpn Cisco
```
chmod 755 Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 
sudo Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 
# Configuration profile /opt/cisco/anyconnect/profile
# Default host name ~/.anyconnect
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
Accessing owncloud cloud storage
```
apt-get install -y owncloud-client
```
zoom - video conferencing
```
$ sudo apt --fix-broken install ./zoom_amd64.deb
```
codecs
```
sudo apt-get install ubuntu-restricted-extras
```
Virtal Machine Manger
```
sudo apt-get install virt-manager
# and
sudo apt-get install ssh-askpass-gnome
```
#Minecraft
```
screen -S "Minecraft server"
java -Xmx1024M -Xms1024M -jar minecraft_server.1.17.1.jar nogui
```
