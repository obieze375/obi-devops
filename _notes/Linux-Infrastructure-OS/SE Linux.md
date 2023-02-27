[[Index]] 

### CONCEPTS

**SELinux = LABELING** system
Every process, file, directory, system object has a LABEL.
Policy rules control access between labeled processes and labeled
objects.
The kernel enforces these rules.

```
Labeling → files, process, ports, etc. (system objects)
Type enforcement → Isolates processes from each other based on types
```
### LABELING

Label format:
user:role:type:level (optional)

user → identity known to the policy authorized for a specific set of roles and a specific MLS/MCS range
role → attribute of RBAC, serves as an intermediary between domains and SELinux users
type → attribute of type enforcement, defines a domain for processes and a type for files
level → attribute of MLS/MCS, pair of levels, written as lowlevel-highlevel if the levels differ, or lowlevel if the
levels are identical
**TYPE ENFORCEMENT**
Targeted: Processes that are targeted run in a confined domain, and processes that are not targeted run in an unconfined domain
Multi-level security (mls): Control processes (domains) based on the level of the data they will be using
Multi-category security (mcs): Protects like processes from each other (like VMs, OpenShift Gears, SELinux sandboxes, containers, etc.)
**SELINUX MODES @ BOOT**
Kernel parameters:
**enforcing= 0** → boot in permissive mode
**selinux= 0** → kernel to not load any part of the SELinux infrastructure
**autorelabel= 1** → forces the system to relabel

If you need to relabel the entire system:
**# touch /.autorelabel
# reboot**
If the system labeling contains a large amount of errors, you might need to boot in permissive mode for the
autorelabel to succeed.
**SELINUX STATES CHECK STATUS:
enforcing** SELinux security policy is enforced Configuration file: Check if SELinux is enabled: **# getenforce
permissive** SELinux prints warnings instead of
enforcing **/etc/selinux/config**

```
SELinux status tool: # sestatus
```
**disabled** No SELinux policy is loaded Enable/disable SELinux (temporarily): **# setenforce [1|0]**

## opensource.com

## Learn more in our sysadmin’s guide to SELinux, by Alex Callejas: https://red.ht/2zpWppY

## Twitter^ @ opensourceway | facebook.com/opensourceway | CC BY-SA 4.


### EXAMPLE OF LABELING: APACHE WEB SERVER CHECK/CREATE/MODIFY SELINUX CONTEXTS/LABELS:

Binary **/usr/sbin/httpd httpd_exec_t** Many commands accept the argument -Z to view, create, and modify context:

- ls -Z
- id -Z
- ps -Z
- netstat -Z
- cp -Z
- mkdir -Z
Contexts are set when files are created based on their parent directory’s
context (with a few exceptions). RPMs can set contexts as part of
installation.

Configuration directory **/etc/httpd httpd_config_t**

Logfile directory **/var/log/httpd httpd_log_t**

Content directory **/var/www/html httpd_sys_content_t**

Startup script **/usr/lib/systemd/system/httpd.service httpd_unit_file_d**
Process running **/usr/sbin/httpd -DFOREGROUND httpd_t**
Ports (netstat -tulpnZ) 80/tcp, 443/tcp **httpd_t**

Port type (semanage port -l) 80, 81, 443, 488, 8008, 8009, 8443, 9000 **http_port_t
TROUBLESHOOTING**
SELinux tools: **# yum -y install setroubleshoot setroubleshoot-server** ← Reboot or restart auditd after you install
Logging: **/var/log/messages /var/log/audit/audit.log /var/lib/setroubleshoot/setroubleshoot_database.xml
journalctl** List all logs related to setroubleshoot: **# journalctl -t setroubleshoot --since=14:**
List all logs related to a particular
SELinux label:

```
# journalctl _SELINUX_CONTEXT=system_u:system_r:policykit_t:s
```
**ausearch** Look for SELinux errors in the audit log: **# ausearch -m AVC,USER_AVC,SELINUX_ERR -ts today -i**
Search for SELinux AVC messages for a
particular service:

```
# ausearch -m avc -c httpd -i
```
Edit/modify labels
(semanage)

know the label: **# semanage fcontext -a -t httpd_sys_content_t ‘/srv/myweb(/.*)?’**
know the file with the equivalent labeling: **# semanage fcontext -a -e /srv/myweb /var/www**
Restore the context (for both cases): **# restorecon -vR /srv/myweb**
Edit/modify labels (chcon) know the label: **# chcon -t httpd_system_content_t /var/www/html/index.html** Note: If you move instead of copy
a file, the file keeps its original
context.

know the file with the equivalent labeling: **# chcon --reference /var/www/html/ /var/www/html/index.html**
Restore the context (for both cases): **# restorecon -vR /var/www/html/index.html**
Add new port to service: **# semanage port -a -t http_port_t -p tcp 8585** ← SELinux needs to know
**Booleans** Booleans allow parts of SELinux policy to be changed at runtime without any knowledge of SELinux policy writing.
To see all booleans: **# getsebool -a** To see the description of each one: **# semanage boolean -l**
To set a boolean execute: **# setsebool [boolean] [1|0]** To configure it permanently, add -P: Example : # setseebol httpd_enable_ftp_server 1 -P

## Opensource.com : SELinux Commands Cheat Sheet PAGE 2 OF 2 BY ALEX CALLEJAS






**Manage SELinux Security**





```
Goal Protect and manage server security by using
SELinux.
```
```
Objectives • Explain how SELinux protects resources,
change the current SELinux mode of a system,
and set the default SELinux mode of a system.
```
- Manage the SELinux policy rules that
    determine the default context for files and
    directories with the semanage fcontext
    command and apply the context defined by the
    SELinux policy to files and directories with the
    restorecon command.
- Activate and deactivate SELinux policy rules
    with the setsebool command, manage the
    persistent value of SELinux Booleans with
    the semanage boolean -l command, and
    consult man pages that end with _selinux
    to find useful information about SELinux
    Booleans.
- Use SELinux log analysis tools and
    display useful information during SELinux
    troubleshooting with the sealert command.

```
Sections • Change the SELinux Enforcement Mode (and
Guided Exercise)
```
- Control SELinux File Contexts (and Guided
    Exercise)
- Adjust SELinux Policy with Booleans (and
    Guided Exercise)
- Investigate and Resolve SELinux Issues (and
    Guided Exercise)

**Lab** Manage SELinux Security



### Change the SELinux Enforcement Mode

**Objectives**

Explain how SELinux protects resources, change the current SELinux mode of a system, and set
the default SELinux mode of a system.

**Describe SELinux Architecture**

_Security Enhanced Linux (SELinux)_ is a critical security feature of Linux. Access to files, ports,
and other resources is controlled at a granular level. Processes are permitted to access only the
resources that their SELinux policy or Boolean settings specify.

File permissions control file access for a specific user or group. However, file permissions do not
prevent an authorized user with file access from using a file for an unintended purpose.

For example, with write access to a file, other editors or programs can still open and modify a
structured data file that is designed for only a specific program to write to, which could result in
corruption or a data security issue. File permissions do not stop such undesired access because
they do not control _how_ a file is used but only _who_ is allowed to read, write, or run a file.

SELinux consists of application-specific policies that the application’s developers define to
declare precisely what actions and accesses are allowed for each binary executable, configuration
file, and data file that the application uses. This policy is known as a _targeted policy_ , because one
policy defines an application’s activities. Policies declare the predefined labels that are configured
on individual programs, files, and network ports.

**SELinux Usage**

SELinux enforces a set of access rules that explicitly define allowed actions between processes
and resources. Any action that is not defined in an access rule is not allowed. Because only defined
actions are allowed, applications with a poor security design are still protected from malicious use.
Applications or services with a targeted policy run in a _confined_ domain, whereas an application
without a policy runs _unconfined_ but without any SELinux protection. Individual targeted policies
can be disabled to assist with application and security policy development and debugging.

SELinux has the following operational modes:

- **Enforcing** : SELinux enforces the loaded policies. This mode is the default in Red Hat
    Enterprise Linux.
- **Permissive** : SELinux loads the policies and is active, but instead of enforcing access control
    rules, it logs access violations. This mode is helpful for testing and troubleshooting applications
    and rules.
- **Disabled** : SELinux is turned off. SELinux violations are not denied or logged. Disabling SELinux
    is strongly discouraged.



```
Important
Starting in Red Hat Enterprise Linux 9, SELinux can be fully disabled only by using
the selinux=0 kernel parameter at boot. RHEL no longer supports setting the
SELINUX=disabled option in the /etc/selinux/config file.
```
```
Starting in RHEL 9, disabling SELinux in the /etc/selinux/config file results
in SELinux starting and performing active enforcement, but without loading any
policies. Because policy rules define allowed actions, if no policies are loaded then
all actions are denied. This behavior is intentional, and is designed to block malicious
attempts to circumvent SELinux protection.
```
**Basic SELinux Concepts**

The primary goal of SELinux is to protect user data from improper use by compromised
applications or system services. Most Linux administrators are familiar with the standard user,
group, and world file permission security model, which is known as _Discretionary Access Control
(DAC)_ because administrators set file permissions as they need. SELinux provides an additional
layer of object-based security, which is defined in granular rules, which are known as _Mandatory
Access Control (MAC)_ because MAC policies apply to all users and cannot be bypassed for specific
users by discretionary configuration settings.

For example, a web server’s open firewall port allows remote anonymous access to a web client.
However, a malicious user that accesses that port might try to compromise a system through an
existing vulnerability. If an example vulnerability compromises the permissions for the apache
user and group, then a malicious user might directly access the /var/www/html document root
content, or the system’s /tmp and /var/tmp directories, or other accessible files and directories.

SELinux policies are security rules that define how specific processes access relevant files,
directories, and ports. Every resource entity, such as a file, process, directory, or port, has a label
called an _SELinux context_. The context label matches a defined SELinux policy rule to allow a
process to access the labeled resource. By default, an SELinux policy does not allow any access
unless an explicit rule grants access. When no allow rule is defined, all access is disallowed.

SELinux labels have user, role, type, and security level fields. Targeted policy, which is
enabled in RHEL by default, defines rules by using the type context. Type context names typically
end with _t.

```
Figure 4.1: SELinux file context
```
**Policy Access Rule Concepts**

For example, a web server process is labeled with the httpd_t type context. Web server files
and directories in the /var/www/html/ directory and other locations are labeled with the
httpd_sys_content_t type context. Temporary files in the /tmp and /var/tmp directories
have the tmp_t type contexts as a label. The web server’s ports have the http_port_t type
context as a label.

An Apache web server process runs with the httpd_t type context. A policy rule
permits the Apache server to access files and directories that are labeled with the
httpd_sys_content_t type context. By default, files in the /var/www/html directory have



the httpd_sys_content_t type context. A web server policy has by default no allow rules for
using files that are labeled tmp_t, such as in the /tmp and /var/tmp directories, thus disallowing
access. With SELinux enabled, a malicious user who uses a compromised Apache process would
still not have access to the /tmp directory files.

A MariaDB server process runs with the mysqld_t type context. By default, files in the
/data/mysql directory have the mysqld_db_t type context. A MariaDB server can access the
mysqld_db_t labeled files, but has no rules to allow access to files for other services, such as
httpd_sys_content_t labeled files.

```
Figure 4.2: SELinux decision-making flow
```
Many commands that list resources use the -Z option to manage SELinux contexts. For example,
the ps, ls, cp, and mkdir commands all use the -Z option.

```
[root@host ~]# ps axZ
LABEL PID TTY STAT TIME COMMAND
system_u:system_r:kernel_t:s0 2? S 0:00 [kthreadd]
system_u:system_r:kernel_t:s0 3? I< 0:00 [rcu_gp]
system_u:system_r:kernel_t:s0 4? I< 0:00 [rcu_par_gp]
...output omitted...
[root@host ~]# systemctl start httpd
[root@host ~]# ps -ZC httpd
LABEL PID TTY TIME CMD
system_u:system_r:httpd_t:s0 1550? 00:00:00 httpd
system_u:system_r:httpd_t:s0 1551? 00:00:00 httpd
system_u:system_r:httpd_t:s0 1552? 00:00:00 httpd
system_u:system_r:httpd_t:s0 1553? 00:00:00 httpd
system_u:system_r:httpd_t:s0 1554? 00:00:00 httpd
```


```
[root@host ~]# ls -Z /var/www
system_u:object_r:httpd_sys_script_exec_t:s0 cgi-bin
system_u:object_r:httpd_sys_content_t:s0 html
```
**Change the SELinux Mode**

Use the getenforce command to view the current SELinux mode. Use the setenforce
command to change the SELinux mode.

```
[root@host ~]# getenforce
Enforcing
[root@host ~]# setenforce
usage: setenforce [ Enforcing | Permissive | 1 | 0 ]
[root@host ~]# setenforce 0
[root@host ~]# getenforce
Permissive
[root@host ~]# setenforce Enforcing
[root@host ~]# getenforce
Enforcing
```
Alternatively, set the SELinux mode at boot time with a kernel parameter. Pass the enforcing=0
kernel parameter to boot the system into permissive mode, or pass enforcing=1 to boot
into enforcing mode. Disable SELinux by passing the selinux=0 kernel parameter, or pass
selinux=1 to enable SELinux.

Red Hat recommends to reboot the server when you change the SELinux mode from
Permissive to Enforcing. This reboot ensures that the services started in permissive mode are
confined in the next boot.

**Set the Default SELinux Mode**

To configure SELinux persistently, use the /etc/selinux/config file. In the following default
example, the configuration sets SELinux to enforcing mode. The comments list other valid
values, such as the permissive and disabled modes.

```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
# enforcing - SELinux security policy is enforced.
# permissive - SELinux prints warnings instead of enforcing.
# disabled - No SELinux policy is loaded.
...output omitted...
#
# NOTE: In earlier Fedora kernel builds, SELINUX=disabled would also
# fully disable SELinux during boot. If you need a system with SELinux
# fully disabled instead of SELinux running with no policy loaded, you
# need to pass selinux=0 to the kernel command line. You can use grubby
# to persistently set the bootloader to boot with selinux=0:
#
# grubby --update-kernel ALL --args selinux=0
#
# To revert back to SELinux enabled:
#
# grubby --update-kernel ALL --remove-args selinux
#
```


```
SELINUX=enforcing
# SELINUXTYPE= can take one of these three values:
# targeted - Targeted processes are protected,
# minimum - Modification of targeted policy. Only selected processes are
protected.
# mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
The system reads this file at boot time and starts SELinux accordingly. The selinux=0|1 and
enforcing=0|1 kernel arguments override this configuration.

```
References
getenforce(8), setenforce(8), and selinux_config(5) man pages
```


**Guided Exercise**

### Change the SELinux Enforcement Mode

```
In this lab, you manage SELinux modes, both temporarily and persistently.
```
**Outcomes**

- View and set the current SELinux mode.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-opsmode
```
**Instructions**

**1.** On the workstation machine, use the ssh command to log in to the servera machine
    as the student user and then switch to the root user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$ sudo -i
[sudo] password for student: student
[root@servera ~]#
```
**2.** Change the default SELinux mode to permissive.

```
2.1. Use the getenforce command to verify the current SELinux mode on the servera
machine.
```
```
[root@servera ~]# getenforce
Enforcing
```
```
2.2. Use the vim /etc/selinux/config command to edit the configuration file.
Change the SELINUX parameter from enforcing to permissive mode.
```
```
[root@servera ~]# vim /etc/selinux/config
```
```
2.3. Use the grep command to confirm that the SELINUX parameter displays the
permissive mode.
```


```
[root@servera ~]# grep '^SELINUX' /etc/selinux/config
SELINUX= permissive
SELINUXTYPE=targeted
```
```
2.4. Use the getenforce command to confirm that the SELINUX parameter displays the
enforcing mode.
```
```
[root@servera ~]# getenforce
Enforcing
```
```
2.5. Use the setenforce command to change the SELINUX mode to permissive
mode and verify the change.
```
```
[root@servera ~]# setenforce 0
[root@servera ~]# getenforce
Permissive
```
**3.** Change the default SELinux mode back to the enforcing mode in the configuration file.

```
3.1. Use the vim /etc/selinux/config command to edit the configuration file.
Change the SELINUX parameter from permissive to enforcing mode.
```
```
[root@servera ~]# vim /etc/selinux/config
```
```
3.2. Use the grep command to confirm that the SELINUX parameter sets the
enforcing mode on booting.
```
```
[root@servera ~]# grep '^SELINUX' /etc/selinux/config
SELINUX= enforcing
SELINUXTYPE=targeted
```
**4.** Set the SELinux mode to enforcing in the command line. Reboot the servera machine
    and verify the SELinux mode.

```
4.1. Use the setenforce command to set the current SELinux mode to the enforcing
mode. Use the getenforce command to confirm that SELinux is set to the
enforcing mode.
```
```
[root@servera ~]# setenforce 1
[root@servera ~]# getenforce
Enforcing
```
```
4.2. Reboot the servera machine to implement the persistent configuration.
```
```
[root@servera ~]# systemctl reboot
Connection to servera closed by remote host.
Connection to servera closed.
[student@workstation ~]$
```
```
4.3. Log in to servera machine and verify the SELinux mode.
```


```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$ sudo -i
[sudo] password for student: student
[root@servera ~]# getenforce
Enforcing
```
**5.** Return to the workstation machine as the student user.

```
[root@servera ~]# exit
logout
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish selinux-opsmode
```
This concludes the section.



### Control SELinux File Contexts

**Objectives**

Manage the SELinux policy rules that determine the default context for files and directories with
the semanage fcontext command and apply the context defined by the SELinux policy to files
and directories with the restorecon command.

**Initial SELinux Context**

All resources, such as processes, files, and ports, are labeled with an SELinux _context_. SELinux
maintains a file-based database of file labeling policies in the /etc/selinux/targeted/
contexts/files/ directory. New files obtain a default label when their file name matches an
existing labeling policy.

When a new file’s name does not match an existing labeling policy, the file inherits the same
label as the parent directory. With labeling inheritance, all files are always labeled when created,
regardless of whether an explicit policy exists for a file.

When files are created in default locations that have an existing labeling policy, or when a policy
exists for a custom location, then new files are labeled with a correct SELinux context. However,
if a file is created in an unexpected location without an existing labeling policy, then the inherited
label might not be correct for the new file’s intended purpose.

Furthermore, copying a file to a new location can cause that file’s SELinux context to change,
with the new context determined by the new location’s labeling policy, or from parent directory
inheritance if no policy exists. A file’s SELinux context can be preserved during a copy to retain the
context label that was determined for the file’s original location. For example, the cp -p command
preserves all file attributes where possible, and the cp -c command preserves only SELinux
contexts, during a copy.

```
Note
Copying a file always creates a new file inode, and that inode’s attributes, including
the SELinux context, must be initially set, as previously discussed.
```
```
However, moving a file does not typically create a new inode if the move occurs
within the same file system, but instead moves the existing inode’s file name to a
new location. Because the existing inode’s attributes do not need to be initialized,
a file that is moved with mv preserves its SELinux context unless you set a new
context on the file with the -Z option.
```
```
After you copy or move a file, verify that it has the appropriate SELinux context and
set it correctly if necessary.
```
The following example demonstrates how this process works.

Create two files in the /tmp directory. Both files receive the user_tmp_t context type.

Move the first file, and copy the second file, to the /var/www/html directory.



- The moved file retains the file context that was labeled from the original /tmp directory.
- The copied file has a new inode and inherits the SELinux context from the destination /var/
    www/html directory.

The ls -Z command displays the SELinux context of a file. Observe the label of the files that are
created in the /tmp directory.

```
[root@host ~]# touch /tmp/file1 /tmp/file2
[root@host ~]# ls -Z /tmp/file*
unconfined_u:object_r: user_tmp_t :s0 /tmp/file1
unconfined_u:object_r: user_tmp_t :s0 /tmp/file2
```
The ls -Zd command displays the SELinux context of the specified directory. Note the label on
the /var/www/html directory and the files inside it.

```
[root@host ~]# ls -Zd /var/www/html/
system_u:object_r: httpd_sys_content_t :s0 /var/www/html/
[root@host ~]# ls -Z /var/www/html/index.html
unconfined_u:object_r: httpd_sys_content_t :s0 /var/www/html/index.html
```
Move one file from the /tmp directory to the /var/www/html directory. Copy the other file to
the same directory. Note the resulting label on each file.

```
[root@host ~]# mv /tmp/file1 /var/www/html/
[root@host ~]# cp /tmp/file2 /var/www/html/
[root@host ~]# ls -Z /var/www/html/file*
unconfined_u:object_r: user_tmp_t :s0 /var/www/html/file1
unconfined_u:object_r: httpd_sys_content_t :s0 /var/www/html/file2
```
The moved file retained its original label and the copied file inherited the destination directory
label. Although not important to this discussion, unconfined_u is the SELinux user, object_r
is the SELinux role, and s0 is the (lowest possible) sensitivity level. Advanced SELinux
configurations and features use these values.

**Change the SELinux Context**

You change the SELinux context on files with the semanage fcontext, restorecon, and
chcon commands.

The recommended method to set the context for a file is to create a file context policy by using
the semanage fcontext command, and then apply the specified context in the policy to the file
by using the restorecon command. This method ensures that you can easily relabel the file to
its correct context with the restorecon command whenever necessary. The advantage of this
method is that you do not need to remember what the context is supposed to be, and you can
easily correct the context on a set of files.

The chcon command sets SELinux context directly on files, but without referencing the system’s
SELinux policy. Although chcon is useful for testing and debugging, setting contexts manually
with this method is temporary. File contexts that are set manually survive a reboot, but might be
replaced if you run restorecon to relabel the contents of the file system.



```
Important
When an SELinux system relabel occurs, all files on a system are labeled with their
policy defaults. When you use restorecon on a file, any context that you set
manually on the file is replaced if it does not match the rules in the SELinux policy.
```
The following example creates a directory with a default_t SELinux context, which it inherited
from the / parent directory.

```
[root@host ~]# mkdir /virtual
[root@host ~]# ls -Zd /virtual
unconfined_u:object_r: default_t :s0 /virtual
```
The chcon command sets the file context of the /virtual directory to the
httpd_sys_content_t type.

```
[root@host ~]# chcon -t httpd_sys_content_t /virtual
[root@host ~]# ls -Zd /virtual
unconfined_u:object_r: httpd_sys_content_t :s0 /virtual
```
Running the restorecon command resets the context to the default value of default_t. Note
the Relabeled message.

```
[root@host ~]# restorecon -v /virtual
Relabeled /virtual from unconfined_u:object_r: httpd_sys_content_t :s0 to
unconfined_u:object_r: default_t :s0
[root@host ~]# ls -Zd /virtual
unconfined_u:object_r: default_t :s0 /virtual
```
**Define SELinux Default File Context Policies**

The semanage fcontext command displays and modifies the policies that determine the
default file contexts. You can list all the file context policy rules by running the semanage
fcontext -l command. These rules use extended regular expression syntax to specify the path
and file names.

When viewing policies, the most common extended regular expression is (/.*)?, which is usually
appended to a directory name. This notation is humorously called _the pirate_ , because it looks like a
face with an eye patch and a hooked hand next to it.

This syntax is described as "a set of characters beginning with a slash and followed by any number
of characters, where the set can either exist or not exist". Stated more simply, this syntax matches
the directory itself, even when empty, but also matches almost any file name that is created within
that directory.

For example, the following rule specifies that the /var/www/cgi-bin directory, and
any files in it or in its subdirectories (and their subdirectories, and so on), should have the
system_u:object_r:httpd_sys_script_exec_t:s0 SELinux context, unless a more
specific rule overrides this one.

```
/var/www/cgi-bin(/.*)? all files system_u:object_r:httpd_sys_script_exec_t:s0
```


**Basic File Context Operations**

The following table is a reference for the semanage fcontext command options to add,
remove, or list SELinux file context policies.

**semanage fcontext commands**

```
option description
```
```
-a, --add Add a record of the specified object type
```
```
-d, --delete Delete a record of the specified object type
```
```
-l, --list List records of the specified object type
```
To manage SELinux contexts, install the policycoreutils and policycoreutils-python-
utils packages, which contain the restorecon and semanage commands.

To reset all files in a directory to the default policy context, first use the semanage fcontext
-l command to locate and verify that the correct policy exists with the intended file context.
Then, use the restorecon command on the wildcarded directory name to reset all the files
recursively. In the following example, view the file contexts before and after using the semanage
and restorecon commands.

First, check the SELinux context for the files:

```
[root@host ~]# ls -Z /var/www/html/file*
unconfined_u:object_r: user_tmp_t :s0 /var/www/html/file1
unconfined_u:object_r: httpd_sys_content_t :s0 /var/www/html/file2
```
Then, use the semanage fcontext -l command to list the default SELinux file contexts:

```
[root@host ~]# semanage fcontext -l
...output omitted...
/var/www(/.*)? all files system_u:object_r:httpd_sys_content_t:s0
...output omitted...
```
The semanage command output indicates that all the files and subdirectories in the /var/
www/ directory will have the httpd_sys_content_t context by default. Running restorecon
command on the wildcarded folder restores the default context on all files and subdirectories.

```
[root@host ~]# restorecon -Rv /var/www/
Relabeled /var/www/html/ file1 from unconfined_u:object_r: user_tmp_t :s0 to
unconfined_u:object_r: httpd_sys_content_t :s0
[root@host ~]# ls -Z /var/www/html/file*
unconfined_u:object_r: httpd_sys_content_t :s0 /var/www/html/ file1
unconfined_u:object_r: httpd_sys_content_t :s0 /var/www/html/ file2
```
The following example uses the semanage command to add a context policy for a new directory.
First, create the /virtual directory with an index.html file inside it. View the SELinux context
for the file and the directory.



```
[root@host ~]# mkdir /virtual
[root@host ~]# touch /virtual/index.html
[root@host ~]# ls -Zd /virtual/
unconfined_u:object_r: default_t :s0 /virtual
[root@host ~]# ls -Z /virtual/
unconfined_u:object_r: default_t :s0 index.html
```
Next, use the semanage fcontext command to add an SELinux file context policy for the
directory.

```
[root@host ~]# semanage fcontext -a -t httpd_sys_content_t '/virtual(/.*)?'
```
Use the restorecon command on the wildcarded directory to set the default context on the
directory and all files within it.

```
[root@host ~]# restorecon -RFvv /virtual
Relabeled /virtual from unconfined_u:object_r:default_t:s0 to
system_u:object_r:httpd_sys_content_t:s0
Relabeled /virtual/index.html from unconfined_u:object_r:default_t:s0 to
system_u:object_r:httpd_sys_content_t:s0
[root@host ~]# ls -Zd /virtual/
drwxr-xr-x. root root system_u:object_r: httpd_sys_content_t :s0 /virtual/
[root@host ~]# ls -Z /virtual/
-rw-r--r--. root root system_u:object_r: httpd_sys_content_t :s0 index.html
```
Use the semanage fcontext -l -C command to view any local customizations to the default
policy.

```
[root@host ~]# semanage fcontext -l -C
SELinux fcontext type Context
```
```
/virtual(/.*)? all files system_u:object_r:httpd_sys_content_t:s0
```
```
References
chcon(1), restorecon(8), semanage(8), and semanage-fcontext(8) man
pages
```


**Guided Exercise**

### Control SELinux File Contexts

```
In this lab, you persistently change the SELinux context of a directory and its contents.
```
**Outcomes**

- Configure the Apache HTTP server to publish web content from a non-standard
    document root.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-filecontexts
```
**Instructions**

**1.** Log in to servera as the student user and switch to the root user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
[student@servera ~]$ sudo -i
[sudo] password for student: student
[root@servera ~]#
```
**2.** Configure Apache to use a document directory in a non-standard location.

```
2.1. Create the /custom directory.
```
```
[root@servera ~]# mkdir /custom
```
```
2.2. Create the index.html file in the /custom directory. The index.html file should
contain the This is SERVERA. text.
```
```
[root@servera ~]# echo 'This is SERVERA.' > /custom/index.html
```
```
2.3. Configure Apache to use the new directory location. Edit the Apache
/etc/httpd/conf/httpd.conf configuration file and replace the two
occurrences of the /var/www/html directory with the /custom directory. You can
use the vim /etc/httpd/conf/httpd.conf command to do so. The following
example shows the expected content of the /etc/httpd/conf/httpd.conf file.
```


```
[root@servera ~]# cat /etc/httpd/conf/httpd.conf
...output omitted...
DocumentRoot "/custom"
...output omitted...
< Directory "/custom">
...output omitted...
```
**3.** Start and enable the Apache web service and confirm that the service is running.

```
3.1. Start and enable the Apache web service by using the systemctl command.
```
```
[root@servera ~]# systemctl enable --now httpd
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/
lib/systemd/system/httpd.service.
```
```
3.2. Verify that the service is running.
```
```
[root@servera ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor
preset: disabled)
Active: active (running) since Wed 2022-04-06 05:21:19 EDT; 22s ago
Docs: man:httpd.service(8)
Main PID: 1676 (httpd)
...output omitted...
Apr 06 05:21:19 servera.lab.example.com systemd[1]: Starting The Apache HTTP
Server...
Apr 06 05:21:19 servera.lab.example.com systemd[1]: Started The Apache HTTP
Server.
Apr 06 05:21:19 servera.lab.example.com httpd[1676]: Server configured, listening
on: port 80
```
**4.** Open a web browser on workstation and try to view the [http://servera/index.html](http://servera/index.html) web
    page. You get an error message that you do not have permission to access the file.
**5.** To permit access to the index.html file on servera, you must configure the
    SELinux context. Define an SELinux file context rule that sets the context type to
    httpd_sys_content_t for the /custom directory and all the files under it.

```
[root@servera ~]# semanage fcontext -a \
-t httpd_sys_content_t '/custom(/.*)?'
```
**6.** Correct the file contexts in the /custom directory.

```
[root@servera ~]# restorecon -Rv /custom
Relabeled /custom from unconfined_u:object_r:default_t:s0 to
unconfined_u:object_r:httpd_sys_content_t:s0
Relabeled /custom/index.html from unconfined_u:object_r:default_t:s0 to
unconfined_u:object_r:httpd_sys_content_t:s0
```


**7.** Try to view [http://servera/index.html](http://servera/index.html) again in the web browser on the workstation
    machine. You should see the This is SERVERA. message.
**8.** Return to the workstation machine as the student user.

```
[root@servera ~]# exit
logout
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish selinux-filecontexts
```
This concludes the section.



### Adjust SELinux Policy with Booleans

**Objectives**

Activate and deactivate SELinux policy rules with the setsebool command, manage the
persistent value of SELinux Booleans with the semanage boolean -l command, and consult
man pages that end with _selinux to find useful information about SELinux Booleans.

**SELinux Booleans**

An application or service developer writes an SELinux targeted policy to define the allowed
behavior of the targeted application. A developer can include optional application behavior
in the SELinux policy that can be enabled when the behavior is allowed on a specific system.
SELinux Booleans enable or disable the SELinux policy’s optional behavior. With Booleans, you
can selectively tune the behavior of an application.

These optional behaviors are application-specific, and must be discovered and selected for
each targeted application. Service-specific Booleans are documented in that service’s SELinux
man page. For example, the web server httpd service has its httpd(8) man page, and an
httpd_selinux(8) man page to document its SELinux policy, including the supported process
types, file contexts, and the available Boolean-enabled behaviors. The SELinux man pages are
provided in the selinux-policy-doc package.

Use the getsebool command to list available Booleans for the targeted policies on this system,
and the current Boolean status. Use the setsebool command to enable or disable the running
state of these behaviors. The setsebool -P command option makes the setting persistent by
writing to the policy file. Only privileged users can set SELinux Booleans.

```
[root@host ~]# getsebool -a
abrt_anon_write --> off
abrt_handle_event --> off
abrt_upload_watch_anon_write --> on
...output omitted...
```
**Example httpd Policy Boolean**

The httpd service policy includes the httpd_enable_homedirs Boolean, which enables the
sharing of home directories with httpd. Typically, a user’s local home directory is accessible to
the user only when logged in to the local system. Alternatively, home directories are shared and
accessed by using a remote file sharing protocol, such as NFS. In both scenarios, home directories
are not shared by using https, by default, and are not available to the user through a browser.

```
[root@host ~]# getsebool httpd_enable_homedirs
httpd_enable_homedirs --> off
```
You can enable sharing and allow users to access their home directories with a browser. When
enabled, the httpd service shares home directories that are labeled with the user_home_dir_t
file context. Users can then access and manage their home directory files from a browser.



**Manage the Policy Boolean**

Setting SELinux Booleans with the setsebool command without the -P option is temporary,
and settings will return to the persistent values after rebooting. View additional information with
the semanage boolean -l command, which lists the Booleans from the policy files, including
whether a Boolean is persistent, the default and current values, and a short description.

```
[root@host ~]# semanage boolean -l | grep httpd_enable_homedirs
httpd_enable_homedirs (off , off) Allow httpd to enable homedirs
[root@host ~]# setsebool httpd_enable_homedirs on
[root@host ~]# semanage boolean -l | grep httpd_enable_homedirs
httpd_enable_homedirs (on , off) Allow httpd to enable homedirs
[root@host ~]# getsebool httpd_enable_homedirs
httpd_enable_homedirs --> on
```
To list only Booleans with a current setting that is different from the default setting at boot, use
the semanage boolean -l -C command. This example has the same result as the previous
example, without requiring the grep filtering.

```
[root@host ~]# semanage boolean -l -C
SELinux boolean State Default Description
```
```
httpd_enable_homedirs (on , off) Allow httpd to enable homedirs
```
The previous example temporarily set the current value for the httpd_enable_homedirs
Boolean to on, until the system reboots. To change the default setting, use the setsebool -P
command to make the setting persistent. The following example sets a persistent value, and then
views the Boolean’s information from the policy file.

```
[root@host ~]# setsebool -P httpd_enable_homedirs on
[root@host ~]# semanage boolean -l | grep httpd_enable_homedirs
httpd_enable_homedirs (on , on) Allow httpd to enable homedirs
```
Use the semanage boolean -l -C command again. The Boolean is displayed despite
the appearance that the current and default settings are the same. However, the -C option
matches when the current setting is different from the default setting from the last boot. For this
httpd_enable_homedirs example, the original default boot setting was off.

```
[root@host ~]# semanage boolean -l -C
SELinux boolean State Default Description
```
```
httpd_enable_homedirs (on , on) Allow httpd to enable homedirs
```
```
References
booleans(8), getsebool(8), setsebool(8), semanage(8), and semanage-
boolean(8) man pages
```


**Guided Exercise**

### Adjust SELinux Policy with Booleans

```
In this exercise, you configure Apache to publish web content from users' home directories.
```
**Outcomes**

- Configure Apache web service to publish web content from the user’s home directory.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-booleans
```
**Instructions**

**1.** On the workstation machine, use the ssh command to log in to the servera machine
    as the student user and then switch to the root user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$ sudo -i
[sudo] password for student: student
[root@servera ~]#
```
**2.** Edit the /etc/httpd/conf.d/userdir.conf configuration file to enable the Apache
    feature so that users can publish web content from their home directory. Comment out the
    line in the IfModule section that sets the UserDir variable to the disabled value, and
    uncomment the line that sets the UserDir variable to the public_html value.

```
[root@servera ~]# vim /etc/httpd/conf.d/userdir.conf
<IfModule mod_userdir.c>
...output omitted...
# UserDir disabled
```
```
...output omitted...
UserDir public_html
```
```
...output omitted...
</IfModule>
```
**3.** Start and enable the Apache web service.



```
[root@servera ~]# systemctl enable --now httpd
```
**4.** Open another terminal window, and use the ssh command to log in to the servera
    machine as the student user. Create the index.html web content file in the
    ~/public_html directory.

```
4.1. In another terminal window, use the ssh command to log in to the servera machine
as the student user.
```
```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$
```
```
4.2. Use the mkdir command to create the ~/public_html directory.
```
```
[student@servera ~]$ mkdir ~/public_html
```
```
4.3. Create the index.html file with the following content:
```
```
[student@servera ~]$ echo 'This is student content on SERVERA.' > \
~/public_html/index.html
```
```
4.4. For the Apache web service to serve the contents of the
/home/student/public_html directory, it must be allowed to share files
and subdirectories in the /home/student directory. When you created the
/home/student/public_html directory, it was automatically configured with
permissions that allow anyone with home directory permission to access its contents.
Change the /home/student directory permissions to allow the Apache web service
to access the public_html subdirectory.
```
```
[student@servera ~]$ chmod 711 ~
[student@servera ~]$ ls -ld ~
drwx--x--x. 16 student student 4096 Nov 3 09:28 /home/student
```
**5.** Open a web browser on the workstation machine and enter the
    [http://servera/~student/index.html](http://servera/~student/index.html) address. An error message states that you do not have
    permission to access the file.
**6.** Switch to the other terminal and use the getsebool command to see if any Booleans
    restrict access to home directories for the httpd service.

```
[root@servera ~]# getsebool -a | grep home
...output omitted...
httpd_enable_homedirs --> off
...output omitted...
```
**7.** Use the setsebool command to enable persistent access to the home directory for the
    httpd service.



```
[root@servera ~]# setsebool -P httpd_enable_homedirs on
```
**8.** Verify that you can now see the This is student content on SERVERA. message in
    the web browser after entering the [http://servera/~student/index.html](http://servera/~student/index.html) address.
**9.** Return to the workstation machine as the student user.

```
[root@servera ~]# exit
logout
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish selinux-booleans
```
This concludes the section.



### Investigate and Resolve SELinux Issues

**Objectives**

Use SELinux log analysis tools and display useful information during SELinux troubleshooting with
the sealert command.

**Troubleshoot SELinux Issues**

When applications unexpectedly fail to work due to SELinux access denials, methods and tools
are available to resolve these issues. It is helpful to start by understanding some fundamental
concepts and behaviors when SELinux is enabled.

- SELinux consists of targeted policies that explicitly define allowable actions.
- A policy entry defines a labeled process and a labeled resource that will interact.
- The policy states the process type, and file or port context, by using labels.
- The policy entry defines one process type, one resource label, and the explicit action to allow.
- An action can be a system call, a kernel function, or another specific programming routine.
- If no entry is created for a specific process-resource-action relationship, then the action is
    denied.
- When an action is denied, the attempt is logged with useful context information.

Red Hat Enterprise Linux provides a stable targeted SELinux policy for almost every service in
the distribution. Therefore, it is unusual to have SELinux access problems with common RHEL
services when they are configured correctly. SELinux access problems occur when services are
implemented incorrectly, or when new applications have incomplete policies. Consider these
troubleshooting concepts before making broad SELinux configuration changes.

- Most access denials indicate that SELinux is working properly by blocking improper actions.
- Evaluating denied actions requires some familiarity with normal, expected service actions.
- The most common SELinux issue is an incorrect context on new, copied, or moved files.
- File contexts are easily fixed when an existing policy references their location.
- Optional Boolean policy features are documented in the _selinux man pages.
- Implementing Boolean features generally requires setting additional non-SELinux configuration.
- SELinux policies do not replace or circumvent file permissions or access control list restrictions.

When a common application or service fails, and the service is known to have a working SELinux
policy, first check the service’s _selinux man page to verify the correct context type label. View
the affected process and file attributes to verify that the correct labels are set.

**Monitor SELinux Violations**

The SELinux troubleshoot service, from the setroubleshoot-server package, provides
tools to diagnose SELinux issues. When SELinux denies an action, an Access Vector Cache
(AVC) message is logged to the /var/log/audit/audit.log security log file. The SELinux
troubleshoot service monitors for AVC events and sends an event summary to the /var/log/
messages file.

The AVC summary includes an event unique identifier (UUID). Use the sealert -l _UUID_
command to view comprehensive report details for the specific event. Use the sealert
-a /var/log/audit/audit.log command to view all existing events.



Consider the following example sequence of commands on a standard Apache web server. You
create /root/mypage and move it to the default Apache content folder (/var/www/html).
Then, after starting the Apache service, you try to retrieve the file content.

```
[root@host ~]# touch /root/mypage
[root@host ~]# mv /root/mypage /var/www/html
[root@host ~]# systemctl start httpd
[root@host ~]# curl http://localhost/mypage
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>403 Forbidden</title>
</head><body>
<h1>Forbidden</h1>
<p>You don't have permission to access this resource.</p>
</body></html>
```
The web server does not display the content and returns a permission denied error. An AVC
event is logged to the /var/log/audit/audit.log and /var/log/messages files. Note the
suggested sealert command and UUID in the /var/log/messages event message.

```
[root@host ~]# tail /var/log/audit/audit.log
...output omitted...
type=AVC msg=audit(1649249057.067:212): avc: denied { getattr }
for pid=2332 comm="httpd" path="/var/www/html/mypage"
dev="vda4" ino=9322502 scontext=system_u:system_r:httpd_t:s0
tcontext=unconfined_u:object_r:admin_home_t:s0 tclass=file permissive=0
...output omitted
[root@host ~]# tail /var/log/messages
...output omitted...
Apr 6 08:44:19 host setroubleshoot[2547]: SELinux is preventing /usr/sbin/httpd
from getattr access on the file /var/www/html/mypage. For complete SELinux
messages run: sealert -l 95f41f98-6b56-45bc-95da-ce67ec9a9ab7
...output omitted...
```
The sealert output describes the event including the affected process, the accessed file, and
the attempted and denied action. The output includes advice for correcting the file’s label, if
appropriate. Additional advice describes how to generate a new policy to allow the denied action.
Use the given advice only when it is appropriate for your scenario.

```
Important
The sealert output includes a confidence rating, which indicates the level of
confidence that the given advice will mitigate the denial. However, that advice might
not be appropriate for your scenario.
```
```
For example, if the AVC denial is because the denied file is in the wrong location,
then advice that states either to adjust the file’s context label, or to create a new
policy for this location and action, is technically accurate, but not the correct
solution for your scenario. If the root cause is a wrong location or file name, then
moving or renaming the file and then restoring a correct file context is the correct
solution instead.
```


```
[root@host ~]# sealert -l 95f41f98-6b56-45bc-95da-ce67ec9a9ab7
SELinux is preventing /usr/sbin/httpd from getattr access on the file /var/www/
html/mypage.
```
```
***** Plugin restorecon (99.5 confidence) suggests ************************
```
```
If you want to fix the label.
/var/www/html/mypage default label should be httpd_sys_content_t.
Then you can run restorecon. The access attempt may have been stopped due to
insufficient permissions to access a parent directory in which case try to change
the following command accordingly.
Do
# /sbin/restorecon -v /var/www/html/mypage
```
```
***** Plugin catchall (1.49 confidence) suggests **************************
```
```
If you believe that httpd should be allowed getattr access on the mypage file by
default.
Then you should report this as a bug.
You can generate a local policy module to allow this access.
Do
allow this access for now by executing:
# ausearch -c 'httpd' --raw | audit2allow -M my-httpd
# semodule -X 300 -i my-httpd.pp
```
```
Additional Information:
Source Context system_u:system_r:httpd_t:s0
Target Context unconfined_u:object_r:admin_home_t:s0
Target Objects /var/www/html/mypage [ file ]
Source httpd
Source Path /usr/sbin/httpd
...output omitted...
```
```
Raw Audit Messages
type=AVC msg=audit(1649249057.67:212): avc: denied { getattr }
for pid=2332 comm="httpd" path="/var/www/html/mypage"
dev="vda4" ino=9322502 scontext=system_u:system_r:httpd_t:s0
tcontext=unconfined_u:object_r:admin_home_t:s0 tclass=file permissive=0
```
```
type=SYSCALL msg=audit(1649249057.67:212): arch=x86_64 syscall=newfstatat
success=no exit=EACCES a0=ffffff9c a1=7fe9c00048f8 a2=7fe9ccfc8830 a3=100
items=0 ppid=2329 pid=2332 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48
egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm=httpd exe=/usr/sbin/httpd
subj=system_u:system_r:httpd_t:s0 key=(null)
```
```
Hash: httpd,httpd_t,admin_home_t,file,getattr
```
In this example, the accessed file is in the correct location, but does not have the correct SELinux
file context. The Raw Audit Messages section displays information from the /var/log/
audit.log event entry. Use the restorecon /var/www/html/mypage command to set the
correct context label. To correct multiple files recursively, use the restorecon -R command on
the parent directory.



Use the ausearch command to search for AVC events in the /var/log/audit.log log file. Use
the -m option to specify the AVC message type and the -ts option to provide a time hint, such as
recent.

```
[root@host ~]# ausearch -m AVC -ts recent
----
time->Tue Apr 6 13:13:07 2019
type=PROCTITLE msg=audit(1554808387.778:4002):
proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type= SYSCALL msg=audit (1554808387.778:4002): arch=c000003e syscall=49
success=no exit=-13 a0=3 a1=55620b8c9280 a2=10 a3=7ffed967661c items=0
ppid=1 pid=9340 auid=4294967295 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0
sgid=0 fsgid=0 tty=(none) ses=4294967295 comm= "httpd" exe="/usr/sbin/httpd"
subj=system_u:system_r: httpd_t :s0 key=(null)
type= AVC msg=audit (1554808387.778:4002): avc: denied { name_bind }
for pid=9340 comm="httpd" src=82 scontext=system_u:system_r:httpd_t:s0
tcontext=system_u:object_r:reserved_port_t:s0 tclass=tcp_socket permissive=0
```
**Troubleshoot SELinux Issues with the Web Console**

The RHEL web console includes tools for troubleshooting SELinux issues. Select SELinux from
the menu on the left. The SELinux policy window displays the current enforcing state. The SELinux
access control errors section lists current SELinux issues.

```
Figure 4.3: SELinux policy and errors in the web console
```
Click the > character to display event details. Click solution details to display all event details and
advice. You can click Apply the solution to apply the suggested advice.

After correcting the issue, the SELinux access control errors section should remove that event
from view. If the No SELinux alerts message appears, then you have corrected all current
SELinux issues.

```
References
sealert(8) man page
```


**Guided Exercise**

### Investigate and Resolve SELinux Issues

```
In this lab, you learn how to troubleshoot SELinux security denials.
```
**Outcomes**

- Gain experience with SELinux troubleshooting tools.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-issues
```
**Instructions**

**1.** From a web browser on the workstation machine, open the [http://servera/index.html](http://servera/index.html)
    web page. An error message states that you do not have permission to access the file.
**2.** Use the ssh command to log in to servera as the student user. Use the sudo -i
    command to switch to the root user.

```
[student@workstation ~]$ ssh student@servera
...output omitted...
[student@servera ~]$ sudo -i
[sudo] password for student: student
[root@servera ~]#
```
**3.** Use the less command to view the contents of the /var/log/messages file. You use
    the / character and search for the sealert text. Press the n key until you reach the last
    occurrence, because previous exercises might also have generated SELinux messages.
    Copy the suggested sealert command so that you can use it in the next step. Use the q
    key to quit the less command.

```
[root@servera ~]# less /var/log/messages
...output omitted...
Apr 7 04:52:18 servera setroubleshoot[20715]: SELinux is preventing /usr/sbin/
httpd from getattr access on the file /custom/index.html. For complete SELinux
messages run: sealert -l 9a96294a-239b-4568-8f1e-9f35b5fb472b
...output omitted...
```


**4.** Run the suggested sealert command. Note the source context, the target objects, the
    policy, and the enforcing mode. Find the correct SELinux context label for the file that the
    httpd service tries to serve.

```
4.1. Run the sealert command.
The output explains that the /custom/index.html file has an incorrect context
label.
```
```
[root@servera ~]# sealert -l 9a96294a-239b-4568-8f1e-9f35b5fb472b
SELinux is preventing /usr/sbin/httpd from getattr access on the file /custom/
index.html.
```
```
***** Plugin catchall_labels (83.8 confidence) suggests *******************
```
```
If you want to allow httpd to have getattr access on the index.html file
Then you need to change the label on /custom/index.html
Do
# semanage fcontext -a -t FILE_TYPE '/custom/index.html'
where FILE_TYPE is one of the following: NetworkManager_exec_t,
NetworkManager_log_t, NetworkManager_tmp_t, abrt_dump_oops_exec_t,
abrt_etc_t, abrt_exec_t, abrt_handle_event_exec_t, abrt_helper_exec_t,
abrt_retrace_coredump_exec_t, abrt_retrace_spool_t, abrt_retrace_worker_exec_t,
abrt_tmp_t, abrt_upload_watch_tmp_t, abrt_var_cache_t, abrt_var_log_t,
abrt_var_run_t, accountsd_exec_t, acct_data_t, acct_exec_t, admin_crontab_tmp_t,
admin_passwd_exec_t, afs_logfile_t, aide_exec_t, aide_log_t, alsa_exec_t,
alsa_tmp_t, amanda_exec_t, amanda_log_t, amanda_recover_exec_t, amanda_tmp_t,
amtu_exec_t, anacron_exec_t, anon_inodefs_t
...output omitted...
```
```
Additional Information:
Source Context system_u:system_r:httpd_t:s0
Target Context unconfined_u:object_r:default_t:s0
Target Objects /custom/index.html [ file ]
Source httpd
Source Path /usr/sbin/httpd
Port <Unknown>
Host servera.lab.example.com
Source RPM Packages httpd-2.4.51-7.el9_0.x86_64
Target RPM Packages
SELinux Policy RPM selinux-policy-targeted-34.1.27-1.el9.noarch
Local Policy RPM selinux-policy-targeted-34.1.27-1.el9.noarch
Selinux Enabled True
Policy Type targeted
Enforcing Mode Enforcing
Host Name servera.lab.example.com
Platform Linux servera.lab.example.com
5.14.0-70.2.1.el9_0.x86_64 #1 SMP PREEMPT Wed Mar
16 18:15:38 EDT 2022 x86_64 x86_64
Alert Count 4
First Seen 2022-04-07 04:51:38 EDT
Last Seen 2022-04-07 04:52:13 EDT
Local ID 9a96294a-239b-4568-8f1e-9f35b5fb472b
```
```
Raw Audit Messages
```


```
type=AVC msg=audit(1649321533.406:1024): avc: denied { getattr } for
pid=20464 comm="httpd" path="/custom/index.html" dev="vda4" ino=25571802
scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0
tclass=file permissive=0
```
```
...output omitted...
```
```
4.2. Check the SELinux context for the directory from where the httpd service serves
the content by default, /var/www/html. The httpd_sys_content_t SELinux
context is appropriate for the /custom/index.html file.
```
```
[root@servera ~]# ls -ldZ /var/www/html
drwxr-xr-x. 2 root root system_u:object_r: httpd_sys_content_t :s0 6 Mar 21 11:47 /
var/www/html
```
**5.** The Raw Audit Messages section of the sealert command contains information
    from the /var/log/audit/audit.log file. Use the ausearch command to search the
    /var/log/audit/audit.log file. The -m option searches on the message type. The
    -ts option searches based on time. The following entry identifies the relevant process and
    file that cause the alert. The process is the httpd Apache web server, the file is /custom/
    index.html, and the context is system_r:httpd_t.

```
[root@servera ~]# ausearch -m AVC -ts today
...output omitted...
----
time->Thu Apr 7 04:52:13 2022
type=PROCTITLE msg=audit(1649321533.406:1024):
proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1649321533.406:1024): arch=c000003e syscall=262 success=no
exit=-13 a0=ffffff9c a1=7fefc403d850 a2=7fefc89bc830 a3=100 items=0 ppid=20461
pid=20464 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48
sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd"
subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1649321533.406:1024): avc: denied
{ getattr } for pid=20464 comm="httpd" path="/custom/index.html"
dev="vda4" ino=25571802 scontext=system_u:system_r:httpd_t:s0
tcontext=unconfined_u:object_r:default_t:s0 tclass=file permissive=0
```
**6.** Resolve the issue by applying the httpd_sys_content_t context.

```
[root@servera ~]# semanage fcontext -a \
-t httpd_sys_content_t '/custom(/.*)?'
[root@servera ~]# restorecon -Rv /custom
Relabeled /custom from unconfined_u:object_r:default_t:s0 to
unconfined_u:object_r:httpd_sys_content_t:s0
Relabeled /custom/index.html from unconfined_u:object_r:default_t:s0 to
unconfined_u:object_r:httpd_sys_content_t:s0
```
**7.** Again, attempt to view [http://servera/index.html.](http://servera/index.html.) The This is SERVERA. message is
    displayed.
**8.** Return to the workstation machine as the student user.



```
[root@servera ~]# exit
logout
[student@servera ~]$ exit
logout
Connection to servera closed.
[student@workstation ~]$
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```
[student@workstation ~]$ lab finish selinux-issues
```
This concludes the section.



**Lab**

### Manage SELinux Security

```
In this lab, you identify issues in system log files and adjust the SELinux configuration.
```
**Outcomes**

- Identify issues in system log files.
- Adjust the SELinux configuration.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-review
```
**Instructions**

**1.** Log in to the serverb machine as the student user and switch to the root user.
**2.** From a web browser on the workstation machine, view the [http://serverb/lab.html](http://serverb/lab.html) web
    page. You see the error message: You do not have permission to access this
    resource.
**3.** Research and identify the SELinux issue that prevents the Apache service from serving web
    content.
**4.** Display the SELinux context of the new HTTP document directory and the original HTTP
    document directory. Resolve the SELinux issue that prevents the Apache server from serving
    web content.
**5.** Verify that the Apache server can now serve web content.
**6.** Return to the workstation machine as the student user.

**Evaluation**

As the student user on the workstation machine, use the lab command to grade your work.
Correct any reported failures and rerun the command until successful.

```
[student@workstation ~]$ lab grade selinux-review
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.



```
[student@workstation ~]$ lab finish selinux-review
```
This concludes the section.



**Solution**

### Manage SELinux Security

```
In this lab, you identify issues in system log files and adjust the SELinux configuration.
```
**Outcomes**

- Identify issues in system log files.
- Adjust the SELinux configuration.

```
Before You Begin
As the student user on the workstation machine, use the lab command to prepare your
system for this exercise.
```
```
This command prepares your environment and ensures that all required resources are
available.
```
```
[student@workstation ~]$ lab start selinux-review
```
**Instructions**

**1.** Log in to the serverb machine as the student user and switch to the root user.

```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```
**2.** From a web browser on the workstation machine, view the [http://serverb/lab.html](http://serverb/lab.html) web
    page. You see the error message: You do not have permission to access this
    resource.
**3.** Research and identify the SELinux issue that prevents the Apache service from serving web
    content.

```
3.1. View the contents of the /var/log/messages file. Use the / key and search for the
sealert string. Use the q key to quit the less command.
```


```
[root@serverb ~]# less /var/log/messages
...output omitted...
Apr 7 06:16:15 serverb setroubleshoot[26509]: failed to retrieve rpm info for /
lab-content/la
b.html
Apr 7 06:16:17 serverb setroubleshoot[26509]: SELinux is preventing /usr/sbin/
httpd from getattr access on the file /lab-content/lab.html. For complete SELinux
messages run: sealert -l 35c9e452-2552-4ca3-8217-493b72ba6d0b
Apr 7 06:16:17 serverb setroubleshoot[26509]: SELinux is preventing /usr/sbin/
httpd from getattr access on the file /lab-content/lab.html
...output omitted...
```
```
3.2. Run the suggested sealert command. Note the source context, the target objects,
the policy, and the enforcing mode.
```
```
[root@serverb ~]# sealert -l 35c9e452-2552-4ca3-8217-493b72ba6d0b
SELinux is preventing /usr/sbin/httpd from getattr access on the file /lab-
content/lab.html.
```
```
***** Plugin catchall_labels (83.8 confidence) suggests *******************
```
```
If you want to allow httpd to have getattr access on the lab.html file
Then you need to change the label on /lab-content/lab.html
Do
# semanage fcontext -a -t FILE_TYPE '/lab-content/lab.html'
where FILE_TYPE is one of the following:
...output omitted...
```
```
Additional Information:
Source Context system_u:system_r:httpd_t:s0
Target Context unconfined_u:object_r:default_t:s0
Target Objects /lab-content/lab.html [ file ]
Source httpd
Source Path /usr/sbin/httpd
Port <Unknown>
Host serverb.lab.example.com
Source RPM Packages httpd-2.4.51-7.el9_0.x86_64
Target RPM Packages
SELinux Policy RPM selinux-policy-targeted-34.1.27-1.el9.noarch
Local Policy RPM selinux-policy-targeted-34.1.27-1.el9.noarch
Selinux Enabled True
Policy Type targeted
Enforcing Mode Enforcing
Host Name serverb.lab.example.com
Platform Linux serverb.lab.example.com
5.14.0-70.2.1.el9_0.x86_64 #1 SMP PREEMPT Wed Mar
16 18:15:38 EDT 2022 x86_64 x86_64
Alert Count 8
First Seen 2022-04-07 06:14:45 EDT
Last Seen 2022-04-07 06:16:12 EDT
Local ID 35c9e452-2552-4ca3-8217-493b72ba6d0b
```
```
Raw Audit Messages
```


```
type=AVC msg=audit(1649326572.86:407): avc: denied { getattr } for
pid=10731 comm="httpd" path="/lab-content/lab.html" dev="vda4" ino=18192752
scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0
tclass=file permissive=0
```
```
type=SYSCALL msg=audit(1649326572.86:407): arch=x86_64 syscall=newfstatat
success=no exit=EACCES a0=ffffff9c a1=7f7c8c0457c0 a2=7f7c887f7830 a3=100 items=0
ppid=10641 pid=10731 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48
egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm=httpd exe=/usr/sbin/httpd
subj=system_u:system_r:httpd_t:s0 key=(null)
```
```
Hash: httpd,httpd_t,default_t,file,getattr
```
```
3.3. The Raw Audit Messages section of the sealert command contains information
from the /var/log/audit/audit.log file. Search the /var/log/audit/
audit.log file. The -m option searches on the message type. The ts option searches
based on time. The following entry identifies the relevant process and file that cause
the alert. The process is the httpd Apache web server, the file is /lab-content/
lab.html, and the context is system_r:httpd_t.
```
```
[root@serverb ~]# ausearch -m AVC -ts recent
...output omitted...
----
time->Thu Apr 7 06:16:12 2022
type=PROCTITLE msg=audit(1649326572.086:407):
proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1649326572.086:407): arch=c000003e syscall=262 success=no
exit=-13 a0=ffffff9c a1=7f7c8c0457c0 a2=7f7c887f7830 a3=100 items=0 ppid=10641
pid=10731 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48
sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd"
subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1649326572.086:407): avc: denied { getattr } for
pid=10731 comm="httpd" path="/lab-content/lab.html" dev="vda4" ino=18192752
scontext=system_u: system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0
tclass=file permissive=0
```
**4.** Display the SELinux context of the new HTTP document directory and the original HTTP
    document directory. Resolve the SELinux issue that prevents the Apache server from serving
    web content.

```
4.1. Compare the SELinux context for the /lab-content and /var/www/html
directories.
```
```
[root@serverb ~]# ls -dZ /lab-content /var/www/html
unconfined_u:object_r: default_t :s0 /lab-content
system_u:object_r: httpd_sys_content_t :s0 /var/www/html
```
```
4.2. Create a file context rule that sets the default type to httpd_sys_content_ for the
/lab-content directory and all the files under it.
```
```
[root@serverb ~]# semanage fcontext -a \
-t httpd_sys_content_t '/lab-content(/.*)?'
```


```
4.3. Correct the SELinux context for the files in the /lab-content directory.
```
```
[root@serverb ~]# restorecon -R /lab-content/
```
**5.** Verify that the Apache server can now serve web content.

```
5.1. Use your web browser to refresh the http://serverb/lab.html link. If the content is
displayed, then your issue is resolved.
```
```
This is the html file for the SELinux final lab on SERVERB.
```
**6.** Return to the workstation machine as the student user.

```
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```
**Evaluation**

As the student user on the workstation machine, use the lab command to grade your work.
Correct any reported failures and rerun the command until successful.

```
[student@workstation ~]$ lab grade selinux-review
```
**Finish**

On the workstation machine, change to the student user home directory and use the lab
command to complete this exercise. This step is important to ensure that resources from previous
exercises do not impact upcoming exercises.

```


