[[Index]] 

## Rule of thumb: Mirror playbook structure to other playbooks in repo i.e to solve and issue take a look and see if other playbooks have are using something like a module that could help. Also use modules as much as possible for cleaner code creation


Playbooks are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. Playbooks are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.

Playbooks contain the steps which the user wants to execute on a particular machine. Playbooks are run sequentially. Playbooks are the building blocks for all the use cases of Ansible.

  

## Playbook Structure

  

Each playbook is an aggregation of one or more plays in it. Playbooks are structured using Plays. There can be more than one play inside a playbook.

The function of a play is to map a set of instructions defined against a particular host.

YAML is a strict typed language; so, extra care needs to be taken while writing the YAML files. There are different YAML editors but we will prefer to use a simple editor like notepad++. Just open notepad++ and copy and paste the below yaml and change the language to YAML (Language → YAML).

A YAML starts with --- (3 hyphens)

  
  

## Create a Playbook

Let us start by writing a sample YAML file. We will walk through each section written in a yaml file.

```yaml
---

- name: install and configure DB
  hosts: testServer
  become: yes
  vars:
    oracle_db_port_value : 1521
  tasks:
    - name: Install the Oracle DB
      yum: <code to install the DB>
    - name: Ensure the installed service is enabled and running
      service:
        name: <your service name>



```



  

The above is a sample Playbook where we are trying to cover the basic syntax of a playbook. Save the above content in a file as test.yml. A YAML syntax needs to follow the correct indentation and one needs to be a little careful while writing the syntax.

  
  

## The Different YAML Tags

  

Let us now go through the different YAML tags. The different tags are described below −

## name

This tag specifies the name of the Ansible playbook. As in what this playbook will be doing. Any logical name can be given to the playbook.

## hosts

This tag specifies the lists of hosts or host group against which we want to run the task. The hosts field/tag is mandatory. It tells Ansible on which hosts to run the listed tasks. The tasks can be run on the same machine or on a remote machine. One can run the tasks on multiple machines and hence hosts tag can have a group of hosts’ entry as well.

  

## vars

Vars tag lets you define the variables which you can use in your playbook. Usage is similar to variables in any programming language.

  

## tasks

All playbooks should contain tasks or a list of tasks to be executed. Tasks are a list of actions one needs to perform. A tasks field contains the name of the task. This works as the help text for the user. It is not mandatory but proves useful in debugging the playbook. Each task internally links to a piece of code called a module. A module that should be executed, and arguments that are required for the module you want to execute.

  

## Example 2: Simple Playbook



```yaml
---

- name: Simple Playbook
  hosts: webservers
  tasks:
    - name: ensure apache is at the latest version
      yum:
        name: httpd
        state: latest


```  






  

## Example 3: Apache

abcd.yml -> Installs Apache  

```yaml
---

- hosts: all
  tasks:
  - name: install apchae2 in all 100 servers
    apt:
      name: apache2
      state: present
  - name: start apche2 in all 100 servers
    service:
      name: apache2
      state: started


  


```

From <https://www.decodingdevops.com/ansible-basics-tutorial-commands/>

 



  



  
  



  

## Example Play1

## Sample Ansible Playbook1.yml

```yaml
---

  name: Play1

  hosts: centos

  tasks:

    - name : Execute command 'date'

      command: date

    - name : Execute script on server

      script: my_script.sh

    - name : Install httpd service

      yum:

        name: httpd

        state: present

    - name : Start web server

      service:

        name: httpd

        state: started
```


 





  

Remember the host we want to perform these operations against is always set at a play level. You could have any host or groups specified here but you must ensure that the host or group is first defined in the inventory file we created earlier. The host defined in the inventory file must match the host used in the Playbook .  

  

All connection information for the host is retrieved from the inventory file. There is no hard rule to use all the hosts defined in the inventory file. We can choose one or multiple or a group or multiple groups from the inventory file in the play.

  

We can also split the list of tasks into two separate plays (using our YAML skills):

  

The'-' indicates that it is an item in the list. So the Playbook is a list of dictionaries. Each play is a dictionary and has a set of properties called name, hosts and tasks . Remember these are properties of a dictionary and so the order doesn't really matter. So even if you swap the position of name and hosts, it's still a valid play.  

  

How ever this is not the same for tasks. The tasks is a list as denoted by the dashes. List are ordered collections, So the position of entries matter. Swapping the position of entries here, really matters.  

  

The different actions run by tasks are called modules. In our example, command, script, yum and service are Ansible Modules. There hundreds of other modules available. We will talk about them later

## Sample Ansible Playbook2.yml

```yaml
---

- name: Play1
  hosts: centos
  tasks:
    - name: Execute command 'date'
      command: date
    - name: Execute script on server
      script: my_script.sh

- name: Play2
  hosts: centos
  tasks:
    - name: Install httpd service
      yum:
        name: httpd
        state: present
    - name: Start web server
      service:
        name: httpd
        state: started

```









  

## Test Connectivity

## ping-playbook.yaml

```yaml
---

- name: Test Connectivity

  hosts: all

  tasks:

    - name: Ping test

      ping:

  

[user1@controller demo-playbook]$ ansible-playbook ping-playbook.yaml

  

PLAY [Test Connectivity] ****************************************************************************************************************

  

TASK [Gathering Facts] ******************************************************************************************************************

ok: [centos]

ok: [ubuntu]

  

TASK [Ping test] ************************************************************************************************************************

ok: [centos]

ok: [ubuntu]

  

PLAY RECAP ******************************************************************************************************************************

centos                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

ubuntu                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

  
```
 







## Yaml Linter:  

~~~~  

The YAML format is key while developing Playbooks. We must pay extra attention to the indentation and structure of this file. For testing yaml files:

• web site: http://www.yamllint.com/

ATOM ide with linter-js-yaml and remote-sync (if you need)

~~~~

## Running Playbook Normally

```bash

 ansible-playbook <playbook>.yml 

```

## Troubleshooting Playbook


```bash

 ansible-playbook <playbook>.yml -vvv

```

## Dynamic environment usage

  
```yaml

host: "{{ env_name }}"

vars:

  env_name: ""

  

```  

```bash

CLI USAGE: ansible-playbook <playbook>.yml --extra-vars "env_name=<value>"
``` 

```yaml
---
- name: harmful playbook
  hosts: "{{ HOSTS }}"
  tasks:
    - name: harmful task
      ansible.builtin.debug:
        msg: "harmful task"
``` 
```bash
ansible-pilot $ ansible-playbook -i limit/inventory -e "HOSTS=demo.example.com" limit/playbook3.yml
PLAY [harmful playbook] ***************************************************************************
TASK [Gathering Facts] ****************************************************************************
ok: [demo.example.com]
TASK [harmful task] *******************************************************************************
ok: [demo.example.com] => {
    "msg": "harmful task"
}
PLAY RECAP ****************************************************************************************
demo.example.com           : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ansible-pilot $

```


host searches for env_name group in inventory file specified under group vars dir


hostname in group_names" # Searches for hostname group in inventory file specified under group vars dir  


