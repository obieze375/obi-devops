[[Index]] 

In this lab, you apply a specific tuning profile and adjust the scheduling priority of an existing
process with high CPU usage. 

Outcomes
• Activate a specific tuning profile for a computer system.
• Adjust the CPU scheduling priority of a process.

``` bash

```

``` bash
[student@serverb ~]$ dnf list tuned
...output omitted...
```





## Installed Packages
tuned.noarch 2.18.0-1.el9 @System
1.3. Verify the tuned service state.
RH134-RHEL9.0-en-3-20220914 89

``` bash
[student@serverb ~]$ systemctl is-active tuned
active
```


## List all available tuning profiles and their descriptions. Note that the current active profile is virtual-guest. 

``` bash
[student@serverb ~]$ sudo tuned-adm list
[sudo] password for student: student
Available profiles:
- accelerator-performance - Throughput performance based tuning with disabled
 higher latency STOP states
- balanced - General non-specialized tuned profile
- desktop - Optimize for the desktop use-case
- hpc-compute - Optimize for HPC compute workloads
- intel-sst - Configure for Intel Speed Select Base Frequency
- latency-performance - Optimize for deterministic performance at the cost
 of increased power consumption
- network-latency - Optimize for deterministic performance at the cost
 of increased power consumption, focused on low
 latency network performance
- network-throughput - Optimize for streaming network throughput, generally
 only necessary on older CPUs or 40G+ networks
- optimize-serial-console - Optimize for serial console use.
- powersave - Optimize for low power consumption
- throughput-performance - Broadly applicable tuning that provides excellent
 performance across a variety of common server
 workloads
- virtual-guest - Optimize for running inside a virtual guest
- virtual-host - Optimize for running KVM guests
Current active profile: virtual-guest
```

## Change the current active tuning profile to the balanced profile. 

``` bash
[student@serverb ~]$ sudo tuned-adm profile balanced
```


## List summary information of the current active tuned profile. Verify that the active profile is the balanced profile.

``` bash
[student@serverb ~]$ sudo tuned-adm profile_info
Profile name:
balanced
Profile summary:
General non-specialized tuned profile
...output omitted...
```

## Two processes on serverb are consuming a high percentage of CPU usage. Adjust each process’s nice level to 10 to allow more CPU time for other processes. 
## Determine the top two CPU consumers on the serverb machine. The ps command lists the top CPU consumers at the bottom of the output. CPU percentage values might vary on your machine.

``` bash
[student@serverb ~]$ ps aux --sort=pcpu
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
...output omitted...
root 1079 98.5 0.1 225340 2300 ? RN 06:25 4:29 sha1sum /dev/zero
root 1095 99.0 0.1 225340 2232 ? R< 06:25 4:30 md5sum /dev/zero
```




## Identify the current nice level for each of the top two CPU consumers. 

``` bash
[student@serverb ~]$ ps -o pid,pcpu,nice,comm \
$(pgrep sha1sum;pgrep md5sum)
 PID %CPU NI COMMAND
 1079 98.8 2 sha1sum
 1095 99.1 -2 md5sum
```


## Adjust the nice level for each process to 10. Use the correct PID values for your processes from the previous command output.

``` bash
[student@serverb ~]$ sudo renice -n 10 1079 1095
[sudo] password for student: student
1079 (process ID) old priority 2, new priority 10
1095 (process ID) old priority -2, new priority 10
2.4. Verify that the current nice level for each process is 10.
[student@serverb ~]$ ps -o pid,pcpu,nice,comm \
$(pgrep sha1sum;pgrep md5sum)
 PID %CPU NI COMMAND
 1079 98.9 10 sha1sum
 1095 99.2 10 md5sum
```

## Return to the workstation machine as the student user. 

``` bash
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

