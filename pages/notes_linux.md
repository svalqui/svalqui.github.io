Linux Notes

## Managing files
Removing a directory
```
$rm -r mydir
```


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

## Unclasified

repair xorg
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg

 ```
## preferred pkgs
```
chmod 755 Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 
sudo Downloads/anyconnect-linux64-4.8.03052-core-vpn-webdeploy-k9.sh 

chmod 777 displaylink-driver-5.3.1.34.run 
sudo ./displaylink-driver-5.3.1.34.run 

sudo apt-get install freeipa-client freeipa-admintools

sudo apt install git

sudo apt-get install openssh-server

$ chmod 0600 .ssh/id_rsa
$ ssh-add

sudo service ssh restart
```
```
smb://<username>@server.dom.com.au/Users/<username>
```
apt-get install -y keepassxc

apt-get install -y owncloud-client
