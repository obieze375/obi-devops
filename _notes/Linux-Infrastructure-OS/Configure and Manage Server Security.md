[[Index]] 

On serverb, generate an SSH key pair for the student user. Do not protect the private key
with a passphrase. 

Log in to serverb as the student user. 

```bash 

[student@workstation ~]$ ssh student@serverb
...output omitted... 

```



Use the ssh-keygen command to generate an SSH key pair. Do not protect the
private key with a passphrase. 

```bash 
[student@serverb ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/student/.ssh/id_rsa): Enter
Created directory '/home/student/.ssh'.
Enter passphrase (empty for no passphrase): Enter
Enter same passphrase again: Enter
Your identification has been saved in /home/student/.ssh/id_rsa.
Your public key has been saved in /home/student/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:+ijpGqjEQSGBR8ORNchiRTHw/URQksVdHjsHqVBXeYI student@serverb.lab.example.com
The key's randomart image is:
+---[RSA 3072]----+
|+BBX+o*+o..=+.. |
|+.O.oooo .oE+o . |
|.+ . . .. .+ .o |
|. o . o |
| . .S |
|... . |
|.o. .. |
|o .o o |
|. .o... . |
+----[SHA256]-----+
```


## Configure the student user on servera to accept login authentication with the SSH key pair that you generated on the serverb machine. The student user on serverb must be able to log in to servera via SSH without entering a password. 

## Send the public key of the newly generated SSH key pair to the student user on the servera machine.

```bash 
[student@serverb ~]$ ssh-copy-id student@servera
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/student/.ssh/
id_rsa.pub"
The authenticity of host 'servera (172.25.250.10)' can't be established.
ED25519 key fingerprint is SHA256:shYfoFG0Nnv42pv7j+HG+FISmCAm4Bh5jfjwwSMJbrw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter
 out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted
 now it is to install the new keys
student@servera's password: student
Number of key(s) added: 1


```



Now try logging in to the machine, with: "ssh 'student@servera'"
and check to make sure that only the key(s) you wanted were added. 


## Verify that the student user can log in to servera from serverb without entering a password. Do not close the connection.

```bash 
[student@serverb ~]$ ssh student@servera
...output omitted...
[student@servera ~]$


```


## On servera, check the /user-homes/production5 directory permissions. Then,configure SELinux to run in the permissive mode by default. Check the /user-homes/production5 directory permissions. 

```bash 
[student@servera ~]$ ls -ld /user-homes/production5
drwx------. 2 production5 production5 62 May 6 05:27 /user-homes/production5
3.2. Edit the /etc/sysconfig/selinux file to set the SELINUX parameter to the
permissive value.
[student@servera ~]$ sudo vi /etc/sysconfig/selinux
...output omitted...
#SELINUX=enforcing
SELINUX=permissive
...output omitted...
```


Reboot the system. 

```bash 

[student@servera ~]$ sudo systemctl reboot
Connection to servera closed by remote host.
Connection to servera closed.
[student@serverb ~]$

```


## On serverb, verify that the /localhome directory does not exist. Then, configure the production5 user’s home directory to mount the /user-homes/production5 network file system. The servera.lab.example.com machine exports the file system as the servera.lab.example.com:/user-homes/production5 NFS share. 

## Use the autofs service to mount the network share. Verify that the autofs service creates the /localhome/production5 directory with the same permissions as on servera. 

Verify that the /localhome directory does not exist. 

```bash 
[student@serverb ~]$ ls -ld /localhome
ls: cannot access '/localhome': No such file or directory



```
On serverb, switch to the root user.

```bash 

[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#

```


## Install the autofs package. 

```bash 
[root@serverb ~]# dnf install autofs
...output omitted...
Is this ok [y/N]: y
...output omitted...
Installed:
 autofs-1:5.1.7-27.el9.x86_64 libsss_autofs-2.6.2-2.el9.x86_64
Complete!


```


## Create the /etc/auto.master.d/production5.autofs map file with the following content:

```bash 

/- /etc/auto.production5 


```

## Determine the production5 user’s home directory. 

```bash 
[root@serverb ~]# getent passwd production5
production5:x:5001:5001::/localhome/production5:/bin/bash

```


## Create the /etc/auto.production5 file with the following content: 

```bash 

/localhome/production5 -rw servera.lab.example.com:/user-homes/production5

```

## Restart the autofs service.

```bash 

[root@serverb ~]# systemctl restart autofs

```

## Verify that the autofs service creates the /localhome/production5 directory with the same permissions as on servera. 

```bash 

[root@serverb ~]# ls -ld /localhome/production5
drwx------. 2 production5 production5 62 May 6 05:52 /localhome/production5

```

## On serverb, adjust the appropriate SELinux Boolean so that the production5 user may use the NFS-mounted home directory after authenticating with an SSH key. If required, use redhat as the password of the production5 user. 

Open a new terminal window and verify from servera that the production5 user is
not able to log in to serverb with SSH key-based authentication. An SELinux Boolean
is preventing the user from logging in. From workstation, open a new terminal and
log in to servera as the student user.

```bash 

[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$

```


## Switch to the production5 user. When prompted, use redhat as the password of the production5 user. 

```bash 

[student@servera ~]$ su - production5
Password: redhat
[production5@servera ~]$

```

## Generate an SSH key pair. 

```bash 

[production5@servera ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/production5/.ssh/id_rsa): Enter
Created directory '/home/production5/.ssh'.
Enter passphrase (empty for no passphrase): Enter
Enter same passphrase again: Enter
Your identification has been saved in /home/production5/.ssh/id_rsa.
Your public key has been saved in /home/production5/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:AbUcIBXneyiGIhr4wS1xzs3WqDvbTP+eZuSRn9HQ/cw
 production5@servera.lab.example.com
The key's randomart image is:
+---[RSA 3072]----+
| ..=++ |
| . = o |
| . . = . . |
|.. * + o + . . .|
|+ = = B S .. o o.|
|.+ + + . .+ . . E|
|. . . . o o o |
| .= . +.o |
| ooo .=+ |
+----[SHA256]-----+
```




## Transfer the public key of the SSH key pair to the production5 user on the serverb machine. When prompted, use redhat as the password of the production5 user. 

```bash 

[production5@servera ~]$ ssh-copy-id production5@serverb
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/
production5/.ssh/id_rsa.pub"
The authenticity of host 'serverb (172.25.250.11)' can't be established.
ECDSA key fingerprint is SHA256:ciCkaRWF4g6eR9nSdPxQ7KL8czpViXal6BousK544TY.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter
 out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted
 now it is to install the new keys
production5@serverb's password: redhat
Number of key(s) added: 1

```

Now try logging in to the machine, with: "ssh 'production5@serverb'"
and check to make sure that only the key(s) you wanted were added. 


Use SSH public key-based authentication instead of password-based authentication to
log in to serverb as the production5 user. This command should fail. 

```bash 
[production5@servera ~]$ ssh -o pubkeyauthentication=yes \
-o passwordauthentication=no production5@serverb
production5@serverb: Permission denied (publickey,gssapi-keyex,gssapi-withmic,password).


```




On the terminal that is connected to serverb as the root user, set the
use_nfs_home_dirs SELinux Boolean to true. 

```bash 

[root@serverb ~]# setsebool -P use_nfs_home_dirs true

```



Return to the terminal that is connected to servera as the production5 user, and
use SSH public key-based authentication instead of password-based authentication to
log in to serverb as the production5 user. This command should succeed.

```bash 

[production5@servera ~]$ ssh -o pubkeyauthentication=yes \
-o passwordauthentication=no production5@serverb
...output omitted... 

[production5@serverb ~]$

```


Exit and close the terminal that is connected to serverb as the production5 user.
Keep open the terminal that is connected to serverb as the root user. 

On serverb, adjust the firewall settings to reject all connection requests that originate from
the servera machine. Use the servera IPv4 address (172.25.250.10) to configure the
firewall rule. 

Add the IPv4 address of servera to the block zone. 

```bash 

[root@serverb ~]# firewall-cmd --add-source=172.25.250.10/32 \
--zone=block --permanent
success

```

## Reload the changes in the firewall settings. 

```bash 

[root@serverb ~]# firewall-cmd --reload
success

```

On serverb, investigate and fix the issue with the failing Apache web service, which listens
on port 30080/TCP for connections. Adjust the firewall settings appropriately so that the
port 30080/TCP is open for incoming connections. 

## Restart the httpd service. This command fails to restart the service. 

```bash 

[root@serverb ~]# systemctl restart httpd.service

```


Job for httpd.service failed because the control process exited with error code.
See "systemctl status httpd.service" and "journalctl -xeu httpd.service" for
 details. 


## Investigate why the httpd service is failing. Notice the permission error that indicates that the httpd daemon failed to bind to port 30080/TCP on startup. SELinux policies can prevent an application from binding to a non-standard port. Press q to quit the command. 

```bash 

[root@serverb ~]# systemctl status httpd.service
× httpd.service - The Apache HTTP Server
 Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor
 preset: disabled)
 Active: failed (Result: exit-code) since Mon 2022-05-02 13:20:46 EDT; 29s ago
 Docs: man:httpd.service(8)
 Process: 2322 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited,
 status=1/FAILURE)
 Main PID: 2322 (code=exited, status=1/FAILURE)
 Status: "Reading configuration..."
 CPU: 30ms

May 02 13:20:46 serverb.lab.example.com systemd[1]: Starting The Apache HTTP
 Server...
May 02 13:20:46 serverb.lab.example.com httpd[2322]: (13)Permission denied:
 AH00072: make_sock: could not bind to address [::]:30080
May 02 13:20:46 serverb.lab.example.com httpd[2322]: (13)Permission denied:
 AH00072: make_sock: could not bind to address 0.0.0.0:30080
May 02 13:20:46 serverb.lab.example.com httpd[2322]: no listening sockets
 available, shutting down
...output omitted...
```


## Determine whether an SELinux policy is preventing the httpd service from binding to port 30080/TCP. The log messages reveal that the port 30080/TCP does not have the appropriate http_port_t SELinux context, which causes SELinux to prevent the httpd service from binding to the port. The log message also produces the syntax of the semanage port command, so that you can easily fix the issue. 

```bash 

[root@serverb ~]# sealert -a /var/log/audit/audit.log
...output omitted...
SELinux is preventing /usr/sbin/httpd from name_bind access on the tcp_socket port
 30080.
***** Plugin bind_ports (92.2 confidence) suggests ************************
If you want to allow /usr/sbin/httpd to bind to network port 30080
Then you need to modify the port type.
Do

```

```bash 
# semanage port -a -t PORT_TYPE -p tcp 30080
 where PORT_TYPE is one of the following: http_cache_port_t, http_port_t,
 jboss_management_port_t, jboss_messaging_port_t, ntop_port_t, puppet_port_t.
...output omitted... 



```

Set the appropriate SELinux context on the port 30080/TCP for the httpd service to
bind to it. 

```bash 

[root@serverb ~]# semanage port -a -t http_port_t -p tcp 30080

```


## Restart the httpd service. This command should successfully restart the service. 

```bash 

[root@serverb ~]# systemctl restart httpd

```


## Add the port 30080/TCP to the default public zone. 

```bash 

[root@serverb ~]# firewall-cmd --add-port=30080/tcp --permanent
success

```
```bash 
[root@serverb ~]# firewall-cmd --reload
success
```
