```yml
---
- name: Playbook to install NGINX
  hosts: webservers
  become: true
  tasks:
    - name: Ansible apt install nginx
      apt:
        name: nginx
        state: present

```



[[Index]] 