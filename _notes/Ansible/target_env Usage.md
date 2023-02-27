[[Index]] 

## Use of target env to use host groups in Ansible effectively

  

## Example main.yml

```yaml
- name: Starting with Myapp Application deployment to tomcat nodes
  hosts: '{{ target_env }}'
  gather_facts: True
  any_errors_fatal: true
  roles:
    - role: deploy
      tags:
        - deploy
      become: yes
      become_user: tomcat
      become_method: sudo

``` 



  

## Example of inventory file

```yaml
[prod_1]

prod-1-myapp

prod-2-myapp

prod-3-myapp

  

[prod_2]

prod-4-myapp

prod-5-myapp

prod-6-myapp

  

[preprod]

preprod-cn-p1

  

[prod:children]

prod_1

prod_2
```



## Example of command

```bash
ansible-playbook myapp-main.yml -e myapp_release_version=5.0.0 --limit prod-1 OR ansible-playbook myapp-main.yml -e myapp_release_version=5.0.0 -e target_env=prod_1
```  


# Dynamic environment usage

```yaml
host: "{{ env_name }}"

vars:

  env_name: ""

  
  CLI USAGE: ansible-playbook .yml --extra-vars "env_name=<value>"
``` 



  

