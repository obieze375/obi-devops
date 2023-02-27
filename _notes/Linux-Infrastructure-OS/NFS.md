[[Index]] 

## RHEL 8 Labs

An IT support company uses a central server, serverb, to host some exported directories
on /shares for their groups and users. Users must be able to log in and have their exported
directories mounted on demand and ready to use, under the /remote directory on servera.
Environment Characteristics:
• The serverb machine is sharing the /shares directory, which in turn contains the
management, production, and operation subdirectories.
• The managers group consists of the manager1 and manager2 users. They have read and write
access to the /shares/management exported directory.
• The production group consists of the dbuser1 and sysadmin1 users. They have read and
write access to the /shares/production exported directory.
• The operators group consists of the contractor1 and consultant1 users. They have read
and write access to the /shares/operation exported directory.
• The main mount point for servera is the /remote directory.
• Use the /etc/auto.master.d/shares.autofs file as the master map file and the /etc/
auto.shares file as the indirect map file.
• The /shares/management exported directory is automounted on /remote/management on
servera.
• The /shares/production exported directory is automounted on /remote/production on
servera.
• The /shares/operation exported directory is automounted on /remote/operation on
servera.



## Install the autofs package. 

```bash 
[root@servera ~]# dnf install autofs
...output omitted...
Is this ok [y/N]: y
...output omitted...
Complete!
```

## Configure an automounter indirect map on servera with exports from serverb. Create an indirect map with files that are named /etc/auto.master.d/shares.autofs for the master map and /etc/auto.shares for the mapping file. Use the /remote directory as the main mount point on servera. Reboot servera to determine whether the autofs service starts automatically. 



## Test the NFS server before you configure the automounter. 

```bash 
[root@servera ~]# mount -t nfs serverb.lab.example.com:/shares /mnt
[root@servera ~]# ls -l /mnt
total 0
drwxrwx---. 2 root managers 25 Apr 4 01:13 management
drwxrwx---. 2 root operators 25 Apr 4 01:13 operation
drwxrwx---. 2 root production 25 Apr 4 01:13 production
[root@servera ~]# umount /mnt
```

## Create a master map file named /etc/auto.master.d/shares.autofs, insert the following content, and save the changes. /remote /etc/auto.shares 


## Create an indirect map file named /etc/auto.shares, insert the following content, and save the changes.

```bash 
* -rw,sync,fstype=nfs4 serverb.lab.example.com:/shares/&
```


## Start and enable the autofs service on servera. 

```bash 
[root@servera ~]# systemctl enable --now autofs
Created symlink /etc/systemd/system/multi-user.target.wants/autofs.service → /usr/
lib/systemd/system/autofs.service.
```


## Reboot the servera machine. 

```bash 
[root@servera ~]# systemctl reboot

```

## Test the autofs configuration with the various users. When done, log out from servera. 

## After the servera machine is finished booting, log in to servera as the student user. 

```bash 
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
```

## Switch to the manager1 user and test access. 

```bash 
[student@servera ~]$ su - manager1
Password: redhat
[manager1@servera ~]$ ls -l /remote/management/
total 4
-rw-r--r--. 1 root managers 46 Apr 4 01:13 Welcome.txt
[manager1@servera ~]$ cat /remote/management/Welcome.txt
###Welcome to Management Folder on SERVERB###
[manager1@servera ~]$ echo TEST1 > /remote/management/Test.txt
[manager1@servera ~]$ cat /remote/management/Test.txt
TEST1
[manager1@servera ~]$ ls -l /remote/operation/
ls: cannot open directory '/remote/operation/': Permission denied
[manager1@servera ~]$ ls -l /remote/production/
ls: cannot open directory '/remote/production/': Permission denied
[manager1@servera ~]$ exit
logout
[student@servera ~]$
```


## Switch to the dbuser1 user and test access. 

```bash 
[student@servera ~]$ su - dbuser1
Password: redhat
[dbuser1@servera ~]$ ls -l /remote/production/
total 4
-rw-r--r--. 1 root production 46 Apr 4 01:13 Welcome.txt
[dbuser1@servera ~]$ cat /remote/production/Welcome.txt
###Welcome to Production Folder on SERVERB###
[dbuser1@servera ~]$ echo TEST2 > /remote/production/Test.txt
[dbuser1@servera ~]$ cat /remote/production/Test.txt
TEST2
[dbuser1@servera ~]$ ls -l /remote/operation/ 
ls: cannot open directory '/remote/operation/': Permission denied
[dbuser1@servera ~]$ ls -l /remote/management/
ls: cannot open directory '/remote/management/': Permission denied
[dbuser1@servera ~]$ exit
logout
[student@servera ~]$
```



## Switch to the contractor1 user and test access. 

```bash 
[student@servera ~]$ su - contractor1
Password: redhat
[contractor1@servera ~]$ ls -l /remote/operation/
total 4
-rw-r--r--. 1 root operators 45 Apr 4 01:13 Welcome.txt
[contractor1@servera ~]$ cat /remote/operation/Welcome.txt
###Welcome to Operation Folder on SERVERB###
[contractor1@servera ~]$ echo TEST3 > /remote/operation/Test.txt
[contractor1@servera ~]$ cat /remote/operation/Test.txt
TEST3
[contractor1@servera ~]$ ls -l /remote/management/
ls: cannot open directory '/remote/management/': Permission denied
[contractor1@servera ~]$ ls -l /remote/production/
ls: cannot open directory '/remote/production/': Permission denied
[contractor1@servera ~]$ exit
logout 
[student@servera ~]$
```


## Explore the mount options for the NFS automounted export. 

```bash 
[student@servera ~]$ mount | grep nfs
rpc_pipefs on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw,relatime)
serverb.lab.example.com:/shares/management on /remote/management type nfs4
(rw,relatime,vers=4.2,rsize=262144,wsize=262144,namlen=255,
sync,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=172.25.250.10,
local_lock=none,addr=172.25.250.11)
serverb.lab.example.com:/shares/operation on /remote/operation type nfs4
(rw,relatime,vers=4.2,rsize=262144,wsize=262144,namlen=255,
sync,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=172.25.250.10,
local_lock=none,addr=172.25.250.11)
serverb.lab.example.com:/shares/production on /remote/production type nfs4
(rw,relatime,vers=4.2,rsize=262144,wsize=262144,namlen=255,
sync,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=172.25.250.10,
local_lock=none,addr=172.25.250.11)

```

