[[Index]] 

Ansible modules are pieces of code that can be invoked from playbooks and also from the command-line to facilitate executing procedures on remote nodes. Examples include the apt module, used to manage system packages on Ubuntu, and the user module, used to manage system users. The ping command used throughout this guide is also a module, typically used to test connection from the control node to the hosts.Ansible ships with an extensive collection of built-in modules, some of which require the installation of additional software in order to provide full functionality. You can also create your own custom modules using your language of choice.

To execute a module with arguments, include the -a flag followed by the appropriate options in double quotes, like this:


~~~~  
  ansible target-i inventory -m module-a "module options"
~~~~

As an example, this will use the apt module to install the package tree on server1:


~~~~
ansible server1-i inventory -m apt-a "name=tree"
~~~~

## Running Bash Commands

When a module is not provided via the -m option, the command module is used by default to execute the specified command on the remote server(s).

This allows you to execute virtually any command that you could normally execute via an SSH terminal, as long as the connecting user has sufficient permissions and there aren’t any interactive prompts.

This example executes the uptime command on all servers from the specified inventory:


~~~~  

ansible all-i inventory -a "uptime"

~~~~

~~~~

Copy

  

Output

server1| CHANGED | rc=0 >>

 14:12:18 up 55 days,  2:15,  1 user,  load average: 0.03, 0.01, 0.00

server2| CHANGED | rc=0 >>

 14:12:19 up 10 days,  6:38,  1 user,  load average: 0.01, 0.02, 0.00

~~~~

  

# Using Privilege Escalation to Run Commands with sudo

If the command or module you want to execute on remote hosts requires extended system privileges or a different system user, you’ll need to use Ansible’s privilege escalation module, become.  

This module is an abstraction for sudo as well as other privilege escalation software supported by Ansible on different operating systems.

For instance, if you wanted to run a tail command to output the latest log messages from Nginx’s error log on a server named server1 from inventory, you would need to include the --become option as follows:

  

~~~~

ansible server1-i inventory -a "tail /var/log/nginx/error.log"--become

~~~~

  

This would be the equivalent of running a sudo tail /var/log/nginx/error.log command on the remote host, using the current local system user or the remote user set up within your inventory file.

Privilege escalation systems such as sudo often require that you confirm your credentials by prompting you to provide your user’s password. That would cause Ansible to fail a command or playbook execution. You can then use the --ask-become-pass or -K option to make Ansible prompt you for that sudo password:

  

~~~~

ansible server1-i inventory -a "tail /var/log/nginx/error.log"--become -K

~~~~

  

Installing & Removing Packages:

  

The following example uses the apt module to install the nginx package on all nodes from the provided inventory file:

  

~~~~

ansible all -i inventory -m apt-a "name=nginx"--become -K

~~~~

  

To remove a package, include the state argument and set it to absent:

~~~~

ansible all -i inventory -m apt-a "name=nginxstate=absent"--become  -K

~~~~

  

## File Transfer

  

With the file module, you can copy files between the control node and the managed nodes, in either direction. The following command copies a local text file to all remote hosts in the specified inventory file:

  

~~~~

ansible all -i inventory -m copy-a "src=./file.txtdest=~/myfile.txt"

~~~~

  

## To copy a file from the remote server to your control node, include the remote_src option:

  

~~~~

ansible all -i inventory -m copy-a "src=~/myfile.txtremote_src=yesdest=./file.txt"  

~~~~

  

## Create new directory:

  

~~~~

$ Ansible abc -m file -a "dest = /path/user1/new mode = 777 owner = user1 group = user1 state = directory"

~~~~

  

## Delete whole directory and files:

  

~~~~

$ Ansible abc -m file -a "dest = /path/user1/new state = absent"

~~~~  

 

## Managing Packages

  
  
  

The Ad-hoc commands are available for yum and apt. Following are some Ad-hoc commands using yum.

The following command checks if yum package is installed or not, but does not update it.

~~~~  

Ansible abc -m yum -a "name = demo-tomcat-1 state = present"   
~~~~

The following command check the package is not installed.

~~~~  

$ Ansible abc -m yum -a "name = demo-tomcat-1 state = absent"  


~~~~

The following command checks the latest version of package is installed.
  
~~~~
$ Ansible abc -m yum -a "name = demo-tomcat-1 state = latest"
~~~~

  

## Gathering facts

Facts can be used for implementing conditional statements in playbook. You can find adhoc information of all your facts through the following Ad-hoc command −

~~~~
$ Ansible all -m setup
~~~~

  

## Changing File Permissions

  

To modify permissions on files and directories on your remote nodes, you can use the file module.

The following command will adjust permissions on a file named file.txt located at /var/www on the remote host. It will set the file’s umask to 600, which will enable read and write permissions only for the current file owner. Additionally, it will set the ownership of that file to a user and a group called sammy:

  

~~~~
ansible all -i inventory -m file-a "dest=/var/www/file.txtmode=600owner=sammygroup=sammy"--become  -K
~~~~

  

Because the file is located in a directory typically owned by root, we might need sudo permissions to modify its properties. That’s why we include the --become and -K options. These will use Ansible’s privilege escalation system to run the command with extended privileges, and it will prompt you to provide the sudo password for the remote user.

  

## Restarting Services

  

You can use the service module to manage services running on the remote nodes managed by Ansible. This will require extended system privileges, so make sure your remote user has sudo permissions and you include the --become option to use Ansible’s privilege escalation system. Using -K will prompt you to provide the sudo password for the connecting user.!

  

To restart the nginx service on all hosts in group called webservers, for instance, you would run:

~~~~

ansible webservers-i inventory -m service-a "name=nginxstate=restarted"--become  -K!

~~~~

  

## Restarting Servers

  

Although Ansible doesn’t have a dedicated module to restart servers, you can issue a bash command that calls the /sbin/reboot command on the remote host.

Restarting the server will require extended system privileges, so make sure your remote user has sudo permissions and you include the --become option to use Ansible’s privilege escalation system. Using -K will prompt you to provide the sudo password for the connecting user.

Warning: The following command will fully restart the server(s) targeted by Ansible. That might cause temporary disruption to any applications that rely on those servers.

To restart all servers in a webservers group, for instance, you would run:

  

~~~~

ansible webservers-i inventory -a "/sbin/reboot"--become  -K

~~~~

  
  
  

By default, Ansible will run the above Ad-hoc commands form current user account. If you want to change this behavior, you will have to pass the username in Ad-hoc commands as follows −

~~~~
$ Ansible abc -a "/sbin/reboot" -f 12 -u username 
~~~~

## Gathering Information about remote Nodes:

  

The setup module returns detailed information about the remote systems managed by Ansible, also known as system facts.

To obtain the system facts for server1, run:

~~~~

ansible server1-i inventory -m setup

~~~~

  

This will print a large amount of JSON data containing details about the remote server environment. To print only the most relevant information, include the "gather_subset=min" argument as follows:

~~~~

ansible server1-i inventory -m setup-a "gather_subset=min"

~~~~

  

To print only specific items of the JSON, you can use the filter argument. This will accept a wildcard pattern used to match strings, similar to fnmatch. For example, to obtain information about both the ipv4 and ipv6 network interfaces, you can use *ipv* as filter:

~~~~

ansible server1-i inventory -m setup-a "filter=*ipv*"

~~~~

  

~~~~

Output

server1 | SUCCESS => {

    "ansible_facts": {

        "ansible_all_ipv4_addresses": [

            "203.0.113.111",

            "10.0.0.1"

        ],

        "ansible_all_ipv6_addresses": [

            "fe80::a4f5:16ff:fe75:e758"

        ],

        "ansible_default_ipv4": {

            "address": "203.0.113.111",

            "alias": "eth0",

            "broadcast": "203.0.113.111",

            "gateway": "203.0.113.1",

            "interface": "eth0",

            "macaddress": "a6:f5:16:75:e7:58",

            "mtu": 1500,

            "netmask": "255.255.240.0",

            "network": "203.0.113.0",

            "type": "ether"

        },

        "ansible_default_ipv6": {}

    },

    "changed": false

}

If you’d like to check disk usage, you can run a Bash command calling the df utility, as follows:

~~~~

  

~~~~  

ansible all -i inventory -a "df -h"

  

Output

server1 | CHANGED | rc=0 >>

Filesystem      Size  Used Avail Use% Mounted on

udev            3.9G     0  3.9G   0% /dev

tmpfs           798M  624K  798M   1% /run

/dev/vda1       155G  2.3G  153G   2% /

tmpfs           3.9G     0  3.9G   0% /dev/shm

tmpfs           5.0M     0  5.0M   0% /run/lock

tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup

/dev/vda15      105M  3.6M  101M   4% /boot/efi

tmpfs           798M     0  798M   0% /run/user/0

server2 | CHANGED | rc=0 >>

Filesystem      Size  Used Avail Use% Mounted on

udev            2.0G     0  2.0G   0% /dev

tmpfs           395M  608K  394M   1% /run

/dev/vda1        78G  2.2G   76G   3% /

tmpfs           2.0G     0  2.0G   0% /dev/shm

tmpfs           5.0M     0  5.0M   0% /run/lock

tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup

/dev/vda15      105M  3.6M  101M   4% /boot/efi

tmpfs           395M     0  395M   0% /run/user/0

~~~~  

Shutting down servers  

~~~~  

[user1@controller demo-adhoc]$ ansible all -b -a "shutdown -r"

centos | CHANGED | rc=0 >>

Shutdown scheduled for Mon 2021-06-28 13:58:27 +0430, use 'shutdown -c' to cancel.

ubuntu | CHANGED | rc=0 >>

Shutdown scheduled for Mon 2021-06-28 02:28:27 PDT, use 'shutdown -c' to cancel.

~~~~