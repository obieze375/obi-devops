
[[Index]] 


## User:

~~~~  

Update the playbook to add a new task at second position (right after adding entry to resolv.conf) to create a new web user.

~~~~

~~~~

Use the user module for this. User details to be used are given below:

Username: web_user

uid: 1040

group: developers

~~~~


```yaml
- name: Execute a script on all web server nodes and start httpd service
  hosts: web_nodes
  tasks:
    - name: Update entry into /etc/resolv.conf
      lineinfile:
        path: /etc/resolv.conf
        line: nameserver 10.1.250.10

    - name: Create a new user
      user:
        name: web_user
        uid: 1040
        group: developers

    - name: Execute a script
      script: /tmp/install_script.sh

    - name: Start httpd service
      service:
        name: httpd
        state: present

```

```yaml
- name: Create devops_user if missing, make sure is member of correct groups
  ansible.builtin.user:
    name: devops_user
    shell: /bin/bash
    groups: sys_admins, developers
    append: true
```

## Commonly Used Parameters for the User Module

comment: Optionally sets the description of a user account.

group: Optionally sets the user's primary group.

groups: Optionally sets a list of supplementary groups for the user.

When set to a null value, all groups except the primary group
are removed. 

home: Optionally sets the user's home directory location. 

create_home: Optionally takes a Boolean value of true or false. A home
directory is created for the user if the value is set to true.

system: Optionally takes a Boolean value of true or false. When
creating an account, this makes the user a system account if
the value is set to true. This setting cannot be changed on
existing users.

uid: Sets the UID number of the user.

state: If set to present, create the account if it is missing (the
default setting). 

If set to absent, remove the account if it is
present.
  