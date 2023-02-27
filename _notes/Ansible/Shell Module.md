
[[Index]] 

## Using Shell Module


```yaml
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

```


Update the playbook with a play to Execute a script on all web server nodes. The script is located at /tmp/install_script.sh


```yaml
- name: 'Execute a script on all web server nodes'
  hosts: web_nodes
  tasks:
    - name: 'Execute a script on all web server nodes'
      script: /tmp/install_script.sh

```


```yaml

---
- hosts: loc
  tasks:
    - name: Ansible Shell chdir and executable parameters
      shell: |
        bash example
        bash example
        bash example
      args:
        chdir: /root/ansible/shell_chdir_example
        executable: /bin/bash
    - name: Ansible Shell chdir and executable parameters
      shell: |
        bash example
        bash example
        bash example
      args:
        chdir: /root/ansible/shell_chdir_example
        executable: /bin/bash

```








