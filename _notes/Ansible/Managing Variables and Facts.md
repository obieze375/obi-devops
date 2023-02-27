## Playbook.yml

```yaml
---
- name: install and configure webserver with basic auth
  hosts: webserver
  vars:
    firewall_pkg: firewalld
    firewall_svc: firewalld
    web_pkg: httpd
    web_svc: httpd
    ssl_pkg: mod_ssl
    httpdconf_src: files/httpd.conf
    httpdconf_dest: /etc/httpd/conf/httpd.conf
    htaccess_src: files/.htaccess
    secrets_dir: /etc/httpd/secrets
    secrets_src: files/htpasswd
    secrets_dest: "{{ secrets_dir }}/htpasswd"
    web_root: /var/www/html
  tasks:
    - name: latest version of necessary packages installed
      ansible.builtin.dnf:
        name:
        - "{{ firewall_pkg }}"
        - "{{ web_pkg }}"
        - "{{ ssl_pkg }}"
        state: latest
    - name: configure web service
      ansible.builtin.copy:
        src: "{{ httpdconf_src }}"
        dest: "{{ httpdconf_dest }}"
        owner: root
        group: root
        mode: 0644
    - name: secrets directory exists
      ansible.builtin.file:
        path: "{{ secrets_dir }}"
        state: directory
        owner: apache
        group: apache
        mode: 0500
    - name: htpasswd file exists
      ansible.builtin.copy:
        src: "{{ secrets_src }}"
        dest: "{{ secrets_dest }}"
        owner: apache
        group: apache
        mode: 0400
    - name: .htaccess file installed in docroot
      ansible.builtin.copy:
        src: "{{ htaccess_src }}"
        dest: "{{ web_root }}/.htaccess"
        owner: apache
        group: apache
        mode: 0400
    - name: create index.html
      ansible.builtin.copy:
        content: "{{ ansible_facts['fqdn'] }} ({{ ansible_facts['default_ipv4']['address'] }}) has been customized by Ansible.\n"
        dest: "{{ web_root }}/index.html"
    - name: firewall service enable and started
      ansible.builtin.service:
        name: "{{ firewall_svc }}"
        state: started
        enabled: true
    - name: open the port for the web server
      ansible.posix.firewalld:
        service: https
        state: enabled
        immediate: true
        permanent: true
    - name: web service enabled and started
      ansible.builtin.service:
        name: "{{ web_svc }}"
        state: started
        enabled: true
- name: test web server with basic auth
  hosts: workstation
  become: false
  vars:
    web_user: guest
  vars_files:
    - vars/secret.yml
  tasks:
    - name: connect to web server with basic auth
      ansible.builtin.uri:
        url: https://serverb.lab.example.com
        validate_certs: no
        force_basic_auth: yes
        user: "{{ web_user }}"
        password: "{{ web_pass }}"
        return_content: yes
        status_code: 200
        register: auth_test
    - ansible.builtin.debug:
        var: auth_test.content

  
```


Create a vars/secret.yml file, encrypted with Ansible Vault. Use the password redhat to
encrypt it. It should set the web_pass variable to redhat, which is the web user's password.

Create a subdirectory named vars in the working directory.

```bash

[student@workstation data-review]$ mkdir vars

```

Create the encrypted variable file, vars/secret.yml, using Ansible Vault. Set the
password for the encrypted file to redhat.


```bash

[student@workstation data-review]$ ansible-vault create vars/secret.yml
New Vault password: redhat
Confirm New Vault password: redhat

```




Add the following variable definition to the file:

```yaml

web_pass: redhat

```

Save and close the file.

Run the playbook.yml playbook. Verify that content is successfully returned from the web
server, and that it matches what was configured in an earlier task.

Before running the playbook, verify that its syntax is correct by running ansible navigator with the --syntax-check option.
Use --vault-id @prompt to be prompted for the Vault password. Enter redhat
when prompted for the password.
If it reports any errors, correct them before moving to the next step.
You should see output similar to the following:


```bash

[student@workstation data-review]$ ansible-navigator run -m stdout \
> --playbook-artifact-enable false \
> playbook.yml --syntax-check --vault-id @prompt
Vault password (default): redhat
playbook: /home/student/data-review/playbook.yml

```



Using the ansible-navigator command, run the playbook with the --vault-id
@prompt option. Enter redhat when prompted for the password.


```bash
[student@workstation data-review]$ ansible-navigator run -m stdout \
> --playbook-artifact-enable false \
> playbook.yml --vault-id @prompt
Vault password: redhat
PLAY [Install and configure webserver with basic auth] *********************
...output omitted...
TASK [connect to web server with basic auth] ***********************************
ok: [workstation]
TASK [debug] *******************************************************************
ok: [workstation] => {
 "auth_test.content": "serverb.lab.example.com (172.25.250.11) has been
 customized by Ansible.\n"
}

PLAY RECAP *********************************************************************
workstation : ok=3 changed=0 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
serverb.lab.example.com : ok=10 changed=8 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
```



