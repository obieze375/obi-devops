[[Index]] 


```yaml

- name: ansible create multiple directory example
  ansible.builtin.file:
    path: "{{ directory }}/{{ item }}"
    state: directory
    recurse: yes
  with_items:
    - '/tmp/devops_system1'
    - '/tmp/devops_system2'
    - '/tmp/devops_system3'
  when: 


```


