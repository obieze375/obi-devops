[[Index]] 

## Control the Boot Process

In this lab, you reset the root password on a system, recover from a misconfiguration, and
set the default boot target.
Outcomes
• Reset a lost password for the root user.
• Diagnose and fix boot issues.
• Set the default systemd target.

On the serverb machine, reset the password to redhat for the root user.
Locate the icon for the serverb machine console as appropriate for your classroom
environment, then open the console. 



Send a Ctrl+Alt+Del to your system by using the relevant button or menu entry.
When the boot-loader menu appears, press any key to interrupt the countdown, except
the Enter key. 

Use the cursor keys to highlight the rescue kernel boot-loader entry (the one with the
word rescue in its name). 

Press e to edit the current entry. 

Use the cursor keys to navigate the line that starts with the linux text. 

Press Ctrl+e to move the cursor to the end of the line. 

Append the rd.break text to the end of the line. 

Press Ctrl+x to boot using the modified configuration.

Press Enter to enter the maintenance mode. 

At the sh-5.1 prompt, remount the /sysroot file system as writable, and then use
the chroot command for the /sysroot directory.

```bash 

sh-5.1# mount -o remount,rw /sysroot
...output omitted...
sh-5.1# chroot /sysroot
```


## Set redhat as the password for the root user.

```bash 
sh-5.1# passwd root
Changing password for user root.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully.

```

## Configure the system to perform a full SELinux relabel after boot automatically. 

```bash 
sh-5.1# touch /.autorelabel

```

## Exit the chroot environment and the sh-5.1 prompt. After the file system is relabeled, the system prompts to enter maintenance mode, but if you wait, it completes the reboot and shows the boot-loader menu. 


## In the boot-loader menu, select the default kernel boot-loader entry. The system fails to boot because a start job does not complete successfully. Fix the issue from the console of the serverb machine.


## Boot the system into emergency mode. Reboot the serverb machine by sending a Ctrl+Alt+Del to your system by using the relevant button or menu entry.

## When the boot-loader menu appears, press any key to interrupt the countdown, except Enter. 

## Use the cursor keys to highlight the default boot-loader entry. 

## Press e to edit the current entry. 

## Use the cursor keys to navigate the line that starts with linux text. 

## Press Ctrl+e to move the cursor to the end of the line. 

## Append the systemd.unit=emergency.target text to the end of the line. 

## Press Ctrl+x to boot using the modified configuration. 

## Log in to emergency mode. 

## Give root password for maintenance 

```bash 
(or press Control-D to continue): redhat
[root@serverb ~]# 

```



## Remount the / file system as writable. 

```bash 
[root@serverb ~]# mount -o remount,rw /
...output omitted...
```

## Mount all file systems.

```bash 
[root@serverb ~]# mount -a
mount: /olddata: can't find UUID=4d5c85a5-8921-4a06-8aff-80567e9689bc.

```

## Edit the /etc/fstab file to remove or comment out the incorrect line mounting the /olddata mount point.

```bash 
[root@serverb ~]# vim /etc/fstab
...output omitted...
#UUID=4d5c85a5-8921-4a06-8aff-80567e9689bc /olddata xfs defaults 0 0
```


## Update the systemd daemon for the system to register the changes in the /etc/fstab file configuration.

```bash 
[root@serverb ~]# systemctl daemon-reload

```

## Verify that the /etc/fstab file configuration is correct by attempting to mount all entries.

```bash 

[root@serverb ~]# mount -a
```

## Reboot the system and wait for the boot to complete. The system should now boot normally.

```bash 
[root@serverb ~]# systemctl reboot
```


## Change the default systemd target on the serverb machine for the system to automatically start a graphical interface when it boots.No graphical interface is installed on the serverb machine. Only set the default target for this exercise and do not install the packages.


## Log in to the servera machine as the student user and switch to the root user.

```bash 
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```

## Set the graphical.target as the default target.

```bash 
[root@serverb ~]# systemctl set-default graphical.target
Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/
graphical.target. 

```


## Verify that the correct default is set.

```bash 

[root@serverb ~]# systemctl get-default
graphical.target

```


## Return to the workstation machine as the student user.

```bash 
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.

```

