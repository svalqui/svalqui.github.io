
## Ping servers
```
$ ansible all -m ping -i my_svrs.txt -u root
```
## run a command on servers
```
$ ansible all -a "lsblk" -i my_svrs.txt -u root
```
## run a playbook
```
ansible-playbook -v -i hosts -c local site.yml
```
-v verbose<br/>
-i inventory-file<br/>
-c check<br/>
-l --limit furhter filter hosts to a pattern name<br/>

## add time for tasks
```
callbacks_enabled = profile_tasks
```
## run task with a given tag
```
ansible-playbook main.yml --tags "stepone"
ansible-playbook main.yml --tags "stepone, steptwo"
```
