Installing Configuring
```
sudo apt install sysstat
sudo systemctl start sysstat
sudo systemctl enable sysstat
```

Version
```
sar -V
```
Cron Jobs
```
cat /etc/cron.d/sysstat
```
Using 
```
CPU
sar -u

Memory
sar -r

Swap
sar -S

Disk
sar -d
sar -p -d

Network
sar -n

```
From a file
```
sar –f FileName
```

Data Retention
```
cat /etc/sysstat/sysstat
HISTORY=
```
Log files location
```
# ls -l /var/log/sa/
```
