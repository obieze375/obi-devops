[[Index]] 

## Change Hostname:

``` 

hostnamectl set-hostname 'host_name'

  

  

hostnamectl set-hostname 'node01'  

``` 

  

## Checking Open files  

  
• lsof

  

``` 

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• init 1 root cwd DIR 253,0 4096 2 /

  

• init 1 root rtd DIR 253,0 4096 2 /

  

• init 1 root txt REG 253,0 145180 147164 /sbin/init

  

• init 1 root mem REG 253,0 1889704 190149 /lib/libc-2.12.so

  

• init 1 root 0u CHR 1,3 0t0 3764 /dev/null

  

• init 1 root 1u CHR 1,3 0t0 3764 /dev/null

  

• init 1 root 2u CHR 1,3 0t0 3764 /dev/null

  

• init 1 root 3r FIFO 0,8 0t0 8449 pipe

  

• init 1 root 4w FIFO 0,8 0t0 8449 pipe

  

• init 1 root 5r DIR 0,10 0 1 inotify

  

• init 1 root 6r DIR 0,10 0 1 inotify

  

• init 1 root 7u unix 0xc1513880 0t0 8450 socket

``` 

  

## List User Specific Open Files  

  

``` 

• lsof -u cyberpunk

  

  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• sshd 1838 cyberpunk cwd DIR 253,0 4096 2 /

  

• sshd 1838 cyberpunk rtd DIR 253,0 4096 2 /

  

• sshd 1838 cyberpunk txt REG 253,0 532336 188129 /usr/sbin/sshd

  

• sshd 1838 cyberpunk mem REG 253,0 19784 190237 /lib/libdl-2.12.so

  

• sshd 1838 cyberpunk mem REG 253,0 122436 190247 /lib/libselinux.so.1

  

• sshd 1838 cyberpunk mem REG 253,0 255968 190256

  

/lib/libgssapi_krb5.so.2.2

  

• sshd 1838 cyberpunk mem REG 253,0 874580 190255 /lib/libkrb5.so.3.3

  

 ``` 

  

## Find Processes running on Specific Port

  


``` 
lsof -i TCP:22
``` 

  
``` 
  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• sshd 1471 root 3u IPv4 12683 0t0 TCP *:ssh (LISTEN)

  

• sshd 1471 root 4u IPv6 12685 0t0 TCP *:ssh (LISTEN)

``` 

  

## List Only IPv4 & IPv6 Open Files  

  

 ``` 
# lsof -i 4

  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• rpcbind 1203 rpc 6u IPv4 11326 0t0 UDP *:sunrpc

  

• rpcbind 1203 rpc 7u IPv4 11330 0t0 UDP *:954

  

• rpcbind 1203 rpc 8u IPv4 11331 0t0 TCP *:sunrpc (LISTEN)

  

• avahi-dae 1241 avahi 13u IPv4 11579 0t0 UDP *:mdns

  

• avahi-dae 1241 avahi 14u IPv4 11580 0t0 UDP *:58600 

``` 


 lsof -i 6

 ``` 

  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• rpcbind 1203 rpc 9u IPv6 11333 0t0 UDP *:sunrpc

  

• rpcbind 1203 rpc 10u IPv6 11335 0t0 UDP *:954

  

• rpcbind 1203 rpc 11u IPv6 11336 0t0 TCP *:sunrpc (LISTEN)

  

• rpc.statd 1277 rpcuser 10u IPv6 11858 0t0 UDP *:55800

  

• rpc.statd 1277 rpcuser 11u IPv6 11862 0t0 TCP *:56428 (LISTEN)

  

• cupsd 1346 root 6u IPv6 12112 0t0 TCP localhost:ipp (LISTEN)

  ```  

 

  

## List Open Files of TCP Port ranges 1-1024  

  

``` 

lsof -i TCP:1-1024 

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• rpcbind 1203 rpc 11u IPv6 11336 0t0 TCP *:sunrpc (LISTEN)

  

• cupsd 1346 root 7u IPv4 12113 0t0 TCP localhost:ipp (LISTEN)

  

• sshd 1471 root 4u IPv6 12685 0t0 TCP *:ssh (LISTEN)

  

• master 1551 root 13u IPv6 12898 0t0 TCP localhost:smtp (LISTEN)

  

• sshd 1834 root 3r IPv4 15101 0t0 TCP 192.168.0.2:ssh->192.168.0.1:conclave-cpp (ESTABLISHED)

  

• sshd 1838 cyberpunk 3u IPv4 15101 0t0 TCP 192.168.0.2:ssh->192.168.0.1:conclave-cpp

  

(ESTABLISHED)

  

• sshd 1871 root 3r IPv4 15842 0t0 TCP 192.168.0.2:ssh->192.168.0.1:groove (ESTABLISHED)

  

• httpd 1918 root 5u IPv6 15991 0t0 TCP *:http (LISTEN)

  

• httpd 1918 root 7u IPv6 15995 0t0 TCP *:https (LISTEN)
```   

  
## Exclude User with ‘^’ Character 


``` 

lsof -i -u^root  
  
• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• rpcbind 1203 rpc 6u IPv4 11326 0t0 UDP *:sunrpc

  

• rpcbind 1203 rpc 7u IPv4 11330 0t0 UDP *:954

  

• rpcbind 1203 rpc 8u IPv4 11331 0t0 TCP *:sunrpc (LISTEN)

  

• rpcbind 1203 rpc 9u IPv6 11333 0t0 UDP *:sunrpc

  

• rpcbind 1203 rpc 10u IPv6 11335 0t0 UDP *:954

  

• rpcbind 1203 rpc 11u IPv6 11336 0t0 TCP *:sunrpc (LISTEN)

  

• avahi-dae 1241 avahi 13u IPv4 11579 0t0 UDP *:mdns

  

• avahi-dae 1241 avahi 14u IPv4 11580 0t0 UDP *:58600

  

• rpc.statd 1277 rpcuser 5r IPv4 11836 0t0 UDP *:soap-beep

  

• rpc.statd 1277 rpcuser 8u IPv4 11850 0t0 UDP *:55146

  

• rpc.statd 1277 rpcuser 9u IPv4 11854 0t0 TCP *:32981 (LISTEN)

  

• rpc.statd 1277 rpcuser 10u IPv6 11858 0t0 UDP *:55800

  

• rpc.statd 1277 rpcuser 11u IPv6 11862 0t0 TCP *:56428 (LISTEN)
 
``` 


## Find Out who’s Looking What Files and Commands? 
  


``` 
lsof -i –u

  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• bash 1839 cyberpunk cwd DIR 253,0 12288 15 /etc

  

• ping 2525 cyberpunk cwd DIR 253,0 12288 15 /etc  

``` 

  

## List all Network Connections  
  

```   
lsof -i


• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• rpcbind 1203 rpc 6u IPv4 11326 0t0 UDP *:sunrpc

  

• rpcbind 1203 rpc 7u IPv4 11330 0t0 UDP *:954

``` 

  

## Search by PID
  

``` 
lsof -p 1  

• COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

  

• init 1 root cwd DIR 253,0 4096 2 /

  

• init 1 root rtd DIR 253,0 4096 2 /

  

• init 1 root txt REG 253,0 145180 147164 /sbin/init

  

• init 1 root mem REG 253,0 1889704 190149 /lib/libc-2.12.so

  

• init 1 root mem REG 253,0 142472 189970 /lib/ld-2.12.so
   

``` 

## Kill all Activity of Particular User 

``` 
kill -9 `lsof -t -u  
``` 


## Network Interfaces:
  
## Bring Interface up

``` 

ifup eth0  

``` 


## Bring Interface down

``` 
  
ifdown eth0  
  

``` 

  

## Network Interface

  
## Check network interfaces:

``` 

 ip addr  

``` 

 

```   

Monitor:

  

root@server1 ~]# ip addr show  

  

  

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN

  

 group default qlen 1000

  

 link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

  

 inet 127.0.0.1/8 scope host lo

  

 valid_lft forever preferred_lft forever

  

 inet6 ::1/128 scope host  

  

 valid_lft forever preferred_lft forever

  

2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel

  

 state UP group default qlen 1000

  

 link/ether 00:0c:29:50:9e:c9 brd ff:ff:ff:ff:ff:ff

  

 inet 192.168.4.210/24 brd 192.168.4.255 scope global dynamic

  

 noprefixroute ens33

  

 valid_lft 1370sec preferred_lft 1370sec

  

 inet6 fe80::959:3b1a:9607:8928/64 scope link noprefixroute  

  

 valid_lft forever preferred_lft forever  

``` 

 
 
 
 
 
## List out all connections  


## The first and most simple command is to list out all the current connections.

  

``` 

$ netstat -a  

  

  

Active Internet connections (servers and established)

  

Proto Recv-Q Send-Q Local Address Foreign Address State

  

tcp 0 0 enlightened:domain : LISTEN

  

tcp 0 0 localhost:ipp : LISTEN

  

tcp 0 0 enlightened.local:54750 li240-5.members.li:http ESTABLISHED

  

tcp 0 0 enlightened.local:49980 del01s07-in-f14.1:https ESTABLISHED

  

tcp6 0 0 ip6-localhost:ipp [::]:* LISTEN

  

``` 

  

## List only TCP or UDP connections
  

``` 

netstat -at

  

  

Active Internet connections (servers and established)

  

Proto Recv-Q Send-Q Local Address Foreign Address State

  

tcp 0 0 enlightened:domain : LISTEN

  

tcp 0 0 localhost:ipp : LISTEN

  

tcp 0 0 enlightened.local:36310 del01s07-in-f24.1:https

  

ESTABLISHED

  

tcp 0 0 enlightened.local:45038 a96-17-181-10.depl:http

  

ESTABLISHED

  
``` 

  

## List udp connections only  

  

``` 

$ netstat -au  

  

  

Active Internet connections (servers and established)

  

Proto Recv-Q Send-Q Local Address Foreign Address State

  

udp 0 0 *:34660 :

  

udp 0 0 enlightened:domain :

  

udp 0 0 *:bootpc :

  

udp 0 0 enlightened.local:ntp :

  

udp 0 0 localhost:ntp :

  

udp 0 0 *:ntp :

  

``` 

  

## Disable reverse dns lookup for faster output  

  

``` 

 $ netstat -ant  

  

  

Active Internet connections (servers and established)

  

Proto Recv-Q Send-Q Local Address Foreign Address State

  

tcp 0 0 127.0.1.1:53 0.0.0.0:* LISTEN

  

tcp 0 0 127.0.0.1:631 0.0.0.0:* LISTEN

  

tcp 0 0 192.168.1.2:49058 173.255.230.5:80 ESTABLISHED

  

tcp 0 0 192.168.1.2:33324 173.194.36.117:443 ESTABLISHED

  

tcp6 0 0 ::1:631 :::* LISTEN

``` 

  

## List out only listening connections  

  

``` 

• $ netstat -tnl

  

  

Active Internet connections (only servers)

  

Proto Recv-Q Send-Q Local Address Foreign Address State

  

tcp 0 0 127.0.1.1:53 0.0.0.0:* LISTEN

  

tcp 0 0 127.0.0.1:631 0.0.0.0:* LISTEN

  

ffctcp6 0 0 ::1:631 :::* LISTEN

``` 

  

## netstat -tulpn  

  

``` 
netstat -tulpn is merge between tcp and udp
```   

  

## Get process name/pid and user id  

  

  
``` 
sudo netstat -nlpt


  
  

• Active Internet connections (only servers)

  

• Proto Recv-Q Send-Q Local Address Foreign Address State

  

PID/Program name

  

• tcp 0 0 127.0.1.1:53 0.0.0.0:* LISTEN

  

1144/dnsmasq

  

• tcp 0 0 127.0.0.1:631 0.0.0.0:* LISTEN

  

661/cupsd

  

• tcp6 0 0 ::1:631 :::* LISTEN 661/cupsd

  

e option along with the p option to get the

  

username too.

``` 

## sudo netstat -ltpe

``` 

• Active Internet connections (only servers)

  

• Proto Recv-Q Send-Q Local Address Foreign Address State

  

User Inode PID/Program name

  

• tcp 0 0 enlightened:domain : LISTEN root

  

11090 1144/dnsmasq

  

• tcp 0 0 localhost:ipp : LISTEN root 9755

  

661/cupsd

  

• tcp6 0 0 ip6-localhost:ipp [::]:* LISTEN root 9754

  

661/cupsd

``` 

  

## Print statistics

  

``` 

• $ netstat -s

  
  
  

Ip:

  

32797 total packets received

  

0 forwarded

  

0 incoming packets discarded

  

32795 incoming packets delivered

  

29115 requests sent out

  

60 outgoing packets dropped

  

``` 

  

## Display kernel routing information

  

``` 

  

• $ netstat -rn  

  

  

• Kernel IP routing table

  

• Destination Gateway Genmask Flags MSS Window irtt

  

Iface

  

• 0.0.0.0 192.168.1.1 0.0.0.0 UG 0 0 0 eth0

  

• 192.168.1.0 0.0.0.0 255.255.255.0 U 0 0 0 eth0

  

``` 

  

## Print network interfaces

  

``` 

• $ netstat -ie  

  

  

Kernel Interface table

  

eth0 Link encap:Ethernet HWaddr 00:16:36:f8:b2:64

  

inet addr:192.168.1.2 Bcast:192.168.1.255 Mask:255.255.255.0

  

inet6 addr: fe80::216:36ff:fef8:b264/64 Scope:Link

  

UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1

  

RX packets:31682 errors:0 dropped:0 overruns:0 frame:0

  

TX packets:27573 errors:0 dropped:0 overruns:0 carrier:0

  

collisions:0 txqueuelen:1000

  

RX bytes:29637117 (29.6 MB) TX bytes:4590583 (4.5 MB)

  

 Interrupt:18 Memory:da000000-da020000  

``` 

  

## Display multicast group information

  

``` 

netstat -g  

  

  

Check if a service is running

  

 [ansibleadm@uk-lon-lofm08 ~]$ netstat -tupn  

  

  

(Not all processes could be identified, non-owned process info

  

will not be shown, you would have to be root to see it all.)

  

Active Internet connections (w/o servers)

  

Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program

  

name

  

tcp 0 0 10.61.223.188:43192 10.61.239.11:1984 TIME_WAIT -

  

tcp 0 0 10.61.223.188:43194 10.61.239.11:1984 TIME_WAIT -

  

tcp 0 0 10.61.223.188:43190 10.61.239.11:1984 TIME_WAIT -

  

tcp 0 0 10.61.223.188:43193 10.61.239.93:1984 TIME_WAIT -

  

tcp 0 0 10.61.223.188:43191 10.61.239.93:1984 TIME_WAIT -

  

tcp 0 0 10.61.223.188:22 10.14.40.60:54034 ESTABLISHED -      
``` 

  

## Netstat with grep  

  

``` 

netstat -nap |grep :3506  

``` 

  

## Testing Connectivity with Ping  

  

  

## Network Troubleshooting:

  

## Schedule

  

``` 

  

. Define problem

. Analyse problem

. Eliminate potential causes

. Propose Hypothesis

. Test Hypothesis

. Solve problem and document solution

``` 

  
  

Format:

  

  

## ping HOST

  

``` 

ping -c COUNT HOST

``` 

``` 

Example:

  

ping -c 3 google.com

  

  

$ ping -c 3 google.com

  

  

PING google.com (216.58.2.7) 56 bytes of data.

  

64 bytes from 216.58.2.7: icmp_seq=1 ttl=53 time=20.1 ms

  

64 bytes from 216.58.2.7: icmp_seq=2 ttl=53 time=20.2 ms

  

64 bytes from 216.58.2.7: icmp_seq=3 ttl=53 time=23.9 ms

  

--- google.com ping statistics ---

  

3 packets transmitted, 3 received, 0% packet loss, time

  

2004ms

  

rtt min/avg/max/mdev = 21.489/22.924/24.154/1.111 ms

``` 

``` 

$ ping -c 3 google.com  

  

  

PING google.com (216.58.2.7) 56 bytes of data.

  

From 216.58.2.7 icmp_seq=1 Destination Host Unreachable

  

From 216.58.2.7 icmp_seq=2 Destination Host Unreachable

  

From 216.58.2.7 icmp_seq=3 Destination Host Unreachable

  

--- google.com ping statistics ---

  

3 packets transmitted, 0 received, +3 errors, 100% packet

  

loss, time 2002ms

  

pipe 3

``` 

``` 

$ ping -c 3 10.0.2.2  

  

  

PING 10.0.2.2 (10.0.2.2) 56(84) bytes of data.

  

64 bytes from 10.0.2.2: icmp_seq=1 ttl=63 time=0.272 ms

  

64 bytes from 10.0.2.2: icmp_seq=2 ttl=63 time=0.103 ms

  

64 bytes from 10.0.2.2: icmp_seq=3 ttl=63 time=0.202 ms

  

--- 10.0.2.2 ping statistics ---

  

3 packets transmitted, 3 received, 0% packet loss, time

  

2001ms

  

rtt min/avg/max/mdev = 0.103/0.192/0.272/0.070 ms

``` 

  

## Traceroute  

  

``` 

traceroute -n google.com

  

  

traceroute to google.com (216.58.2.7), 30 hops

  

max, 60 byte packets

  

Diagnosing Network Connections 413

  

1 10.0.2.2 0.296 ms 0.178 ms 0.220 ms

  

2 192.168.1.1 2.529 ms 2.713 ms 2.630 ms

  

3 72.14.237.231 23.750 ms 22.087 ms

  

12.122.132.137 22.701 ms

  

4 216.58.216.78 20.549 ms 12.250.16.30 22.904

  

ms 216.58.216.78 20.724 ms

``` 

  

``` 

$ tracepath -n google.com

  

1?: [LOCALHOST] pmtu 1500

  

1: 10.0.2.2 0.470ms

  

1: 10.0.2.2 0.649ms

  

2: 192.168.1.1 2.147ms asymm 64

``` 

...

  

## Using curl to check firewall connectivity  

``` 

curl -k https://<url>:<port.no>

``` 

  

## Download files with wget

``` 

wget --no-check-certificate

``` 

## Netstat  

  

  

## The netstat Command

  

``` 

-n Display numerical addresses and ports.

  

-i Displays a list of network interfaces.

  

-r Displays the route table. (netstat -rn)

  

-p Display the PID and program used.

  

-l Display listening sockets. (netstat -nlp)

  

-t Limit the output to TCP (netstat -ntlp)

  

-u Limit the output to UDP (netstat -nulp)

``` 

``` 

[jason@linuxsvr ~]$ netstat -i

  

  

Kernel Interface table

  

Iface MTU RX-OK RX-ERR RX-DRP RX-OVR TX-OK TX-ERR TX-DRP TX-OVR Flg

  

eth0 1500 3975 0 0 0 2627 0 0 0 BMRU

  

lo 65536 8 0 0 0 8 0 0 0 LRU

``` 

``` 

[jason@linuxsvr ~]$ netstat -rn  

  

  

Kernel IP routing table

  

Destination Gateway Genmask Flags MSS Window irtt Iface

  

0.0.0.0 10.0.2.2 0.0.0.0 UG 0 0 0 eth0

  

10.0.2.0 0.0.0.0 255.255.255.0 U 0 0 0 eth0

``` 

``` 

[jason@linuxsvr ~]$ sudo netstat -ntlp  

  

  

Active Internet connections (only servers)

  

Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program

  

name

  

tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN 943/sshd

  

tcp 0 0 127.0.0.1:25 0.0.0.0:* LISTEN 1313/master

``` 

  

## Telnet  

  

``` 

telnet HOST_OR_IP PORT_NUMBER

  

$ telnet google.com 80

  

Trying 216.58.2.7...

  

Connected to google.com.

  

Escape character is '^]'.

  

GET /

  

HTTP/1.0 200 OK

  

^]

  

telnet> quit

  

closed.

``` 

  

``` 

  

[root@server1 ~]# nmcli con show  

  

  

NAME UUID TYPE DEVICE  

  

ens33 db6f53bd-654e-45dd-97ef-224514f8050a ethernet ens33  

  

Connection Properties

  

[root@server1 ~]# nmcli con show ens33

  

connection.id: ens33

  

connection.uuid: db6f53bd-654e-45dd-97ef-

  

 224514f8050a

  

connection.stable-id: --

  

connection.type: 802-3-ethernet

  

connection.interface-name: ens33

  

connection.autoconnect: yes

  

connection.autoconnect-priority: 0

  

connection.autoconnect-retries: -1 (default)

  

connection.multi-connect: 0 (default)

  

connection.auth-retries: -1

  

connection.timestamp: 1558778720

  

connection.read-only: no

  

connection.permissions: --

  

connection.zone: --

  

connection.master: --

  

connection.slave-type: --

  

connection.autoconnect-slaves: -1 (default)  

``` 

  

## Configure eth0 interface with ipv6 and DNS

  

```   

• Already existing IPv4 network configurations should not be impacted.  

  

  

Command Action/Description  

  

  

Configuring ipv6 on ethernet interface - nmcli connection modify system ipv6.addresses 2020::1/64  

  

ipv6.dns 2020::2 ipv6.method manual

  

  

To restart/activate connection - nmcli connection up system  

  

  

To display IP Address configurations - ip address show  

  

  

To display connection information - nmcli connection show system  

  

  

To verify configured DNS IP address - more /etc/resolv.conf  

  

  

To display Manual page for nmcli - man nmcli  

  

  

To display Manual page for nmcli-examples - man nmcli-examples  

  

``` 

  

## Configure Static Route:

  

Configure static route on system.example.com for destination 10.1.1.0/24 via 192.168.99.30.  

  

``` 

• Route configuration must be persistent after reboot.

  

• eth0 should be used as exit interface.  

  

  

Command Action/Description  

  

  

Adding static route in runtime - ip route add 10.1.1.0/24 via 192.168.99.30  

  

  

To display route(s) - ip route show or route -n  

  

  

To add persistent route using command line - nmcli connection modify system ipv4.routes “10.1.1.0/24 192.168.99.30”  

  

  

To add persistent route using config file  

  

  

vim /etc/sysconfig/network-scripts/route-system  

  

  

10.1.1.0/24 via 192.168.99.30 dev eth0  

  

  

:wq  

``` 

## Configure hostname resolution:

  
  

• Set the hosts file as priority for hostname resolution in nsswitch.conf file.  

  

• Test if hostname resolution is working fine.  

  
## Command Action/Description
  

``` 

To add entry in hosts file  

  

  

vim /etc/hosts

  

192.168.99.20 system1.example.com  

  

:wq

  

  

To verify hostname resolution is working fine - getent hosts system1.example.com  

  

To restart/activate connection - nmcli connection up system  

  
  

Note :

  

Remove ssh service from services list ,if you don’t remove ssh service ,then rich rule configured to accept ssh traffic from 192.168.99.0/24

  

network only will not be effective. This is due to order in which firewalld evaluates the different definitions on firewall. If firewalld will find

  

ssh service on services list, it will allow access irrespective of accessing network and rich rule will be ignored.

  

  

To Test This :  

  

We have only one network, so it is not possible to test this. To test this working of rule , you just add this rule to allow access for some host  

  

not on 192.168.99.0/24 network and then test ssh connection from ipaserver.example.com, it must be denied.

  

``` 

  

Command Action/Description  

  

``` 

  

Displaying firewall configurations - firewall-cmd --list-all  

  

  

Adding firewalld rich rule to accept traffic form 192.168.99.0/24 network - firewall-cmd --add-rich-rule ‘rule family=“ipv4” source address=“192.168.99.0/24” service name=“ssh” accept’ --permanent

  

  

Removing ssh service from services list - firewall-cmd --remove-service=ssh --permanent  

  

  

Reloading firewall to make changes effective - firewall-cmd --reload  

  

  

To verify firewall configs after making changes - firewall-cmd --list-all  

``` 

## Packet Capture:

  

``` 

tcpdump

  

  

-n Display numerical addresses and ports.

  

-A Display ASCII (text) output.

  

-v Verbose mode. Produce more output.

  

-vvv Even more verbose output.

``` 

  

$ sudo tcpdump  

  

``` 

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode

  

listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes

  

19:25:49.639495 IP linuxsvr.ssh > 10.0.2.2.64440: Flags [P.], seq 3312803324:3312803408, ack

  

2443835, win 40880, length 84

  

19:25:49.639586 IP linuxsvr.ssh > 10.0.2.2.64440: Flags [P.], seq 84:120, ack 1, win 40880, length 36

  

19:25:49.639750 IP 10.0.2.2.64440 > linuxsvr.ssh: Flags [.], ack 84, win 65535, length 0

  

19:25:49.639763 IP 10.0.2.2.64440 > linuxsvr.ssh: Flags [.], ack 120, win 65535, length 0  

``` 



``` 

$ sudo tcpdump –Anvvv  

  

  

tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes

  

19:44:27.067530 IP (tos 0x10, ttl 64, id 5120, offset 0, flags [DF], proto TCP (6), length 64)

  

10.0.2.44.37534 > 10.0.2.15.80: Flags [P.], cksum 0xfe34 (incorrect -> 0xce40), seq 1:13, ack 1, win

  

683, options [nop,nop,TS val 1585227 ecr 1584441], length 12

  

E..@..@.@.(............P..>.:........4.....

  

..0K..-9GET /about

``` 

  

  

``` 

tcpdump -w /temp/traffic.pcap -i eno1 -v host 9.9.9.9  

``` 

  

``` 

tcpdump -w /<dir>/traffic.pcap -i <interface> -v host <ip_addr_interface_is_sending_traffic_to>

``` 

  

## Firewall Management

  

## IP Tables Method

## Start/Stop/Restart Iptables Firewall

  

``` 

------------ On Cent/RHEL 7 and Fedora 22+ ------------

  

  • yum install iptables-services

  

  • systemctl status iptables

  

  • systemctl start iptables

  

  • systemctl stop iptables

  • systemctl restart iptables

``` 

  
  

## On SysVinit based Linux Distributions

  

``` 

------------ On Cent/RHEL 6/5 and Fedora ------------

  

  • -/etc/init.d/iptables start

  

  • /etc/init.d/iptables stop

  • /etc/init.d/iptables restart

``` 

## Check all IPtables Firewall Rules

``` 

iptables -L -n -v

``` 

## Block Specific IP Address in IPtables Firewall

``` 

iptables -A INPUT -s xxx.xxx.xxx.xxx -j DROP

  

iptables -A INPUT -p tcp -s xxx.xxx.xxx.xxx -j DROP


iptables -D INPUT -s xxx.xxx.xxx.xxx -j DROP
``` 

## Block Specific Port on IPtables Firewall

## To block outgoing connections on a specific port use: 

``` 
iptables -A OUTPUT -p tcp --dport xxx -j DROP
``` 

## Unblock IP Address in IPtables Firewall

## To allow incoming connections use:

``` 

iptables -A INPUT -p tcp --dport xxx -j ACCEPT

``` 



``` 
In both examples change "xxx" with the actual port you wish to allow. If you want to block UDP traffic instead of TCP, simply change "tcp" with

"udp" in the above iptables rule.

``` 



## Allow Multiple Ports on IPtables using Multiport

``` 

You can allow multiple ports at once, by using multiport, below you can

find such rule for both incoming and outgoing connections:

iptables -A INPUT -p tcp -m multiport --dports 22,80,443 -j ACCEPT

  
iptables -A OUTPUT -p tcp -m multiport --sports 22,80,443 -j ACCEPT



``` 

## Allow Specific Network Range on Particular Port on IPtables

``` 
You may want to limit certain connections on specific port to a given

network. Let’s say you want to allow outgoing connections on port 22

to network 192.168.100.0/24.

``` 



``` 
iptables -A OUTPUT -p tcp -d 192.168.100.0/24 --dport 22 -j ACCEPT
``` 

## Block Facebook on IPtables Firewall

``` 
host facebook.com

facebook.com has address 66.220.156.68
``` 
  
``` 
whois 66.220.156.68 | grep CIDR

CIDR: 66.220.144.0/20

``` 

You can then block that Facebook network with:

  

``` 

iptables -A OUTPUT -p tcp -d 66.220.144.0/20 -j DROP

``` 

# Setup Port Forwarding in Iptables

``` 

iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 25 -j REDIRECT -- to-port 2525

``` 

# Block Network Flood on Apache Port with Iptables

``` 

iptables -A INPUT -p tcp --dport 80 -m limit --limit 100/minute --limitburst 200 -j ACCEPT

``` 

# Block Incoming Ping Requests on IPtables

  

``` 

iptables -A INPUT -p icmp -i eth0 -j DROP

``` 

# Block Incoming Ping Requests on IPtables

``` 

iptables -A INPUT -p icmp -i eth0 -j DROP

``` 

  

# Allow loopback Access

  

Loopback access (access from 127.0.0.1) is important and you should

always leave it active:

``` 

iptables -A INPUT -i lo -j ACCEPT

  

iptables -A OUTPUT -o lo -j ACCEPT

``` 

# Keep a Log of Dropped Network Packets on Iptables

  

If you want to log the dropped packets on network interface eth0, you

can use the following command:

``` 

iptables -A INPUT -i eth0 -j LOG --log-prefix "IPtables dropped packets:"  
  
``` 

``` 
You can change the value after "--log-prefix" with something by your

choice. The messages are logged in /var/log/messages and you can

search for them with:

``` 



``` 

grep "IPtables dropped packets:" /var/log/messages
  

You can block access to your system from specific MAC address by

using:

``` 

## Block Access to Specific MAC Address on IPtables

``` 
iptables -A INPUT -m mac --mac-source 00:00:00:00:00:00 -j DROP

Of course, you will need to change "00:00:00:00:00:00" with the actual

MAC address that you want to block.

``` 

# Limit the Number of Concurrent Connections per IP Address

  

• If you don’t want to have too many concurrent connection established

from single IP address on given port you can use the command below:

``` 

iptables -A INPUT -p tcp --syn --dport 22 -m connlimit --connlimit-above 3 -j REJECT

``` 

The above command allows no more than 3 connections per client. Of

course, you can change the port number to match different service. Also

the --connlimit-above should be changed to match your requirement.

  

# Search within IPtables Rule

  

Once you have defined your iptables rules, you will want to search from time

to time and may need to alter them. An easy way to search within your rules

is to use:

``` 

iptables -L $table -v -n | grep $string

``` 

In the above example, you will need to change $table with the actual table

within which you wish to search and $string with the actual string for which

you are looking for.

Here is an example:

``` 

iptables -L INPUT -v -n | grep 192.168.0.100

``` 

# Define New IPTables Chain

  

With iptables, you can define your own chain and store custom rules in

it. To define a chain, use:

``` 

iptables -N custom-filter

``` 

Now you can check if your new filter is there: 

``` 
iptables -L

``` 

## Flush IPtables Firewall Chains or Rules

If you want to flush your firewall chains, you can use:

``` 

iptables -F
``` 

You can flush chains from specific table with:

```   
iptables -t nat -F
``` 

You can change "nat" with the actual table which chains you wish to flush.

## Save IPtables Rules to a File

If you want to save your firewall rules, you can use the iptables-save

command. You can use the following to save and store your rules in a

file:

``` 

 iptables-save > ~/iptables.rules

``` 

It’s up to you where will you store the file and how you will name it.

## Restore IPtables Rules from a File

``` 

If you want to restore a list of iptables rules, you can use iptablesrestore. The command looks like this:

  

iptables-restore < ~/iptables.rules

Of course the path to your rules file might be different.

Setup IPtables Rules for PCI Compliance

Some system administrators might be required to configure their servers to be PCI compiliant. There

are many requirements by different PCI compliance vendors, but there are few common ones.

In many of the cases, you will need to have more than one IP address. You will need to apply the

rules below for the site’s IP address.

  

Be extra careful when using the rules below and use them onlyif you are sure what you are doing:

``` 


``` 
iptables -I INPUT -d SITE -p tcp -m multiport --dports 21,25,110,143,465,587,993,995 -j DROP
``` 

``` 

If you use cPanel or similar control panel, you may need to block it’s’ ports as well. Here is an example:

``` 

``` 

iptables -I in_sg -d DEDI_IP -p tcp -m multiport --dports 2082,2083,2095,2096,2525,2086,2087 -j DROP

``` 

## Allow Established and Related Connections

  

As the network traffic is separate on incoming and outgoing, you will

want to allow established and related incoming traffic. For incoming

connections do it with:

``` 
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
``` 

For outgoing use:

``` 
iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED -j ACCEPT
``` 


## Drop Invalid Packets in Iptables


``` 

It’s possible to have some network packets marked as invalid. Some

people may prefer to log those packages, but others prefer to drop

them. To drop invalid the packets, you can use:

``` 


``` 
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
``` 

## Block Connection on Network Interface


Some systems may have more than one network interface. You can

limit the access to that network interface or block connections from

certain IP address.

For example:

``` 
iptables -A INPUT -i eth0 -s xxx.xxx.xxx.xxx -j DROP
``` 

Change “xxx.xxx.xxx.xxx” with the actual IP address (or network) that

you wish to block.

## Disable Outgoing Mails through IPTables

If your system should not be sending any emails, you can block

outgoing ports on SMTP ports. For example you can use this:


``` 
iptables -A OUTPUT -p tcp --dports 25,465,587 -j REJECT
``` 

## UFW Method

## Enable UFW

```             

sudo ufw enable

``` 

# Using IPv6 with UFW (Optional)

  
``` 
sudo nano /etc/default/ufw
``` 
  

Then make sure the value of IPV6 is yes. It should look like this:

``` 

/etc/default/ufw excerpt

IPV6=yes

``` 

Save and close the file. Now, when UFW is enabled, it will be configured to write

both IPv4 and IPv6 firewall rules. However, before enabling UFW, we will want to

ensure that your firewall is configured to allow you to connect via SSH. Let’s start

with setting the default policies.

  

## Setting Up Default Policies

• If you’re just getting started with your firewall, the first rules to define are your default policies.

These rules control how to handle traffic that does not explicitly match any other rules. By

default, UFW is set to deny all incoming connections and allow all outgoing connections. This

means anyone trying to reach your server would not be able to connect, while any application

within the server would be able to reach the outside world.

• Let’s set your UFW rules back to the defaults so we can be sure that you’ll be able to follow along

with this tutorial. To set the defaults used by UFW, use these commands:

``` 

sudo ufw default deny incoming

sudo ufw default allow outgoing

``` 

These commands set the defaults to deny incoming and allow outgoing connections. These

firewall defaults alone might suffice for a personal computer, but servers typically need to

respond to incoming requests from outside users. We’ll look into that next.

  

## Allowing SSH Connections

``` 

sudo ufw allow ssh

  

sudo ufw allow 22

``` 

If you configured your SSH daemon to use a different port, you will

have to specify the appropriate port. For example, if your SSH server is

listening on port 2222, you can use this command to allow connections

on that port:

``` 

sudo ufw allow 2222

``` 

## Allowing Other Connections

  

• Specific Port Ranges

• You can specify port ranges with UFW. Some applications use multiple

ports, instead of a single port.

  

• For example, to allow X11 connections, which use ports 6000-6007, use

these commands:

``` 

sudo ufw allow 6000:6007/tcp

sudo ufw allow 6000:6007/udp

``` 

## Allowing specific addresses and ports

``` 

sudo ufw allow from 203.0.113.4

``` 

## Allow specific ip addresses at ports:  

``` 

sudo ufw allow from 203.0.113.4 to any port 22

``` 

Allowing Subnets using CIDR notation to specify a

netmask.

  

Example:

``` 
203.0.113.1 to 203.0.113.254
``` 

sudo ufw allow from 203.0.113.0/24

``` 
sudo ufw allow from 203.0.113.0/24 to any port 22
``` 

## Connections to a Specific Network Interface

  

Use ip addr or ifconfig to obtain network interface

``` 

sudo ufw allow in on eth0 to any port 80

  

sudo ufw allow in on eth1 to any port 3306

``` 

## Denying Connections

For example, to deny HTTP connections, you could use this command:

``` 
sudo ufw deny http
``` 

Or if you want to deny all connections from 203.0.113.4 you could use

this command:

``` 
sudo ufw deny from 203.0.113.4
``` 

## Deleting Rules

  

By Rule Number:

``` 
sudo ufw status numbered
``` 


``` 
Numbered Output:

Status: active

Action

From

To

[ 1] 22

[ 2] 80

ALLOW IN

ALLOW IN

15.15.15.0/24

Anywhere

 ```  

If we decide that we want to delete rule 2, the one that allows port 80 (HTTP) connections,

we can specify it in a UFW delete command like this:

``` 

sudo ufw delete 2

``` 

## Deleting by Actual Rule

  

For example to remove the allow http rule, you could write it like this:

``` 
sudo ufw delete allow http
```   

You could also specify the rule by allow 80, instead of by service name:

``` 
sudo ufw delete allow 80
``` 

This method will delete both IPv4 and IPv6 rules, if they exist.

  

## Checking UFW Status

At any time, you can check the status of UFW with this command:

``` 

sudo ufw status verbose

``` 

If UFW is disabled, which it is by default, you’ll see something like this:

  

Output

```           

Status: inactive

Checking UFW Status Continued

• Output

• Status: active

• Logging: on (low)

• Default: deny (incoming), allow (outgoing), disabled (routed)

• New profiles: skip

Action

From

• To

• --

• 22/tcp ALLOW IN Anywhere

• Use the status command if you want to check how UFW has configured the firewall.

Disabling or Resetting UFW (optional)

``` 

If you decide you don’t want to use UFW, you can disable it with this command:

``` 

sudo ufw disable

``` 

• Any rules that you created with UFW will no longer be active. You can

always run sudo ufw enable if you need to activate it later.

• If you already have UFW rules configured but you decide that you want to

start over, you can use the reset command:

``` 
sudo ufw reset
```   


## Alternative way to allow and deny connections

``` 

ufw allow from 0.0.0.0 to any port 9090 proto tcp/udp

ufw deny from 0.0.0.0 to any port 9090 proto tcp/udp

``` 


[[Index]] 


