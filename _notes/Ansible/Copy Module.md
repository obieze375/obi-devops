
[[Index]] 


```yaml
- name: copy files to destination
  hosts: localhost
  connection: local
  tasks:
    - name: copy src.txt as dest.txt in the same dir
      copy:
        src: files/src.txt
        dest: files/dest.txt
        remote_src: yes # Use for copying to remote locations or servers
      tags:
        - simple_copy

```


```yaml
  - name: Copy file with owner and permissions
    ansible.builtin.copy:
      src: /example_directory/test
      dest: /target_directory/test
      owner: joe
      group: admins
      mode: '0755'


```
