

You have inherited a playbook from the previous administrator of some web servers.
The playbook is used to configure a web service on servera.lab.example.com,
serverb.lab.example.com, serverc.lab.example.com, and
serverd.lab.example.com. The playbook also configures the firewall on the four managed
hosts so that web traffic is allowed. 

Make the following changes to the playbook.yml playbook file so that it is easier to manage.

Simplify the list of managed hosts used by the play in the /home/student/project

review/playbook.yml playbook by using a wildcard host pattern.
You have a second playbook, /home/student/projects-review/host-test.yml, that
contains a play that you can use to test host patterns before you use them in the play in the
playbook.yml playbook. 

Change into the /home/student/projects-review directory. Review the hosts parameter in the playbook.yml file.

```bash
[student@workstation ~]$ cd ~/projects-review
[student@workstation projects-review]$ cat playbook.yml
---
- name: Install and configure web service
 hosts:
 - servera.lab.example.com
 - serverb.lab.example.com
 - serverc.lab.example.com
 - serverd.lab.example.com
...output omitted...
```



```bash
[student@workstation projects-review]$ cat host-test.yml
---
- name: List inventory hostnames
 hosts: server*.lab.example.com
 gather_facts: no
 tasks:
 - name: List inventory hostnames
 ansible.builtin.debug:
 msg: "{{inventory_hostname}}"
[student@workstation projects-review]$ ansible-navigator run \
> -m stdout host-test.yml
PLAY [List inventory hostnames] ********************************************
TASK [List inventory hostnames] ********************************************
ok: [servera.lab.example.com] => {
 "msg": "servera.lab.example.com"
}
ok: [serverb.lab.example.com] => {
 "msg": "serverb.lab.example.com"
}
ok: [serverc.lab.example.com] => {
 "msg": "serverc.lab.example.com"
}
ok: [serverd.lab.example.com] => {
 "msg": "serverd.lab.example.com"
}
PLAY RECAP *****************************************************************
servera.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverb.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverc.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverd.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
```


Verify that the host pattern server*.lab.example.com correctly identifies the four
managed hosts that are targeted by the playbook.yml playbook. View the contents
of the host-test.yml playbook, then run the playbook.



```bash
[student@workstation projects-review]$ cat host-test.yml


---
- name: List inventory hostnames
 hosts: server*.lab.example.com
 gather_facts: no
 tasks:
 - name: List inventory hostnames
 ansible.builtin.debug:
 msg: "{{inventory_hostname}}"
```

```bash
[student@workstation projects-review]$ ansible-navigator run \
> -m stdout host-test.yml
PLAY [List inventory hostnames] ********************************************
TASK [List inventory hostnames] ********************************************
ok: [servera.lab.example.com] => {
 "msg": "servera.lab.example.com"
}
ok: [serverb.lab.example.com] => {
 "msg": "serverb.lab.example.com"
}
ok: [serverc.lab.example.com] => {
 "msg": "serverc.lab.example.com"
}
ok: [serverd.lab.example.com] => {
 "msg": "serverd.lab.example.com"
}
PLAY RECAP *****************************************************************
servera.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverb.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverc.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverd.lab.example.com : ok=1 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0


```









Replace the host list in the playbook.yml playbook with the
server*.lab.example.com host pattern.




```yaml
---
- name: Install and configure web service
 hosts: server*.lab.example.com
...output omitted...

```




Restructure the playbook.yml playbook so that the first three tasks in its play are kept in
an external task file located at tasks/web_tasks.yml. Use the import_tasks feature to
incorporate this task file into the play.

Create the tasks subdirectory.

```bash

[student@workstation projects-review]$ mkdir tasks


```






Place the contents of the first three tasks in the play in the playbook.yml playbook
into the tasks/web_tasks.yml file. The task file should contain the following
content:


```yaml
---
- name: Install httpd
 ansible.builtin.dnf:
 name: httpd
 state: latest
- name: Enable and start httpd
 ansible.builtin.service:
 name: httpd
 enabled: true
 state: started
- name: Tuning configuration installed
 ansible.builtin.copy:
 src: files/tune.conf
 dest: /etc/httpd/conf.d/tune.conf
 owner: root
 group: root
 mode: 0644
 notify:
 - restart httpd


```



Remove the first three tasks from the play in the playbook.yml playbook. Put the
following lines in their place to import the tasks/web_tasks.yml task file. 


```yaml

 - name: Import the web_tasks.yml task file
 import_tasks: tasks/web_tasks.yml

```



Restructure the playbook.yml playbook so that the fourth, fifth, and sixth tasks in its
play are kept in an external task file located at tasks/firewall_tasks.yml. Use the
import_tasks feature to incorporate this task file into the play.

Place the contents of the three remaining tasks in the play in the playbook.yml
playbook into the tasks/firewall_tasks.yml file. The task file should contain the
following content.




```yaml
---
- name: Install firewalld
 ansible.builtin.dnf:
 name: firewalld
 state: latest
- name: Enable and start the firewall
 ansible.builtin.service:
 name: firewalld
 enabled: true
 state: started
- name: Open the port for http
 ansible.posix.firewalld:
 service: http
 immediate: true
 permanent: true
 state: enabled
```





Remove the remaining three tasks from the play in the playbook.yml playbook. Put
the following lines in their place, which imports the tasks/firewall_tasks.yml
task file.

```bash
 - name: Import the firewall_tasks.yml task file
 import_tasks: tasks/firewall_tasks.yml
```

Both the tasks/web_tasks.yml file and the tasks/firewall_tasks.yml file contain
tasks that install packages and enable services. Those could be consolidated into a single
task file and you could use variables to control which packages and services are installed and
enabled by those tasks.

Move the tasks that install packages and enable services into a new file named tasks/
install_and_enable.yml and update them to use variables. Replace the original tasks
with import_tasks statements, passing in appropriate values to the new variables.


Copy the ansible.builtin.dnf and ansible.builtin.service tasks from
tasks/web_tasks.yml into a new file named tasks/install_and_enable.yml. 




```yaml
---
- name: Install httpd
 ansible.builtin.dnf:
 name: httpd
 state: latest
- name: Enable and start httpd
 ansible.builtin.service:
 name: httpd
 enabled: true
 state: started

```




Replace the package and service names in tasks/install_and_enable.yml with
the variables package and service.


```yaml
---
- name: Install {{ package }}
 ansible.builtin.dnf:
 name: "{{ package }}"
 state: latest
- name: Enable and start {{ service }}
 ansible.builtin.service:
 name: "{{ service }}"
 enabled: true
 state: started
```




Replace the ansible.builtin.dnf and ansible.builtin.service tasks in
tasks/web_tasks.yml and tasks/firewall_tasks.yml with import_tasks
statements that import tasks/install_and_enable.yml and set appropriate
values on task variables to install the correct package and start the correct service.


```yaml
---
- name: Install and start httpd
 import_tasks: install_and_enable.yml
 vars:
 package: httpd
 service: httpd
---
- name: Install and start firewalld
 import_tasks: install_and_enable.yml
 vars:
 package: firewalld
 service: firewalld
```




Confirm that you made the changes to the play in playbook.yml correctly, and then run the
playbook.

Verify that the playbook.yml playbook contains the following contents:
```yaml
---
- name: Install and configure web service
 hosts: server*.lab.example.com
 tasks:
 - name: Import the web_tasks.yml task file
 import_tasks: tasks/web_tasks.yml
 - name: Import the firewall_tasks.yml task file
 import_tasks: tasks/firewall_tasks.yml
 handlers:
 - name: restart httpd
 ansible.builtin.service:
 name: httpd
 state: restarted


```


Run the playbook.yml playbook with ansible-navigator run --syntax check to verify the playbook contains no syntax errors. 

Correct any reported errors
before preceding.

```bash
[student@workstation projects-review]$ ansible-navigator run \
> -m stdout playbook.yml --syntax-check
playbook: /home/student/projects-review/playbook.yml
5.3. Run the playbook.yml playbook.
[student@workstation projects-review]$ ansible-navigator run \
> -m stdout playbook.yml
PLAY [Install and configure web service] ***********************************
TASK [Gathering Facts] *****************************************************
ok: [serverd.lab.example.com]
ok: [serverc.lab.example.com]
ok: [serverb.lab.example.com]
ok: [servera.lab.example.com]
TASK [Install httpd] *******************************************************
changed: [serverb.lab.example.com]
changed: [servera.lab.example.com]
changed: [serverd.lab.example.com]
changed: [serverc.lab.example.com]
TASK [Enable and start httpd] **********************************************
changed: [servera.lab.example.com]
changed: [serverb.lab.example.com]
changed: [serverd.lab.example.com]
changed: [serverc.lab.example.com]
TASK [Tuning configuration installed] **************************************
changed: [serverd.lab.example.com]
changed: [serverc.lab.example.com]
changed: [serverb.lab.example.com]
changed: [servera.lab.example.com]
TASK [Install firewalld] ***************************************************
ok: [serverb.lab.example.com]
ok: [servera.lab.example.com]
ok: [serverd.lab.example.com]
ok: [serverc.lab.example.com]
TASK [Enable and start firewalld] ******************************************
ok: [servera.lab.example.com]
ok: [serverb.lab.example.com]
ok: [serverc.lab.example.com]
ok: [serverd.lab.example.com]

TASK [Open the port for http] **********************************************
changed: [serverd.lab.example.com]
changed: [serverb.lab.example.com]
changed: [servera.lab.example.com]
changed: [serverc.lab.example.com]
RUNNING HANDLER [restart httpd] ********************************************
changed: [serverd.lab.example.com]
changed: [serverb.lab.example.com]
changed: [serverc.lab.example.com]
changed: [servera.lab.example.com]
PLAY RECAP *****************************************************************
servera.lab.example.com : ok=8 changed=5 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverb.lab.example.com : ok=8 changed=5 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverc.lab.example.com : ok=8 changed=5 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverd.lab.example.com : ok=8 changed=5 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0



```










```bash



```


```yaml



```


```bash



```


```yaml



```