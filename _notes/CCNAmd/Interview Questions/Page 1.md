
   ##  Networking Questions

● What is localhost and why would ping localhost fail? 

    Localhost is the host name for this computer and iptables setting and DNS entries in the host file

-   Yes, iptables can firewall localhost from itself. If you've been playing with that, it would be an excellent place to look first.

-   Try running iptables -L -n. If you're not sure about how to read the output, post it.

-   A classic mistake is to set the policy to DENY and not to add an exception for localhost.

● What is the similarity between "ping" & "traceroute" ? How is traceroute able to find the hops.  

    Ping is a quick and easy utility to tell if the specified server is reachable and how long will it take to send and receive data from the server whereas Traceroute finds the exact route taken to reach the server and time taken by each step (hop). The packet expires after a period of time it hits a target before it expires it’s found a hop   

● What command or commands can be used to show all open ports and/or socket connections on a machine? 

  netstat -tulpn / netstat -tupn 

● Is 300.168.0.123 a valid IPv4 address?   

 IP address classes

Class    Address range

Class A    1.0.0.1 to 126.255.255.254

Class B    128.1.0.1 to 191.255.255.254

Class C    192.0.1.1 to 223.255.254.254

Class D    224.0.0.0 to 239.255.255.255

● Which IP ranges/subnets are "private" or "non-routable" (RFC 1918)? 

10.0.0.0 – 10.255.255.255       

172.16.0.0 – 172.31.255.255   

192.168.0.0 – 192.168.255.255

● What is a VLAN? 

  A VLAN is a group of devices on one or more LANs that are configured to communicate as if they were attached to the same wire, when in fact they are located on a number of different LAN segments. 

● What is ARP and what is it used for?  

   Address Resolution Protocol (ARP) is a procedure for mapping a dynamic Internet Protocol address (IP address) to a permanent physical machine address in a local area network (LAN). The physical machine address is also known as a Media Access Control or MAC address.

169 is an APIPA

