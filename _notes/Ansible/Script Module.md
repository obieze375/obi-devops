[[Index]] 

Update the playbook with a play to Execute a script on all web server nodes. The script is located at /tmp/install_script.sh

```yaml

- name: 'Execute a script on all web server nodes'
  hosts: web_nodes
  tasks:
    - name: 'Execute a script'
      script: /tmp/install_script.sh
    - name: 'Start httpd service'
      service:
        name: httpd
        state: started

  

```

  

## Sample Inventory File

  

## Web Servers

```yaml

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

```

  
