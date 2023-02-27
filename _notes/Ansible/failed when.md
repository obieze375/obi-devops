
[[Index]] 

```yaml
---

- hosts: all
  tasks:
    - name: "shut down Rhel 6 and Debian 7 systems"
      command: /sbin/shutdown -t now
      when: 
        (ansible_distribution == "CentOS" and ansible_distribution_major_version == "6") or
        (ansible_distribution == "Debian" and ansible_distribution_major_version == "7") or
        (ansible_distribution == "Debian" and ansible_distribution_major_version == "8")


```