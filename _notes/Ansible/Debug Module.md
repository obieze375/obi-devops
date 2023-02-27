
[[Index]] 

## Rule of thumb: Mirror playbook structure to other playbooks in repo i.e to solve and issue take a look and see if other playbooks have are using something like a module that could help. Also use modules as much as possible for cleaner code creation


```yaml
- name: Ansible debug module in action
  hosts: all
  tasks:
    - name: Print system uptime
      shell: uptime -p
      register: system_uptime

    - name: Print uptime of managed node
      debug:
        msg: "{{ system_uptime }}"

```


```yaml
- name: Ansible debug module in action

  hosts: all

  vars:

          greetings: Hello World!

          site: Linuxtechi

  tasks:

    - name: Print the value of a variable

      debug:

      msg: "{{ greetings }}, Welcome to {{ site }}."

```  


```yaml

- name: Ansible debug module in action
  hosts: all
  tasks:

    - name: Print a simple statement
      debug:
      msg: "Hello World! Welcome to Linuxtechi"

```

# Exception Handling

  

Exception handling in Ansible is similar to exception handling in any programming language. An example of the exception handling in playbook is shown below.

```yaml
---

tasks:

   - name: Name of the task to be executed

      block:

         - debug: msg = 'Just a debug message , relevant for logging'

         - command: <the command to execute>

      rescue:

         - debug: msg = 'There was an exception.. '

         - command: <Rescue mechanism for the above exception occurred)

      always:

         - debug: msg = "this will execute in all scenarios. Always will get logged"
```
  

Following is the syntax for exception handling.

  
  

rescue and always are the keywords specific to exception handling.

  
  

Block is where the code is written (anything to be executed on the Unix machine).

  
  

If the command written inside the block feature fails, then the execution reaches rescue block and it gets executed. In case there is no error in the command under block feature, then rescue will not be executed.

  
  

Always gets executed in all cases.

  
  

So if we compare the same with java, then it is similar to try, catch and finally block.

  
  







  
  
  
  

