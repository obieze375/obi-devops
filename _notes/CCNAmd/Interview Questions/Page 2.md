


What is the difference between TCP and UDP? 

   Connection and connectionless

TCP is a connection-oriented protocol and UDP is a connection-less protocol. TCP establishes a connection between a sender and receiver before data can be sent. UDP does not establish a connection before sending data.

## Reliability

TCP is reliable. Data sent using a TCP protocol is guaranteed to be delivered to the receiver. If data is lost in transit it will recover the data and resend it. TCP will also check packets for errors and track packets so that data is not lost or corrupted.

UDP is unreliable, it does not provide guaranteed delivery and a datagram packet may become corrupt or lost in transit.

## Flow control

TCP uses a flow control mechanism that ensures a sender is not overwhelming a receiver by sending too many packets at once. TCP stores data in a send buffer and receives data in a receive buffer. When an application is ready, it will read the data from the receive buffer. If the receive buffer is full, the receiver would not be able to handle more data and would drop it. To maintain the amount of data that can be sent to a receiver, the receiver tells the sender how much spare room in the receive buffer there is (receive window). Every time a packet is received, a message is sent to the sender with the value of the current receive window.

UDP does not provide flow control. With UDP, packets arrive in a continuous stream or they are dropped.

## Ordering

TCP does ordering and sequencing to guarantee that packets sent from a server will be delivered to the client in the same order they were sent. On the other hand, UDP sends packets in any order.

## Speed

TCP is slower than UDP because it has a lot more to do. TCP has to establish a connection, error-check, and guarantee that files are received in the order they were sent.

Usage

TCP is best suited to be used for applications that require high reliability where timing is less of a concern.

-   World Wide Web (HTTP, HTTPS)
-   Secure Shell (SSH)
-   File Transfer Protocol (FTP)
-   Email (SMTP, IMAP/POP)

UDP is best suited for applications that require speed and efficiency.

-   VPN tunneling
-   Streaming videos
-   Online games
-   Live broadcasts
-   Domain Name System (DNS)
-   Voice over Internet Protocol (VoIP)
-   Trivial File Transfer Protocol (TFTP)

● What is the purpose of a default gateway? 

A default gateway serves as an access point or IP router that a networked computer uses to send information to a computer in another network or the internet.

● What is command used to show the routing table on a Linux box? 

   netstat -r

● A TCP connection on a network can be uniquely defined by 4 things. What are those things?

        Connection type

         Ordering

          Reliability

           Speed 

● When a client running a web browser connects to a web server, what is the source port and destination port of the connection?  

Destination port is 80 and source port is chosen at random

● How do you add an IPv6 address to a specific interface?                        

Usage:

## /sbin/ip -6 addr add <ipv6address>/<prefixlength> dev <interface>

Example:

# /sbin/ip -6 addr add 2001:0db8:0:f101::1/64 dev eth0

● You have added an IPv4 and IPv6 address to interface eth0. A ping to the v4 address is working but a ping to the v6 address gives you the response sendmsg: operation not permitted. What could be wrong? 

This means that your server is not allowed to send ICMP packets.  
Check firewall rules:  
$ ip6tables -P INPUT ACCEPT  
$ ip6tables -P OUTPUT ACCEPT  
$ ip6tables -P FORWARD ACCEPT

From <[https://jivoi.github.io/2016/01/22/linux-sysadm-devops-interview-questions/#networking-questions](https://jivoi.github.io/2016/01/22/linux-sysadm-devops-interview-questions/#networking-questions)>

● What is SNAT and when should it be used?       

   Source Network Address Translation (source-nat or SNAT) allows traffic from a private network to go out to the internet. Virtual machines launched on a private network can get to the internet by going through a gateway capable of performing SNAT.

● Explain how could you ssh login into a Linux system that DROPs all new incoming packets using an SSH tunnel. 

## Reverse SSH tunnelling             

● How do you stop a DDoS attack? 

  Change url

   Block ip’s

● How can you see the contents of an ip packet? 

Wireshark  

● What is IPoAC (RFC 1149)? 

IP over Avian Carriers (IPoAC) is a proposal to carry [Internet Protocol](https://en.wikipedia.org/wiki/Internet_Protocol) (IP) [traffic](https://en.wikipedia.org/wiki/Internet_traffic) by [birds](https://en.wikipedia.org/wiki/Bird) such as [homing pigeons](https://en.wikipedia.org/wiki/Homing_pigeon). 

● What will happen when you bind port 0?             

     will ask the OS to listen on port 0. But as has been remarked above, most operating systems will see the 0 and invoke their particular customs for choosing a positive-numbered port for your application, so nc winds up listening on some port like 56514.

-   What is a Routing Table                                                          

Routers examine the destination IP address of a received packet and make routing decisions accordingly. To determine out which interface the packet will be sent, routers use routing tables. A routing table lists all networks for which routes are known. Each router’s routing table is unique and stored in the RAM of the device.  

Dynamic routing can be used to update this table 

-   The router first delivers and then receives routing messages over the router interface.
-   Dynamic Router messages also share the information with different routers that make use of the very same protocol.
-   Routers would then swap their routing information in order to discover the data regarding remote networks.
-   When soever the router finds a [change in the topology](https://www.educba.com/what-is-network-topology/), routing protocol advertises that particular topology change to all other routers.

From <[https://www.educba.com/dynamic-routing/](https://www.educba.com/dynamic-routing/)>

-   What Unicast, Broadcast, Multicast   

unicast: 1 to 1 transmission 

broadcast: transmission to all devices on the network 

Multicast: transmission to more than 2 devices on a network

-   What is 239 Addresses used for  

Multicast messages

networking fundamental knowledge (DHCP, DNS, STP, Inter-VLAN Routing, Spanning Tree, CAM Table and ARP, basic networking, TCP IP Model, IP Addressing,Cabling & Packet flows,loopback,TCP/UDP, Difference between Routing and Switching,

What does an 400 error mean and what does a 500 error

Routing and Switching on the OSI Model

From <[https://d.docs.live.net/147782087d2b6672/Documents/Linux_DevOps%20Interview%20Questions%20(1)%20(Repaired).docx](https://d.docs.live.net/147782087d2b6672/Documents/Linux_DevOps%20Interview%20Questions%20(1)%20(Repaired).docx)>