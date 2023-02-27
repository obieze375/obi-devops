```yaml
- name: Verify that the auditors group exists
  ansible.builtin.group:
    name: auditors
    state: present

```



