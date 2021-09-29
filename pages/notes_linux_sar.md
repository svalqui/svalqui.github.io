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
sar -u
```
Data Retention
```
cat /etc/sysstat/sysstat
HISTORY=
```
