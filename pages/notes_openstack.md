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

```$ openstack role assignment list --user USER_NAME```

Users role assignement for a project:

```openstack role assignment list --project PROJECT_NAME --names ```

### maintenence

remove a role/user from a project:

```$ openstack role remove --user USER_NAME --project PROJECT_NAME ROLE_NAME```
