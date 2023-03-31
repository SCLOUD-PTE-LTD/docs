---  
layout: default
title: Cloud Disk
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/cloud-disk/
nav_order: 6
---
# Cloud Disk
## Cloud Disk Overview
EVS disk is a block device that provides persistent storage space for virtual machines based on a distributed storage system. It has an independent life cycle, supports binding/unbinding to multiple virtual machines at will, and can expand the capacity of EVS disks when the storage space is insufficient, providing cloud hosts with highly secure, reliable, high-performance, and scalable data disks based on network distributed access.

![1](/assets/images/product-functional-architecture-8.jpg)

The storage system is compatible with and supports a variety of underlying storage hardware, such as general-purpose servers (computing and storage hyper-converged or independent general-purpose storage servers) and commercial storage, and the underlying storage hardware is abstracted into storage resource pools of different types of clusters, which are uniformly scheduled and managed by distributed storage systems. In practical application scenarios, the mechanical disks of ordinary `SATA` interfaces can be uniformly abstracted as `SATA` storage clusters, and SSD all-flash disks can be abstracted into SSD storage clusters, which can be used by platform users after being packaged by unified storage.

As shown in the schematic diagram, the resources of the `SATA` storage cluster are encapsulated as normal cloud disks, and the resources of the SSD all-flash storage cluster are encapsulated as high-performance cloud disks. The platform's virtual machines and database services can mount disks of different storage cluster types based on demand, and support attaching EVS disks of multiple cluster types at the same time. Cloud platform administrators can customize the alias of the storage cluster type through the administrator console, which is used to identify storage clusters with different disk media, different brands, different performance, or different underlying hardware, such as EMC storage clusters, SSD storage clusters, etc.

Generally, the performance of EVS disks of SSD disk media is linearly related to the size of the capacity, and the larger the capacity, the higher the IO performance.

The underlying data of distributed storage is stored through PG mapping, and data security is ensured in the form of multi-copy storage, that is, data blocks written to the cloud platform storage cluster will be saved to disks of different server nodes at the same time. Data stored in multiple copies provides consistency guarantees, which may cause multiple copies of data to be written due to mis-operation or abnormal data of the original data, resulting in inaccurate data. In order to ensure the accuracy of data, the cloud platform provides hard disk snapshot capabilities, backs up the data files and status of cloud disk data at a certain point in time, and can quickly restore data through snapshots, including database data, application data, and file directory data, in the event of data loss or corruption, which can achieve minute-level recovery.

### Functions and Features
EVS disks are allocated from the storage cluster capacity by unified storage, providing block storage devices for platform virtual resources and sharing the capacity and performance of the entire distributed storage cluster. At the same time, the block storage system provides users with EVS disk resources and full lifecycle management, including EVS disk creation, binding, unbinding, expansion, cloning, snapshot, and deletion.

- EVS disk capacity is allocated from the storage cluster capacity of unified storage, and all EVS disks share the capacity and performance of the entire distributed storage pool.
- EVS disk creation, mounting, unmounting, disk expansion, and deletion are supported, and only one virtual machine can be attached to a single EVS disk.
- Supports online and offline disk capacity expansion, and after disk capacity is expanded, you need to expand the disk capacity in the operating system of the virtual machine.
- To ensure data security and accuracy, EVS disks support disk resizing only and does not support disk shrinking.
- EVS disks can be configured at least 10 GB in steps of 1 GB, and you can customize and control the maximum capacity of a single EVS disk.
- EVS disks have an independent life cycle, can be freely bound to any virtual machine or database service, and can be reattached to other virtual machines after unbinding.
- A virtual machine with x86 architecture supports binding up to six EVS disks, and an ARM virtual machine can bind up to three EVS disks.
- Supports EVS disk cloning, that is, copying the data in the EVS disk into a new EVS disk.
- You can perform snapshot backup of EVS disks, including system disk snapshots and elastic cloud disk snapshots of virtual machines, and roll back data from snapshots to EVS disks for data recovery and restoration scenarios.
- Supports global and per-EVS disk QoS configuration, and can adjust disk performance according to different service models to balance the overall performance of the platform.
- You can set the storage cluster type permission, that is, you can set some storage resources to tenant exclusive, which meets the scenarios where the underlying storage resources need to be exclusive.

Supports thin provisioning, and only the allocated logical virtual capacity is presented when you create an EVS disk. When a user writes data to a logical storage capacity, the actual capacity is allocated from the physical space according to the storage capacity allocation policy. If an EVS disk created by a user has a capacity of 1 TB, the storage system allocates and presents a 1 TB logical volume to the user, and the physical disk capacity is allocated only when the user writes data to the EVS disk.

## Create a cloud disk
On the platform console, users can quickly create a cloud hard disk as a data disk of a virtual machine by specifying the type, capacity, and name of the cloud hard disk. Before creating, you need to confirm that the account balance and hard disk quota are sufficient.

Enter the hard disk resource console through the console, and click the "Create Hard Disk" button to enter the cloud hard disk creation wizard page, as shown in the figure below, select and configure parameters such as hard disk type, hard disk capacity, and hard disk name according to requirements.

![1](/assets/images/user-guide/user-guide-158.png)

- Hard disk type: the cloud disk type, that is, the storage cluster type, which is customized by the platform administrator, such as HDD cloud disk or SSD high-performance cloud disk;
- Hard disk capacity: the logical capacity allocated by the cloud hard disk, the default minimum is 10GB, the step size is 10GB, and the maximum support is 8000GB. The cloud platform administrator can customize the capacity specification in the console;
- Cloud disk name: the name of the cloud disk to be created;

Select the purchase quantity and payment method, confirm the order as shown in the figure below and click "Buy Now" to purchase and create cloud hard drives:

![1](/assets/images/user-guide/user-guide-155.png)

- Purchase Quantity: Select the number of cloud hard disks to be created. A maximum of 10 cloud hard disks of the same specification can be created in batches at a time.
- Payment method: select the billing method of the virtual machine, support on-time, yearly, and monthly, and choose the appropriate payment method according to your needs;
- Total cost: the user chooses to create a cloud hard disk resource to be displayed according to the cost of the payment method;
- Buy Now: After clicking Buy Now, you will return to the cloud hard disk resource list page. On the list page, you can view the creation process of the cloud hard disk. After the creation is successful, the status of the cloud hard disk is displayed as "unbound".

## View cloud disk
Enter the virtual machine console through the navigation bar, switch to the hard disk management page to view the list of cloud hard disk resources under the current account and related detailed information, including name, resource ID, cluster type, hard disk capacity, bound resources, billing method, status , creation time, expiration time and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-159.png)

- Name/ID: the name and globally unique identifier of the cloud drive;
- Cluster type: the cloud disk type, that is, the storage cluster type, which is customized by the platform administrator, such as HDD cloud disk or SSD high-performance cloud disk;
- Hard disk capacity: the capacity of the cloud hard disk, in GB;
- Bound resource: the name and ID of the virtual machine bound to the cloud disk, if not specified, it will be empty;
- Billing method: the billing method specified when the cloud disk is created, such as monthly, yearly, and hourly;
- Creation time/expiration time: the creation time of the cloud disk and the expiration time of the billing cycle;
- Status: The current status of the cloud disk, including creating, unbinding, binding, bound, unbinding, expanding, being cloned, snapshotting, and deleting, etc., where being cloned means that the current hard disk is being cloned, Snapshot means that the current hard disk is in snapshot backup.

The operation items on the list refer to the operations on a single hard disk, including binding, unbinding, expansion, cloning, snapshot, and deletion. You can search and filter the hard disk list through the search box, and support fuzzy search.

In order to facilitate the statistics and maintenance of hard disk resources for tenants, the platform supports downloading the list information of all hard disk resources owned by the current user as an Excel table; at the same time, it supports batch unbinding and batch deletion of hard disks.

## Bind cloud hard disk
Binding refers to mounting a cloud disk to a virtual machine, and adding a data disk to the virtual machine for data storage.

- Only hard disks whose status is "unbound" are supported for binding. To ensure data security, a cloud hard disk can only be bound to one virtual machine at the same time;
- The cloud hard disk has the attribute of region (data center), and only supports virtual machines that are bound to the same data center and are powered off or running;
- X86 architecture virtual machines can bind up to 6 hard disks, and ARM architecture virtual machines can bind up to 3 hard disks;

The hard disk binding operation can be performed through the "Bind" function of the operation item of the hard disk management resource list, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-160.png)

When binding, you need to select the virtual machine to which the hard disk is bound. During the binding process, the status of the hard disk is "binding". When the status changes to "bound", it means that the binding is successful. Users can view the bound cloud disk resources and information through the hard disk information of the virtual machine, including capacity, mount, etc. At the same time, users can also log in to the virtual machine operating system to check whether a new disk device has been recognized, such as Linux operating system users You can enter `fdisk -l` to view information about the newly added block device.

After the cloud disk is bound, formatting (if necessary) and system mounting operations are not performed by default. Users need to log in to the mounted virtual machine operating system, and format and mount the cloud disk according to requirements. Format and mount a data disk in the operating system, see Formatting and Mounting a Data Disk for details.

## Unbind cloud hard disk
Unbinding the cloud hard disk means separating the cloud hard disk from the virtual machine. The unbound cloud hard disk can be re-bound to other virtual machines. After unbinding, the data on the cloud hard disk will not be lost. After remounting the new virtual machine, Use the data on the cloud disk directly.

Only hard disk resources that have been bound are supported. Users can unbind hard disks through the hard disk list or the detailed hard disk page of the bound virtual machine, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-161.png)

When unbinding, the status of the virtual machine must be in the shutdown or running state. During the execution of the unbinding operation, the status of the cloud hard disk will change to "unbinding"; if the status changes to "unbound", it means that the unbinding is successful, and the hard disk can be rebound to other virtual machines.

In order to preserve data integrity, it is recommended to suspend the read and write operations on all file systems of the current hard disk before the unbinding operation, and enter the operating system for umount or offline operation (Linux system needs to confirm the file system corresponding to the umount hard disk; Windows system needs to Confirm to go to the disk management to perform the disk offline operation) to avoid file system damage or loss due to forced unbinding of the cloud hard disk.

## Format and mount the data disk
After the cloud hard disk is successfully mounted to the virtual machine, it needs to be formatted before the data can be read and written normally. This chapter mainly describes how to use a new cloud hard disk to create a single-partition data disk. Linux virtual machines and Windows virtual machines use cloud disks in different ways. After a Linux virtual machine is formatted, it needs to be mounted to a directory in the file system for use; a Windows virtual machine first needs to initialize the disk, partition and format it. It can be used normally.

Formatting and partitioning disks has certain risks. After formatting, the data in the cloud disk will be cleared. Please operate with caution.

### Linux virtual machine
The cloud disk device name mounted by the Linux virtual machine is assigned by the system by default, from `/dev/vdb` to `/dev/vdb`, including `/dev/vdb` to `/dev/vdz`. This example mounts a 100GB cloud hard disk to a Linux virtual machine, and the device name is `/dev/vdb`. The specific operation steps are as follows:

![1](/assets/images/user-guide/user-guide-162.png)

(1) Create a cloud hard disk, mount it to a Linux virtual machine, and remotely connect and log in to the virtual machine through SSH;

(2) Use the `fdisk -l` command to view the cloud hard disk on the virtual machine and check whether the mount is successful. As shown in the figure below, the mounted data disk is a 100GB `/dev/vdb` device;

(3) Create a file system, use the `mkfs.ext4 /dev/vdb` command to format and create a new file system, partition format can choose ext3, ext4 and other file system formats, the example uses ext4 format;
```
[root@localhost ~]# mkfs.ext4 /dev/vdb
mke2fs 1.41.12 (17-May-2010)
filesystem-label=
OS: Linux
block size=4096 (log=2)
Chunk size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
6553600 inodes, 26214400 blocks
1310720 blocks (5.00%) reserved for the super user
first data block = 0
Maximum filesystem blocks=4294967296
800 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
     32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
     4096000, 7962624, 11239424, 20480000, 23887872

Writing inode table: done
Creating journal (32768 blocks): Done
Writing superblocks and filesystem accounting information: Done
```

(4) Mount the data disk, create the mount point `/data` directory, use the mount `/dev/vdb /data` command to mount the new partition, and use `df -h` to verify whether the cloud disk is mounted successfully;

```
[root@localhost ~]# mkdir /data
[root@localhost ~]# mount /dev/vdb/data
[root@localhost ~]# df -h
Filesystem Size Used Avail Use% Mounted on
/dev/vda1 20G 874M 18G 5% /
tmpfs 1.9G 0 1.9G 0% /dev/shm
/dev/vdb 99G 188M 94G 1% /data
```

(5) Configure automatic mounting at startup, and add the mounting information of the cloud disk to `/etc/fstab` , as follows:

```
echo '/dev/vdb /data ext4 defaults 0 0' >> /etc/fstab
```

(6) After the mount is successful, you can use the cloud hard disk normally. If the cloud hard disk is unbound in the console, after rebinding to the virtual machine, you need to repeat the mount `/dev/vdb /data` command, or you need to restart the virtual machine to automatically mount ;

(7) If the cloud disk is unbound in the console, after rebinding to another Linux virtual machine, you need to perform the mount operation according to steps 4~5.

## Expand cloud hard disk
### Expand cloud disk capacity
The platform supports users to expand the capacity of cloud hard disks, which is suitable for scenarios where business changes require disk capacity expansion. The platform only supports disk capacity expansion, not disk capacity reduction. Support online and offline hard disk expansion methods:

- Online means to expand the capacity of the cloud disk bound to the running virtual machine;
- Offline means to expand the capacity of cloud disks that are not bound to a virtual machine or bound to a virtual machine in the shutdown state.

The disk capacity expansion range refers to the specifications of the current hard disk type, and the default is 10GB~8000GB. The platform administrator can go to the global configuration of the platform management background to configure the disk specification.

Expansion of the hard disk capacity will have an impact on the cost of the virtual machine. For hard disks paid by the hour, the next payment period for capacity expansion will be deducted according to the new configuration; for hard disks paid by the year and month, the capacity expansion will take effect immediately, and the difference will be automatically compensated in proportion. The user can click "Expand" in the operation of the cloud hard disk console to expand the hard disk capacity, as shown in the figure below:

![1](/assets/images/user-guide/user-guide-163.png)

As shown in the figure, to expand the hard disk, you need to specify the size of the changed capacity, that is, the capacity of the hard disk that needs to be expanded. The platform has displayed the capacity of the current hard disk. Since shrinking capacity is not supported, the changed capacity must be greater than the current capacity during capacity expansion.

During the expansion process, the status of the hard disk changes to "upgrading", and the expansion is successful when the status changes to "bound" or "unbound". Users can view the new capacity of the hard disk through the hard disk list or virtual machine hard disk information; if the hard disk has been bound to the virtual machine, the user can also log in to the virtual machine operating system to view the capacity of the bound disk device. For example, Linux operating system users can enter `fdisk - l` Check the information about the newly added block device.

*Because the MBR format partition does not support disk capacity greater than 2TB. When expanding the cloud hard disk, if the hard disk to be expanded uses the MBR partition format and needs to be expanded to 2TB or above, it is recommended to re-create and mount a hard disk, use the GPT partition method and copy the data to the new hard disk.*

The expansion operation only increases the block device capacity of the hard disk, and does not expand the file system and partitions in the operating system. After the capacity expansion is successful, you need to enter the mounted virtual machine operating system to perform partition expansion or create a new partition operation.

### Disk partition expansion
After expanding the hard disk capacity, you need to enter the operating system to expand the disk partition, that is, you need to expand the file system, so that the operating system can normally use the expanded disk capacity. The partition expansion operation is different for different operating systems. For example, Linux usually uses `fdisk` or parted tools; Windows usually uses its own disk management tools to perform expansion operations. According to different disk expansion scenarios, partition expansion can be roughly divided into the following scenarios:

- Raw disk expansion (Linux)
- Single Partition Disk Capacity Expansion (Linux)
- Single Partition Disk Capacity Expansion (Windows)
- Multi-partition disk expansion (Linux)
- Multi-partition disk expansion (Windows)
- 2TB Hard Disk Partition Expansion (Linux)
- 2TB Hard Disk Partition Expansion (Windows)

## Hard disk cloning
Disk cloning refers to copying the data in the cloud disk to a new cloud disk with the same size and type as the original disk. Only hard disks whose cloning status is unbound are supported. At the same time, during the hard disk cloning process, the source hard disk cannot be bound, cloned, or expanded.

Users can use the "Clone" function on the cloud hard disk resource list to perform cloud hard disk cloning operations, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-164.png)

- Source hard disk name/ID: the name and ID of the hard disk to be cloned;
- Source hard disk capacity: the capacity of the hard disk that needs to be cloned, that is, the capacity of the source hard disk;
- Target Hard Disk Name: The name of the newly cloned hard disk.

Cloning will copy a new hard disk based on the source hard disk, and you need to select the relevant billing configuration of the new hard disk, including the purchase quantity, payment method and total cost, etc.:
- Purchase Quantity: Create multiple cloud hard disks in batches according to the selected configuration and parameters. Currently, only one hard disk is cloned at the same time.
- Payment method: Choose the billing method of the hard disk, support on-time, yearly, and monthly, and choose the appropriate payment method according to your needs;
- Total cost: the user chooses to create a cloud hard disk resource to be displayed according to the cost of the payment method;
- Confirm: After clicking confirm purchase, it will return to the cloud hard disk resource list page. On the list page, you can view the cloning process of the cloud hard disk. After the clone is successful, the status of the cloud hard disk is displayed as "unbound".

Hard disk cloning can be used in application scenarios such as backup and snapshot of hard disk data, and the cloned hard disk is completely consistent with the data on the source hard disk.

## Delete cloud hard disk
Users are supported to delete cloud hard disk resources in the "unbound" state, and the deleted cloud hard disk will automatically enter the "recycle bin" for restoration and destruction. After the cloud disk is deleted, the snapshot resources created through the current cloud disk will be destroyed at the same time.

Users can delete cloud hard disks through the "Delete" function of the cloud hard disk management console. After deletion, users can view the deleted cloud hard disk resources in the recycle bin. as the picture shows:

![1](/assets/images/user-guide/user-guide-165.png)

## Modify name and remarks
Modify the name and remarks of the hard disk, which can be operated in any state. It can be modified through the "Edit" button on the right side of each hard disk name on the hard disk list page.

## Renew cloud disk
Users are supported to manually renew the cloud hard disk. The renewal operation is only for the resource itself, and does not renew the virtual machine resources that are additionally associated with the resource. After the additional associated resources expire, they will be automatically unbound. In order to ensure the normal use of the business, it is necessary to renew the relevant resources in a timely manner.

When the cloud hard disk is renewed, it will be charged according to the renewal time. The renewal time matches the billing method of the resource. When the billing method of the cloud hard disk is [hour], the renewal time can be selected from 1 to 24 hours; If the method is [Monthly], the renewal period can be selected from 1 to November; when the billing method of the cloud disk is [Yearly], the renewal period of the cloud disk is 1 to 5 years.

## Snapshot Management
The distributed storage of the cloud platform supports the disk snapshot capability, which can reduce the risk of data loss caused by mis-operation and version upgrade, and is an important measure for the platform to ensure data security.

### Snapshot Overview
A snapshot is a data status file of a cloud disk at a certain point in time. It can understand the data backup of a cloud disk at a certain moment, and the data writing and modification of the cloud disk will not affect the created snapshot.

Supports timing snapshot policy, that is, a policy that can automatically create snapshots that can be executed periodically. The snapshot policy is separated from the snapshot and has an independent life cycle. In practical applications, disk snapshots can reduce the risk of data loss caused by mis-operations, version upgrades, etc., and can be roughly applied to the following business scenarios:

- Disaster recovery backup: Make snapshots for the cloud hard disk at regular intervals. When a problem occurs in the system, it can be rolled back quickly to avoid data loss.
- Version rollback: When doing a major business upgrade, it is recommended to make a snapshot in advance. When there is a system problem in the upgraded version that cannot be repaired, the snapshot can be used to restore to the historical version.

The platform supports the snapshot operation of the system disk and data disk bound to the virtual machine, and also supports the snapshot rollback operation, that is, the snapshot data is rolled back to the associated cloud hard disk to meet the application scenarios of data recovery.

### Create a snapshot
Users can create a snapshot for a certain cloud hard disk on the cloud hard disk list page; if the hard disk has been mounted to a virtual machine, they can also perform snapshot operations on the hard disk through the hard disk information list on the virtual machine details page, and support snapshots for the virtual machine system disk backup. To ensure data and disk security:

- Only unbound and bound hard disks are supported for snapshot operations. If the hard disk is in the process of expansion or snapshot, snapshot backup cannot be performed;
- When creating a snapshot, it is not allowed to mount/unmount the disk and modify the state of the virtual machine (such as power on or off), otherwise it may cause abnormal snapshot creation;
- The snapshot only captures the data that has been written to the hard disk, and does not include the data cached in the memory by the application or the operating system. It is recommended to make a snapshot after the I/O operations on the hard disk are suspended, such as shutting down or uninstalling the hard disk.

In actual operation, you can create a snapshot for the cloud hard disk through the "snapshot" in the operation item of the cloud hard disk list page or the virtual machine detailed disk list. As shown on the snapshot creation wizard page, the user can create a snapshot by verifying the hard disk information to be created and entering a snapshot name.

![1](/assets/images/user-guide/user-guide-166.png)

A hard disk can only create one snapshot at the same time. During the snapshot creation process, the status of the hard disk is "Snapshot", and when the status changes to "Unbound" or "Binded", it means that the snapshot is created successfully. View the created snapshot status and related information on the page.

### View snapshot information
After the snapshot is successfully created, the user can switch to the snapshot management page through the virtual machine console to view the snapshot resource list information and related information, including name, resource ID, disk, disk type, status, creation time and operation items, as shown in the following figure :

![1](/assets/images/user-guide/user-guide-167.png)

- Resource name/ID: represents the name and globally unique identifier of the current snapshot;
- Disk: represents the disk corresponding to the current snapshot, which means that the snapshot was created by this disk;
- Disk type: represents the attributes of the hard disk to which the current snapshot belongs, such as data disk or system disk;
- Status: Represents the running status of the current snapshot, including creating, normal, rolling back, and deleting, where rolling back means that the current snapshot is being rolled back;
- Creation Time: The creation time of the current snapshot.

Operation items on the list refer to operations on a single snapshot, including rollback and delete. In order to facilitate the statistics and maintenance of snapshot resources by tenants, the platform supports downloading the list information of all snapshot resources owned by the current user as an Excel table, and supports batch deletion of snapshots.

### Rolling back a snapshot
Rollback snapshot is to roll back the snapshot data at a certain moment to the associated cloud hard disk to deal with the application scenario of snapshot data recovery.

When rolling back, the cloud disk must be unbound or the bound virtual machine must be powered off;
Only normal snapshots are supported for rollback.
Users can roll back the snapshot through "Rollback" in the operation item of the snapshot resource list, and only support the rollback of the snapshot to the hard disk to which it belongs, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-168.png)

After clicking Confirm, it will return to the snapshot list page. Both the snapshot and its hard disk will be in the "rolling back" state. After the rollback is successful, the hard disk will be in the "unbound" state, and the snapshot will be in the "normal" state. After the snapshot is successfully rolled back, the data on the parent hard disk before the rollback operation will be cleared and overwritten by the data in the snapshot, that is, the data in the parent hard disk is consistent with the data captured in the current snapshot.

![1](/assets/images/user-guide/user-guide-169.png)

If the hard disk to which the snapshot belongs is mounted and the mounted virtual machine is powered on, the data rollback operation cannot be performed. As shown in the figure below, you need to shut down the virtual machine or unbind the hard disk first.

### Delete snapshot
Platform snapshots are incremental snapshots. Subsequent snapshots only retain the changed data of the previous snapshot. When the user deletes a snapshot in the middle, only the blocks in the snapshot that are not referenced by the subsequent snapshots will be deleted, and the blocks that are referenced will be recorded in Subsequent snapshots.

Users are supported to delete any snapshot of a hard disk. Assuming that the user has made 10 snapshots of a hard disk, deleting any snapshot will not affect the data after the snapshot is rolled back.

- If the user deletes the first snapshot, the system will merge the data in the first snapshot into the second snapshot to ensure the accuracy of the data rolled back through the second snapshot;
- If the user only deletes the second snapshot, the system will only delete the data blocks in snapshot 2 that are not referenced by snapshot 3, and the data blocks referenced by 3 will be automatically recorded in snapshot 3 to ensure that snapshot 3 rolls back the data accuracy.

![1](/assets/images/user-guide/user-guide-170.png)

Only snapshots in a normal state are supported to be deleted. As shown in the figure above, users can delete a snapshot through the console snapshot list page, and the snapshot will be completely destroyed after deletion.

### Modify the snapshot name
Modify the name and comment of the snapshot, which can be operated in any state. It can be modified through the "Edit" button to the right of each snapshot name on the snapshot resource list page.