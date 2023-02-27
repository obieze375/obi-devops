[[Index]] 


## Custom role creation with Ansible Galaxy

  

## How do we create Ansible Roles?

  

To create a Ansible roles, use ansible-galaxy command which has the templates to create it. This will create it under the default directory /etc/ansible/roles and do the modifications else we need to create each directories and files manually.

  

~~~~

[root@learnitguide ~]# ansible-galaxy init /etc/ansible/roles/apache --offline

- apache was created successfully

~~~~

  

where, ansible-glaxy is the command to create the roles using the templates.

  

init is to initiliaze the role.

  

apache is the name of role,

  

offline - create offline mode rather than getting from online repository.

  
  

## List out the directory created under /etc/ansible/roles.

  

~~~~

[root@learnitguide ~]# tree /etc/ansible/roles/apache/

/etc/ansible/roles/apache/

|-- README.md

|-- defaults

|   `-- main.yml

|-- files

|-- handlers

|   `-- main.yml

|-- meta

|   `-- main.yml

|-- tasks

|   `-- main.yml

|-- templates

|-- tests

|   |-- inventory

|   `-- test.yml

`-- vars

    `-- main.yml

8 directories, 8 files

[root@learnitguide ~]#

~~~~

  

We have got the clean directory structure with the ansible-galaxy command. Each directory must contain a main.yml file, which contains the relevant content.

  

## Directory Structure:

  
```
tasks - contains the main list of tasks to be executed by the role.

  

handlers - contains handlers, which may be used by this role or even anywhere outside this role.

  

defaults - default variables for the role.

  

vars - other variables for the role. Vars has the higher priority than defaults.

  

files - contains files required to transfer or deployed to the target machines via this role.

  

templates - contains templates which can be deployed via this role.

  

meta - defines some data / information about this role (author, dependency, versions, examples, etc,.)

 ``` 

Lets take an example to create a role for Apache Web server.

  

Below is a sample playbook codes to deploy Apache web server. Lets convert this playbook codes into Ansible roles.

  

~~~~

---

- hosts: all

  tasks:

  - name: Install httpd Package

    yum: name=httpd state=latest

  - name: Copy httpd configuration file

    copy: src=/data/httpd.original dest=/etc/httpd/conf/httpd.conf

  - name: Copy index.html file

    copy: src=/data/index.html dest=/var/www/html

    notify:

    - restart apache

  - name: Start and Enable httpd service

    service: name=httpd state=restarted enabled=yes

  handlers:

  - name: restart apache

    service: name=httpd state=restarted

~~~~

  

First, move on to the Ansible roles directory and start editing the yml files.

  

~~~~

cd /etc/ansible/roles/apache

~~~~

  

## Tasks

  

Edit main.yml available in the tasks folder to define the tasks to be executed.

  
[root@learnitguide apache]# vi tasks/main.yml

~~~~



---

- name: Install httpd Package

  yum: name=httpd state=latest

- name: Copy httpd configuration file

  copy: src=/data/httpd.original dest=/etc/httpd/conf/httpd.conf

- name: Copy index.html file

  copy: src=/data/index.html dest=/var/www/html

  notify:

  - restart apache

- name: Start and Enable httpd service

  service: name=httpd state=restarted enabled=yes

~~~~

  

Altogether, you can add all your tasks in this file or just break the codes even more as below using "import_tasks" statements.

  
[root@learnitguide apache]# cat tasks/main.yml

~~~~



---

# tasks file for /etc/ansible/roles/apache

- import_tasks: install.yml

- import_tasks: configure.yml

- import_tasks: service.yml

~~~~

  

## Lets create install.yml, confgure.yml, service.yml included in the main.yml with actions in the same directory.

install.yml

  
[root@learnitguide apache]# cat tasks/install.yml

~~~~



---

- name: Install httpd Package

  yum: name=httpd state=latest

~~~~

  
  

## configure.yml

  
[root@learnitguide apache]# cat tasks/configure.yml

~~~~

---

- name: Copy httpd configuration file

  copy: src=files/httpd.conf dest=/etc/httpd/conf/httpd.conf

- name: Copy index.html file

  copy: src=files/index.html dest=/var/www/html

  notify:

  - restart apache

~~~~

  

## service.yml

  
[root@learnitguide apache]# cat tasks/service.yml

~~~~
---

- name: Start and Enable httpd service

  service: name=httpd state=restarted enabled=yes

~~~~

  

## Files

  

Copy the required files (httpd.conf and index.html) to the files directory.

  [root@learnitguide apache]# ll files/*

~~~~

-rw-r--r-- 1 root root 11753 Feb  4 10:01 files/httpd.conf

-rw-r--r-- 1 root root    66 Feb  4 10:02 files/index.html

[root@learnitguide apache]# cat files/index.html

This is a homepage created by learnitguide.net for ansible roles.

[root@learnitguide apache]#

~~~~

  

## Handlers

  

Edit handlers main.yml to restart the server when there is a change. Because we have already defined it in the tasks with notify option. Use the same name "restart apache" within the main.yml file as below.

  [root@learnitguide apache]# cat handlers/main.yml

~~~~
---

# handlers file for /etc/ansible/roles/apache

- name: restart apache

  service: name=httpd state=restarted

~~~~

  

## Meta

  

Edit meta main.yml to add the information about the roles like author, descriptions, license, platforms supported.

  [root@learnitguide apache]# vim meta/main.yml

~~~~

galaxy_info:

  author: LearnItGuide.net

  description: Apache Webserver Role

  company: LearnITGuide.net

  # If the issue tracker for your role is not on github, uncomment the

  # next line and provide a value

  # issue_tracker_url: http://example.com/issue/tracker

  # Some suggested licenses:

  # - BSD (default)

  # - MIT

  # - GPLv2

  # - GPLv3

  # - Apache

  # - CC-BY

  license: license (GPLv2, CC-BY, etc)

  min_ansible_version: 1.2

  # If this a Container Enabled role, provide the minimum Ansible Container version.

------skipped

~~~~

List out the created files now,

  [root@learnitguide apache]# tree

~~~~



.

|-- README.md

|-- defaults

|   `-- main.yml

|-- files

|   |-- httpd.conf

|   `-- index.html

|-- handlers

|   `-- main.yml

|-- meta

|   `-- main.yml

|-- tasks

|   |-- configure.yml

|   |-- install.yml

|   |-- main.yml

|   `-- service.yml

|-- templates

|-- tests

|   |-- inventory

|   `-- test.yml

`-- vars

    `-- main.yml

8 directories, 13 files

[root@learnitguide apache]#

~~~~

  

We have got all the required files for Apache roles. Lets apply this role into the ansible playbook "runsetup.yml" as below to deploy it on the client nodes.

  
  

[root@learnitguide apache]# cat /etc/ansible/runsetup.yml

  

~~~~

---

 - hosts: node2

   roles:

   - apache

~~~~

  

[root@learnitguide apache]#

We have defined this changes should be run only on node2, you can also use "all" if need. Specify the role name as "apache", also if you have created multiple roles, you can use the below format to add it.

- apache

- nfs

- ntp

  

## Lets verify for syntax errors:

  
  

[root@learnitguide apache]# ansible-playbook /etc/ansible/runsetup.yml --syntax-check

playbook: /etc/ansible/runsetup.yml

  

[root@learnitguide apache]#

  

## No errors found. Let move on to deploy the roles.

  

~~~~

[root@learnitguide apache]# ansible-playbook /etc/ansible/runsetup.yml

PLAY [node2]

***********************************************************************************************

TASK [Gathering Facts]

***********************************************************************************************

ok: [node2]

TASK [apache : Install httpd Package]

***********************************************************************************************

changed: [node2]

TASK [apache : Copy httpd configuration file]

***********************************************************************************************

Changed: [node2]

TASK [apache : Copy index.html file]

***********************************************************************************************

changed: [node2]

TASK [apache : Start and Enable httpd service]

***********************************************************************************************

changed: [node2]

RUNNING HANDLER [apache : restart apache]

***********************************************************************************************

changed: [node2]

PLAY RECAP

***********************************************************************************************

node2                      : ok=6    changed=5    unreachable=0    failed=0

~~~~

  

That's it, We have successfully deployed the Apache webserver using Ansible Roles to the client node "node2".

  

## Login into the client node "node2" and verify the following things.

  

~~~~

[root@node2 ~]# rpm -q httpd

httpd-2.4.6-67.el7.centos.6.x86_64

[root@node2 ~]# systemctl status httpd

httpd.service - The Apache HTTP Server

   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled)

   Active: active (running) since Sun 2018-02-04 10:23:44 IST; 1min 58s ago

     Docs: man:httpd(8)

           man:apachectl(8)

~~~~

  

## Access the webpage using elinks command or using browser, you will be able to access it.

  

~~~~

[root@node2 ~]# elinks http://node2

This is a homepage created by learnitguide.net for ansible roles.

~~~~

[[Index]] 