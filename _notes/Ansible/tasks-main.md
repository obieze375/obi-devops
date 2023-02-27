
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
# tasks file for /etc/ansible/roles/oracle-sql-example
- name: Create temporary build directory
  ansible.builtin.tempfile:
    state: directory
  register: temp_dir

- name: Create python file from templates
  template:
     src: test.py.j2
     dest: "{{ temp_dir.path }}/test.py.j2"
     mode: "0644 or whatever here"


- name: Run script
  shell: |
    ./test.py
  args:
    chdir: "{{ temp_dir.path }}"
    environment:
        ORACLE_HOME: "ORACLE_HOME_PATH"

```