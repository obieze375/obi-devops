[[Index]] 


## Access Man Page

```bash
man <command_name>

man passwd 

man -k <passwd> - Obtains all the parameters that can be used with it

```

## Write a Long Command on Multiple Lines

```bash
[user@host ~]$ head -n 3 \
/usr/share/dict/words \
/usr/share/dict/linux.words
==> /usr/share/dict/words <==
```





## Prepare Man page for Printing

```bash
[student@worksation ~]$ man man
...output omitted...
 man -t bash | lpr -Pps
 Format the manual page for bash into the default troff or groff format
 and pipe it to the printer named ps. The default output for groff
 is usually PostScript. man --help should advise as to which processor
 is bound to the -t option.
...output omitted...
```

## Push Output of Man Page to file

```bash
[student@workstation ~]$ man -t passwd > passwd.ps
[student@workstation ~]$ ls -al
...output omitted...
-rw-r--r--. 1 student student 20168 Mar 8 09:02 passwd.ps
...output omitted...
```
## View the Contents of Files

The cat command is one of the most simple and frequently used commands in Linux. Use this
command to create single or multiple files, view the contents of files, concatenate the contents
from various files, and redirect contents of the file to a terminal or to files.
The following example shows how to view the contents of the /etc/passwd file.

## Cat Command

```bash
[user@host ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
...output omitted...
```

To display the contents of multiple files, add the file names to the cat command as arguments.

```bash
[user@host ~]$ cat file1 file2
Hello World!!
```


## Head & Tail Commands

Some files are long and might need more space to be displayed than the terminal provides. The
cat command does not display the contents of a file as pages. The less command displays one
page of a file at a time and you can scroll at your leisure.
Use the less command to page forward and backward through longer files than can fit on one
terminal window. Use the UpArrow key and the DownArrow key to scroll up and down. Press q to
exit the command.

The head and tail commands display the beginning and the end of a file, respectively. By
default, these commands display 10 lines of the file, but they both have a -n option to specify a
different number of lines.

## Head Command 

```bash
[user@host ~]$ head /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
```

## Tail Command

```bash
[user@host ~]$ tail -n 3 /etc/passwd
gdm:x:42:42::/var/lib/gdm:/sbin/nologin
gnome-initial-setup:x:980:978::/run/gnome-initial-setup/:/sbin/nologin
dnsmasq:x:979:977:Dnsmasq DHCP and DNS server:/var/lib/dnsmasq:/sbin/nologin
```
## WC 

The wc command counts lines, words, and characters in a file. Use the -l, -w, or -c options to
display only the given number of lines, words, or characters, respectively.

```bash
[user@host ~]$ wc /etc/passwd
41 98 2338 /etc/passwd
[user@host ~]$ wc -l /etc/passwd ; wc -l /etc/group
41 /etc/passwd
63 /etc/group
[user@host ~]$ wc -c /etc/group /etc/hosts
883 /etc/group
114 /etc/hosts
997 total
```


## clear – Clear the terminal window

```bash
clear
```

  

## echo $? - Check if previous bash command run was successful 0 means yes and 1 means no

```bash
echo $?
```
  

## reset – Resets the terminal

```bash
reset
```
  

## history – Lists all recent commands

```bash
history

  

history | grep <value> - Search for a specific command from the history

  

• 3. Delete a line from the history with history -d line_number

• 4. If you want your commands not to remain in the history, run them

with a space before them after you will have set

HISTCONTROL=ignorespace

  

• ! e.g. !357 -> re-runs command if entered into terminal
```
  

## xkill – Close a frozen window/application


```bash
xkill
```


## Tab Completion

Use TAB key for completion of characters on CLI 

```bash
[user@host ~]$ pasTab+Tab

passwd paste pasuspender

[user@host ~]$ passTab
[user@host ~]$ passwd
Changing password for user user.
Current password:
```

## Edit the command Line  

```bash

• ctrl + u = Clear everything before the cursor

  

• ctrl + a = To beginning of line

  

• ctrl + e = To end of line

  

• ctrl + b = Back one word

  

• ctrl + f = Forward one word

  

• ctrl + w = Cut last word

  

• ctrl + k = Clear everything after the cursor

• ctrl + _ = Undo

• Ctrl+LeftArrow = Jump to the beginning of the previous word on the command line.

• Ctrl+RightArrow = Jump to the end of the next word on the command line.

```

## Date Command

```bash
[student@workstation ~]$ date
Mon Feb 28 01:57:25 PM PDT 2022

```

## Date Command 24 hr Clock

```bash
[student@workstation ~]$ date +%R
13:58

```
  

## Alter Screen Resolution

```bash
xrandr --output --mode 1920x1200
```

  

## Ctrl + r - Reverse search

```bash
Type Ctrl+r to open the reverse search tool.

Search for any command from the history and press Ctrl+r again for

the previous one that contain the string you’re looking for.
```




## General commands

```bash
• ./ - Runs file as executable

  

• -- version – Checks version of installation

  

• wget – Download file over internet

  

• reboot – Restarts the system

  

• Shutdown – shuts down server  

  

shutdown -r -> Shutdowns and reboots the node

command; command; -> Separates commands 

• -- help – use to get information on command e.g. ls --help
```





  
  

## tmux

  
  

## To install : yum install tmux

  

## start new:


```bash
tmux
```

  

## start new with session name:

```bash
tmux new -s myname 
```  

## Attach:

```bash
tmux a  #  (or at, or attach)
```  



  

## Attach to named:  

  
```bash
tmux a -t myname
```
  

## list sessions:

```bash
tmux ls
```

## kill session:

```bash
tmux kill-session -t myname
```

## Kill all the tmux sessions:

```bash
tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill
```  


In tmux, hit the prefix ctrl+b (my modified prefix is ctrl+a) and then:

  

From <https://gist.github.com/MohamedAlaa/2961058>

  
  
  

## 4 pane window:

```bash 

tmux new-window \; split-window -p 66 \; split-window -d \; split-window -h

``` 

  

The flow is:

```bash 

  1. tmux new-window: create a window (ok, you wanted a new-session, that does create a window upon startup)

  2. split-window -p 66: allocate bottom two thirds of the vertical space to a secondary pane and focus it

  3. split-window -d: split bottom pane in half, vertically, without focusing the new pane (i.e. focus stays on second – now center – pane)

  4. split-window -h: split center pane in half, horizontally

``` 

  

From <https://superuser.com/questions/1155771/tmux-split-into-4-panes>

  
  

```bash 

:new<CR>  new session

s  list sessions

$  name session

``` 

From <https://gist.github.com/MohamedAlaa/2961058>

```bash 

Panes (splits)

%  vertical split

"  horizontal split

o  swap panes

q  show pane numbers

x  kill pane

+  break pane into window (e.g. to select text by mouse to copy)

-  restore pane from window

⍽  space - toggle between layouts

<prefix> q (Show pane numbers, when the numbers show up type the key to goto that pane)

<prefix> { (Move the current pane left)

<prefix> } (Move the current pane right)

<prefix> z toggle pane zoom

  

From <https://gist.github.com/MohamedAlaa/2961058>

```

```bash 

Windows (tabs)

c  create window

w  list windows

n  next window

p  previous window

f  find window

,  name window

&  kill window

``` 

From <https://gist.github.com/MohamedAlaa/2961058>

  

## Sync Panes

You can do this by switching to the appropriate window, typing your Tmux prefix (commonly Ctrl-B or Ctrl-A) and then a colon to bring up a Tmux command line, and typing:  

```bash   

:setw synchronize-panes

``` 

 

You can optionally add on or off to specify which state you want; otherwise the option is simply toggled. This option is specific to one window, so it won’t change the way your other sessions or windows operate. When you’re done, toggle it off again by repeating the command.

  

From <https://gist.github.com/MohamedAlaa/2961058>

```bash  

d  detach

t  big clock

?  list shortcuts

:  prompt

``` 

From <https://gist.github.com/MohamedAlaa/2961058>  

  
  
  

## Crontab

  

```bash 

[Crontab schedule]

+---------------- minute (0 - 59)

|  +------------- hour (0 - 23)

|  |  +---------- day of month (1 - 31)

|  |  |  +------- month (1 - 12)

|  |  |  |  +---- day of week (0 - 6) (Sunday=0)

|  |  |  |  |

*  *  *  *  * command to be executed <script>

-- -- -- -- - ---------------------------------

``` 


```bash 

crontab  -e  

  

crontab -l | grep <script_name>

  
 ```
 

# Check Version

  

```bash 

cat /etc/oracle-release

``` 

  

# Diagnostics

  

## RAM

```bash 

  • free -th

``` 

  

## RAM & CPU

  

```bash 

top

  

top –c - Filtering

``` 

## OS:

  

```bash   

hostnamectl

``` 

  

## SAR:

  

## Basic Output:

```bash 

sar  

``` 

## CPU Usage per Core:

```bash 

sar -P ALL

``` 

## Memory Usage:

```bash 

sar -r  

``` 

## Swap Usage:

```bash 

sar -S

``` 

  

From <https://www.gxd88.cn/sar>  

  

## I/O

```bash 

sar -b

``` 

  

## I/O by Block Device:

```bash 

sar -d -p

``` 

## Check Run queue and Load Average:

```bash 

sar -q

``` 

  

## Network Stats:

```bash 

 sar -n DEV

  

Where DEV can be one of the following

  

DEV – Displays network devices vital statistics for eth0, eth1, etc.,

  

EDEV – Display network device failure statistics

  

NFS – Displays NFS client activities

  

NFSD – Displays NFS server activities

  

SOCK – Displays sockets in use for IPv4

  

IP – Displays IPv4 network traffic

  

EIP – Displays IPv4 network errors

  

ICMP – Displays ICMPv4 network traffic

  

EICMP – Displays ICMPv4 network errors

  

TCP – Displays TCPv4 network traffic

  

ETCP – Displays TCPv4 network errors

  

UDP – Displays UDPv4 network traffic

  

SOCK6, IP6, EIP6, ICMP6, UDP6 are for IPv6

  

ALL – This displays all of the above information. The output will be very long.

``` 

  

## Historic Information:

```bash 

 sar -f /var/log/sa/sa15 -n DEV

```
  

## Uptime:

  

```bash 

 w

``` 

## Windows Shortcut Notes

```bash 

Ctrl + X - Cut the selected item.

Ctrl + C (or Ctrl + Insert) - Copy the selected item.

Ctrl + V (or Shift + Insert) - Paste the selected item.

Ctrl + Z - Undo an action.

``` 


## nohup & disown command usage

```bash 

  

nohup <bash_command> & disown

  

 -> If you use all three together, the process is running in the background, is removed from the shell's job control and is effectively disconnected from the terminal

``` 


## Exit Terminal

```bash 
exit
```

## Man Page

Use the man man command to determine how to prepare a man page for printing.

```bash 
[student@worksation ~]$ man man
...output omitted...
 man -t bash | lpr -Pps
 Format the manual page for bash into the default troff or groff format
 and pipe it to the printer named ps. The default output for groff
 is usually PostScript. man --help should advise as to which processor
 is bound to the -t option.
...output omitted...

```
Press q to quit the man page.


Push Content of directory into file

```bash 

[student@workstation ~]$ ls > /tmp/my-file-names

```


## Use the man -t command to create a formatted file of the passwd man page.

```bash 
[student@workstation ~]$ man -t passwd > passwd.ps
[student@workstation ~]$ ls -al
...output omitted...
-rw-r--r--. 1 student student 20168 Mar 8 09:02 passwd.ps
...output omitted...
```


Use the file command to determine the file format.

[student@workstation ~]$ file /home/student/passwd.ps

/home/student/passwd.ps: PostScript document text conforming DSC level 3.0
Use the less command to view the /home/student/passwd.ps file.

[student@workstation ~]$ less /home/student/passwd.ps
%!PS-Adobe-3.0
%%Creator: groff version 1.22.3
%%CreationDate: Tue Feb 26 11:14:40 2019
%%DocumentNeededResources: font Times-Roman
%%+ font Times-Bold
%%+ font Times-Italic
%%+ font Symbol
%%DocumentSuppliedResources: procset grops 1.22 3
...output omitted...




Note
The output of file states that the file is in PostScript format, and you can confirm
it by viewing the file's contents. Notice the header lines of PostScript information.
Use q to quit the less command.


By using the man pages, find which commands you can use for viewing and printing
PostScript files.

Search in the man pages for information about PostScript files. Use the -k option for
this purpose.

```bash 
[student@workstation ~]# man -k postscript viewer
enscript (1) - convert text files to PostScript, HTML, RTF, ANSI, and
 overstrikes
eps2eps (1) - Ghostscript PostScript "distiller"
evince (1) - GNOME document viewer
evince-previewer (1) - show a printing preview of PostScript and PDF documents
evince-thumbnailer (1) - create png thumbnails from PostScript and PDF documents
gcm-viewer (1) - GNOME Color Manager Profile Viewer Tool
...output omitted...

```


Note

Using multiple words with the -k option finds man pages that match any word;
those with "postscript" or "viewer" in their descriptions. Notice the evince(1)
commands in the output.


## Learn how to use the evince(1) viewer in preview mode. 

Also, determine how to open a document to start on a specific page. Open your PostScript file by using evince three times:
first by using the default mode, then with the preview mode option, and finally to start at
page 3. Close your document file when you finish.


## Use the man evince command to learn how to use the viewer in preview mode.

```bash 
[student@workstation ~]$ man evince
...output omitted...
 -i, --page-index=NUMBER
 Open the document on the page with the specified page index (this is
 the exact page number, not a page label).
...output omitted...
 -w, --preview
 Run evince as a previewer.
...output omitted...
Press q to quit the man page.
```

The -w (or --preview) option opens evince in preview mode. The -i option
opens evince at the specified starting page.

Use the evince command to open the /home/student/passwd.ps file.

```bash 
[student@workstation ~]$ evince /home/student/passwd.ps
```

Note

## Use the evince -w /home/student/passwd.ps command to open the file in preview mode.

```bash 
[student@workstation ~]$ evince -w /home/student/passwd.ps
```

Use the evince -i 3 /home/student/passwd.ps command to open the file at


[student@workstation ~]$ evince -i 3 /home/student/passwd.ps

While the normal evince mode supports full-screen and presentation-style
viewing, the evince preview mode is useful for quick browsing and printing. Notice
the print icon at the top.

By using the man pages, research lp(1) to determine how to print any document to start on a
specific page. Without entering any commands (in the absence of printers), learn the syntax,
in one command, to print only pages 2 and 3 of your PostScript file.

## Use the man lp command to determine how to print specific pages of a document.

```bash 
[student@workstation ~]$ man lp
...output omitted...
 -P page-list
 Specifies which pages to print in the document. The list can contain
 a list of numbers and ranges (-) separated by commas, e.g., "1,3-5, 16". The page
 numbers refer to the output pages and not the document's original pages - options
 like "number-up" can affect the numbering of the pages.
...output omitted...

```







[[Index]] 