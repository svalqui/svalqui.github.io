Windows Notes

logoff
```
logoff
```


Power shell
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


```
