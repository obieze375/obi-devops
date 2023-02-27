[[Index]] 

```yml
  - name: Add a line to a file if it doesn't exist
    ansible.builtin.lineinfile:
      path: /tmp/example_file
      line: "This line must exist in the file"
      state: present

```


