[[Index]] 

 An Ansible template is a text file built with the Jinja2 templating language with a j2 file extension. A Jinja2 template looks exactly like the text file you’d like to get onto a remote host. The only difference is that instead of static values, the file contains variables.

 For demonstration, lets install web server on both centos and ubuntu in our lab environment and make sure everything is going fine :



~~~~

[user1@controller demo-temp]$ ansible ubuntu -b -m apt -a "name=apache2 state=present"

[user1@controller demo-temp]$ ansible centos -b -m yum -a "name=httpd state=present"

  
  

Next create a template for web server default page (index.html.j2):

  

<html>

<center>

<h1> This machine's hostname is {{ ansible_hostname }}</h1>

<h2> os family is {{ ansible_os_family }}</h2>

<small>file version is {{ file_version }}</small>

{# This is comment, it will not appear in final output #}

</center>

</html>

~~~~

 and related playpook to use this template:

  

~~~~

---

#sample playbook using template for web server- template-playbook.yaml

  

- hosts: all

  become: yes

  vars:

   file_version: 1.0

  

  tasks:

   - name: install default web page

     template:

       src: index.html.j2

       dest: /var/www/html/index.html

       mode: 0777

  
  

~~~~