[[Index]] 



## Sample Ansible Playbook-firewalld.yml

```yaml
---
- name: Set Firewall Configurations
  hosts: centos
  become: yes
  tasks:
    - firewalld:
        service: https
        permanent: true
        state: enabled
    - firewalld:
        port: 8080/tcp
        permanent: true
        state: disabled
    - firewalld:
        port: 162-162/udp
        permanent: true
        state: disabled
    - firewalld:
        source: 192.168.100.0/24
        zone: internal
        state: enabled



```


