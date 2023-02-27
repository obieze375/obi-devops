```yml
---
- name: Install the required packages on the web server
  hosts: servera.lab.example.com
  tasks:
- name: Install the httpd packages
  ansible.builtin.dnf:
  name: httpd 
  state: present
```

## Ansible Dnf Task 1

```yml
- name: Install httpd
  ansible.builtin.dnf:
  name: httpd
  state: present

```


## DNF Command

```bash
dnf install httpd
```


## Ansible Dnf Task 2

```yml
---
- name: Install the required packages on the web server
  hosts: servera.lab.example.com
  tasks:
- name: Install the httpd packages
  ansible.builtin.dnf:
  name: httpd 
  state: present
```


## DNF Command

```bash
dnf upgrade httpd or dnf install
httpd if the package is not yet installed.

```

## Ansible Dnf Task 3

```yml
---
- name: Remove httpd
  ansible.builtin.dnf:
  name: httpd
  state: absent

```


## DNF Command

```bash
dnf remove httpd
```

## Ansible Dnf Task 4

```yml
---
- name: Upgrade all packages
  ansible.builtin.dnf:
  name: '*'
  state: latest
```


## DNF Command

```bash
dnf upgrade
```

## Ansible Dnf Task 5

```yml
---
- name: Install Development Tools
  ansible.builtin.dnf:
  name: '@Development Tools' 
  state: present

```


## DNF Command

```bash
dnf group install "Development
Tools"
```

## Ansible Dnf Task 6

```yml
---
- name: Remove Development Tools
  ansible.builtin.dnf:
  name: '@Development Tools'
  state: absent


```


## DNF Command

```bash
dnf group remove "Development
Tools"
```

## Ansible Dnf Task 7

```yml
---
- name: Install perl DNF module
  ansible.builtin.dnf:
  name: '@perl:5.26/minimal' 
  state: present

```


## DNF Command

```bash
dnf module install perl:5.26/
minimal
```


## Optimizing Multiple Package Installation

```yml
---
- name: Install the required packages on the web server
  hosts: servera.lab.example.com
  tasks:
    - name: Install the packages
      ansible.builtin.dnf:
        name:
          - httpd
          - mod_ssl
          - httpd-tools
        state: present
```

## GPG

```yml
---
- name: Configure the company Yum/DNF repositories
  hosts: servera.lab.example.com
  tasks:
    - name: Deploy the GPG public key
      ansible.builtin.rpm_key:
        key: http://materials.example.com/yum/repository/RPM-GPG-KEY-example
        state: present
    - name: Ensure Example Repo exists
      ansible.builtin.yum_repository:
        file: example
        name: example-internal
        description: Example Inc. Internal YUM/DNF repo
        baseurl: http://materials.example.com/yum/repository/
        enabled: yes
        gpgcheck: yes
        state: present


```

## repo-playbook

```yml
---
  name: Repository Configuration
  hosts: all
  vars:
    custom_pkg: simple-agent
  tasks:
    - name: Gather Package Facts
      ansible.builtin.package_facts:
        manager: auto
    - name: Show Package Facts for the custom package
      ansible.builtin.debug:
        var: ansible_facts['packages'][custom_pkg]
      when: custom_pkg in ansible_facts['packages']
    - name: Ensure Example Repo exists
      ansible.builtin.yum_repository:
        name: example-internal
        description: Example Inc. Internal YUM repo
        file: example
        baseurl: http://materials.example.com/yum/repository/
        gpgcheck: yes
    - name: Ensure Repo RPM Key is Installed
      ansible.builtin.rpm_key:
        key: http://materials.example.com/yum/repository/RPM-GPG-KEY-example
        state: present
    - name: Install Example package
      ansible.builtin.dnf:
        name: "{{ custom_pkg }}"
        state: present
    - name: Gather Package Facts
      ansible.builtin.package_facts:
        manager: auto
    - name: Show Package Facts for the custom package
      ansible.builtin.debug:
        var: ansible_facts['packages'][custom_pkg]
      when: custom_pkg in ansible_facts['packages']


``` 




## create_crontab_file.yml

```yml
---
- name: Recurring cron job
  hosts: webservers
  become: true
  tasks:
    - name: Crontab file exists
      ansible.builtin.cron:
        name: Add date and time to a file
        job: "date >> /home/devops/my_date_time_cron_job"
        minute: "*/2"
        hour: "9-16"
        weekday: "1-5"
        user: devops
        cron_file: add-date-time
        state: present
```



## remove_cron_job.yml

```yml
---
- name: Remove scheduled cron job
  hosts: webservers
  become: true
  tasks:
    - name: Cron job removed
      ansible.builtin.cron:
        name: Add date and time to a file
        user: devops
        cron_file: add-date-time
        state: absent

```



## schedule_at_task.yml

```yml
---
- name: Schedule at task
  hosts: webservers
  become: true
  become_user: devops
  tasks:
    - name: Create date and time file
      ansible.posix.at:
        command: "date > ~/my_at_date_time"
        count: 1
        units: minutes
        unique: yes
        state: present
```



## set_default_boot_target_graphical.yml

```yml
---
- name: Change default boot target
  hosts: webservers
  become: true
  gather_facts: false
  vars:
    default_target: "graphical.target"
  tasks:
    - name: Get current boot target
      ansible.builtin.command:
        cmd: systemctl get-default
        changed_when: false
        register: target
    - name: Set default boot target
      ansible.builtin.command:
        cmd: systemctl set-default {{ default_target }}
        when: default_target not in target['stdout']
```




## reboot.yml 

```yml
---
- name: Reboot hosts
  hosts: webservers
  become: true
  tasks:
    - name: Hosts are rebooted
      ansible.builtin.reboot:

set_default_boot_target_multi-user.yml
```


```yml
---
- name: Change default boot target
  hosts: webservers
  become: true
  gather_facts: false
  vars:
    default_target: "multi-user.target"
  tasks:
    - name: Get current boot target
      ansible.builtin.command:
        cmd: systemctl get-default
        changed_when: false
        register: target
    - name: Set default boot target
      ansible.builtin.command:
        cmd: systemctl set-default {{ default_target }}
        when: default_target not in target['stdout']
```




##  Create playbooks for configuring a software repository, users and groups, logical volumes, cron jobs, and additional network interfaces on a managed host.


Create a playbook named repo_playbook.yml to run on the webservers host group
that configures those managed hosts to use the Yum internal repository located at http://
materials.example.com/yum/repository, and then installs the rhelver package
available from that repository on those managed hosts.

The repository configuration must satisfy the following requirements: 

• The configuration is in the /etc/yum.repos.d/example.repo file.
• The repository ID is example-internal.
• The base URL is http://materials.example.com/yum/repository.
• The repository is configured to check RPM GPG signatures.
• The repository description is Example Inc. Internal YUM repo.

All RPM packages in that repository are signed with an organizational GPG key pair. That
GPG public key is available at http://materials.example.com/yum/repository/
RPM-GPG-KEY-example. You need to make sure that the key is configured on all managed
hosts in the webservers host group.
Run the playbook. You should confirm that the playbook installed the package on your
managed hosts after it runs.

The configuration must satisfy the following requirements:
• The configuration is stored in the /etc/yum.repos.d/example.repo file.
• The repository ID is example-internal.
• The base URL is http://materials.example.com/yum/repository.
• The repository is configured to check RPM GPG signatures.
• The repository description is Example Inc. Internal YUM repo.

Add a second task to the play that uses the ansible.builtin.rpm_key module
to ensure that the repository public key is present on the remote host. The repository
public key URL is http://materials.example.com/yum/repository/RPM-GPG-KEY-example

Add a third task to install the rhelver package available in the Yum internal repository.
The third task contains the following content:

## repo_playbook.yml playbook:

```yml
---
- name: Repository Configuration
  hosts: webservers
  tasks:
    - name: Ensure Example Repo exists
      ansible.builtin.yum_repository:
        name: example-internal
        description: Example Inc. Internal YUM repo
        file: example
        baseurl: http://materials.example.com/yum/repository/
        gpgcheck: yes
    - name: Ensure Repo RPM Key is Installed
      ansible.builtin.rpm_key:
        key: http://materials.example.com/yum/repository/RPM-GPG-KEY-example
        state: present
    - name: Install rhelver package
      ansible.builtin.dnf:
        name: rhelver
        state: present


```

## Run repo_playbook.yml

```bash
[student@workstation system-review]$ ansible-navigator run \
> -m stdout repo_playbook.yml
PLAY [Repository Configuration] ************************************************
TASK [Gathering Facts] *********************************************************
ok: [serverb.lab.example.com]


```



Create a playbook named users.yml to run on the webservers host group that creates
the webadmin user group, adds the ops1 and ops2 users, and ensures that both users have
webadmin as a supplementary group on those managed hosts.
Run the playbook. You should confirm that the users exist on the managed hosts and have
webadmin as a supplementary group after the playbook runs.

Create a vars/users_vars.yml variable file, which defines two users, ops1 and
ops2, which belong to the webadmin user group.
You might need to create the vars subdirectory.

```yml
[student@workstation system-review]$ mkdir vars
[student@workstation system-review]$ vim vars/users_vars.yml
---
users:
 - username: ops1
 groups: webadmin
 - username: ops2
 groups: webadmin

```

 

Create the users.yml playbook. Define a single play in the playbook that targets the
webservers host group.
Add a vars_files clause that defines the location of the vars/users_vars.yml
file.
Add a task that uses the group module to create the webadmin user group on the
remote host.


The playbook contains the following content:

```yml
---
- name: Create multiple local users
  hosts: webservers
  vars_files:
  - vars/users_vars.yml
  tasks:
  - name: Add webadmin group
    ansible.builtin.group:
      name: webadmin
      state: present
  - name: Create user accounts
    ansible.builtin.user:
      name: "{{ item['username'] }}"
      groups: webadmin
    loop: "{{ users }}"

```



Add a second task to the playbook that uses the ansible.builtin.user module to
create the users.

Add a loop: "{{ users }}" clause to the task to loop through the variable file for
every username found in the vars/users_vars.yml file.
Use the item['username'] variable for the name: value. The variable file might
contain additional information useful for creating the users, such as the groups that the
users should belong to.
The second task contains the following content:

Install the redhat-rhel_system_roles-1.19.3.tar.gz Ansible Content Collection
provided in the project directory to the collections directory.
Create a playbook named storage.yml to run on the webservers host group and
configure those managed hosts by using the redhat.rhel_system_roles.storage
system role, as follows:

• Uses the /dev/vdb device as an LVM physical volume for the apache-vg volume group.
• Creates the content-lv logical volume, 64 MB in size, in the apache-vg volume group.
• Creates the logs-lv logical volume, 128 MB in size, in the apache-vg volume group.
• Formats each logical volume with an XFS file system.
• Mounts the content-lv logical volume on the /var/www directory.
• Mounts the logs-lv logical volume on the /var/log/httpd directory.

Install the redhat-rhel_system_roles collection from the ~/system-review/
redhat-rhel_system_roles-1.19.3.tar.gz file into the ~/system-review/
collections directory

```bash
[student@workstation system-review]$ ansible-galaxy collection \
> install ./redhat-rhel_system_roles-1.19.3.tar.gz -p collections
```




Create the group_vars/webservers subdirectory.

```bash
[student@workstation system-review]$ mkdir -pv group_vars/webservers
mkdir: created directory 'group_vars'
mkdir: created directory 'group_vars/webservers'
```

## Run users.yml

```bash
[student@workstation system-review]$ ansible-navigator run \
> -m stdout users.yml
PLAY [Create multiple local users] *********************************************
TASK [Gathering Facts] *********************************************************
ok: [serverb.lab.example.com]
TASK [Add webadmin group] ******************************************************
changed: [serverb.lab.example.com]
TASK [Create user accounts] ****************************************************
changed: [serverb.lab.example.com] => (item={'username': 'ops1', 'groups':
 'webadmin'})
changed: [serverb.lab.example.com] => (item={'username': 'ops2', 'groups':
 'webadmin'})
PLAY RECAP *********************************************************************
serverb.lab.example.com : ok=3 changed=2 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
```




## Create the ~/system-review/group_vars/webservers/storage_vars.yml variables file.

In the variable file, define a storage_pool variable with the pool name apache-vg
for the volume group on the /dev/vdb device, and with the type set to lvm.
Within the apache-vg pool define two logical volumes:

• Define the content-lv logical volume with a size of 64 MB formatted with the XFS
file system, mounted at /var/www.

• Define the logs-lv logical volume with a size of 128 MB formatted with the XFS file
system, mounted at /var/log/httpd.
When completed, the ~/system-review/group_vars/webservers/
storage_vars.yml variables file should contain the following content

 ~/system-review/group_vars/webservers/
storage_vars.yml 

```yml
---
storage_pools:
  - name: apache-vg
    type: lvm
    disks:
      - /dev/vdb
    volumes:
      - name: content-lv
        size: 64m
        mount_point: "/var/www"
        fs_type: xfs
        state: present
      - name: logs-lv
        size: 128m
        mount_point: "/var/log/httpd"
        fs_type: xfs
        state: present

```




## storage.yml 

```yml
---
- name: Configure storage on webservers
  hosts: webservers
  roles:
    - name: redhat.rhel_system_roles.storage

```



## Run the storage.yml playbook.

```bash
[student@workstation system-review]$ ansible-navigator run \
> -m stdout storage.yml
PLAY [Configure storage on webservers] *****************************************
...output omitted...
TASK [redhat.rhel_system_roles.storage : make sure blivet is available] ********
changed: [serverb.lab.example.com]
...output omitted...
TASK [redhat.rhel_system_roles.storage : manage the pools and volumes to match the
 specified state] ***
changed: [serverb.lab.example.com]
...output omitted...
TASK [redhat.rhel_system_roles.storage : set up new/current mounts] ************
changed: [serverb.lab.example.com] => (item={'src': '/dev/mapper/apache--vg-content--lv', 'path': '/var/www', 'fstype': 'xfs', 'opts': 'defaults', 'dump': 0,
 'passno': 0, 'state': 'mounted'})
changed: [serverb.lab.example.com] => (item={'src': '/dev/mapper/apache--vg-logs--
lv', 'path': '/var/log/httpd', 'fstype': 'xfs', 'opts': 'defaults', 'dump': 0,
 'passno': 0, 'state': 'mounted'})
...output omitted...
PLAY RECAP *********************************************************************
serverb.lab.example.com : ok=21 changed=3 unreachable=0 failed=0 
 skipped=12 rescued=0 ignored=0

```



Create a playbook named create_crontab_file.yml to run on the webservers host
group that uses the ansible.builtin.cron module to create a system crontab file that
schedules a recurring Cron job. Create the /etc/cron.d/disk_usage file as the system
crontab file. Configure that file's system Cron job as follows:

• It must run as the devops user.
• It must run every two minutes from 9:00 to 16:59 on Monday through Friday.
• It must run the command df >> /home/devops/disk_usage.

## create_crontab_file.yml

Run the playbook. You should confirm that it set up the Cron job correctly after running the
playbook. You could look to see if the file deployed correctly, or inspect the /var/log/cron
log file as root to see if the job is running.

```yml
---
- name: Recurring cron job
  hosts: webservers
  tasks:
    - name: Crontab file exists
      ansible.builtin.cron:
        name: Add date and time to a file
        minute: "*/2"
        hour: 9-16
        weekday: 1-5
        user: devops
        job: df >> /home/devops/disk_usage
        cron_file: disk_usage
        state: present


```

## Run create_crontab_file.yml

```bash
[student@workstation system-review]$ ansible-navigator run \
> -m stdout create_crontab_file.yml
PLAY [Recurring cron job] ******************************************************
TASK [Gathering Facts] *********************************************************
ok: [serverb.lab.example.com]
TASK [Crontab file exists] *****************************************************
changed: [serverb.lab.example.com]
PLAY RECAP *********************************************************************
serverb.lab.example.com : ok=2 changed=1 unreachable=0 failed=0 
 skipped=0 rescued=0 ignored=0
```


## Create a playbook named network_playbook.yml to run on the webservers host group

that uses the redhat.rhel_system_roles.network role to configure the network
interface eth1 with the 172.25.250.40/24 IP address.
Run the playbook. Confirm that the playbook ran correctly.

Create a playbook named network_playbook.yml, with one play that targets the
webservers host group.
Include the redhat.rhel_system_roles.network role in the roles section of the
play.


```yml
---
- name: NIC Configuration
  hosts: webservers
  roles:
    - redhat.rhel_system_roles.network

```






## Create a new file named network.yml to define role variables.

Because these variable values apply to the hosts on the webservers host group, you
need to create that file in the group_vars/webservers directory.
Add variable definitions to support the configuration of the eth1 network interface.
The variable file contains the following content:

```yml
[student@workstation system-review]$ vim group_vars/webservers/network.yml
---
network_connections:
 - name: eth1
 type: ethernet
 ip:
 address:
 - 172.25.250.40/24

```

```bash
[student@workstation system-review]$ ansible-navigator run \
> -m stdout network_playbook.yml
PLAY [NIC Configuration] *******************************************************
TASK [Gathering Facts] *********************************************************
ok: [serverb.lab.example.com]
...output omitted...
TASK [redhat.rhel_system_roles.network : Configure networking connection profiles]
 ***
changed: [serverb.lab.example.com]
TASK [redhat.rhel_system_roles.network : Show stderr messages] *****************
ok: [serverb.lab.example.com] => {
 "__network_connections_result.stderr_lines": [
 "[002] <info> #0, state:None persistent_state:present, 'eth1': add
 connection eth1, b2332ada-021d-4f1b-a228-0e342034f95e"
 ]
}
...output omitted...
PLAY RECAP *********************************************************************
serverb.lab.example.com : ok=10 changed=1 unreachable=0 failed=0 
 skipped=12 rescued=0 ignored=0

```

