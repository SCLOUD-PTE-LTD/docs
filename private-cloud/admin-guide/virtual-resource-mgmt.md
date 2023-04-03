---
layout: default
title: Virtual Resource Management
parent: Admin Guide
grand_parent: Private Cloud
permalink: /private-cloud/admin-guide/virtual-resource-management/
nav_order: 4
---
# Virtual resource management

The platform provides administrators with the full life cycle operation and management capabilities of virtual resources of all tenants on the platform, so that platform administrators can uniformly manage and control the overall virtual resources of the platform through the console, including all products and services on the tenant side, such as virtual machines and virtual machine templates , elastic network card, VPC network, external network IP, security group, load balancing, NAT gateway, cloud hard disk, snapshot and other resources, and supports single-disk QoS control for cloud hard disk resources, which is used to control the read and write IOPS of a single cloud hard disk and read and write bandwidth.

## Virtual Machine Management
The platform supports administrators to specify tenants to create virtual machines with Windows, Redhat, Ubuntu, and CentOS operating systems, and the created virtual machines belong to the specified tenants.

At the same time, it supports administrators to manage the full lifecycle of virtual machines of all tenants on the platform, such as viewing virtual machine details, VNC login, startup/restart/shutdown/power-off, password reset, system disk expansion, mirroring, and system reinstallation , Bind the external network IP, unbind the external network IP, set the default exit, modify the internal/external network security group, modify the virtual machine configuration, perform an upgrade, modify the name and notes, modify the alarm template, and delete and destroy the virtual machine.

## Virtual Machine Template
The platform supports administrators to designate tenants to create virtual machine templates, and the created virtual machine templates belong to the specified tenants; at the same time, it supports administrators to manage the full life cycle of virtual machine templates of all tenants on the platform, such as viewing, creating virtual machines, deleting, etc. .

## ENI Management
Supports administrators to designate tenants to create ENIs. The created ENIs belong to the specified tenant and can only be bound to virtual machines of tenants. Administrators are also supported to manage the entire life cycle of ENIs of all tenants on the platform, such as viewing, binding Set, unbind, delete, etc.

## VPC network management
Support administrators to designate tenants to create VPC networks and subnets. The created VPCs and subnets belong to the specified tenants. When creating virtual resources, only the VPC and subnet resources owned by the tenants can be selected; at the same time, administrators are supported to control the VPC networks and subnets of all tenants on the platform. Perform full lifecycle management, such as viewing, deleting, creating subnets, deleting subnets, etc.

## External network IP management
Support the administrator to designate tenants to apply for the external network IP address of the specified IP network segment. The applied IP address belongs to the designated tenant, and only supports the application for the network segment that the specified tenant has permission to apply for IP address; at the same time, it supports the administrator to apply for the external network IP address of all tenants on the platform. Network IP address management, such as viewing, binding, unbinding, modifying bandwidth, deleting, etc., only supports binding virtual resources to tenants with the same IP address as the external network.

## Security Group Management
Support administrators to designate tenants to create security groups. The created security groups are owned by the specified tenants. Only virtual resources bound to the same tenant as the security group are supported. At the same time, administrators are supported to manage the entire life cycle of security groups of all tenants on the platform, such as Change and management of security group rules.

## Load Balance Management
Support administrators to designate tenants to create load balancing instances for tenants. The instances belong to the designated tenants. Only virtual machine resources that the designated tenants have permission to be added to service nodes are supported. At the same time, administrators are supported to manage all load balancing on the platform, such as VServer and service nodes. , domain name forwarding strategy and SSL certificate management, etc.

## NAT Gateway Management
Supports administrators to specify tenants to create NAT gateway instances for tenants. The instances belong to the specified tenants, and only supports adding virtual resources that the specified tenant has permission to SNAT and DNAT rules; at the same time, it supports administrators to manage all NAT gateways on the platform, such as all NAT gateways SNAT and DNAT rule management.

## Hard Disk Management
### Tenant Cloud Disk Management
Supports administrators to designate tenants to create cloud hard disks. The created cloud hard disks are owned by the designated tenants. Only application for cluster types that the specified tenants have permission to create cloud hard disks is supported. At the same time, administrators are supported to manage the full life cycle of cloud hard disk resources of all tenants on the platform. , such as view, bind, unbind, expand, snapshot, delete, etc., the deleted cloud hard disk will enter the tenant’s recycle bin, only support binding to the virtual machine of the same tenant as the cloud hard disk, and the snapshot made at the same time belongs to the tenant.

### Cloud Disk QoS
The platform globally provides global cloud disk QoS configuration by default, that is, newly created cloud disks will be assigned QoS values according to platform formulas to limit platform users from forcibly occupying disk performance. At the same time, the platform supports the administrator to customize the QoS value for the cloud disks of all tenants on the platform. Only when the global QoS configuration is enabled, the QoS customized by the administrator for each cloud disk will take effect.

After each cloud hard disk is created, the administrator can perform "QoS configuration" on the storage resource - hard disk list, and at the same time perform QoS configuration on the system disk in the virtual machine details disk, as shown in the figure below. The QoS items that can be set include :

![1](/assets/images/admin-guide/admin-guide-10.png)

- Read/Write IOPS<br/>
When the Arch architecture of the disk is HDD, the range of read/write IOPS that can be set is 0~50000, the default value is 1000, and the configuration is 0 for unlimited speed.<br/>
When the Arch architecture of the disk is SDD, the range of read/write IOPS that can be set is 0~50000. The default value is the value calculated by the calculation formula based on the current hard disk capacity, and the configuration is 0 without speed limit.
- Read and write bandwidth (MBps)<br/>
When the Arch architecture of the disk is HDD, the read/write bandwidth range that can be set is 0~1000Mbps, the default is 100, and the configuration is 0 for unlimited speed.<br/>
When the Arch architecture of the disk is SSD, the read/write bandwidth range that can be set is 0~1000Mbps. The default is the value calculated by the calculation formula based on the current hard disk capacity. If it is set to 0, the speed is not limited.

After the capacity of the hard disk is expanded, the QoS value of the new capacity will be recalculated according to the calculation formula, and the QoS of the hard disk will be reset according to the calculated QoS value.

- If the QoS value set before hard disk expansion is < the new capacity QoS value, the new capacity QoS value shall prevail.
- If the QoS value set before hard disk capacity expansion > new capacity QoS value, the value set before capacity expansion shall prevail.

The hardware medium and capacity will affect the read/write IOPS and broadband speed of the hard disk. If the configured QOS exceeds the performance of the hardware itself, the hardware performance shall prevail. The system will assign QOS values by default. If you want to cancel the speed limit function of a hard disk, you can configure both IOPS and bandwidth to 0.

## Snapshot Management
Supports administrators to view and manage cloud disk snapshot resources of all tenants on the platform, such as rollback and deletion, and only supports rollback of snapshot data to the original cloud disk.

## External storage management
Support administrators to view and manage all external storage LUN storage volume information assigned to tenants on the platform, and support administrators to bind and unbind storage volume LUN devices, only support binding LUN devices to virtual machines belonging to tenants Instances provide data disk services for virtual machines belonging to tenants.

When creating a virtual machine for a tenant, the administrator can also choose an authorized external storage LUN device as the system disk of the virtual machine.