[[Index]] 

## Rule of thumb: Mirror playbook structure to other playbooks in repo i.e to solve and issue take a look and see if other playbooks have are using something like a module that could help. Also use modules as much as possible for cleaner code creation


## File module & unarchive module example

~~~~

---

- name: Playbook to download and install tomcat8

  hosts: appservers

  

  tasks:

  - name: install Java

    become: yes

    yum:

      name: java-1.8.0-openjdk-devel

      state: present

  

  - name: crate a directory

    become: yes

    file:

      path: "/opt/tomcat8"

      state: directory

      mode: 0755

  

  - name : Download and install tomcat

    become: yes

    tags: installtc

    unarchive:

      src: "http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.49/bin/apache-tomcat-8.5.49.tar.gz"

      dest: "/opt/tomcat8/"

      mode: 0755

      remote_src: yes

    register: "tcinstall"

  

  - name: Start the tomcat instance

    become: yes

    shell:

      "./startup.sh"

    args:

      chdir: "/opt/tomcat8/apache-tomcat-8.5.49/bin"

~~~~


## Debug Module

~~~~

---

- name: Ansible debug module in action

  hosts: all

  tasks:

          - name: Print system uptime

            shell: uptime -p

            register: system_uptime

          - name: Print uptime of managed node

            debug:

              msg: "{{ system_uptime }}"

  

---

- name: Ansible debug module in action

  hosts: all

  vars:

          greetings: Hello World!

          site: Linuxtechi

  tasks:

          - name: Print the value of a variable

            debug:

              msg: "{{ greetings }}, Welcome to {{ site }}."

  
  

---

- name: Ansible debug module in action

  hosts: all

  tasks:

          - name: Print a simple statement

            debug:

              msg: "Hello World! Welcome to Linuxtechi"

  
  
  
  

~~~~


## Use of target env to use host groups in Ansible effectively

  

## Example main.yml

  

~~~~

---

 - name: Starting with  Myapp Application deployment to tomcat nodes

   hosts: '{{ target_env }}'

   gather_facts: True

   any_errors_fatal: true

   roles:

     - role: deploy

       tags:

         - deploy

       become: yes

       become_user: tomcat

       become_method: sudo

~~~~

  

## Example of inventory file

~~~~  

[prod_1]

prod-1-myapp

prod-2-myapp

prod-3-myapp

  

[prod_2]

prod-4-myapp

prod-5-myapp

prod-6-myapp

  

[preprod]

preprod-cn-p1

  

[prod:children]

prod_1

prod_2

~~~~

## Example of command

  

~~~~  

ansible-playbook myapp-main.yml -e myapp_release_version=5.0.0 --limit prod-1 OR ansible-playbook myapp-main.yml -e myapp_release_version=5.0.0 -e target_env=prod_1

~~~~


## Using Shell Module

  

~~~~

---

- hosts: loc

  tasks:

  - name: Ansible Shell chdir and executable parameters

    shell: ls -lrt > temp.txt

    args:

      chdir: /root/ansible/shell_chdir_example

      executable: /bin/bash

- hosts: loc

  tasks:

  - name: Ansible command module with chdir and executable parameters

    command: ls -lrt

    args:

      chdir: /home/mdtutorials2/command_chdir_example

      executable: /bin/bash

~~~~


Update the playbook with a play to Execute a script on all web server nodes. The script is located at /tmp/install_script.sh

~~~~

-

    name: 'Execute a script on all web server nodes'

    hosts: web_nodes

    tasks:

        -

            name: 'Execute a script on all web server nodes'

            script: /tmp/install_script.sh

  

~~~~

  

## Sample Inventory File

  

## Web Servers

~~~~

sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

web_node1 ansible_host=web01.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

web_node2 ansible_host=web02.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

web_node3 ansible_host=web03.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  

[db_nodes]

  

sql_db1

sql_db2

  

[web_nodes]  

  

web_node1

web_node2

web_node3

  

[all_nodes:children]

db_nodes

web_nodes

~~~~

  

## Service Module:

  

Update the playbook to add a new task to start httpd services on all web nodes

Use the Service module

  

~~~~

-

    name: 'Execute a script on all web server nodes'

    hosts: web_nodes

    tasks:

        -

            name: 'Execute a script'

            script: /tmp/install_script.sh

        -

            name: 'Start httpd service'

            service: 'name=httpd state=started'  

  

~~~~

  

## Sample Inventory File



~~~~

  # Web Servers

  sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

  sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

  web_node1 ansible_host=web01.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  web_node2 ansible_host=web02.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  web_node3 ansible_host=web03.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  

  [db_nodes]

  sql_db1

  sql_db2

  

  [web_nodes]

  web_node1

  web_node2

  web_node3

  

  [all_nodes:children]

  db_nodes

  web_nodes

~~~~

  

## Lineinfile Module

  

Update the playbook to add a new task in the beginning to add an entry into /etc/resolv.conf file for hosts. The line to be added is nameserver 10.1.250.10

Note: The new task must be executed first, so place it accordingly.

  

## Use the Lineinfile module

~~~~

-

    name: 'Execute a script on all web server nodes and start httpd service'

    hosts: web_nodes

    tasks:

        -

            name: 'Update entry into /etc/resolv.conf'

            lineinfile:

                path: /etc/resolv.conf

                line: 'nameserver 10.1.250.10'

        -

            name: 'Execute a script'

            script: /tmp/install_script.sh

        -

            name: 'Start httpd service'

            service:

                name: httpd

                state: present

  

~~~~

  

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

~~~~

-

    name: 'Execute a script on all web server nodes and start httpd service'

    hosts: web_nodes

    tasks:

        -

            name: 'Update entry into /etc/resolv.conf'

            lineinfile:

                path: /etc/resolv.conf

                line: 'nameserver 10.1.250.10'

        -

            name: 'Create a new user'

            user:

                name: web_user

                uid: 1040

                group: developers

        -

            name: 'Execute a script'

            script: /tmp/install_script.sh

        -

            name: 'Start httpd service'

            service:

                name: httpd

                state: present

~~~~

  

## Sample Inventory File

~~~~

  # Web Servers

  sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

  sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

  web_node1 ansible_host=web01.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  web_node2 ansible_host=web02.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  web_node3 ansible_host=web03.xyz.com ansible_connection=ssh ansible_user=administrator ansible_ssh_pass=Win$Pass

  

  [db_nodes]

  sql_db1

  sql_db2

  

  [web_nodes]

  web_node1

  web_node2

  web_node3

  

  [all_nodes:children]

  db_nodes

  web_nodes

~~~~

  
  
  
  

## Yum:  

## Sample Ansible yum-playbook.yml

~~~~

---



-

  name: Install package(s) using yum

  hosts: centos

  become: yes

  tasks:

    - name: Install the latest version of Apache

      yum:

        name: httpd

        state: latest

  

    - name: Install apache >= 2.4

      yum:

        name: httpd>=2.4

        state: present

    - name: Install a list of packages (suitable replacement for 2.11 loop deprecation warning)

      yum:

        name: Install apache and postgresql

          - httpd

          - postgresql

          - postgresql-server

        state: present

~~~~

  
## Firewall

## Sample Ansible Playbook-firewalld.yml

~~~~

---



-

  name: Set Firewall Configurations

  hosts: centos

  become: yes

  tasks:

    -  firewalld:

         service: https

         permanent: true

         state: enabled

    -  firewalld:

         port: 8080/tcp

         permanent: true

         state: disabled

    -  firewalld:

         port: 162-162/udp

         permanent: true

         state: disabled

    -  firewalld:

         source: 192.168.100.0/24

         zone: internal

         state: enabled        

~~~~