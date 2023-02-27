
## Syntax Check

[student@workstation playbook-multi]$ ansible-navigator run \
> -m stdout intranet.yml --syntax-check
playbook: /home/student/playbook-multi/intranet.yml

 10. Run the playbook using the ansible-navigator run command. Read through the
generated output to ensure that all tasks completed successfully. Verify that an HTTP GET
request to http://servera.lab.example.com provides the correct content.
10.1. Run the playbook using the ansible-navigator run command.

## Running Playbook

[student@workstation playbook-multi]$ ansible-navigator run \
> -m stdout intranet.yml
PLAY [Enable intranet services] ************************************************
TASK [Gathering Facts] *********************************************************
ok: [servera.lab.example.com]
TASK [Latest version of httpd and firewalld installed] *************************
changed: [servera.lab.example.com]
TASK [Test html page is installed] *********************************************
changed: [servera.lab.example.com]
TASK [Firewall enabled and running] ********************************************
changed: [servera.lab.example.com]
TASK [Firewall permits access to httpd service] ********************************
changed: [servera.lab.example.com]
TASK [Web server enabled and running] ******************************************
changed: [servera.lab.example.com]
PLAY [Test intranet web server] ************************************************
TASK [Gathering Facts] *********************************************************

## Checking Playbook

[student@workstation troubleshoot-host]$ ansible-navigator run \
> -m stdout mailrelay.yml --check
PLAY [Create mail relay servers] ***********************************************
...output omitted...
TASK [Check main.cf file] ******************************************************
ok: [servera.lab.example.com]
TASK [Verify main.cf file exists] **********************************************
ok: [servera.lab.example.com] => {
 "msg": "The main.cf file exists"
}
...output omitted...
TASK [Start and enable mail services] ******************************************
fatal: [servera.lab.example.com]: FAILED! => {"changed": false, "msg": "Could not
 find the requested service postfix: host"}


## Troubleshooting Errors with verbosity

[student@workstation troubleshoot-host]$ ansible-navigator run \
> -m stdout -vvvv samba.yml
ansible-playbook [core 2.13.0]
 config file = /home/student/troubleshoot-host/ansible.cfg
 configured module search path = ['/home/runner/.ansible/plugins/modules', '/usr/
share/ansible/plugins/modules']
 ansible python module location = /usr/lib/python3.9/site-packages/ansible
 ansible collection location = /home/runner/.ansible/collections:/usr/share/
ansible/collections
 executable location = /usr/bin/ansible-playbook
 python version = 3.9.7 (default, Sep 13 2021, 08:18:39) [GCC 8.5.0 20210514 (Red
 Hat 8.5.0-3)]