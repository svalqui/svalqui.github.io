# Prerequisite

1. Install the OpenStack client:  
```sudo pip install python-openstackclient```

2. Configure your authentication credentials for the project:
    1. obtain your rest-api password from the dashboard: https://dashboard.mycloud.org/settings/reset-password/
    2. obtain your rc authentication file, usually from User -> Openstack RC file
4. Set your source keystone (your Token, Service Catalog)   
```$ source your-prj-on-mycloud-openrc.sh```

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

List servers for a project as admin

```$ openstack server list --all-projects --project <project-name-here>
or
nova list --all --tenant <project-id> 
```

Quota for a project
```
$ openstack quota list --project PROJECT_NAME --compute
$ openstack quota list --project PROJECT_NAME --volume
```
or

```
$ openstack quota show PROJECT_NAME
```
Quota network items for a project
```
$ openstack quota list --project PROJECT-NAME --network
```

Allocation for a project
```
$ openstack allocation show PROJECT_NAME
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
## Servers
Show server details
```
$ openstack server show <server-id>
```
Show Server logs, for troubleshooting (ssh)
```
$ openstack console log show <server-id>
```
Show which actions has taken on the server
```
nova instance-action-list <server-id>
```
