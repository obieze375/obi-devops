[[Index]] 


## Groups & Users

  
  

``` 

groupadd group_name  

  

  

Example:  

  

  

groupadd friends  

``` 

  

## usermod

``` 

usermod [ options ] username  

  

  

-c "COMMENT"

  

-g GROUP

  

Comments account.

  

Specify the default group.

  

-G GROUP1,GROUPN Additional groups.

  

-s /shell/path Path to the user's shell.

``` 

  

## Usermod  

``` 

grep mysql /etc/passwd

``` 

``` 

mysql:x:97:1003:MySQL Server:
  
``` 



``` 
/opt/mysql:/usr/sbin/nologin
``` 

usermod -c "MySQL User" mysql

```   

 

grep mysql /etc/passwd

  

mysql:x:97:1003:MySQL User:

 

   

/opt/mysql:/usr/sbin/nologin

  

``` 

  

``` 

/etc/group

  

root:x:0:

  

sales:x:1001:john,mary

  

The format of the /etc/group file:

  

group_name:password:GID:account1,accountN

``` 

``` 

groups [username]

  

groups root

  

  

root

  

``` 

  

## groupadd [ options ]group_name

  

``` 

groupadd web

  

tail -1 /etc/group

  

web:x:1003:

  

groupadd -g 2500 db

  

tail -1 /etc/group

  

db:x:2500:

  

``` 

  

  

## Groupmod  

``` 

groupmod [options] group_name

  

-g GID

  

-n GROUP

  

Change the group ID to GID.

``` 

  

# Rename the group to GROUP.

  

``` 

groupmod

``` 

``` 

grep web /etc/group

  

web:x:1003:

``` 

``` 

groupmod -g 1234 web

  

grep web /etc/group

  

web:x:1234:

``` 

``` 

groupmod -n http web

  

grep http /etc/group

  

http:x:1234:  

``` 

  

## Group Deletion  

  

``` 

groupdel <group_name>

  

groupdel db  

``` 

  

## cat and grep file for particular expression:

```   

cat /etc/group | grep <part_group_name>  

```  

# Troubleshooting User accounts:

  

The issue was the client didn't have access to the BE of the Prod domain so they couldn't use winscp to login

  

  

# Create BE and check if they can login  

  
```  
If they can't, try and login into their account from another Linux box  e.g. ssh <theiruserid>@<targetserver>  

  

If you can login, then check their if their user permissions are correct via lslogins <username> compare the results to their other BE accounts in other domains  

  

Check if the client can telnet to port 22 on the server (This rules out any network issues on their side)  

  
```  

## Create new user with Useradd  

  

  

useradd[options] username  

  

  

-c "COMMENT"

  

-m

  

-s /shell/path

  

Comments for the account.

  

Create the home directory.

  

The path to the user's shell.

  

  

## Useradd  

  

``` 

useradd –c "Grant Stewart" –m –s /bin/bash grant

``` 

  

Note:This is one command line. It's displayed on

  

multiple lines for readability.

  

``` 

useradd –c "Grant Stewart" –m –s /bin/bash grant

``` 

  

## SECONDARY USER ACCOUNT Creation:  

  

``` 

adduser jbloggs <Create user>

``` 

``` 

passwd jbloggs  <Set Password>

``` 

``` 

chage -d 0 jbloggs  Triggers user to change it when they log in

``` 

``` 

chage -E 6 jbloggs - 6 month expiry date

``` 

``` 

id jbloggs

``` 

``` 

usermod -a -G <group_name> jbloggs - add to group  

``` 

  

## Create a password using passwd  

  

``` 

passwd grant

  

  

Enter new UNIX password:

  

Retype new UNIX password:

  

passwd: password updated

  

successfully

``` 

``` 

tail -1 /etc/passwd

  

  

grant:x:1000:1000:Grant Stewart:/home/grant

  

:/bin/bash

  

``` 

  

``` 

tail -1 /etc/shadow

grant:66iDDsgsPYtR8c2Uc.:16507:0:99999:7:::

``` 

Account information for "grant"

  

  

## More useradd options  

  

``` 

  

-g GROUP Specify the default group.

  

-G GROUP1,GROUPN Additional groups.

``` 

``` 

useradd –c "Eddie Harris" –m –s

``` 

``` 

/bin/bash –g sales –G projectx

  

eharris

``` 

``` 

useradd

  

passwd eharris

  

  

Enter new UNIX password:

  

Retype new UNIX password:

  

passwd: password updated successfully

``` 

  

## System or ApplicationAccounts

  

``` 

useradd –c "Apache Web Server

``` 

``` 

User" –d /opt/apache –r –s

  

/usr/sbin/nologin apache

``` 

``` 

tail -1 /etc/passwd

``` 

``` 

apache:x:999:999:Apache Web Server

  

User:/opt/apache:/usr/sbin/nologin

``` 

``` 

/etc/skel

  

  

● When using -m the home directory for the

  

account is created.

  

● The contents of /etc/skel are copied into the

  

home directory.

  

● /etc/skel typically contains shell configuration

  

files. (.profile, .bashrc,etc)

``` 

  

## More user adoptions II

  
``` 

-r Create a system account.

  

-d /home/dir Specify the home directory.

``` 

``` 

Use -u to specify the UID

useradd –c "MySQL Server" –d  

``` 

  

``` 

/opt/mysql -u 97 –s /usr/sbin/nologin

  

mysql

``` 

``` 

tail -1 /etc/passwd

  

  

mysql:x:97:1003:MySQL Server:

  

/opt/mysql:/usr/sbin/nologin  

``` 

  

## Delete user account:  

  
``` 

userdel [-r]username

  

ls /home

  

  

eharris grant

  

  

userdel eharris

  

  

ls /home

  

  

eharris grant

  

  

userdel -r grant

  

  

ls /home

  

  

eharris  

  

lslogins <user_account_name>  

``` 

``` 

$ id

uid=503(tclark) gid=506(authors)

groups=504(tclark),506(authors)  

``` 

## Logging into new group

  

``` 

$ id

uid=503(tclark) gid=504(tclark)

groups=504(tclark),506(authors) 

$ newgrp authors

  

``` 

``` 
$ id

uid=503(tclark) gid=506(authors)

groups=504(tclark),506(authors)  

``` 

RHCSA RHEL 8 Example

## Set PASS_MAX_DAYS to 30 in the /etc/login.defs file. Use administrative rights while opening the file with the text editor. You can use the vim /etc/login.defs command to perform this step.

Create the consultants group with a GID of 35000. 

``` bash 
[root@serverb ~]# groupadd -g 35000 consultants

```



## Configure administrative rights to enable all consultants group members to execute any command as any user.

Create the /etc/sudoers.d/consultants file and add the following content to
it. You can use the vim /etc/sudoers.d/consultants command to perform this
step.


``` bash 
%consultants ALL=(ALL) ALL

```



Create the consultant1, consultant2, and consultant3 users with the consultants
group as their secondary group.


``` bash 
[root@serverb ~]# useradd -G consultants consultant1
[root@serverb ~]# useradd -G consultants consultant2
[root@serverb ~]# useradd -G consultants consultant3

```

Set the consultant1, consultant2, and consultant3 passwords to redhat.


``` bash 
[root@serverb ~]# passwd consultant1
Changing password for user consultant1.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully.
[root@serverb ~]# passwd consultant2
Changing password for user consultant2.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully
[root@serverb ~]# passwd consultant3
Changing password for user consultant3.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully

```



## Set the consultant1, consultant2, and consultant3 accounts to expire in 90 days from the current day.

Determine the date 90 days in the future. This returned date is an example; the value
that you see, to use in the following step, is based on the current date and time in your
system.


``` bash 
[root@serverb ~]# date -d "+90 days" +%F
2022-06-08

```


## Set the account expiry date of the consultant1, consultant2, and consultant3 accounts to the same value as determined in the preceding step. For example:


``` bash 
[root@serverb ~]# chage -E 2022-06-08 consultant1
[root@serverb ~]# chage -E 2022-06-08 consultant2
[root@serverb ~]# chage -E 2022-06-08 consultant3

```



## Change the password policy for the consultant2 account to require a new password every 15 days.


``` bash 
[root@serverb ~]# chage -M 15 consultant2

```

Additionally, force the consultant1, consultant2, and consultant3 users to change
their passwords on the first login.

Set the last day of the password change to 0 so that users must change the password
when they first log in to the system.


``` bash 
[root@serverb ~]# chage -d 0 consultant1
[root@serverb ~]# chage -d 0 consultant2
[root@serverb ~]# chage -d 0 consultant3

```





[[Index]] 



