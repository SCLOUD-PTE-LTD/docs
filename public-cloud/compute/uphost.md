---
layout: default
title: UPHost
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/uphost/
nav_order: 2
---

# UPHost（SCloud Physical Host）
## What is a physical cloud host?
SCloud Physical Host provides dedicated physical servers. It provides excellent computing performance to meet the high performance and stability requirements of core application scenarios, and can be flexibly combined with other cloud products.
## Product advantages
### Excellent performance
Dedicated physical cloud hosts with high-specification hardware and I/O devices provide excellent server performance to meet the needs of your high-performance application scenarios.
### Meet special needs
Dedicated server configuration specially created for special application scenarios, high-quality resources and the most suitable price, both fish and bear's paw.
### Fast onboarding
Eliminating the O&M costs of traditional physical hosts, transportation, construction, deployment, and debugging, and lengthy processes are all missing, enjoying the same convenient management functions as cloud hosts, and the system is quickly launched.
### Cloud portfolio use
The physical cloud host can be seamlessly connected with the cloud host and a variety of products. Depending on your application needs, you can combine the benefits of multiple products to achieve a rich cloud architecture for better performance, security, ease of use, and affordability.
### Stable and secure
Network multi-link and isolation technology ensures internal and external security, and RAID10 provides data redundancy technology to maintain data worry-free.
## Region and Availability Zone

An Availability Zone is a combination of physically andpower-isolatedresources. An availability zone may be designed by combining one data center or multiple data centers. After proper design, the fault affectedareawill be isolated ina singlezone.

Several availability zones that are geographically close to each other can be connectedthroughhigh-speedand stablenetwork connections toform a region. Intranets can be interconnectedbetween zones in the same region.

At present, the regions where physical cloud products are available includeChinaNorth 1, Shanghai Financial Cloud, Shanghai 2, Guangzhou, Fujian, Hong Kong, Singapore andLondon.

The detailed Availability Zones are shown in the following table:

| Region | Availability Zone |
| --- | --- |
| North China One | Availability Zones B, C, D, E |
| Shanghai Financial Cloud | Zone A |
| Shanghai II | Availability Zones B and C |
| Guangzhou | Zone B |
| Fujian | GPU Zone A |
| Hong Kong | Zone A |
| Singapore | Zone A |
| Renton | Zone A |

## Mirror image

An image is a template for the running environment of a physical cloud host instance, including the operating system, preinstalled software, and configuration.

### Mirrors fall into two categories:
- Standard images are officially provided by SCloud and include various Linux, Windows and other operating systems.
- A self-made image is a dedicated image created by a user using a physical cloud host and is visible only to the user.

### Homemade mirroring

- Bare metal 2.0 adopts smart network card and RSSD cloud disk technology, for Linux images, after testing, the standard image CentOS uses the 3.10.0 kernel, and Ubuntu uses the 5.4 kernel.

- During the process of making homemade images, please note the following:
If it is not necessary, do not upgrade the kernel, as upgrading the kernel will cause the intelligent NIC driver to be incompatible and cause the machine to fail to boot and system instability. If necessary, please contact sales or technical support.

- The basic system has a root partition and a boot partition, the boot partition is a UEFI boot partition, do not modify, modification will cause the machine to fail to boot.

The base image is equipped with SCloud custom version cloud-init, cloud-init initializes the necessary software for the machine, do not uninstall or modify it.
Currently, the image of the physical cloud host only supports CentOS, Ubuntu, and Windows, and is not interoperable with the image of the cloud host.

## Network and security

### Private IP

In the basic network mode, the private IP address is uniformly assigned by the system. If you change the private IP address within the operating system, intranet communication will be interrupted. Communication between hosts in the same data center via private IP is free of charge. 

Private IP addresses can be used for intranet access between physical cloud instances, or for intranet access between physical cloud hosts and other cloud services, such as Uhost, UDB, UMem.
In addition, SCloud also supports intranet virtual IPs, which can be directly set on physical cloud hosts after application.

### Internet IP

The public IP address is the main way for users to access the cloud host and the host instance to provide external services. In SCloud, the external IP address is elastic migration, and when a host fails, the external IP address can be easily migrated to another host, that is, the "elastic IP". 

When you create a physical ECS, if you select both an EIP and an Internet bandwidth quota, an EIP is allocated and bound to your physical ECM. You can also find this IP resource information in the data panel of the network product.

### How to manage Elastic IPs firewall

Provides firewall functions for physical cloud hosts, and by binding firewall rules to physical cloud hosts, the public network access of physical cloud hosts can be controlled and managed, providing necessary guarantees for host security. The firewall supports TCP/UDP/ICMP/GRE protocols. 

We have created several sets of default firewalls for you, TCP ports 22 and 3389 and PING are enabled by default, and you can adjust or create more firewall policies according to your business conditions.
How to manage firewalls

## RAID

### RAID
Redundant Arrays of Independent Drives. It refers to the combination of multiple disks through disk array cards to improve disk IO capabilities or provide redundancy. SCloud Physical Cloud offers RAID 1, 0, 10, 5 and NoRaid 5 different RAID options that can be selected when creating/reinstalling a physical cloud host.
### RAID 1
RAID 1 ensures data security through data mirroring, the read and write performance is equivalent to that of a single disk, data is backed up in mirrors, and the utilization rate of storage space is 50%, which is suitable for occasions with high requirements for data security.
### RAID 0
Risk warning: RAID 0 has no redundancy and data repair capabilities, and no data security guarantee. In the event of a single disk failure, the entire RAID group is unavailable.
RAID 0 adopts data stripe distribution, has the highest read and write performance, and has 100% disk utilization, but there is no redundancy guarantee, and cannot be used in situations with high data security requirements.
### RAID 5
RAID 5 provides parity redundancy for data, data stripe distribution, high read and write performance, storage space utilization greater than 50%, but slightly less secure than RAID 10 and RAID 1. It is suitable for more reads and less writes, and is the best compromise between performance, data redundancy and cost.
### RAID 10
Combining the characteristics of RAID 0 data stripe distribution and RAID 1 mirror redundancy, it can not only provide high read and write speed, but also make data be mirrored and protected, and the storage space utilization rate is 50%, which is more commonly used in practical applications.
### NoRAID
Risk warning: NoRAID has no redundancy and data repair capabilities, and no data security guarantee.
Do not make RAID. N disks with exclusive IO are visible in the system. In the event of a single disk failure, the other disks remain available.
Some models do not have a disk array card, so only NoRAID is selected.