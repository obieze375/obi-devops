[[Index]] 

## Run ansible playbook

```bash

ansible-playbook -i /home/ansible/inventory /home/ansible/web.yml

```  

```bash

ansible-playbook -i <play_book_name>.yml

``` 

```bash

ansible-playbook <play_book_name>.yml 

```  


  

## Running the Playbook


```bash

ansible-playbook user.yml --extra-vars "target = "<your host variable>"

```  

If {{ target }} isn't defined, the playbook does nothing. A group from the hosts file can also be passed through if need be. This does not harm if the extra vars is not provided.

  

## Playbook targeting a single host

```bash

ansible-playbook user.yml --extra-vars "target = <your hosts variable>" --listhosts 

```  
  

  

## Syntax Check

```bash

ansible-playbook  <play_book_name>.yml  --syntax-check

```  



## Testing Connection to Ansible Hosts

The following command will test connectivity between your Ansible control node and all your Ansible hosts. This command uses the current system user and its corresponding SSH key as the remote login, and includes the -m option, which tells Ansible to run the ping module. It also features the -i flag, which tells Ansible to ping the hosts listed in the specified inventory file


```bash

ansible all-i inventory-m ping 

```  


If this is the first time you’re connecting to these servers via SSH, you’ll be asked to confirm the authenticity of the hosts you’re connecting to via Ansible. When prompted, type yes and then hit ENTER to confirm.

You should get output similar to this:

```bash
Output

server1 | SUCCESS => {

    "changed": false,

    "ping": "pong"

}

server2 | SUCCESS => {

    "changed": false,

    "ping": "pong"

}



```


```bash

ansible all -i inventory -m ping-u sammy
```


If you’re using a custom SSH key to connect to the remote servers, you can provide it at execution time with the --private-key option:

```bash
ansible all -i inventory -m ping--private-key=~/.ssh/custom_id

```


Once you’re able to connect using the appropriate options, you can adjust your inventory file to automatically set your remote user and private key, in case they are different from the default values assigned by Ansible. Then, you won’t need to provide those parameters in the command line.

The following example inventory file sets up the ansible_user variable only for the server1 server:

~/ansible/Inventory

```yaml


server1 ansible_host=203.0.113.111 ansible_user=sammyserver2 ansible_host=203.0.113.112

```



Ansible will now use sammy as the default remote user when connecting to the server1 server.

To set up a custom SSH key, include the ansible_ssh_private_key_file variable as follows:


```yaml
~/ansible/inventory
  
server1 ansible_host=203.0.113.111 ansible_ssh_private_key_file=/home/sammy/.ssh/custom_idserver2 ansible_host=203.0.113.112

```


In both cases, we have set up custom values only for server1. If you want to use the same settings for multiple servers, you can use a child group for that:


```yaml
~/ansible/inventory
  
[group_a]203.0.113.111

203.0.113.112

[group_b]203.0.113.113

[group_a:vars]ansible_user=sammyansible_ssh_private_key_file=/home/sammy/.ssh/custom_id

```


This example configuration will assign a custom user and SSH key only for connecting to the servers listed in group_a.



## Defining targets for command execution  

  

When running ad hoc commands with Ansible, you can target individual hosts, as well as any combination of groups, hosts and subgroups. For instance, this is how you would check connectivity for every host in a group named servers:

```bash
ansible servers-i inventory -m ping

```  

  

You can also specify multiple hosts and groups by separating them with colons:

```bash
ansible server1:server2:dbservers-i inventory -m ping

```

  

To include an exception in a pattern, use an exclamation mark, prefixed by the escape character \, as follows. This command will run on all servers from group1, except server2:

```bash

ansible group1:\!server2-i inventory -m ping

```



  

In case you’d like to run a command only on servers that are part of both group1 and group2, for instance, you should use & instead. Don’t forget to prefix it with a \ escape character:

```bash

ansible group1:\&group2-i inventory -m ping

```



## To ping all connections:


```bash

[user1@controller demo-playbook]$  ansible all -m ping

centos | SUCCESS => {

    "ansible_facts": {

        "discovered_interpreter_python": "/usr/bin/python"

    },

    "changed": false,

    "ping": "pong"

}

ubuntu | SUCCESS => {

    "ansible_facts": {

        "discovered_interpreter_python": "/usr/bin/python3"

    },

    "changed": false,

    "ping": "pong"

}


```

