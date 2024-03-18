
## ping servers
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
local run it locally<br/>

otherwise
```
ansible-playbook -v -i hosts -c site.yml
```
## add time for tasks

on `/etc/ansible/ansible.cfg` add:
```
callbacks_enabled = profile_tasks
```
# selecting playbooks
## run task with a given tag
```
ansible-playbook main.yml --tags "stepone"
ansible-playbook main.yml --tags "stepone, steptwo"
```
## skip playbooks with a tag
```
ansible-playbook example.yml --skip-tags "stepone"
```
## Tags on a taks
```
- name: Name
  command: "{{ cmd }}"
  with_items:
    - "time"
    - "date"
  notify: Task One
  tags:
    - stepone
```





