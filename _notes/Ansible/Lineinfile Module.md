[[Index]] 


Update the playbook to add a new task in the beginning to add an entry into /etc/resolv.conf file for hosts. The line to be added is nameserver 10.1.250.10

Note: The new task must be executed first, so place it accordingly.

  

## Use the Lineinfile module

```yaml

- name: 'Execute a script on all web server nodes and start httpd service'
  hosts: web_nodes
  tasks:
    - name: 'Update entry into /etc/resolv.conf'
      lineinfile:
          path: /etc/resolv.conf
          line: 'nameserver 10.1.250.10'
    - name: 'Execute a script'
      script: /tmp/install_script.sh
    - name: 'Start httpd service'
      service:
          name: httpd
          state: present
```



```yaml
- name: Add a line to a file if it doesn't exist
  ansible.builtin.lineinfile:
    path: /tmp/example_file
    line: "This line must exist in the file"
    state: present

```     