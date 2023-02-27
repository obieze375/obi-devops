[[Index]] 

## RHEL 8 Lab

## Basic LVM

In this lab, you create several partitions on a new disk, formatting some with file systems and
mounting them, and activating others as swap spaces.
Outcomes
• Display and create partitions with the parted command.
• Create file systems on partitions and persistently mount them.
• Create swap spaces and activate them at boot.

```bash

```

```bash
[root@serverb ~]# lsblk
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
vda 252:0 0 10G 0 disk
├─vda1 252:1 0 1M 0 part
├─vda2 252:2 0 200M 0 part /boot/efi
├─vda3 252:3 0 500M 0 part /boot
└─vda4 252:4 0 9.3G 0 part /
vdb 252:16 0 5G 0 disk
vdc 252:32 0 5G 0 disk
vdd 252:48 0 5G 0 disk
```


## Confirm that the /dev/vdb disk has no label. 

```bash
[root@serverb ~]# parted /dev/vdb print
Error: /dev/vdb: unrecognised disk label
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags:
```


## Define the GPT partitioning scheme. 

```bash
[root@serverb ~]# parted /dev/vdb mklabel gpt
```

Information: You may need to update /etc/fstab.


## Create the 2 GB backup partition with an xfs file-system type. Start the partition at sector 2048. 

```bash
[root@serverb ~]# parted /dev/vdb mkpart backup xfs 2048s 2GB
```
Information: You may need to update /etc/fstab.

## Confirm the creation of the backup partition. 

```bash
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:
Number Start End Size File system Name Flags
1 1049kB 2000MB 1999MB backup
```


## Run the udevadm settle command. This command waits for the system to detect the new partition and to create the /dev/vdb1 device file. 

```bash
[root@serverb ~]# udevadm settle
```

## Format the 2 GB backup partition with an XFS file system and persistently mount it to the /backup directory. 

```bash
[root@serverb ~]# mkfs.xfs /dev/vdb1
meta-data=/dev/vdb1 isize=512 agcount=4, agsize=121984 blks
 = sectsz=512 attr=2, projid32bit=1
 = crc=1 finobt=1, sparse=1, rmapbt=0
 = reflink=1 bigtime=1 inobtcount=1
data = bsize=4096 blocks=487936, imaxpct=25
 = sunit=0 swidth=0 blks
naming =version 2 bsize=4096 ascii-ci=0, ftype=1
```

## Format the /dev/vbd1 partition.

```bash
log =internal log bsize=4096 blocks=2560, version=2
 = sectsz=512 sunit=0 blks, lazy-count=1
realtime =none extsz=4096 blocks=0, rtextents=0
```

## Create the /backup mount point. 

```bash
[root@serverb ~]# mkdir /backup
```

## Before adding the new file system to the /etc/fstab file, retrieve its UUID. The UUID on your system might be different. 

```bash
[root@serverb ~]# lsblk --fs /dev/vdb1
NAME FSTYPE FSVER LABEL UUID FSAVAIL FSUSE%
 MOUNTPOINTS
vdb1 xfs f74ed805-b1fc-401a-a5ee-140f97c6757d
```


## Edit the /etc/fstab file and define the new file system. 

```bash
[root@serverb ~]# vim /etc/fstab
...output omitted...
UUID=f74ed805-b1fc-401a-a5ee-140f97c6757d /backup xfs defaults 0 0
```

## Force the systemd daemon to reread the /etc/fstab file. 

```bash
[root@serverb ~]# systemctl daemon-reload
```

Manually mount the /backup directory to verify your work. Confirm that the mount is
successful. 

```bash
[root@serverb ~]# mount /backup
[root@serverb ~]# mount | grep /backup
/dev/vdb1 on /backup type xfs
 (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
```


## On the same disk, create two 512 MB GPT partitions called swap1 and swap2. A size between 460 MB and 564 MB is acceptable. Configure the file-system types of the partitions to host swap spaces. 


## Retrieve the end position of the first partition by displaying the current partition table on the /dev/vdb disk. In the next step, you use that value as the start of the swap1 partition. 

```bash
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:
Number Start End Size File system Name Flags
 1 1049kB 2000MB 1999MB xfs backup
```


## Create the first 512 MB swap1 partition. Set its type to linux-swap. Use the end position of the first partition as the starting point. The end position is 2000 MB +512 MB = 2512 MB 

```bash
[root@serverb ~]# parted /dev/vdb mkpart swap1 linux-swap 2000M 2512M
```


Information: You may need to update /etc/fstab. 


## Create the second 512 MB swap2 partition. Set its type to linux-swap. Use the end position of the previous partition as the starting point: 2512M. The end position is 2512 MB + 512 MB = 3024 MB 

```bash
[root@serverb ~]# parted /dev/vdb mkpart swap2 linux-swap 2512M 3024M
```

Information: You may need to update /etc/fstab. 

## Display the partition table to verify your work. 

```bash
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:
Number Start End Size File system Name Flags
 1 1049kB 2000MB 1999MB xfs backup
 2 2000MB 2512MB 513MB swap1 swap
 3 2512MB 3024MB 512MB swap2 swap
```

## Run the udevadm settle command. The command waits for the system to register the new partitions and to create the device files. 

```bash
[root@serverb ~]# udevadm settle
```


## Initialize the two 512 MiB partitions as swap spaces and configure them to activate at boot. Set the swap space on the swap2 partition to be preferred over the other. 

```bash

``` 


## Use the mkswap command to initialize the swap partitions. Note the UUIDs of the two swap spaces, because you will use that information in the next step. If you clear the mkswap output, then use the lsblk --fs command to retrieve the UUIDs. 

```bash
[root@serverb ~]# mkswap /dev/vdb2
Setting up swapspace version 1, size = 489 MiB (512749568 bytes)
no label, UUID=87976166-4697-47b7-86d1-73a02f0fc803 

[root@serverb ~]# mkswap /dev/vdb3
Setting up swapspace version 1, size = 488 MiB (511700992 bytes)
no label, UUID=4d9b847b-98e0-4d4e-9ef7-dfaaf736b942
```


## Edit the /etc/fstab file and define the new swap spaces. To set the swap space on the swap2 partition to be preferred over the swap1 partition, give the swap2 partition a higher priority with the pri option.

```bash
[root@serverb ~]# vim /etc/fstab
...output omitted...
UUID=a3665c6b-4bfb-49b6-a528-74e268b058dd /backup xfs defaults 0 0
UUID=87976166-4697-47b7-86d1-73a02f0fc803 swap swap pri=10 0 0
UUID=4d9b847b-98e0-4d4e-9ef7-dfaaf736b942 swap swap pri=20 0 0 
```



Force the systemd daemon to reread the /etc/fstab file. 

```bash
[root@serverb ~]# systemctl daemon-reload 
```



Activate the new swap spaces. Verify the correct activation of the swap spaces. 

```bash
[root@serverb ~]# swapon -a
[root@serverb ~]# swapon --show
NAME TYPE SIZE USED PRIO
/dev/vdb2 partition 489M 0B 10
/dev/vdb3 partition 488M 0B 20 
```



## To verify your work, reboot the serverb machine. Confirm that the system automatically mounts the first partition onto the /backup directory. Also, confirm that the system activates the two swap spaces.

## Reboot serverb. 

```bash
[root@serverb ~]# systemctl reboot
Connection to serverb closed by remote host.
Connection to serverb closed.
[student@workstation ~]$
```

## Wait for serverb to boot and then log in as the student user. 

```bash
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$
```


## Verify that the system automatically mounts the /dev/vdb1 partition onto the /backup directory. 

```bash
[student@serverb ~]$ mount | grep /backup
/dev/vdb1 on /backup type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
```

## Verify that the system activates both swap spaces. 

```bash
[student@serverb ~]$ swapon --show
NAME TYPE SIZE USED PRIO
/dev/vdb2 partition 489M 0B 10
/dev/vdb3 partition 488M 0B 20
```

## Manage Storage 

In this lab, you resize an existing logical volume, add LVM resources as necessary, and then
add a new logical volume with a persistently mounted XFS file system on it.

```bash
[root@serverb ~]# parted /dev/vdb mkpart primary 514MiB 1026MiB
[root@serverb ~]# parted /dev/vdb set 2 lvm on
```


## Register the new partition with the kernel. 

```bash
[root@serverb ~]# udevadm settle
```

## Initialize the partition as a PV.

```bash
[root@serverb ~]# pvcreate /dev/vdb2
 Physical volume "/dev/vdb2" successfully created.
```



## Extend the serverb_01_vg VG using the new /dev/vdb2 PV. 

```bash
[root@serverb ~]# vgextend serverb_01_vg /dev/vdb2
 Volume group "serverb_01_vg" successfully extended
```

## Extend the serverb_01_lv logical volume to 768 MiB. 


## Extend the serverb_01_lv LV to 768 MiB. 

Alternatively, you can also use the lvcreate command -L +512M option to resize the
LV.  

```bash
[root@serverb ~]# lvextend -L 768M /dev/serverb_01_vg/serverb_01_lv
 Size of logical volume serverb_01_vg/serverb_01_lv changed from 256.00 MiB (64
 extents) to 768.00 MiB (192 extents).
 Logical volume serverb_01_vg/serverb_01_lv successfully resized.
```


## Extend the XFS file system consuming the remaining space on the LV. 

```bash
[root@serverb ~]# xfs_growfs /storage/data1
meta-data=/dev/mapper/serverb_01_vg-serverb_01_lv isize=512 agcount=4,
 agsize=16384 blks
...output omitted... 
data blocks changed from 65536 to 196608

```

The xfs_growfs command introduces an additional step to extend the file system.
An alternative would be using the lvextend command -r option. 

## In the existing volume group, create the new serverb_02_lv logical volume with 128 MiB. Add an XFS file system and mount it persistently on the /storage/data2 directory. 


## Create the serverb_02_lv LV with 128 MiB from the serverb_01_vg VG. 

```bash
[root@serverb ~]# lvcreate -n serverb_02_lv -L 128M serverb_01_vg
 Logical volume "serverb_02_lv" created.
```

## Create the xfs file system on the serverb_02_lv LV. 

```bash
[root@serverb ~]# mkfs -t xfs /dev/serverb_01_vg/serverb_02_lv
...output omitted...
```

## Create the /storage/data2 directory as the mount point. 

```bash
[root@serverb ~]# mkdir /storage/data2
```

## Add the following line to the end of the /etc/fstab file: 

```bash
/dev/serverb_01_vg/serverb_02_lv /storage/data2 xfs defaults 0 0
```

## Update the systemd daemon with the new /etc/fstab configuration file. 

```bash
[root@serverb ~]# systemctl daemon-reload
```

## Mount the serverb_02_lv LV.
```bash
[root@serverb ~]# mount /storage/data2
```


## Verify that the newly created LV is mounted with the desired size. 


## Use the df command to verify the serverb_01_lv LV size. 

```bash
[root@serverb ~]# df -h /storage/data1
Filesystem Size Used Avail Use% Mounted on
/dev/mapper/serverb_01_vg-serverb_01_lv 763M 19M 744M 3% /storage/data1
```

## Verify the serverb_02_lv LV size. 

```bash
[root@serverb ~]# df -h /storage/data2
Filesystem Size Used Avail Use% Mounted on
/dev/mapper/serverb_01_vg-serverb_02_lv 123M 7.6M 116M 7% /storage/data2 
```


## Verify the serverb_01_lv LV details. 
```bash
[root@serverb ~]# lvdisplay /dev/serverb_01_vg/serverb_01_lv
 --- Logical volume ---
 LV Path /dev/serverb_01_vg/serverb_01_lv
 LV Name serverb_01_lv
 VG Name serverb_01_vg
 LV UUID 1pY3DZ-fs1F-mptC-fL32-e8tG-PFBT-bs7LSJ
 LV Write Access read/write
 LV Creation host, time serverb.lab.example.com, 2022-05-05 14:40:51 -0400
 LV Status available 
  # open 1
 LV Size 768.00 MiB
 Current LE 192
 Segments 2

 Allocation inherit
 Read ahead sectors auto
 - currently set to 8192
 Block device 253:0

```



## Verify the serverb_02_lv LV details. 

```bash
[root@serverb ~]# lvdisplay /dev/serverb_01_vg/serverb_02_lv
 --- Logical volume ---
 LV Path /dev/serverb_01_vg/serverb_02_lv
 LV Name serverb_02_lv
 VG Name serverb_01_vg
 LV UUID 0aJIb6-Ti2b-jLCk-imB6-rkLx-mUoX-acjkz9
 LV Write Access read/write
 LV Creation host, time serverb.lab.example.com, 2022-05-05 14:45:46 -0400
 LV Status available
 # open 1
 LV Size 128.00 MiB
 Current LE 32
 Segments 1
 Allocation inherit
 Read ahead sectors auto
 - currently set to 8192
 Block device 253:1
```



