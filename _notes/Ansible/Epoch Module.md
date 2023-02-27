[[Index]] 

[[Custom-Module-Input-Manipulation]] 

[[Custom Modules]] 

## Module Python

## Shows how to take an input via a playbook and pass to a python module which generates an output

```

#!/usr/bin/python

from __future__ import (absolute_import, division, print_function)
__metaclass__ = type
import datetime

DOCUMENTATION = r'''
---
module: epoch_converter

short_description: This module converts an epoch timestamp to human-readable date.

# If this is part of a collection, you need to use semantic versioning,
# i.e. the version is of the form "2.5.0" and not "2.4".
version_added: "1.0.0"

description: This modules takes a string that represents a Unix epoch timestamp and displays its human-readable date equivalent.

options:
    epoch_timestamp:
        description: This is the string that represents a Unix epoch timestamp.
        required: true
        type: str
    state_changed:
        description: This string simulates a modification of the target's state.
        required: false
        type: bool

author:
    - Ioannis Moustakis (@Imoustak)
'''

EXAMPLES = r'''
# Convert an epoch timestamp
- name: Convert an epoch timestamp
  epoch_converter:
    epoch_timestamp: 1657382362
'''

RETURN = r'''
# These are examples of possible return values, and in general should use other names for return values.
human_readable_date:
    description: The human-readable equivalent of the epoch timestamp input.
    type: str
    returned: always
    sample: '2022-07-09T17:59:22'
original_timestamp:
    description: The original epoch timestamp input.
    type: str
    returned: always
    sample: '16573823622'

'''

from ansible.module_utils.basic import AnsibleModule


def run_module():
    # define available arguments/parameters a user can pass to the module
    module_args = dict(
        epoch_timestamp=dict(type='str', required=True),
        state_changed=dict(type='bool', required=False)
    )

    # seed the result dict in the object
    # we primarily care about changed and state
    # changed is if this module effectively modified the target
    # state will include any data that you want your module to pass back
    # for consumption, for example, in a subsequent task
    result = dict(
        changed=False,
        human_readable_date='',
        original_timestamp=''
    )

    # the AnsibleModule object will be our abstraction working with Ansible
    # this includes instantiation, a couple of common attr would be the
    # args/params passed to the execution, as well as if the module
    # supports check mode
    module = AnsibleModule(
        argument_spec=module_args,
        supports_check_mode=True
    )

    # if the user is working with this module in only check mode we do not
    # want to make any changes to the environment, just return the current
    # state with no modifications
    if module.check_mode:
        module.exit_json(**result)

    # manipulate or modify the state as needed (this is going to be the
    # part where your module will do what it needs to do)
    result['original_timestamp'] = module.params['epoch_timestamp']
    result['human_readable_date'] = datetime.datetime.fromtimestamp(int(module.params['epoch_timestamp']))

    # use whatever logic you need to determine whether or not this module
    # made any modifications to your target
    if module.params['state_changed']:
        result['changed'] = True

    # during the execution of the module, if there is an exception or a
    # conditional state that effectively causes a failure, run
    # AnsibleModule.fail_json() to pass in the message and the result
    if module.params['epoch_timestamp'] == 'fail':
        module.fail_json(msg='You requested this to fail', **result)

    # in the event of a successful module execution, you will want to
    # simple AnsibleModule.exit_json(), passing the key/value results
    module.exit_json(**result)


def main():
    run_module()


if __name__ == '__main__':
    main()

```



## Module .yml

```
- name: Test my new module
  hosts: localhost
  vars: 
    epoch_var: ""
  tasks:
  - name: Run the new module
    epoch_converter:
      epoch_timestamp: '{{ epoch_var }}'
      state_changed: yes
    register: show_output
  - name: Show Output
    debug:
      msg: '{{ show_output }}'
```

## CLI Output

```
eze@eze-virtual-machine:~/Blog-Technical-Content/ansible-modules$ ansible-playbook test_custom_module.yml -e epoch_var=1657382362

PLAY [Test my new module] ***********************************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [localhost]

TASK [Run the new module] ***********************************************************************************
changed: [localhost]

TASK [Show Output] ******************************************************************************************
ok: [localhost] => {
    "msg": {
        "changed": true,
        "failed": false,
        "human_readable_date": "2022-07-09T16:59:22",
        "original_timestamp": "1657382362"
    }
}

PLAY RECAP **************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

eze@eze-virtual-machine:~/Blog-Technical-Content/ansible-modules$ ll
total 32
drwxrwxr-x  3 eze eze 4096 Dec 24 12:32 ./
drwxrwxr-x 13 eze eze 4096 Dec 24 12:32 ../
-rw-rw-r--  1 eze eze  114 Dec 24 12:32 ansible.cfg
-rw-rw-r--  1 eze eze 2477 Dec 24 12:32 examples_common_modules.yml
-rw-rw-r--  1 eze eze   41 Dec 24 12:32 hosts
drwxrwxr-x  2 eze eze 4096 Dec 24 12:32 library/
-rw-rw-r--  1 eze eze   91 Dec 24 12:32 README.md
-rw-rw-r--  1 eze eze  287 Dec 24 13:18 test_custom_module.yml
eze@eze-virtual-machine:~/Blog-Technical-Content/ansible-modules$ 
```

