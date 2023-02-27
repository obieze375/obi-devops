```yml
  - name: Add a block of configuration options at the end of the file if it doesn't exist
    ansible.builtin.blockinfile:
      path: /etc/example_directory/example.conf
      block: |
        feature1_enabled: true
        feature2_enabled: false
        feature3_enabled: true
      insertafter: EOF

```


