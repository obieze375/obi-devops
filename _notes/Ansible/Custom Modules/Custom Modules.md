[[Index]] 

[[Epoch Module]] 

[[Custom-Module-Input-Manipulation]] 

Ansible modules are in fact python programs which are located on /usr/lib/pythonX.Y/dist-packages/ansible/modules. You can write down any custom program in python langiage and place it there and use it. Check ansible github web page for default modules ( https://github.com/ansible/ansible/tree/devel/lib/ansible/modules) but that's more advanced topic.

 After creating the .py file we can add the module to**/usr/share/ansible/plugins/modules/** or **~/.ansible/plugins/modules/** to use the it with **-m** option. When runnig the playbook 
  

## Debug

When you are working with Ansible playbooks, it’s great to have some debug options. Ansible provides a debug module that makes this task easier. It is used to print the message in the log output. The message is nothing but any variable values or output of any task.

## Sample playbook for debugging debug-playbook.yaml


~~~~

---

- hosts: centos

  vars:

    - first_var: "HAhaha"

  

  tasks:

    - name: show results

      debug: msg="The variable first_var is set to - {{ first_var }}"

  

[user1@controller demo-var]$ ansible-playbook debug-playbook.yaml

  

PLAY [centos] ***************************************************************************************************************************

  

TASK [Gathering Facts] ******************************************************************************************************************

ok: [centos]

  

TASK [show results] *********************************************************************************************************************

ok: [centos] => {

    "msg": "The variable first_var is set to - HAhaha"

}

  

PLAY RECAP ******************************************************************************************************************************

centos                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

  

~~~~

## Creating custom modules  

  

## Python Library

~~~~  

#!/usr/bin/python3

ANSIBLE_METADATA = {

    'metadata_version': '1.1',

    'status': ['preview'],

    'supported_by': 'community'

}

DOCUMENTATION = '''

---

module: icmp

short_description: simple module for icmp ping

version_added: "2.10"

description:

    - "simple module for icmp ping"

options:

    target:

        description:

            - The target to ping

        required: true

author:

    - James Spurin (@spurin)

'''

EXAMPLES = '''

# Ping an IP

- name: Ping an IP

  icmp:

    target: 127.0.0.1

# Ping a host

- name: Ping a host

  icmp:

    target: centos1

'''

RETURN = '''

'''

from ansible.module_utils.basic import AnsibleModule

def run_module():

    # define the available arguments/parameters that a user can pass to

    # the module

    module_args = dict(

        target=dict(type='str', required=True)

    ) # Module_args contains 'target' var in dictionary

    # seed the result dict in the object

    # we primarily care about changed and state

    # change is if this module effectively modified the target

    # state will include any data that you want your module to pass back

    # for consumption, for example, in a subsequent task

    result = dict(

        changed=False

    )

    # the AnsibleModule object will be our abstraction working with Ansible

    # this includes instantiation, a couple of common attr would be the

    # args/params passed to the execution, as well as if the module

    # supports check mode

    module = AnsibleModule(

        argument_spec=module_args,

        supports_check_mode=True

    ) # module_args passes through module  

    # if the user is working with this module in only check mode we do not

    # want to make any changes to the environment, just return the current

    # state with no modifications

    if module.check_mode:

        return result

    # manipulate or modify the state as needed (this is going to be the

    # part where your module will do what it needs to do)

ping_result = module.run_command('ping -c 1 {}'.format(module.params['target'])) # runs command and pings target in module

    # use whatever logic you need to determine whether or not this module

    # made any modifications to your target

    if module.params['target']:

        result['debug'] = ping_result

        result['rc'] = ping_result[0]

        if result['rc']:

          result['failed'] = True

          module.fail_json(msg='failed to ping', **result)

        else:

          result['changed'] = True

          module.exit_json(**result)

def main():

    run_module() # run run_module

if __name__ == '__main__':

    main() #run main

  
  

~~~~


## Ansible playbook that uses custom module

~~~~

---

# YAML documents begin with the document separator ---

# The minus in YAML this indicates a list item.  The playbook contains a list

# of plays, with each play being a dictionary

-

  # Hosts: where our play will run and options it will run with

  hosts: linux

  # Tasks: the list of tasks that will be executed within the play, this section

  # can also be used for pre and post tasks

  tasks:

    - name: Test icmp module

      icmp:

        target: 127.0.0.1 # target is passed to the python script which is the module in the library directory

# Three dots indicate the end of a YAML document

...

  

~~~~ 

[[Index]] 