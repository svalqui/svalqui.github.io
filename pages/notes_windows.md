Windows Notes

Power shell

Get-LocalUser 

Get-Process -IncludeUserName
Get-Process | Where {$_.MainWindowTitle} | Select-Object ProcessName, MainWindowTitle
Get-Process -IncludeUserName | Where UserName -match joe
Get-Process program.exe -IncludeUserName | Where UserName -match joe | Stop-Process

Stop-Process -Name msedge
Stop-Process -Name RuntimeBroker
Stop-Process -Name Minecraft.Windows
Stop-process -Id processId

