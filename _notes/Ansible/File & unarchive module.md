[[Index]] 

## Rule of thumb: Mirror playbook structure to other playbooks in repo i.e to solve and issue take a look and see if other playbooks have are using something like a module that could help. Also use modules as much as possible for cleaner code creation


## File module & unarchive module example

```yaml
--- 

- name: Playbook to download and install tomcat8
  hosts: appservers
  tasks:
    - name: install Java
      become: yes
      yum:
        name: java-1.8.0-openjdk-devel
        state: present
    - name: crate a directory
      become: yes
      file:
        path: "/opt/tomcat8"
        state: directory
        mode: 0755
    - name : Download and install tomcat
      become: yes
      tags: installtc
      unarchive:
        src: "http://apachemirror.wuchna.com/tomcat/tomcat-8/v8.5.49/bin/apache-tomcat-8.5.49.tar.gz"
        dest: "/opt/tomcat8/"
        mode: 0755
        remote_src: yes
      register: "tcinstall"
    - name: Start the tomcat instance
      become: yes
      shell:
        "./startup.sh"
      args:
        chdir: "/opt/tomcat8/apache-tomcat-8.5.49/bin"

```




