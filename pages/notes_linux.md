Linux Notes
some of this comes from https://stackoverflow.com or, https://askubuntu.com/ 

Training
https://github.com/chlebik/rhcsa-practice-questions
https://github.com/topics/rhcsa-practice-questions

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

## grep
```
# match to include System, Memory, Processor
| grep -i -e System -e Memory -e Processor
-e pattern
-i ignore case

# include x lines before and after match
| grep -A 10 -i -e product
| grep -A 10 -B 10 -i -e product

egrep "Failed|failure" /var/log/auth.log
```

find files containing, use grep
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

## find

find file by name, use find
```
find . -name "*<str-here>*"
find /. -name 'toBeSearched.file' 2>/dev/null # send error to null
find / -type f -name "*.txt"
find . | grep <file-name>
find / | grep <directory/filename>
find / -type d -name "<directory-name>"
```

list files mounted on nautilius - Connect to Server
```
ls -al /run/user/$UID/gvfs 
```
## tail
```
shows the last 20 lines
tail -n 20 <filename>

Keeps showing you additions to the file, as it gets updated, in real time, shoe errors in real time
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
Add
```
$tar -rvf  /home/my-tar-file.tar /home/dir2/.
```
Extract
```
tar xvzf my-file.tar.gz
tar -xzvf my-file.tar.gz -C /to-this-directory
```
Show
```
tar -tf my-file.tar
# or tar -tvf my-file.tar
```
## rsync
copy(backup) files to /home/usr/Desktop
```
rsync -aAXvPh /media/usr/Data/bkp-laptop/home/usr/Desktop/ /home/usr/Desktop/
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
For how long they have been opened
```
lsof | grep -e "[[:digit:]]\+w"
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
```
All in /
```
sudo du -smx /* | sort -n
sudo du -hsx /* | sort -rh | head -n 40
# All in home, just parent directories
du -h --max-depth=1 /home | sort -rh | head -10

Top biggest
find / -printf '%s %p\n'| sort -nr | head -10

by top directories
du -cha --max-depth=1 / | grep -E "M|G|T"
sudo du -aBM -d 1 . | sort -nr | head -20
-c total
-h human
-a all
-BM in MBytes
-d depth
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
or
cat /proc/partitions
````
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
## Disk storage performance benchmark
Check that you have the space to create the file first
```
sysbench fileio --file-total-size=16G --file-num=1 prepare
```
hdparm
```
sudo hdparm -Tt /dev/nvme0n1p5
-t displays the speed of reading through the buffer cache to the disk without any prior caching of data.
-T displays the speed of reading directly from the Linux buffer cache without disk access.
```
## Clear Space, root full
Purge old kernels
```
dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge
```
clean packages
```
sudo apt-get clean
sudo apt-get autoremove
```
Other things to check
```
# find big files
find / -size +10M

# All files open for writing
lsof | grep -e "[[:digit:]]\+w"

# check deleted+opened files
sudo lsof +L1

# Check there is inodes free
df -i
```
Purge Docker
```
# docker system prune
```
Check space used by journal
```
journalctl --disk-usage
# Set the max use on /etc/systemd/journald.conf
SystemMaxUse=500M
# restart the service
service systemd-journald restart
# Try
sudo journalctl --vacuum-size=100M
sudo journalctl --vacuum-time=120d
https://askubuntu.com/questions/1238214/big-var-log-journal
```
Check unused snaps
```
snap list --all
# used
sudo du -sh /var/lib/snapd/cache/
LANG=C snap list --all | awk '/disabled/{print $1" --revision "$3}'
# Remove unused
LANG=C snap list --all | awk '/disabled/{print $1" --revision "$3}' | xargs -rn3 snap remove
https://askubuntu.com/questions/1036633/how-to-remove-disabled-unused-snap-packages-with-a-single-line-of-command
```
Check what is using the disk
```
lsof | grep -e "[[:digit:]]\+w"
```

## Disk format and partitions
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
### extend partiion

1. extend the partition
```
fdisk /dev/vdb
# Print the partions
Command (m for help): p
# take note where the partition start, First Sector
# and delete the partition
Command (m for help): d
# create a new partition, match the first sector
Command (m for help): n
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-419430399, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-419430399, default 419430399): 
# Keep the signature
Partition #1 contains a ext4 signature.
Do you want to remove the signature? [Y]es/[N]o: N
# check the start is the same as initial
Command (m for help): p
# write the new partition table
Command (m for help): w
```
2. resize the fs
```
# resize2fs /dev/vdb1
```
3. mount the partition

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
## Fix disk with errors, corrupted files, clean structure

for Ext4 partitions
```
fsck -yf /dev/<your-disk-partition-here>
```
for xfs partitions
```
xfs_repair /dev/vdb4
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
or
mount.cifs //mediaflux.domain/proj-number /mnt -o user=youruser,domain=YOUR.DOMAIN
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
## ordered by memeory use
```
ps aux --sort=-%mem
```
## print a process tree
```
ps axjf
```
## stats
```
top
# top graph
htop
```

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
# CPU

list
```
lscpu
```
average use
```
iostat
```
monitor, average every 5 seconds
```
sar -u 5
```
## speed/performance benchmark
```
sudo apt install sysbench
sysbench --test=cpu run
```
## stress test
```
 stress --cpu 128 --vm 256 --vm-bytes 7000M --timeout 15m
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
## Memory speed/perfomance benchmark
```
sysbench --test=memory run
sysbench --test=memory --memory-block-size=1G --memory-total-size=250G run

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

# show live errors, monitor live
dmesg -w

sudo find /var/log -type f -mtime -1 -exec tail -Fn0 {} +
https://www.hpe.com/us/en/insights/articles/the-first-5-things-to-do-when-your-linux-server-keels-over-1705.html

tail -f my_log.log | grep look_for_this
```

# Multicommand

## pdsh
```
pdsh -l root -w svr[01-10],svr[22-31],svr218 'systemctl restart dhcp'
pdsh -R ssh -l root -w ^server_names.txt "dpkg -l | grep ssh-server" 2>/dev/null | dshbak -c
```

## clush
```
clush -bL -l root -w svr[211-317]-storage 'ls /dev/nvm* | sort'
clush -bL -l root -w svr[01-17,20-45,89,150-211] 'apt install tmux -y'
clush -bL -o "-o StrictHostKeyChecking=no" -l root -w svr[01-17,20-45,89,150-211] 'apt install tmux -y'
```

-b clush waits for command completion and then displays gathered output results  
-w allows you to specify remote hosts by using ClusterShell NodeSet syntax  
-L disable header block and order output by nodes; additionally, when used in conjunction with -b/-B, it will enable "life gathering" of results by line mode, such as the next line is displayed as soon as possible  
-l USER, --user=USER
-o ssh options, add ssh options -o "-i ~/.ssh/myidrsa"

# Virtualization - hypervisor

install qemu
```
sudo apt install qemu qemu-kvm libvirt-bin
```

## Tools
```
sudo apt install virt-manager
```
```
virt-clone --auto-clone --original <vm-name>
```
```
# virsh start instance-XXXXX # To boot it up

# virsh domifaddr m-vm-name # list ip addresses

# virsh dumpxml my-vm-name # show all details

# virsh list --all| grep instance-00114043

# virsh shutdown instance-00114043

# virsh console instance-00114043
Connected to domain instance-00114043
Escape character is ^

# virsh undefine  instance-00114043
Domain instance-00114043 has been undefined
```

# journalctl
```
Live what is been loged
journalctl -fan100
The last 100
journalctl -n 100
The last in 1 hour
journalctl --since "1 hour ago"
The last day
journalctl --since "1 day ago"
Reverse order
journalctl -r
```
from last boot (-b -1) extended (-xe)
```
journalctl -b -1 -xe
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

```
# Only Networkmanager messages
sudo journalctl -u NetworkManager.service

# Only lightdm messages
sudo journalctl -u lightdm

# Only gpg
sudo journalctl -u gpg-agent
```
By Process
```
# systemd
sudo journalctl -t systemd

# puppet agent
journalctl -rt puppet-agent

# Cisco Anyconect
sudo journalctl -t acvpnagent 
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
In a time frame
```
# journalctl --since "2024-10-26 00:00:00" --until "2024-12-31 00:00:00"
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

service sssd stop ; rm -rf /var/log/sssd/* ; rm -rf /var/lib/sss/db/* ; service sssd start
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
sudo service NetworkManager restart
sudo service network-manager restart
sudo restart network-manager

# on CentOS
systemctl restart network.service
```
bring interface up
```
sudo ip link set <int-name> up
```

ping
```
ping server.org.au | while read pong; do echo "$(date): $pong"; done
```
show routes
```
ip r 
```
delete route
```
ip route del default via <router-ip> dev <int> proto dhcp src <host-ip>
```
show leases
```
cat /var/lib/dhcp/dhclient.et0.leases
```
## whois
Show icann information of an IP address
```
whois <ip-add>
```
## mtr
my traceroute with live information
```
mtr <ip/or_site>
```

## nc netcat
check if ports are open, reads and writes data on network connections
```
nc -v -w1 -z <ip> <port>
nc -zv <ip> <port> # Check if port on ip is open
timeout 3 nc -z <ip> <port> && echo good || echo nope
nc -l -s <ip> <port> # Listen source ip on port
nc -l -p <port>  # Listen on port
```
create an in/out pipe
```
nc -lp 45678 | nc -lp 45679
#
# on another session
nc localhost 45678
# type what you want to transfer to the other port
# on another session
nc localhost 45679
# you shuld see what is typed on the other session
```
## netstat
```
# Network stats
netstat -s

# show the progran each socket belongs to
netstat -p

# show listening sockets
netstat -l

# show established socket, tcp + program
netstat -tp

# Interfaces stats, including error
netstat -i
netstat -i | column -t  # Show it on a table

# Show the routing tables
netstat -nr
# n use numberial address form
# r routing tables
```

## nmap
````
No probes
sudo nmap -v -sS -A -T4 -Pn <ip-add>

details of specific ports
nmap -sV -sT -sC -p 443,80,22 <ip-add>

Nmap:
sudo nmap -v -sS -A -T4 <hostname>

Check if a given port is open
sudo nmap -p <port-number> <ip-add> 

Subnet check
nmap -sP 192.168.1.0/24 --scan-delay 1s

````
- A Enable OS/version detection, script scanning and traceroute
- sV Probe open ports
- sT Scan connect technic
- sS Scan TCP Syc technic
- sC Script default
- T4 Timeing template
- p port number
- v versosity


## ports listening
```
sudo lsof -nP -iTCP -sTCP:LISTEN
```
## ports open
```
sudo apt install net-tools
$ netstat -tapn
sudo netstat -plunt
```
or
```
sudo lsof -nP -iTCP -sTCP:ESTABLISHED
```
All net services running
```
netstat -lepunt
```
## DNS
```
systemd-resolve --status
```
Check DNS resolvers, this is a dynamic file do not change it directly, it would recreate when restarting system. changes via resolvconf, which doesn't install by default.
```
cat /etc/resolv.conf

domain your.domain.org
nameserver <x.x.x.x> # dns ip
nameserver <x.x.x.x> # dns ip sec
search sub1.your.domain.org here2.your.domain.org  # subdomains to look into
```
If you need to refresh you will need to restart the Network manager, but that will restart all network
```
sudo restart network-manager
```
## Dig
Check time pending to update ip to name, on section "Answer Section"
```
$ dig @<ip> <domain-name>. 
```
Check the name from an IP
```
dig -x <ip-add> +short
```
Check the IP from a Name
```
dig <domain-dns-name> +short
dig @1.1.1.1 <dns-name>  +noall +answer
```
## network monitor, interface stats
```
ip -s link # Show the interfaces stats
cat /proc/net/dev  # As above
ethtool <interface> 
$ sudo iptraf
ip link show  # list the interfaces, link status and ip addresses
lspci | grep Ethernet # Show show Ethernet controller, NIC
ifconfig -a | grep Link  # Show interfaces stats
ip route | column -t # Show the routes on a table
netstat -r  # show the kernal ip routing table
```
### show errors
```
ip -s link
ifconfig -a
```
## tcpdump 
dumps headers and packets of network traffic 
```
# tcpdump -vvv -i <int-name> udp port <port-num>  # -v verbosity, full protocol decode on interface, protocol and port
# tcpdump -nnni <int-name> port 443   # -n disable DNS translations, ouout is more concise
# tcpdump -i eth0 src <source-ip>  # packets from a source ip
# tcpdump -i eth0 dst <des-ip>  # packet to a destination ip
# tcpdump -c 8 -i <int-name>  # set the number of packets capture to 8
# tcpdump -i any host <host-ip>  # any packet from a given host
```
## Network performance

### iperf3, between 2 hosts
- test the delfault port is open
```
# nmap -Pn -p 5201 x.x.x.x
```
- On one host run
```
# iperf3 -s
##or to test via a port
# iperf3 -s -p <a-given-port> 
```
- On the other run
```
# iperf3 -c x.x.x.x
## or if testing on a port
# iperf3 -c <ip-svr-running-s> -p <a-given-port> 
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
## Network receive traffic via 2 interfaces for ssh
network rules Network receive traffic via 2 interfaces for ssh

```
# identify the interfaces: route -n
ip route add table 1921681    default via <rtr-ip-internal> dev eth1
ip route add table 19216817  default via <rtr-ip-ext> dev eth0

# identify the subnet: ip r
ip rule add from <subnet-cidr-internal>  lookup 1921681
ip rule add from <subnet-cidr-external> lookup 19216817
```

## iptables
```
List
iptables -L
iptables -S 
iptables -L --line-numbers

-L List all rules
-S print all rules
--line-numbers

Delete
iptables -D <sec-group> <line-number>

Add
sudo iptables -A <sec-group> -p tcp -s <cidr1>,<cidr2>,<cidr3> -m tcp --dport 22 -j ACCEPT

Save changes
iptables-save
```

# SSH
 
Adding a public key to your computer so you can connect to a remote server; you need the remote server public key (Remote_Servers.pub)
```
$ chmod 0600 .ssh/Remote_Servers.pub
$ ssh-add Remote_Servers.pub
``` 
Adding a public key to a server so the user can ssh to the server
```
From the server
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

## ssh lock and warn root
```
# cat /root/.ssh/authorized_keys
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as your own user rather than the user \"root\".';echo;sleep 10;exit" ssh-rsa AAABBB...
```

## ssh file copy scp

[COMMAND] [OPTIONAL ARGUMENTS] [SOURCE] [DESTINATION]

Copy a file
```
# From local to remote host
scp my-file <username>@<host-name>:/home/

# From a remote server to local
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

## ssh tunnel
```
SSH Tunnel creation for printing:
$ ssh -L 1631:127.0.0.1:631 root@ip -N -v -v
# redirects remote ip port 631 to localhost port 1631, on local 1631 you will see what is on remote 631; -v verbose, Local forwards to remote

$ ssh -R 6311:localhost:631 remotehost
# Remote forwards to Local

```
## ssh good to know
Show auth methods
```
$ ssh -o PreferredAuthentications=none localhost
```
Check root authentication
```
# sudo sshd -T | grep -E -i 'ChallengeResponseAuthentication|PasswordAuthentication|UsePAM|PermitRootLogin'
```
Update a global parameter
```
$ sudo sshd -T -o PasswordAuthentication=no
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
## ssh forward your local agent to remote servers

if using extra security find your local extra socket
```
$ gpgconf --list-dir agent-extra-socket
/Users/<user>/.gnupg/S.gpg-agent.extra
```
find you remote agent, by login to the remote server
```
$ gpgconf --list-dirs agent-socket
/run/user/<uid>/gnupg/S.gpg-agent
```
Configure ssh
```
# cat .ssh/config
Host *.my-org.org.au 
  ForwardAgent yes
Host *.my-other-org.org.au
  ForwardAgent yes
  ExitOnForwardFailure yes
  RemoteForward /run/user/<uid>/gnupg/S.gpg-agent /run/user/<uid>/gnupg/S.gpg-agent.extra # asociate the agents
```

## Connecting to legacy hosts
```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 user@legacyhost
```

## Automating on ssh login
edit /etc/bashrc
```
if [[ -n $SSH_CONNECTION ]] ; then
    # Do something here for all users connecting via ssh
    echo "Message here to users login via ssh"
    sleep t # send them to wait t time
    
fi
```

## ssh remote
```
install xtightvncviewer
sudo apt-get install xtightvncviewer

create a file (shorcut) for your ssh call, and give it x permissions
touch remoteto
chmod 777 remoteto

remoteto file 
#!/bin/bash
ssh -n -L 5900:localhost:5900 root@$1 "x11vnc -xkb -safer -nopw -once -geometry 1280x800 -auth /var/run/lightdm/root/\:0 -display :0" & /usr/bin/xtightvncviewer localhost:5900

execute 
$ ./remoteto <ip-remote>
```
## SSH Tunnel creation for RDP via remmina:

    Let say you PC 2.2.2.2 has access to an RDP Server(Windows) 3.3.3.3
    You are at your laptop on 1.1.1.1 and you can ssh to your PC 2.2.2.2

    Open a terminal on your Laptop and ssh to you pc
    Tunnel the port
```
$ ssh -L 13389:localhost:3389 root@2.2.2.2 -N
```
    redirects remote ip port 3389 to localhost port 13389, on local 13389 you will see what is on remote 3389, Local forwards to remote
    on remmina connect to 1.1.1.1:13389


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

speed of a website
```
curl -s -w %{time_total}\\n -o /dev/null http://my.web.site.org
```

# Repositories
list repos
```
grep ^[^#] /etc/apt/sources.list /etc/apt/sources.list.d/*
```
Add a repo
```

# Add a new fil e on directory  /etc/apt/sources.list.d/my-repo.list
# Sources are located in /etc/apt/sources.list.d
# there should be a files for you repo, my-cia.list containing all the repos for your cia
#
#  /etc/apt/sources.list.d/my-cia.list
deb https://my-repo.org focal main
```
list packages on a repo 

```
- First list the repos
# grep ^[^#] /etc/apt/sources.list /etc/apt/sources.list.d/*
/etc/apt/sources.list.d/ubuntu-focal.list:
- look for the correspondig file on /var/lib/apt/lists/
- then grep the packages
# grep ^Packa /var/lib/apt/lists/au.archive.ubuntu.com_ubuntu_dists_focal_main_binary-amd64_Packages

```

Adding the key to the remote repo
```
# wget -O - https://my-repo/keys/my-pub.key | apt-key add -
```

# Packages 

Install a package
```
sudo dpkg -i DEB_PACKAGE
# sudo apt --fix-broken install ./filename.deb # to install dependencies

# Install a particular version shown on apt-cahe policy
sudo apt-get install http-server=3.11.28-1
```
install a package including dependencies
```
sudo apt-get install gnome-terminal
```
remove a Package
```
sudo dpkg -r PACKAGE_NAME
```
remove a package and its dependencies
```
sudo apt-get --purge remove gnome-terminal
sudo apt-get remove --purge <packge-name>
```
check packages installed
```
dpkg -l
dpkg -l | grep openssh-server
or
apt list --installed
```
check package running
```
sudo service <service> status
sudo service ssh status
```
check package installed and next version available and repository
```
apt policy <package-name>
apt policy openssh-server
apt-cache policy rabbitmq-server
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
show all packages in a repo
```
# ls /var/lib/apt/lists/*_Packages
and open or grep  the corresponding repo file
# grep ^Package: /var/lib/apt/lists/<repo_here>_Packages
```
show deb info
```
dpkg-deb --info my.deb
```
show deb content
```
dpkg -c my.deb
```
extract a deb file
```
ar vx file.deb
tar xvf control.tar.xz
tar data.tar.xz
```
look if a file is installed by a package, which package
```
# grep is-this-file-installed.py /var/lib/dpkg/info/*.list
```
show the files installed by a package
```
# cat /var/lib/dpkg/info/<pkg-name>.list
```
Packages history, when thay have been added, removed, upgraded
```
# vi /var/log/apt/history.log
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
To list sessions
```
tmux ls
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
sync typing, parallel typing
```
# Open a tmux session
tmux
# split the screen on 2
Ctrl+b %   
# ssh to your remote hosts
# call the tmaaux command line
Ctrl-b :
# on the cli enable sync typing 
setw synchronize-panes on 
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
    Ctrl+b ] , enter in scroll mode, the you can use the arrows 'q' to quit scroll mode
    Ctrl+b Arrowa, to move between panes
    Ctrl+b :  Opens tmux command pront
Command prompt commnads
setw synchronize-panes on   , all typed on one panes will be replicated on the others, sync typing, parallel tyeping
setw synchronize-panes off  , switches off sync typing 

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
### all banned IPs
```
sudo grep Ban /var/log/fail2ban.log*
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
### jail times and configuration
```
# cat /etc/fail2ban/jail.conf
bantime      = 1h
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
### facter
```
# facter -j | jq '.hostname'
# facter -j | jq '.os.distro'
# facter -j | jq '.dmi'

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

# Bash
## Bash Prompt
```
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]($ENV-VAR)\[\033[00m\]\$ '
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;33m\]($ENV-VAR)\[\033[00m\]\$ '
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;33m\]$OS_PROJECT_NAME\[\033[00m\]\$ '
```
## for loop
This
```
for n in {1..5};
do
    echo $n
    for cmd in time date;
    do
        echo $cmd
    done
done

```
is the same as this
```
for n in {1..5}; do echo $n; for cmd in time date; do echo $cmd; done; done
```
## while
use read to read the output of a command line by line

```
# Read the pid of processes
ps -ef | while read line; do my_pid=$(echo $line | cut -d' ' -f2); echo $my_pid; done
```
## Array to string
```
my_arr=("a" "b" "c")

my_string="${my_arr[*]}"
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

#replace
:%s/ReplaceThis/WithThis/gc

/string 	search forward
?string 	search backward 
n 	move to next 
N 	move to next in opposite direction

^g line number

0 move to the begining of line
$ move to the end of line
:0<Return> or 1G  move to the first line
:$<Return> or G  move to the last line

u undo
Crtl-r redo

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
Compare files on directories
```
diff -qrs dir1/ dir2/
```
Compare and show the differences
```
diff -bur dir1/ dir2/

```

# Unclasified

repair xorg
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg

```
Add the date to the system bar
``` 
 dconf write /org/gnome/desktop/interface/clock-show-date 'true'
```
ldap query
```
ldapsearch -x -h your.domain.org -D "user@your.domain.org" -W -b "ou=People,dc=domain,dc=org" -s sub "(cn=*)" cn mail sn
```
sssd reset cache
```
systemctl stop sssd
sudo sss_cache -E  # or rm -rf /var/lib/sss/db/*
systemctl start sssd
```
debug a bash scrpt
```
strace ./my_bashscript.sh
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
or
```
sudo apt install ubuntu-mate-desktop
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
Change your default desktop to Mate
```
sudo dpkg-reconfigure lightdm
# select ligthdm and reboot
```
# configuring a yubikey
https://gist.github.com/artizirk/d09ce3570021b0f65469cb450bee5e29

# ssl certificates 

## verify
```
openssl x509 -in /peth/to/file.crt -text -noout
openssl x509 -in /etc/ssl/certs/my-cert.pem -noout -text
```
## Names registered
```
openssl x509 -in /etc/ssl/private/my-priv.pem -noout -subject
openssl x509 -in /etc/ssl/private/my-priv.pem -noout -ext subjectAltName

```
## when expires
```
openssl x509 -in /peth/to/file.crt -noout -enddate
openssl x509 -in /etc/ssl/private/my-priv.pem -noout -enddate
```
## check the md5 encryption
```
openssl x509 -in /etc/ssl/certs/my-cert.pem -noout -modulus | openssl md5
```
## convert crt to PEM
https://stackoverflow.com/questions/4691699/how-to-convert-crt-to-pem
```
openssl x509 -in mycert.crt -out mycert.pem -outform PEM
```
# sound audio

## microphone loopback to headset
https://pupuweb.com/how-enable-microphone-loopback-headset-ubuntu/

Enable microphone loopback into headset
```
pactl load-module module-loopback latency_msec=1
# it would show the index of the load module, keep it handy as you will need it to disable the loopback
```
Disable the loopback
```
pactl unload-module [index]
```
List the modules
```
pactl list modules
```
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
