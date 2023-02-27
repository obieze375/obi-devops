[[Index]] 

## In this lab, you configure several services to be enabled or disabled, running or stopped,based on a specification that is provided to you.


```bash 

[student@workstation ~]$ ssh student@serverb
...output omitted... 

```

```bash 
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]# 
```




## Verify the status of the psacct service. The psacct service is in an inactive and disabled state. The disabled state confirms that the service does not start at boot time. 


```bash 

[root@serverb ~]# systemctl status psacct
○ psacct.service - Kernel process accounting
 Loaded: loaded (/usr/lib/systemd/system/psacct.service; disabled; vendor
 preset: disabled)
 Active: inactive (dead) 

```


## Start the psacct service. 

```bash 

[root@serverb ~]# systemctl start psacct

```

```bash 
[root@serverb ~]# systemctl is-active psacct
active

```
## Configure the psacct service to start at system boot. 

```bash 

[root@serverb ~]# systemctl enable psacct

```





## Created symlink /etc/systemd/system/multi-user.target.wants/psacct.service → /usr/lib/systemd/system/psacct.service. 


Verify that the psacct service is enabled to start at system boot. 

```bash 
[root@serverb ~]# systemctl is-enabled psacct
enabled
```


## Stop the rsyslog service. 

Verify the status of the rsyslog service. Notice that the rsyslog service is running
and enabled to start at boot time. Press q to exit the command. 

```bash 
[root@serverb ~]# systemctl status rsyslog
● rsyslog.service - System Logging Service
 Loaded: loaded (/usr/lib/systemd/system/rsyslog.service; enabled; vendor
 preset: enabled)
 Active: active (running) since Thu 2022-05-19 04:04:43 EDT; 38min ago
 ...output omitted...
```


## Stop the rsyslog service. 

```bash 

[root@serverb ~]# systemctl stop rsyslog

```


Verify that the rsyslog service is stopped. 

```bash 
[root@serverb ~]# systemctl is-active rsyslog
inactive
```


## Configure the rsyslog service so that it does not start at system boot. 

Disable the rsyslog service so that it does not start at system boot. 

```bash 

[root@serverb ~]# systemctl disable rsyslog

```


## Removed /etc/systemd/system/multi-user.target.wants/rsyslog.service. 


## Verify that the rsyslog service does not start during the boot process.

```bash 

[root@serverb ~]# systemctl is-enabled rsyslog
disabled 

```



## Reboot the serverb machine before evaluating the lab.

```bash 

[root@serverb ~]# systemctl reboot
Connection to serverb closed by remote host.
Connection to serverb closed.
[student@workstation ~]$


```




