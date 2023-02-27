
 [[Index]]  

# Highly Available vs. Fault Tolerant

  

• Being highly available means you’ve architected a system/environment/process that allows for an application or service to be available to interested parties based on an agreed upon service level (SLA)

• Fault tolerance are techniques used when no service interruption is tolerable.  These techniques typically involve hardware-level duplication of critical components and thus incur significantly higher cost.

• HA minimizes downtime but does not guarantee 100% availability

  
  

Storage area networks (SANs) are the most common storage networking architecture used by enterprises for business-critical applications that need to deliver high throughput and low latency. 

A rapidly growing portion of SAN deployments leverages all-flash storage to gain its high performance, consistent low latency, and lower total cost when compared to spinning disk. By storing data in centralized shared storage, SANs enable organizations to apply consistent methodologies and tools for security, data protection, and disaster recovery.

A SAN is block-based storage, leveraging a high-speed architecture that connects servers to their logical disk units (LUNs). A LUN is a range of blocks provisioned from a pool of shared storage and presented to the server as a logical disk. The server partitions and formats those blocks—typically with a file system—so that it can store data on the LUN just as it would on local disk storage.

SANs make up about two-thirds of the total networked storage market. They are designed to remove single points of failure, making SANs highly available and resilient. A well-designed SAN can easily withstand multiple component or device failures.

  

# SAN Use Cases

  

Storage area networks are frequently deployed in support of business-critical, performance-sensitive applications such as:

  

  • Oracle databases. These are frequently business-critical and require the highest performance and availability.

  

  • Microsoft SQL Server databases. Like Oracle databases, MS SQL Server databases commonly store an enterprise’s most valuable data, so they require the highest performance and availability.

  

  • Large virtualization deployments using VMware, KVM, or Microsoft Hyper-V. These environments often extend to thousands of virtual machines running a broad range of operating systems and applications, with different performance requirements.

  Virtualized environments concentrate many applications, so infrastructure reliability becomes even more important because a failure can cause multiple application outages.

  

  • Large virtual desktop infrastructures (VDIs). These environments serve virtual desktops to large numbers of an organization’s users. Some VDI environments can easily number in the tens of thousands of virtual desktops. By centralizing the virtual desktops, organizations can more easily manage data protection and data security.

  

  • SAP or other large ERP or CRM environments. SAN architectures are ideal for enterprise resource planning and customer resource management workloads.

  

From <https://www.netapp.com/data-storage/what-is-san-storage-area-network/>

  
  

# SAN vs. NAS

  

Both SAN and network-attached storage (NAS) are methods of managing storage centrally and sharing that storage with multiple hosts (servers). 

However, NAS is Ethernet-based, while SAN can use Ethernet and Fibre Channel. In addition, while SAN focuses on high performance and low latency, NAS focuses on ease of use, manageability, scalability, and lower total cost of ownership (TCO). Unlike SAN, NAS storage controllers partition the storage and then own the file system. Effectively this makes a NAS server look like a Windows or UNIX/Linux server to the server consuming the storage.

  

SAN Protocols

  

  • Fibre Channel Protocol (FCP)

  

  • Internet Small Computer System Interface (iSCSI)

  

  • Fibre Channel over Ethernet (FCoE)

  

  • Non-Volatile Memory Express over Fibre Channel (FC-NVMe)

  

NAS Protocols

  

  • Common Internet File Services / Server Message Block (CIFS/SMB). This is the protocol that Windows usually uses.

  

  • Network File System (NFS). NFS was first developed for use with UNIX servers and is also a common Linux protocol.

  

From <https://www.netapp.com/data-storage/what-is-san-storage-area-network/>

 ![](image/SANs.jpg)


  
![](image/SANs2.png)


![](image/SANs3.png)



  
  
  
  

The image example on the left illustrates a highly available system.

  

It includes architecture decisions to minimize downtime, such as shared disks, two nodes, and two lan and san switches.  

  

The individual pieces are not necessarily built with fault tolerance in mind.

  

The image on the right shows the rear panel of a rack mounted server.  

  

This server has four power supplies.  

  

This is an example of a decision made during design of this server to make the power supplies fault tolerant.  

  

If one power supply were to fail, the other three could maintain power to the system.

  

We’re also assuming here that the power supplies can be replaced while the system is up and running.

 ![](image/SANs4.png) 
  




![](image/SANs5.png)



  
  
  

In this example diagram we have two nodes in a simple cluster.

  

The application services they offer are made available on the application tier network (public network).

  

Both nodes communicate with the same shared storage via a SAN switch.

  

Both nodes also communicate directly with each other through a private network.  

  

This network can be used to send heartbeats or to communicate w/ a fencing agent.

  

This network is not accessible to the public.

  

A fencing agent is used to forcibly evict a misbehaving or failed node from the cluster.

  

A fencing agent could be a network connected power switch, or some other type of out-of-band device that can either logically or physically disconnect a node.

  

# Jump Server Architecture

![](image/jump%20servers.jpg)

  

A common practice in today’s data centers is to allow Systems Administrators Remote Desktop  (RDP) or Secure Shell (SSH) access to the servers they are administrating, directly from their desktops.  Regardless of where they are located!

  

Although restricting Lateral access between servers is quite easily achieved through group policy on Windows, or source whitelisting local firewall rules for both Windows and UNIX/Linux, these are not enabled by default. Typically, even with network segmentation and access control lists, is is possible to jump from server to server unhindered, by simply having access to the appropriate credentials.

  

The concept of a “jump” server has been around for decades, but is rarely in use or enforced.  One popular use of jump servers is to restrict access into a DMZ. This allows administrative control of servers in the DMZ to be regulated and audited as per compliance rules.

  
  

# SDDC - Software Defined Data Centre

![](image/sddc.png)




  

An SDDC (software-defined data center) is a data storage facility in which all infrastructure elements -- networking, storage, CPU and security -- are virtualized and delivered as a service. Deployment, operation, provisioning and configuration are abstracted from hardware. Those tasks are implemented through software intelligence.

  

# Blades Server


![](image/Blade_Server_Chassis.png)

A blade server, sometimes referred to as a high-density server, is a compact device containing a computer used to manage and distribute data in a collection of computers and systems, called a network. Its role is to act as a conduit between computers, programs, applications and systems.

  

In general, a blade server consists of a chassis, or box-like structure, housing multiple thin, modular electronic circuit boards, known as server blades. They are called blades because of their ultra-thin shape. Each blade contains a single server, often dedicated to a single application. The information within blade servers is stored on a memory card or other memory device. In addition, the individual blades contain processors, memory, integrated network controllers, an optional Fibre Channel host bus adaptor (HBA) and other input/output (IO) ports. These are used to connect server blades to other server blade units within the system, or to connect individual blades to power sources.

[[Index]] 