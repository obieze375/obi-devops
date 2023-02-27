```yml
- name: Copy and template the Nginx configuration file to the host
  hosts: web_nodes
  tasks:
    - name: Copy and template the Nginx configuration file
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/sites-available/default

```    