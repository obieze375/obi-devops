 [[Index]]  


# Start-up + Shutdown Cluster

  

1) Verify Cluster status initially

``` 
pcs status

pcs status nodes both

pcs status groups

pcs status resources

pcs status cluster
``` 
  

## Place cluster into standby mode

  

pcs cluster standby < cluster_name >

  
``` 
pcs status

pcs status resources
``` 
  

## Stop RHCS

  
``` 
pcs cluster stop < cluster_name >

```   
  
  

# Create Constraints

  

1 – Verify the current list of constraints using the pcs constraint list command.

  

``` 

pcs constraint list --full

``` 

  

Example Output

  

Location Constraints:

Ordering Constraints:

Colocation Constraints:

Ticket Constraints:

2 – Use the pcs constraint location command to assign your resources or resource groups to the preferred node in the cluster.

  

``` 

pcs constraint location test01_sg prefers server01-cpn

pcs constraint location test02_sg prefers server02-cpn

pcs constraint location test03_sg prefers server02-cpn

pcs constraint location test04_sg prefers server01-cpn

pcs constraint location test05_sg prefers server02-cpn

``` 

  

3 – Verify the constraints are configured properly using the pcs constraint list command.

  

``` 

pcs constraint list --full

``` 

  

``` 

Example Output

  

Location Constraints:

  Resource: test04_sg

​    Enabled on: server01-cpn (score:INFINITY) (id:location-test04_sg-server01-cpn-INFINITY)

  Resource: test02_sg

​    Enabled on: server02-cpn (score:INFINITY) (id:location-test02_sg-server02-cpn-INFINITY)

  Resource: test03_sg

​    Enabled on: server02-cpn (score:INFINITY) (id:location-test03_sg-server02-cpn-INFINITY)

  Resource: test05_sg

​    Enabled on: server02-cpn (score:INFINITY) (id:location-test05_sg-server02-cpn-INFINITY)

  Resource: test01_sg

​    Enabled on: server01-cpn (score:INFINITY) (id:location-test01_sg-server01-cpn-INFINITY)

Ordering Constraints:

Colocation Constraints:

Ticket Constraints:

``` 

  

# Remove Constraints

  

1 – List the currently configured constraints using the pcs constraint list --full command.

  

``` 

pcs constraint list --full

``` 

``` 

Example Output

  

Location Constraints:

  Resource: test_sg

    Disabled on: server01-cpn (score:-INFINITY) (role: Started) (id:cli-ban-test_sg-on-server01-cpn)

Ordering Constraints:

Colocation Constraints:

Ticket Constraints:

``` 

  

2 – Note the id string and use that to remove the constraints use the pcs constraint remove id command.

  

``` 

pcs constraint remove cli-ban-test_sg-on-server01-cpn

``` 

3 – Verify the constraints have been removed using the pcs constraint list --full command.

  

``` 

pcs constraint list --full

``` 

  

``` 

Example Output

  

Location Constraints:

Ordering Constraints:

Colocation Constraints:

Ticket Constraints:

``` 

  
  

# Pacemaker Enable Maintenance Mode or Freeze Cluster

  

Enable Maintenance Mode

  

1 – Run the pcs property set maintenance-mode=true command to place the cluster into maintenance mode.

  

``` 

pcs property set maintenance-mode=true

``` 

  

2 – Next run the pcs property command to verity that it displays maintenance-mode: true which means the cluster is in maintenance mode.

  

``` 

pcs property

``` 

``` 

Example Output

  

Cluster Properties:

 cluster-infrastructure: cman

 dc-version: 1.1.15-5.el6-e174ec8

 have-watchdog: false

 last-lrm-refresh: 1527095308

 maintenance-mode: true

 no-quorum-policy: freeze

``` 

  

3 – Next run the pcs status --full command and you will see an alert at the top of the status output showing the cluster is in maintenance mode.

  

``` 

pcs status --full

``` 

  

Example Output

  

Cluster name: TEST_CLUSTER

Stack: cman

Current DC: server01-cpn (version 1.1.15-5.el6-e174ec8) - partition with quorum

Last updated: Fri Jun  1 09:25:24 2018          Last change: Fri Jun  1 09:20:51 2018 by root via cibadmin on server01-cpn

​              *** Resource management is DISABLED ***

  The cluster will not attempt to start, stop or recover services

2 nodes and 44 resources configured

# Disable Maintenance Mode

  

1 – Run the pcs property set maintenance-mode=false command to take the cluster out of maintenance mode.

  

``` 

pcs property set maintenance-mode=false

``` 

  

2 – Next run the pcs property command to verity that it does not display maintenance-mode: true which means the cluster is not in maintenance mode.

  

``` 

pcs property

``` 

  

``` 

Example Output

  

Cluster Properties:

 cluster-infrastructure: cman

 dc-version: 1.1.15-5.el6-e174ec8

 have-watchdog: false

 last-lrm-refresh: 1527095308

 no-quorum-policy: freeze

``` 

  
  

# File and Directory Locations

  

``` 

LOCATION  DESCRIPTION

  

/var/lib/pacemaker/cib/cib.xml  - Primary cluster configuration file

  

/var/log/cluster/corosync.log - Primary cluster log file

  

/usr/lib/ocf/resource.d/heartbeat/  - Directory where resource scripts are located

``` 

  

Check Cluster Status

  

COMMAND DESCRIPTION

  

``` 

pcs cluster status  - Display status of cluster nodes

  

pcs status –full (double dashes)  - Display detailed cluster status of nodes and resources

  

pcs resource  - Display status of all resources and resource groups

``` 

  

# Modify Cluster Nodes

  

COMMAND DESCRIPTION

  

``` 

pcs cluster standby - Place node in standby mode

  

pcs cluster unstandby - Remove node from standby mode

``` 

  

# Managing Running Resources

  

COMMAND DESCRIPTION

``` 

  

pcs resource move [resource_name] node name - Move resource to another node

  

pcs resource restart [resource_name] - Restart resource on current node

  

pcs resource enable [resource_name]  - Start resource on current node

  

pcs resource disable [resource_name] - Stop resource on current node

``` 

  

# Debugging Resources

  

COMMAND DESCRIPTION

  

``` 

pcs resource debug-start [source_name]  - Force resource to start on node where command is executed showing debug information. Use --full for even more verbose output.

  

pcs resource debug-stop [source_name] - Force resource to stop on node where command is executed showing debug information. Use --full for even more verbose output.

  

pcs resource debug-monitor [source_name] - Force resource to be monitored on node where command is executed showing debug information. Use --full for even more verbose output.

``` 

  

# Creating and Modifying Resources

  

COMMAND DESCRIPTION

  

``` 

pcs resource agents - List available resource agents

  

pcs resource describe [resource] - List configuration setting for resource

  

pcs resource create [resource id][resource] options… - Create resource

  

pcs resource show [resource id] - Display currently configured setting of resource

  

pcs resource update [resource id] options….  - Update resource configuration

  

pcs resource delete [resource id] - Delete resource

  

pcs resource cleanup [resource id] - Cleanup resource failures

``` 

  

# Creating and Modifying Stonith Resources

  

COMMAND DESCRIPTION

  

``` 

  

pcs stonith list  - List available fence agents

  

pcs stonith describe [fence agent]  - List configuration settings for fence agent

  

pcs stonith describe [stonith_id] - List configuration setting for stonith agent

  

pcs stonith create [stonith_id][resource] options…  - Create stonith agent

  

pcs stonith show [stonith_id] - Display currently configured setting of stonith agent

  

pcs stonith update [stonith_id] options…. - Update stonith configuration

  

pcs stonith delete [stonith_id] - Delete stonith agent

  

pcs stonith cleanup [stonith_id] - Cleanup stonith agent failures

  

``` 

  

# HP iLO SSH Fencing Notes

  

During my initial testing of HP iLO fencing I ran into an issue using the method=cycle in my HP iLO4 ssh configuration. Using method=cycle caused an error with stonith displaying no route to host. This was an ambiguous error and gave no help in identifying the issue. I am still not sure what the issue was but removing the method=cycle from my setting resolved the error and fencing worked properly going forward. So bottom line is leave method at its default value.

  

# HP iLO 4 SSH Configuration Settings

  

Using the pcs stonith describe fence_ilo4_ssh command you can view all configurable options for the fence_ilo4_ssh resource.

  

``` 

pcs stonith describe fence_ilo4_ssh

``` 

  

Example Output

  

``` 

fence_ilo4_ssh - Fence agent for HP iLO over SSH

fence_ilo_ssh is a fence agent that connects to iLO device. It logs into device via ssh and reboot a specified outlet.

``` 

  

Stonith options:

  action: Fencing Action WARNING: specifying 'action' is deprecated and not necessary with current Pacemaker

          versions.

  cmd_prompt: Force Python regex for command prompt

  identity_file: Identity file for ssh

  inet4_only: Forces agent to use IPv4 addresses only

  inet6_only: Forces agent to use IPv6 addresses only

  ipaddr (required): IP Address or Hostname

  ipport: TCP/UDP port to use for connection with device

  login (required): Login Name

  method: Method to fence (onoff|cycle)

  passwd: Login password or passphrase

  passwd_script: Script to retrieve password

  secure: SSH connection

  ssh_options: SSH options to use

  delay: Wait X seconds before fencing is started

  login_timeout: Wait X seconds for cmd prompt after login

  power_timeout: Test X seconds for status change after ON/OFF

  power_wait: Wait X seconds after issuing ON/OFF

  shell_timeout: Wait X seconds for cmd prompt after issuing command

  retry_on: Count of attempts to retry power on

  ssh_path: Path to ssh binary

  telnet_path: Path to telnet binary

  priority: The priority of the stonith resource. Devices are tried in order of highest priority to lowest.

  pcmk_host_map: A mapping of host names to ports numbers for devices that do not support host names. Eg.

                 node1:1;node2:2,3 would tell the cluster to use port 1 for node1 and ports 2 and 3 for node2

  pcmk_host_list: A list of machines controlled by this device (Optional unless pcmk_host_check=static-list).

  pcmk_host_check: How to determine which machines are controlled by the device. Allowed values: dynamic-list (query

                   the device), static-list (check the pcmk_host_list attribute), none (assume every device can fence

                   every machine)

  pcmk_delay_max: Enable random delay for stonith actions and specify the maximum of random delay This prevents

                  double fencing when using slow devices such as sbd. Use this to enable random delay for stonith

                  actions and specify the maximum of random delay.

  pcmk_action_limit: The maximum number of actions can be performed in parallel on this device Pengine property

                     concurrent-fencing=true needs to be configured first. Then use this to specify the maximum

                     number of actions can be performed in parallel on this device. -1 is unlimited.

# HP iLO 4 SSH Fencing Recommended Settings

  

SETTING DESCRIPTION REQUIRED

login Login Name  Yes

ipaddr  IP Address or Hostname  Yes

passwd  Login password or passphrase  Yes (Only if not using identity_file)

identity_file Identity file for ssh Yes (Only if not using passwd)

pcmk_host_list  List of nodes controlled by the HP iLO  Yes

delay Wait X seconds before fencing is started  Yes (Only for one of the nodes in the cluster)

Test HP iLO 4 SSH Settings

Before configuring the HP iLO SSH fencing use the fence_ilo4_ssh command to verify your iLO settings are configured correctly.

  

You can also add the -v option to obtain verbose information. You can change the -o status to -o reboot to test the user has the proper permissions to reboot the server.

  

1 – Test iLO configuration on first node.

  

``` 

fence_ilo4_ssh -a 192.168.1.100 -x -l hpilofence -p hpilopass -o status

``` 

  

Example Output

  

Status: ON

2 – Test iLO configuration on remaining nodes.

  

``` 

fence_ilo4_ssh -a 192.168.1.101 -x -l hpilofence -p hpilopass -o status

``` 

  

Example Output

  

Status: ON

# HP iLO 4 SSH Fencing Configuration

  

Use the pcs stonith create command to create the stonith fencing device.

  

1 – Configure fencing on first node.

  

``` 

pcs stonith create server105-cpn_ilo4_fence fence_ilo4_ssh ipaddr="192.168.1.100" login="hpilofence" secure="true" passwd=hpilopass  pcmk_host_list="server105-cpn" delay=10 op monitor interval=60s

``` 

  

2 – Configure fencing on the remaining nodes.

  

``` 

pcs stonith create server106-cpn_ilo4_fence fence_ilo4_ssh ipaddr="192.168.1.101" login="hpilofence" secure="true" passwd=hpilopass  pcmk_host_list="server106-cpn" op monitor interval=60s

``` 

  

# Verify HP iLO 4 SSH Fencing Settings

  

1 – Verify stonith device was created by using the pcs stonith show --full command.

  

``` 

pcs stonith show --full

``` 

  

Example Output

  

 Resource: server105-cpn_ilo4_fence (class=stonith type=fence_ilo4_ssh)

  Attributes: ipaddr=192.168.1.100 login=hpilofence secure=true passwd=hpilopass pcmk_host_list=server105-cpn delay=10

  Operations: monitor interval=60s (server105-cpn_ilo4_fence-monitor-interval-60s)

 Resource: server106-cpn_ilo4_fence (class=stonith type=fence_ilo4_ssh)

  Attributes: ipaddr=192.168.1.101 login=hpilofence secure=true passwd=hpilopass pcmk_host_list=server106-cpn

  Operations: monitor interval=60s (server106-cpn_ilo4_fence-monitor-interval-60s)

# Test HP iLO Fencing Settings

  

Once fencing devices are created use the pcs stonith fence [node_name] to verify fencing is working properly. The fencing should be tested on all nodes in the cluster.

  

1 – Test fencing on first node of the cluster.

  

``` 

pcs stonith fence server105-cpn

``` 

  

Example Output

  

Node: server105-cpn fenced

2 – Test fencing on the remaining nodes in the cluster.

  

``` 

pcs stonith fence server106-cpn

``` 

  

Example Output

  

``` 

Node: server106-cpn fenced

``` 

  
  

# Pacemaker Clear Resource Failures

  

1 – The pcs status command displays failed starting of the server106-cpn_ilo4_fence_start_0 resource.

  

``` 

pcs status

``` 

``` 

Example Output

  

    Cluster name: TEST_DB_ORA

    Stack: cman

    Current DC: server105-cpn (version 1.1.15-5.el6-e174ec8) - partition with quorum

    Last updated: Thu May 17 15:13:23 2018          Last change: Thu May 17 12:40:04 2018 by root via cibadmin on server105-cpn

    2 nodes and 73 resources configured

    Online: [ server105-cpn server106-cpn ]

    Full list of resources:

    Resource Group: testdb_sg

         testdb_ip (ocf::heartbeat:IPaddr):        Started server105-cpn

         testdbvg_lvm      (ocf::heartbeat:LVM):   Started server105-cpn

         testdb_adm001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_arch001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_dat001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo002_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo003_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo004_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_sys001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_temp001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_undo001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_ora_db     (ocf::heartbeat:oracle):        Started server105-cpn

         testdb_ora_listener       (ocf::heartbeat:oralsnr):       Started server105-cpn

    server105-cpn_ilo4_fence (stonith:fence_ilo4_ssh):       Started server105-cpn

    server106-cpn_ilo4_fence (stonith:fence_ilo4_ssh):       Started server106-cpn

    Failed Actions:

        * server106-cpn_ilo4_fence_start_0 on server105-cpn 'unknown error' (1): call=418, status=Error, exitreason='none', last-rc-change='Thu May 17 20:45:50 2018', queued=0ms, exec=12170ms

    Node Attributes:

    * Node server105-cpn:

    * Node server106-cpn:

    PCSD Status:

      server105-cpn: Online

      server106-cpn: Online

    Daemon Status:

      cman: active/enabled

      corosync: active/disabled

      pacemaker: active/enabled

      pcsd: active/enabled

 ``` 

  

2 – To remove this Failed Action from the output issue the pcs resource cleanup [resource-name] command.

  

``` 

pcs resource cleanup server106-cpn_ilo4_fence

``` 

Example Output

  

``` 

Cleaning up server106-cpn_ilo4_fence on server105-cpn, removing fail-count-server106-cpn_ilo4_fence

Cleaning up server106-cpn_ilo4_fence on server106-cpn, removing fail-count-server106-cpn_ilo4_fence

Waiting for 2 replies from the CRMd.. OK

``` 

3 – Once this has completed issue the pcs status command and the Failed Action will be removed.

``` 

pcs status

``` 

  

``` 

Example Output

  

    Cluster name: TEST_DB_ORA

    Stack: cman

    Current DC: server105-cpn (version 1.1.15-5.el6-e174ec8) - partition with quorum

    Last updated: Thu May 17 15:13:23 2018          Last change: Thu May 17 12:40:04 2018 by root via cibadmin on server105-cpn

    2 nodes and 73 resources configured

    Online: [ server105-cpn server106-cpn ]

    Full list of resources:

    Resource Group: testdb_sg

         testdb_ip (ocf::heartbeat:IPaddr):        Started server105-cpn

         testdbvg_lvm      (ocf::heartbeat:LVM):   Started server105-cpn

         testdb_adm001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_arch001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_dat001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo002_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo003_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_redo004_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_sys001_fs  (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_temp001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_undo001_fs (ocf::heartbeat:Filesystem):    Started server105-cpn

         testdb_ora_db     (ocf::heartbeat:oracle):        Started server105-cpn

         testdb_ora_listener       (ocf::heartbeat:oralsnr):       Started server105-cpn

    server105-cpn_ilo4_fence (stonith:fence_ilo4_ssh):       Started server105-cpn

    server106-cpn_ilo4_fence (stonith:fence_ilo4_ssh):       Started server106-cpn

    Node Attributes:

    * Node server105-cpn:

    * Node server106-cpn:

    PCSD Status:

      server105-cpn: Online

      server106-cpn: Online

    Daemon Status:

      cman: active/enabled

      corosync: active/disabled

      pacemaker: active/enabled

      pcsd: active/enabled

``` 

 [[Catagories]]  