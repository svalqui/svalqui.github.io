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
Reports historical
https://www.unix.com/solaris/282143-getting-sar-report-past-one-month.html
https://www.howtoforge.com/install-and-configure-sar-and-ksar-for-daily-monitoring-on-linux-and-generate-pdf-reports/

memory to a file
```
ls -tr /var/log/sa/sa[0-9][0-9] | 
while read safile
do
  sar -r -f "$safile"
done > reportfile-Mem
```
memory to screen
```
ls -tr /var/log/sa/sa[0-9][0-9] | 
while read safile
do
  sar -r
done
```
Procesor to a file
```
ls -tr /var/log/sa/sa[0-9][0-9] | 
while read safile
do
  sar -u -f "$safile"
done > reportfile-cpu
```
```
ls -tr /var/log/sa/sa[0-9][0-9] | 
while read sarfile
do
  LC_ALL=C sar -u -f "$sarfile" >> sar-cpu
done
```
Processor to screen
```
ls -tr /var/log/sa/sa[0-9][0-9] | 
while read safile
do
  sar -u
done
```




