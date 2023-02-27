[[Index]]  

## To be incorporated into Obsidian all yml files have been converted to .md extensions -> To get back into yml format change all file extensions to back .yml

Role Name
=========

This is example of a custom made role that leverages the Oracle sql python library to run sql commands in a playbook, the py file when run in the templates folder carried the needed operations. The role itself will always live in it's own repo. and it's directories and subdirectories can by using the ansible-galaxy command i.e ansible-galaxy init test-role-1

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```

    - hosts: servers

    - name: Using oracle role
      include_role:
        name: oracle role
      when: role_name | default(true)|bool == true # role_name variable is set to false in variable yamls for e.g. prod, or test or any other env you don't want this command to run in the default value is set false and the condition is checing to run if the value is true

      when: role_name | default(true) == true
```
License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

## Generic Role creation process

![](assets/role-structure.png)

## Oracle SQL Role files 

[[Role Creation]]

[[defaults-main]] 

[[handlers-main]] 

[[meta-main]] 

[[tasks-main]]

[[templates-test.py.j2]]  

[[tests-inventory]]  

[[tests-test]] 

[[vars-main]] 

[[vars-requirements]]

[[vars.travis]]  
