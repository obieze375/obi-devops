[[Index]] 

## Configuration for remote server being accessed 

** Make sure ssh is installed on both sides as in the servers and either the firewall is down or it allows ssh via s pecific port to both Ip's of the servers

```bash 

eze@ubuntu:~$ sudo apt install openssh-server

eze@ubuntu:~$ sudo systemctl status ssh

eze@ubuntu:~$ sudo ufw allow ssh

root@ubuntu:~# ufw status

``` 

## Key Generation:

```bash 

eze@ubuntu:~$ sudo apt install openssh-server

  

eze@ubuntu:~$ sudo apt-get update / yum update (redhat)

  

eze@ubuntu:~$ sudo apt-get upgrade /yum update (redhat)

  

systemctl start sshd  

  

systemctl status sshd

  

eze@ubuntu:~$ sudo systemctl status ssh

  

eze@ubuntu:~$ sudo ufw allow ssh

  

eze@ubuntu:~$ sudo ufw enable

  

eze@ubuntu:~$ ssh-keygen -t rsa

  

eze@ubuntu:~$ cd .ssh

  

eze@ubuntu:~/.ssh$ ssh-keygen

  

eze@ubuntu:~/.ssh$ ls

key1 key1.pub

eze@ubuntu:~/.ssh$ ls –ltr

total 8

-rw-r--r-- 1 eze eze 392 Dec 19 16:11 key1.pub

-rw------- 1 eze eze 1679 Dec 19 16:11 key1

  

eze@ubuntu:~/.ssh$ ssh-copy-id -i ~/.ssh/key1.pub 192.168.142.136

  

eze@ubuntu:~/.ssh$ ssh 192.168.142.136

``` 

  

```bash   

 chmod 600 or 700 for ssh key to work

``` 

  

## Copy SSH credentials

```bash 
cat id_rsa.pub | pbcopy

```   
  

## SSH File Config

  Used for multiple users trying to access a box

```bash 

  1. Create new user using adduser command on the box you want them to have access

``` 


```bash 
2) use this command to copy the public key to the server you're trying to access:  

   ssh-copy-id -i ~/.ssh/obi_rsa.pub ezeaka01@10.61.240.244 (username@ip_addr)

``` 

  
```bash 

3) nano config in /.ssh directory

Edit config file like below:

Config file:

Host *

   IdentityFile ~/.ssh/obi_rsa

   User ezeaka01

Example:

ssh ezeaka01@10.61.240.244

``` 

## How to create ssh config

```bash 

  ssh 10.61.240.244 -v

  ssh ezeaka01@10.61.240.244

  ll

  ssh-copy-id -i ~/.ssh/obi_rsa.pub ezeaka01@10.61.240.244  

  nano config

  Config file:

Host *

   IdentityFile ~/.ssh/obi_rsa

   User ezeaka01

  ssh 10.61.240.244

  history

``` 

  

## Config for extending SSH timeout

  

```bash 

~/.ssh/config

  

Host *

  ServerAliveInterval 120

``` 

  

## File Transfer

  
## Transfer Remote Files with the Secure File Transfer Program
The OpenSSH suite is useful for securely running shell commands on remote systems. Use the
Secure File Transfer Program (SFTP) to interactively upload to or download files from an SSH
server. This program is part of the OpenSSH suite. A session with the sftp command uses the
secure authentication mechanism and encrypted data transfer to and from the SSH server.
Specify a remote location for the source or destination of the files to copy. For the format of the
remote location, use [user@]host:/path. The user@ portion of the argument is optional. If this
portion is missing, then the sftp command uses your current local username. When you run the
sftp command, your terminal provides an sftp> prompt. 

```bash
[user@host ~]$ sftp remoteuser@remotehost
remoteuser@remotehost's password: password
Connected to remotehost.
sftp>

```


The interactive sftp session accepts various commands that work the same way on the remote
file system as in the local file system, such as the ls, cd, mkdir, rmdir, and pwd commands. The
put command uploads a file to the remote system. The get command downloads a file from the
remote system. The exit command exits the sftp session.
List the available sftp commands by using the help command in the sftp session: 

```bash
sftp> help
Available commands:
bye Quit sftp
cd path Change remote directory to 'path'
chgrp [-h] grp path Change group of file 'path' to 'grp'
chmod [-h] mode path Change permissions of file 'path' to 'mode'
chown [-h] own path Change owner of file 'path' to 'own'
...output omitted...

```

In an sftp session, you might want to run some commands on your local host. For most available
commands, add the l character before the command. For example, the pwd command prints the
current working directory on the remote host. To print the current working directory on your local
host, use the lpwd command.

```bash
sftp> pwd
Remote working directory: /home/remoteuser
sftp> lpwd

```

Local working directory: /home/user
The next example uploads the /etc/hosts file on the local system to the newly created /home/
remoteuser/hostbackup directory on the remotehost machine. The sftp session expects
that the put command is followed by a local file in the connecting user's home directory, in this
case the /home/remoteuser directory: 

```bash
sftp> mkdir hostbackup
sftp> cd hostbackup
sftp> put /etc/hosts

```


Uploading /etc/hosts to /home/remoteuser/hostbackup/hosts
/etc/hosts 100% 227 0.2KB/s 00:00
To copy a whole directory tree recursively, use the sftp command -r option. The following
example recursively copies the /home/user/directory local directory to the remotehost
machine. 

```bash
sftp> put -r directory
Uploading directory/ to /home/remoteuser/directory
Entering directory/
file1 100% 0 0.0KB/s 00:00
file2 100% 0 0.0KB/s 00:00
sftp> ls -l
drwxr-xr-x 2 student student 32 Mar 21 07:51 directory

```

To download the /etc/yum.conf file from the remote host to the current directory on the local
system, execute the get /etc/yum.conf command, then exit the sftp session. 

```bash
sftp> get /etc/yum.conf
Fetching /etc/yum.conf to yum.conf
/etc/yum.conf 100% 813 0.8KB/s 00:00
sftp> exit 
```

```bash
[user@host ~]$ 

To get a remote file with the sftp command on a single command line, without opening an
interactive session, use the following syntax. You cannot use single command line syntax to put
files on a remote host. 

[user@host ~]$ sftp remoteuser@remotehost:/home/remoteuser/remotefile
Connected to remotehost.
Fetching /home/remoteuser/remotefile to remotefile
remotefile 100% 7
 15.7KB/s 00:00

```












[[Index]] 