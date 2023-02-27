[[Index]] 

```yaml
- name: Wait until a string is in the file before continuing
  ansible.builtin.wait_for:
    path: /tmp/example_file
    search_regex: "String exists, continue"
```


