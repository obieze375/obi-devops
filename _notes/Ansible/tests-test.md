[[defaults-main]]  
[[handlers-main]]  
[[meta-main]] 
[[tasks-main]]
[[templates-test.py.j2]]  
[[tests-inventory]]  
[[tests-test]] 
[[vars-main]]  
[[vars-requirements]]
[[vars.travis]] 


```yml
---
- hosts: localhost
  remote_user: root
  roles:
    - /etc/ansible/roles/oracle-sql-example

test.yml

```