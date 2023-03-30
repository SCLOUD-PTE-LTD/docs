---
layout: default
title: External Storage
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/external-storage/
nav_order: 5
---
# External storage
## Product Description
### Overview
By default, the cloud platform provides distributed storage as virtualized back-end storage, providing high-availability, high-performance, highly reliable, and highly secure storage services for cloud platform users. At the same time, cloud platform virtualization supports docking with commercial storage devices, such as IPSAN and other storage arrays, to provide high-performance block storage services in the cluster for cloud platform virtual machines, and at the same time can benefit the centralized storage devices of old enterprise users, saving the total cost of ownership of information transformation as a whole.

External storage service is a commercial storage service provided by the cloud platform for enterprise users, which connects commercial storage through the ISCSI protocol, uses commercial storage as a virtualized back-end storage pool, provides storage pool management and logical volume allocation, and can be directly used as the system disk and data disk of the virtual machine, that is, as long as the storage device that supports the ISCSI protocol can be used as the back-end storage of platform virtualization, adapting to a variety of application scenarios.

The platform supports the docking and management of storage devices, and supports assigning LUNs in storage devices to tenants, and tenants assigning or mounting LUNs to the system disk or data disk of virtual machines for reading and writing data, with the following specific features:
- Supports input management of storage device resource pools, and one-click scanning of LUN storage volume information created in ISCSI devices.
- Support for assigning scanned LUN storage volumes to platform tenants, giving tenants permission to use disks as system or data disks for virtual machines.
- Tenants can use the LUN storage volume information of the privileged as the system disk of the virtual machine, so that the virtual machine can run directly into the direct commercial storage to improve performance.
- Enables tenants to use privileged LUN storage volume information as data disks for virtual machines.
- Support for reassigning storage volumes to other tenants of the platform.

Based on the above features, the platform can support the direct use of commercial storage devices as virtualized back-end storage, providing storage space for virtual machines with traditional commercial storage devices, without affecting other LUNs in commercial storage to provide storage services for other services.

The platform is based on the ISCSI protocol to dock commercial storage, and the LUNs of storage devices need to be mapped to platform compute nodes during the docking, so that virtual machines running on platform compute nodes can directly use the mapped LUNs; At the same time, in order to ensure the high availability of virtual machines, it is necessary to map LUNs to all compute nodes in a cluster at the same time, that is, all compute nodes can mount and use the mapped storage volumes, so as to ensure that the storage volume information can be mounted on each compute node during the outage migration.
- When the compute node on which the virtual machine resides fails, the platform automatically triggers the virtual machine downtime migration, that is, the virtual machine is migrated to the normal computing node in the computing cluster, so that the virtual machine can provide services normally.
- When the virtual machine is migrated to a new node in the cluster, you can directly use the mapped LUN storage to start the system disk or data disk of the virtual machine and mount it to the virtual machine normally to ensure the normal business after the virtual machine is migrated.

The platform only uses commercially stored LUNs as storage volumes, and does not manage the storage volumes themselves, such as LUN creation, mapping, expansion, snapshots, backups, rollbacks, cloning, and so on.

### Usage Process
Before using external storage, the platform manager or storage device manager needs to connect the external storage with the platform's computing node network, so that the computing nodes can communicate with the storage device directly on the internal network.

After the physical storage device and network are ready, you can connect with the platform and use the external storage services provided by the platform, and the entire docking process requires three roles of storage device administrator, platform administrator and platform tenant to operate, of which the operation related to the platform is the platform administrator and the platform tenant, as shown in the following figure process:

![1](/assets/images/product-functional-architecture-2.jpg)

- Storage device administrators manage storage volumes <br/>
All storage volume management is handled by the storage device administrator on the commercial storage management system, including the creation and mapping of storage volumes (LUNs), as well as the related lifecycle management of storage volume expansion, snapshots, backups, and deletions.
- The storage device administrator maps storage volumes to cluster compute nodes<br/>
The created LUN is mapped to all compute nodes on the storage device by the storage device administrator (if new compute nodes are added, they need to be mapped again), and multipath mapping is also possible.
- The platform administrator logs in and manages storage devices<br/>
After the storage volume LUN mapping is successful, the platform administrator enters the ISCSI storage pool or storage device in the management console "External Storage Cluster", and you need to specify the ISCSI address of the storage device, such as 172.18.12.8:8080 when entering.
- The platform administrator scans the mapped LUN information<br/>
After the storage device is entered, the platform administrator scans the storage device and information that have been mapped to the cluster node in the storage device with one click.
- The platform administrator assigns LUN devices to the tenant<br/>
The LUN storage volume device that successfully scans is assigned to the tenant by the Platform Administrator, and a storage volume can only be assigned to one tenant at a time, after allocation, the tenant can query the allocated storage volume device in the external storage device, and create virtual machines or mount virtual machines.
- Platform tenants use LUN storage volume devices<br/>
Platform tenants can directly query the allocated storage volumes through the console external storage, and specify the system disk type as external storage when creating a virtual machine, or they can directly attach LUN storage volumes to existing virtual machines and use them as data disks of virtual machines.

The premise for platform tenants to use external storage services is that storage volumes are mapped and assigned to tenants, and tenants only need simple binding to easily use the external storage devices provided by the platform, and can be elastically bound and unbound.

## View external storage devices
When the platform has provided external storage services and allocated storage volume LUN devices to tenants, the primary and sub-accounts of the tenants can directly query the authorized storage volume devices on the platform and attach the storage volume devices to virtual machines for data storage. The administrator allocates the storage volume as shown in the following figure:

You can log in to the console, enter the External Storage resource console through the console navigation bar [Hard Disk], and view the LUN storage volume information of the existing permissions through the external storage list, including name, resource ID, type, capacity size, status, cluster, LUNID, mounted resources, and operation items, as shown in the figure:

![1](/assets/images/user-guide/user-guide-51.png)

- Name/Resource ID: The name of the current storage volume and its globally unique identifier.
- Type: The type of storage volume, including system disk and data disk, and the type is system disk when you select an external storage volume as the system disk when creating a virtual machine. When a storage volume is bound to a virtual machine, the type is Datadisk.
- Capacity size: The capacity size of the storage volume, which is specified by the storage device administrator when it is created.
- Status: The status of the storage volume, including unbound, bound.
- Cluster: The storage pool cluster to which the volume belongs.
- LUNID: The LUNID of the current storage volume on commercial storage.
- Mount resource: The name and ID of the virtual machine that is mounted to the current storage volume.

The operation items on the list refer to the binding and unbinding operations of storage volumes, which support batch unbinding operations, and support searching storage volume devices and fuzzy search on the list.

## External storage as a system disk
When an external storage volume is allocated to a tenant, the LUN device with permission is automatically obtained in the external storage volume list, and the system disk type can be selected as a storage pool cluster with a qualified storage volume when creating a virtual machine.

You only need to select the [Storage Pool Name] entered by the platform administrator as the system disk type when creating the virtual machine, and select the storage volume device with permissions in the storage pool as the system disk of the virtual machine, so that the system disk of a virtual machine can be run directly on the external storage volume device.

As shown in the preceding figure, you can select an external storage volume from EMCPowerStore commercial storage as the system disk, which allows you to use the external storage volume as the system disk of the virtual machine. After you use the system disk of a virtual machine, you can view the used external storage volume information in External Storage in the virtual machine details.

The external storage LUN device that is the system disk of the virtual machine is consistent with the lifecycle of the virtual machine and does not support the unbind operation.

## External storage as a data disk
When a tenant is assigned an external storage volume, the LUN device with permissions is automatically obtained in the external storage volume list, and the LUN storage volume is used as a data disk in the same way as the default EVS disk of the platform, and only a simple binding operation is required.

Users only need to bind the LUN device directly to the virtual machine on the external storage device list, and a LUN storage volume can be used as a data disk of the virtual machine, only the storage volume device with the binding status of `unbound` state is supported, and the platform supports users to unbind the storage volume from the virtual machine at any time.

## Un-tethering External Storage
The platform supports unbinding storage volumes that have been mounted to virtual machine LUNs and rebinding them to other virtual machines. Only storage volumes with a bound status of "Bound" are supported, and the unbinding operation does not affect the normal access of the system disk of the virtual machine or the data in the storage volume device.