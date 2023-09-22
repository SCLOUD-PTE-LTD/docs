---
layout: default
title: External Storage
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/external-storage/
nav_order: 7
---
# 7. External Storage

## 7.1 Product Overview

### 7.1.1 Overview

The cloud platform provides distributed storage as the backend storage for virtualization by default, providing cloud platform users with high availability, high performance, high reliability, and high-security storage services. At the same time, the cloud platform's virtualization supports integration with commercial storage devices such as IPSAN storage arrays, providing high-performance block storage services for virtual machines in the cluster. It also enables old centralized storage devices of enterprise users to be used, thereby reducing the total cost of information technology transformation.

External storage services are commercial storage services provided by the cloud platform for enterprise users. Through the ISCSI protocol and FC protocol, commercial storage is integrated as a backend storage pool for virtualization, providing storage pool management and logical volume allocation. It can be directly used as the system disk and data disk of virtual machines. Therefore, any storage device that supports the ISCSI protocol and FC protocol can be used as the backend storage for the platform's virtualization, making it suitable for various application scenarios.

The platform supports the integration and management of storage devices and supports the allocation of LUNs in storage devices to tenants. Tenants can assign or mount LUNs to the system disk or data disk of virtual machines for data read and write. The specific functional characteristics are as follows:

* Supports the input and management of storage device resource pools and supports one-click scanning of the created LUN storage volume information in ISCSI devices.
* Supports scanning the LUN storage volume information already created in FC-SAN devices connected to the cloud platform.
* Supports assigning the scanned LUN storage volume to the platform tenant, giving the tenant permission to use the disk as the virtual machine's system disk or data disk.
* Supports tenants using LUN storage volume information to run virtual machines directly in commercial storage, enhancing performance.
* Supports tenants using LUN storage volume information as the data disk of virtual machines.
* Supports reallocating storage volumes to other tenants on the platform.

Based on the above functional features, the platform supports using commercial storage devices as virtualized backend storage, providing virtual machines with storage space from traditional commercial storage devices, without affecting other LUNs in the commercial storage that provide storage services for other businesses.

The platform interfaces with commercial storage using the ISCSI protocol and FC protocol. In the process of interfacing, the storage device's LUN needs to be mapped to the platform's computing nodes, so that virtual machines running on the platform's computing nodes can directly use the mapped LUN. At the same time, to ensure high availability of the virtual machines, the LUN needs to be mapped to all computing nodes within a cluster, meaning that all computing nodes can mount and use the mapped storage volume to ensure that the information stored in that volume is accessible from any computing node during a failure migration.

* When the computing node where the virtual machine is located fails, the platform automatically triggers a virtual machine shutdown and migration process, which migrates the virtual machine to a normally functioning computing node within the cluster, allowing the virtual machine to continue providing services.
* The LUN storage volumes used by the virtual machines are mapped to all computing nodes in the cluster. After the virtual machine is migrated to a new node within the cluster, it can directly use the already mapped LUN storage to boot the system disk or data disk, and mount them to the virtual machine to ensure normal business operations after migration.

The platform only uses commercial storage LUNs as storage volumes and does not manage the LUNs themselves, such as creating, mapping, expanding, snapshotting, backing up, rolling back, cloning, etc.

### 7.1.2 Usage Procedure

Before using external storage, the platform administrator or storage device administrator needs to connect the external storage to the platform's computing node network, so that the computing nodes can communicate directly with the storage devices on the intranet.

Once the physical storage device and network are ready, it can be integrated with the platform and use the external storage service provided by the platform. The entire integration process requires actions from three roles: the storage device administrator, the platform administrator, and the platform tenant. The platform-related operations are performed by the platform administrator and platform tenant, as shown in the flowchart below:

![storageflow](/assets/images/userguide/storageflow.png)

1. **Storage Device Administrator Manages Storage Volumes**

   All storage volumes are managed by the storage device administrator through the management system of the commercial storage device, including the creation and mapping of storage volumes (LUNs), as well as related lifecycle management such as expansion, snapshots, backups, and deletion.

2. **Storage Device Administrator Maps Storage Volumes to Cluster Computing Nodes**

   The created LUNs are mapped by the storage device administrator to all computing nodes on the storage device (if a new computing node is added, the mapping needs to be repeated), and multipath mapping can also be performed.

3. **Platform Administrator Enters and Manages Storage Devices**

   After the storage volume LUN is successfully mapped, the platform administrator enters the iSCSI storage pool or storage device in the "External Storage Cluster" of the management console, and specifies the iSCSI address of the storage device during entry, such as 172.18.12.8:8080.

4. **Platform Administrator Scans Mapped LUN Information**

   After entering the storage device, the platform administrator scans the storage device to obtain information about the storage volume devices already mapped to the cluster nodes in the iSCSI storage device; when an FC-SAN storage device is connected to the computing cluster, the already mapped storage volume devices and information on the cluster nodes are obtained through scanning.

5. **Platform Administrator Allocates LUN Devices to Tenants**

   The platform administrator designates the successfully scanned LUN storage volume devices to tenants, and a storage volume can only be assigned to one tenant at a time. After allocation, the tenant can query the allocated storage volume devices in the external storage device, and create virtual machines or mount existing virtual machines.

6. **Platform Tenant Uses LUN Storage Volume Devices**

   Platform tenants can directly query the allocated storage volumes through the external storage console, and specify the system disk type as external storage when creating a virtual machine, or directly mount LUN storage volumes to existing virtual machines as data disks.

The prerequisite for using external storage services by platform tenants is that the storage volumes have been mapped and allocated to the tenants, and tenants only need to bind them simply to use the external storage devices provided by the platform, with flexible binding and unbinding.

## 7.2 Viewing External Storage Devices

When the platform has provided external storage services and allocated storage volume LUN devices to tenants, the main account and sub-accounts of the tenant can directly query the authorized storage volume devices on the platform, and mount the storage volume devices to virtual machines for data storage. The allocation of storage volumes by administrators is shown in the following figure:

![AllocateLUN](/assets/images/userguide/AllocateLUN.png)

Users can log in to the console, enter the "External Storage Disk" resource control panel through the console navigation bar "Storage", and view the authorized LUN storage volume information, including name, resource ID, type, capacity size, status, cluster, LUNID, mounted resources, and operations, as shown in the figure below:

![lunlist](/assets/images/userguide/lunlist.png)

* Name/Resource ID: The name and globally unique identifier of the current storage volume.
* Type: The type of the storage volume, including system disk and data disk. When an external storage volume is selected as the system disk when creating a virtual machine, the type is system disk; when the storage volume is bound to a virtual machine, the type is data disk.
* Capacity size: The capacity size of the storage volume, specified by the storage device administrator during creation.
* Status: The status of the storage volume, including unbound and bound.
* Cluster: The storage pool cluster to which the storage volume belongs.
* LUNID: The LUNID of the storage volume in the commercial storage device.
* Mounted Resources: The name and ID of the virtual machine to which the storage volume is mounted.

The operations on the list refer to binding and unbinding the storage volumes, supporting batch unbinding operations, and also supporting search for storage volume devices on the list, with support for fuzzy search.

## 7.3 External storage as system disk

The platform supports using a LUN storage volume device as a virtual machine's system disk. When a tenant is assigned an external storage volume, they will automatically obtain the authorized LUN devices in the list of external storage volumes, and when creating a virtual machine, the system disk type can select the storage pool cluster of authorized storage volumes.

Users only need to select the storage pool name entered by the platform administrator when creating a virtual machine, and select the authorized storage volume device in the storage pool for the virtual machine's system disk. This allows a virtual machine's system disk to be directly operated on an external storage volume device.

> The external storage LUN device used as a virtual machine's system disk has the same lifecycle as the virtual machine and does not support unbinding operations.

## 7.4 External storage as data disk

The platform supports using a LUN storage volume device as a virtual machine's data disk. When a tenant is assigned an external storage volume, they will automatically obtain the authorized LUN devices in the list of external storage volumes. The usage of LUN storage volumes as data disks is the same as the platform's default cloud disk, requiring only simple binding operations.

Users only need to directly bind the LUN device to the virtual machine in the list of external storage devices to use a LUN storage volume as a virtual machine's data disk. Only storage volume devices in an "unbound" status can be bound, and the platform supports users unbinding the storage volume from a virtual machine at any time.

## 7.5 Unbinding external storage

The platform supports unbinding a LUN storage volume that has been mounted to a virtual machine and re-binding it to another virtual machine. Only storage volumes in a "bound" state can be unbound. Unbinding does not affect normal access to the virtual machine system disk or data stored in the storage volume device.

## 7.6 Setting as shared disk

The platform supports setting an external storage volume as a shared disk, and this operation is irreversible.

![setluntosharedisk](/assets/images/userguide/setluntosharedisk.png)
