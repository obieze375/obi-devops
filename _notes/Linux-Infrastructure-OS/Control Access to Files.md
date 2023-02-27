[[Index]] 

# In this lab, you configure permissions on files and set up a directory that users in a particular group can use to conveniently share files on the local file system.



Create a /home/techdocs directory. 

Use the mkdir command to create a /home/techdocs directory.

```bash 
[root@serverb ~]# mkdir /home/techdocs

```





## Change the group ownership of the /home/techdocs directory to the techdocs group.

Use the chown command to change the group ownership for the /home/techdocs
directory to the techdocs group. 

```bash 

[root@serverb ~]# chown :techdocs /home/techdocs

```



## Verify that users in the techdocs group cannot create files in the /home/techdocs directory 

```bash 
Use the su command to switch to the tech1 user.
[root@serverb ~]# su - tech1
[tech1@serverb ~]$


```



## Create a techdoc1.txt file in the /home/techdocs directory. This step should fail.
Although the /home/techdocs directory is owned by the techdocs group and
tech1 is part of the techdocs group, you cannot create a file in that directory. The
reason is because the techdocs group does not have write permission. 

```bash 
[tech1@serverb ~]$ touch /home/techdocs/techdoc1.txt
touch: cannot touch '/home/techdocs/techdoc1.txt': Permission denied 


```



## List the directory's permissions. 

```bash 
[tech1@serverb ~]$ ls -ld /home/techdocs/
 drwxr-xr-x. 2 root techdocs 6 Feb 5 16:05 /home/techdocs/ 


```


 
## Set permissions on the /home/techdocs directory. On the /home/techdocs directory, configure setgid (2); read, write, and execute permissions (7) for the owner/user and group; and no permissions (0) for other users.
 
## Exit from the tech1 user shell.

```bash 
[tech1@serverb ~]$ exit
logout
[root@serverb ~]#


```

## Set the group permission for the /home/techdocs directory. Configure setgid;read, write, and execute permissions for the owner and group; and no permissions for others. 

```bash 

[root@serverb ~]# chmod 2770 /home/techdocs

```


## Verify that the permissions are set properly.

```bash 
[root@serverb ~]# ls -ld /home/techdocs
drwxrws---. 2 root techdocs 6 Feb 4 18:12 /home/techdocs/
The techdocs group now has write permission. 

```



## Confirm that users in the techdocs group can now create and edit files in the /home/techdocs directory. Users that are not in the techdocs group cannot edit or create files in the /home/techdocs directory. The tech1 and tech2 users are in the techdocs group. The database1 user is not in that group. 

## Switch to the tech1 user. Create a techdoc1.txt file in the /home/techdocs directory. Exit from the tech1 user shell.

```bash 

[root@serverb ~]# su - tech1
[tech1@serverb ~]$ touch /home/techdocs/techdoc1.txt
[tech1@serverb ~]$ ls -l /home/techdocs/techdoc1.txt
-rw-rw-r--. 1 tech1 techdocs 0 Feb 5 16:42 /home/techdocs/techdoc1.txt
[tech1@serverb ~]$ exit
logout
[root@serverb ~]#
```
 



## Switch to the tech2 user. Add some content to the /home/techdocs/techdoc1.txt file. Exit from the tech2 user shell. 

```bash 
[root@serverb ~]# su - tech2
[tech2@serverb ~]$ cd /home/techdocs
[tech2@serverb techdocs]$ echo "This is the first tech doc." > techdoc1.txt
[tech2@serverb techdocs]$ exit
logout
[root@serverb ~]#
```

Switch to the database1 user. Append some content to the /home/techdocs/
techdoc1.txt file. You get a Permission Denied message. Verify that
database1 does not have access to the file. Exit from the database1 user shell.
Enter the following long echo command on a single line: 

```bash 
[root@serverb ~]# su - database1
[database1@serverb ~]$ echo "This is the first tech doc." >> \
/home/techdocs/techdoc1.txt
-bash: /home/techdocs/techdoc1.txt: Permission denied
[database1@serverb ~]$ ls -l /home/techdocs/techdoc1.txt
ls: cannot access '/home/techdocs/techdoc1.txt': Permission denied
[database1@serverb ~]$ exit
logout
[root@serverb ~]#
```


## Modify the global login scripts. Normal users should have a umask setting that allows the user and group to create, write and execute files and directories, while preventing other users from viewing, modifying, or executing new files and directories. 

Determine the umask of the student user. Switch to the student login shell. When
done, exit from the shell.

```bash 
[root@serverb ~]# su - student
[student@serverb ~]$ umask
0002
[student@serverb ~]$ exit
logout
[root@serverb ~]#


```


## Edit the /etc/profile file and set the following umask properties. The /etc/profile file already contains a umask definition. Search the file and update with the appropriate values.

Set a umask of 007 for users with a UID greater than 199 and with a matching
username and primary group name. Set a umask of 022 for everyone else.
The following example shows the expected added content in the /etc/profile file. 

```bash 
[root@serverb ~]# cat /etc/profile
...output omitted...


```


```bash 

# Overrides default umask configuration
if [ $UID -gt 199 ] && [ "`id -gn`" = "`id -un`" ]; then
 umask 007
else
 umask 022
fi
...output omitted...

```



## As the student user, verify that the global umask changes to 007.

```bash 
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ umask
0007



```


```bash 



```