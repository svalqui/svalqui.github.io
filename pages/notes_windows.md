Windows Notes


# Files/ Directories related

## Copy files batch
```
 bkp-files.bat
 set source=D:\SynergyData
 set destination=N:\BkpSynergyData
 xcopy %source% %destination% /X /H /E /C /F /D /Y /V
```
## Auto Map batch for XP
```
 net use X: \\Hostname\Share /savecred /p:yes
 It will then prompt for a username and password, which will be saved and will not prompt even after a reboot.
```
or
```
 @echo off
 net use x: /delete
 net use x: \\server\share /USER:COMPUTER\User password
 exit
 
 Save this batch on the startup, 
 https://superuser.com/questions/23646/how-can-i-map-a-network-drive-with-password-so-it-stays-mapped-after-a-reboot details
```

# logoff
```
logoff
```


# Power shell
```
Get-LocalUser 

Get-Process -IncludeUserName
Get-Process | Where {$_.MainWindowTitle} | Select-Object ProcessName, MainWindowTitle
Get-Process -IncludeUserName | Where UserName -match joe
Get-Process program.exe -IncludeUserName | Where UserName -match joe | Stop-Process
Get-Process *edge -IncludeUserName  | Where UserName -match
Get-Process Wind* -IncludeUserName  | Where UserName -match

Stop-Process -Name msedge
Stop-Process -Name RuntimeBroker
Stop-Process -Name Minecraft.Windows
Stop-process -Id processId
Stop-Process -Name Wind*
Stop-Process -Name Windows10Universal
Stop-Process -Name steam
Stop-Process -Name steamwebhelper

```
