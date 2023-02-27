
## Playbook.yml

In this lab, you install the Apache web server and secure it using mod_ssl. You use
conditions, handlers, and task failure handling in your playbook to deploy the environment.


```yaml
- name: Playbook Control Lab
  hosts: webservers
  vars_files: vars.yml
  tasks:
    #Fail Fast Message
    - name: Show Failed System Requirements Message
      ansible.builtin.fail:
        msg: "The {{ inventory_hostname }} did not meet minimum reqs."
        when: >
          ansible_facts['memtotal_mb'] < min_ram_mb or
          ansible_facts['distribution'] != "RedHat"
    #Install all Packages
    - name: Ensure required packages are present
      ansible.builtin.dnf:
        name: "{{ packages }}"
        state: latest
    #Enable and start services
    - name: Ensure services are started and enabled
      ansible.builtin.service:
        name: "{{ item }}"
        state: started
        enabled: yes
        loop: "{{ services }}"
    #Block of config tasks
    - name: Setting up the SSL cert directory and config files
      block:
        - name: Create SSL cert directory
          ansible.builtin.file:
            path: "{{ ssl_cert_dir }}"
            state: directory
        - name: Copy Config Files
          ansible.builtin.copy:
            src: "{{ item['src'] }}"
            dest: "{{ item['dest'] }}"
            loop: "{{ web_config_files }}"
        notify: restart web service
        rescue:
          - name: Configuration Error Message
            ansible.builtin.debug:
              msg: >
                One or more of the configuration
                changes failed, but the web service
                is still active.
    #Configure the firewall
    - name: ensure web server ports are open
      ansible.builtin.firewalld:
        service: "{{ item }}"
        immediate: true
        permanent: true
        state: enabled
        loop:
        - http
        - https
  #Add handlers
  handlers:
    - name: restart web service
      ansible.builtin.service:
        name: "{{ web_service }}"
        state: restarted
  
```

```bash

[student@workstation control-review]$ ansible-navigator run \
> -m stdout playbook.yml
  
```

## Simple Loops

```yaml

- name: Postfix and Dovecot are running
  ansible.builtin.service:
    name: "{{ item }}"
    state: started
  loop:
    - postfix
    - dovecot

vars:
  mail_services:
    - postfix
    - dovecot

tasks:
  - name: Postfix and Dovecot are running
    ansible.builtin.service:
      name: "{{ item }}"
      state: started
    loop: "{{ mail_services }}"

  
```

## Loops Over list of Dictionaries

```yaml
- name: Users exist and are in the correct groups
  user:
    name: "{{ item['name'] }}"
    state: present
    groups: "{{ item['groups'] }}"
  loop:
    - name: jane
      groups: wheel
    - name: joe
      groups: root


  
```

## Writing Loops and Conditional Tasks

```yaml
---
- name: MariaDB server is running
  hosts: database_dev
  vars:
    mariadb_packages:
      - mariadb-server
      - python3-PyMySQL
  tasks:
    - name: MariaDB packages are installed
      ansible.builtin.dnf:
        name: "{{ item }}"
        state: present
      loop: "{{ mariadb_packages }}"

```

## Handlers

You should be able to define handlers in playbooks and notify them to apply configuration
changes

```yaml
---
- name: Web application server is deployed
  hosts: webapp
  vars:
    packages:
      - nginx
      - php-fpm
    web_service: nginx
    app_service: php-fpm
    resources_dir: /home/student/control-handlers/files
    web_config_src: "{{ resources_dir }}/nginx.conf.standard"
    web_config_dst: /etc/nginx/nginx.conf
    app_config_src: "{{ resources_dir }}/php-fpm.conf.standard"
    app_config_dst: /etc/php-fpm.conf
  tasks:
    - name: "{{ packages }} packages are installed"
      ansible.builtin.dnf:
        name: "{{ packages }}"
        state: present
    - name: Make sure the web service is running
      ansible.builtin.service:
        name: "{{ web_service }}"
        state: started
        enabled: true
    - name: Make sure the application service is running
      ansible.builtin.service:
        name: "{{ app_service }}"
        state: started
        enabled: true
    - name: The {{ web_config_dst }} file has been deployed
      ansible.builtin.copy:
        src: "{{ web_config_src }}"
        dest: "{{ web_config_dst }}"
        force: true
        notify:
          - restart web service
    - name: The {{ app_config_dst }} file has been deployed
      ansible.builtin.copy:
        src: "{{ app_config_src }}"
        dest: "{{ app_config_dst }}"
        force: true
        notify:
          - restart app service
  handlers:
    - name: restart web service
      ansible.builtin.service:
        name: "{{ web_service }}"
        state: restarted
    - name: restart app service
      ansible.builtin.service:
        name: "{{ app_service }}"
        state: restarted


  
```

```yaml


  
```

```yaml


  
```


