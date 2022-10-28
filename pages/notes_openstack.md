# Prerequisite

1. Install the OpenStack client:  
```sudo pip install python-openstackclient```

2. Configure your authentication credentials for the project:
    1. obtain your rest-api password from the dashboard: https://dashboard.mycloud.org/settings/reset-password/
    2. obtain your rc authentication file, usually from User -> Openstack RC file
4. Set your source keystone (your Token, Service Catalog)   
```$ source your-prj-on-mycloud-openrc.sh```

## Other modules dependencies

- sudo pip install python-novaclient
- sudo pip install python-cinderclient
- sudo pip install python-keystoneclient
- sudo pip install python-glanceclient
- sudo pip install python-quantumclient
- sudo pip install python-swiftclient
- sudo pip install python-troveclient

- sudo apt install python3-ceilometerclient

# admin
## Projects users and roles
"To assign a user to a project, you must assign the role to a user-project pair. To do this, you need the user, role, and project IDs."

https://docs.openstack.org/keystone/latest/admin/cli-manage-projects-users-and-roles.html

### Assign a user/role to a project

Note the project ID

```$ openstack project list```

Note the user ID

```$ openstack user list```

Note the role ID

```$ openstack role list```

Assign a new role:

```$ openstack role add --user USER_NAME --project PROJECT_NAME ROLE_NAME```

Verify:

```$ openstack role assignment list --user USER_NAME --project PROJECT_NAME --names```

### Good to know

Projects and roles for a user:

```$ openstack role assignment list --user USER_NAME --names```

User/role assignement for a project:

```$ openstack role assignment list --project PROJECT_NAME --names ```

Projects with a given role:

```$ openstack role assignment list --role <role-id-here> --names```

List projects which name contains a string

```$ openstack project list --sort name | grep <txt-to-look-for>```

**List servers for a project as admin**

```
$ nova list --all --tenant <project-id> --fields name,status,networks,metadata,flavor:original_name
or
$ openstack server list --all-projects --project <project-name-here>
```

Quota for a project
```
$ openstack quota show PROJECT_NAME --max-width 120
```
or
```
$ openstack quota list --project PROJECT_NAME --compute
$ openstack quota list --project PROJECT_NAME --volume
```

Quota for a project including details of the allocation
```
$ openstack allocation quota list PROJECT_NAME 
```
Quota network items for a project
```
$ openstack quota list --project PROJECT-NAME --network
```

Allocation for a project
```
$ openstack allocation show PROJECT_NAME
```
Allocation history/status for a project
```
$ openstack allocation history PROJECT-NAME
```
 Project server usage
 ```
 $ nova usage --tenant PROJECT_ID
 ```

### maintenence

remove a role/user from a project:

```$ openstack role remove --user USER_NAME --project PROJECT_NAME ROLE_NAME```

## Project quotas updating

Setting the number of security group rules for a project
```
$ openstack quota set --secgroup-rules <New-number-of-rules> PROJECT_NAME
```

## Project Security
List Security Groups for a project, Security Groups containg rules
```
$ openstack security group list --project PROJECT_NAME
```
List Security Groups details, rules specify which ports/subnets are allow /deny, to see the rules show the group details
```
$ openstack security group show <group-id>
```

## Services, catalog, endpoints

List the catalog of all services
```
$ openstack catalog list
```
List enpoints providing services
```
$ openstack endpoint list
```
List enpoint providing a given service
```
$ openstack endpoint list --service identity
```
List all services
```
$ openstack service list
```
# Nova Computer Service

## Logs
/var/log/nova/nova-compute.log

## Good to know

list nodes running compute service
```    
nova service-list
# or $ openstack compute service list
# or $ openstack host list
```
list endpoints 
```
openstack endpoint list
```

## Servers

List Server with IP address
```
$ nova list --ip 192\.0\.0\.111 --all-tenants

$ nova list --all --ip <ip of server> --fields name,tenant_id,status,networks

$ openstack server list --all-projects --ip x.y.z

# Devices using floating IP
$ openstack floating ip show <ip>

```

Show servers in a given project
```
nova list --all-tenants --tenant <project-id>
nova list --all --tenant <project-id> --fields name,status,networks,flavor:original_name,metadata
```
Show server details
```
nova show <server-id>
```
Show servers in an availability zone
```
nova list --all --availability-zone <zone-name>
or
openstack server list --all-projects --availability-zone <zone-name>
```
Show Server logs, for troubleshooting (ssh, etc)
```
$ nova console-log <server-id>
or
$ openstack console log show <server-id>
```
Show which actions has taken on the server
```
nova instance-action-list <server-id>
```
Create
    
```
nova boot
or 
openstack server create
```
Add volume to instance
```
nova volume-attach instance vol_id
```
Remove volume to instance
```
nova volume-detach instance_id vol_id
```
Show console link
```
openstack console url show -f value -curl <instance-id>
```
Show console log
```
openstack console log show <instance-id>
```
### Server Maintenence
```
nova stop <svr-id>
nova start <svr-id>
or 
nova reboot <svr-id>
```

```
$ openstack server unpause 
$ openstack server stop 
```

```
nova reset-state <svr-id> --active
```
### Server Cloning/replicating

1. Get an image of the server
```
$ nova image-create existing_vm_name new_vm_img
```
2. Get an snapshot of the server
Note, snapshoot is of the sercer only, doesn't automatically take an snapshoot of the added volumes
```
$ openstack server image create --name new_vm_snapshot existing_vm_name
# or using glance
```

### Flavors

Flavors for your Project
```
$ nova flavor-list
```
Flavors all, Admin
```
$ nova flavor-list --all
```
Flavor details
```
$ nova flavor-show <flavor-name>
```
Projects using the Flavor
```
$ nova flavor-access-list --flavor <flavor-name>
```
# Cinder, volumes
List the volumes for a project
```
openstack volume list  --project <project-name>
```
Add volumen to instance
```
nova volume-attach instance vol_id
```

# Glance, images/snapshots

List snapshot for a given project
```
glance image-list --owner PRJ-ID
# $ openstack image list --project PRJ-NAME
```

Create an snapshoot
```
glance image-create --name <NAME> --instance-uuid <INSTANCE_UUID>
# glance image-create --name <NAME> --instance-uuid <INSTANCE_UUID> --owner <PRJ-ID>
# openstack server image create --name <svr-img-name> --property owner=<PRJ-ID> SVR-ID
# openstack server image create --name <svr-img-name> --property owner=<PRJ-ID> --property visibility=<value> SVR-ID
```
Adding an image to a project
```
openstack image add project --project-domain default <img-id> <prj-id>
```
Accept the image in the project
```
openstack image set --accept <img-id> --project <prj-id>
```
# Designate
Show recordsets on a zone
```
openstack recordset list --sort-column records <zone-id-or-name>
```
Show record
```
$ openstack recordset show <zone-id> <recordset-id>
```
Create a record, the record name sometimes matches the record name, unless a more intuitive recorname is required.
```
openstack recordset create <zone-id> host-name.<zone-id> --type CNAME --record record-name.<zone-id>

openstack recordset create <zone-id> host-name.<zone-id> --type A --record <ip>
```
Change a record
```
openstack recordset set <zone-id> existing-host-name.<zone-id> --type CNAME --record new-record-name.<zone-id>
```
# Neutron - Network

## Good to know

list nodes running neutron service
```
openstack network agent list
```
list mac/IP address for a given network
```
neutron port-list --network_id <netwrok-id>
```
show host-id for a port-id
```
neutron port-show <port-id>
```

# Trove DB as a service
Show details of a DB instance
```
$ openstack database instance show <trove-id>
```
Show all DB instances for a project
```
$ openstack database instance list --all-projects --project-id <prj-id>
```
Delete a DB instance
```
$ openstack database instance delete <trove-id>
```
Update security cidr
```
openstack database instance update --allowed-cidr <ip or subnet> <trove_instance_id>
```
