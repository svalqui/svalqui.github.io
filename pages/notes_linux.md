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
 
 ### SSH
 Adding a public key to your computer so you can connect to a remote server; you need the remote server public key (Remote_Servers.pub)
```
$ chmod 0600 .ssh/Remote_Servers.pub
$ ssh-add Remote_Servers.pub
``` 

## Unclasified

repair xorg
```
sudo apt-get remove --purge xserver-xorg
sudo apt-get install xserver-xorg
sudo dpkg-reconfigure xserver-xorg

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
