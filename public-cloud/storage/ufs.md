---
layout: default
title: UFS
parent: Storage
grand_parent: Public Cloud
permalink: /public-cloud/storage/ufs/
nav_order: 1
---
# File Storage (UFS)
## Product overview
File Storage (UFS) is a distributed file system product that provides highly available, reliable, and easily scalable file storage functions for various hosts running on SCloud public, physical, and managed clouds. The shared storage function provided by UFS products can easily provide strong support for various application scenarios such as data backup, serverless, AI data analysis, and high-performance web sites.

File storage products are divided into two types: capacity and performance. Capacity file storage uses common `SATA` media and is characterized by low price, high capacity. Performance file storage uses SSD media to provide low-latency IO access.

The file storage product adopts the multi-replica mechanism in the availability zone and ensures disaster recovery across physical machines in the cabinet, and the data durability is `99.999999%`. The TCP-based NFSv4 protocol is currently supported. The capacity of a single file system can reach `100PB` and supports online expansion, and the service is not aware of the expansion, see Product Limitations for specific restrictions.

File storage products are designed based on VPC virtual networks at the transport layer to ensure the security and reliability of data in network transmission.

## Product advantages
### Reliability
File storage uses a three-copy mechanism, and the replicas are deployed across cabinets to ensure that they are not affected by single-machine and single-disk failures, and can reconstruct data according to the priority of replica reliability to ensure controllable `MTTR`. The product comes with a 99.999999% data reliability guarantee.
### High elasticity
File storage supports elastic expansion, and scaling is not aware of applications that use file storage. Supports a maximum capacity of 100 petabytes (see Product Restrictions for the current space restrictions in each region).
### Shareability
File storage products use mount points to allow any host in the same VPC to access a specified file system to achieve data sharing. File storage does not limit the number of hosts on which a file system can be mounted, which is more friendly for scenarios that require a large number of nodes for shared data processing.

File storage also supports access to different host products such as public, physical, and managed clouds, providing a unified data sharing method for services with hybrid deployment in multiple scenarios.
### Ease of use
File storage products can be easily created, rescaled, deleted, and managed by the console for mount points. And because file storage supports the NFS protocol, `POSIX` semantics are well compatible, allowing `POSIX-based` applications to seamlessly interface with file storage products. All kinds of `Linux-based` file system management tools can be used directly without any changes to the previous development and operation habits.

## Application scenarios
File System (UFS) products are suitable for:

Due to its multi-node *mountability* and large flexible data space, file storage provides strong support for scenarios such as data sharing. You can select different types of file storage products according to the performance requirements of each business scenario, and the following is a recommended scenario classification.

Capacity Type:
- Log backups
- Database backups
- Web sites that require low latency
- Docker image repository
- Enterprise data management: internal sites, data archiving, etc

Performance Type:

- AI data analysis
- Big data processing, multi-node data sharing
- Web sites with low latency requirements
- Data sharing for containers
- Code hosting

## Key concepts
### File system
The file storage instance that the user applies for through the console and APIs, and the file system is the entity that the user performs operations such as data management and mount management.

### File system name
A user-defined string that describes the usage scenario and service type of a file system to identify a specific file system instance.
### Storage type
The product type of file system, the current storage type of file storage is divided into capacity type (SATA media) and performance type (SSD media) according to the storage media used by its data.
### Capacity
The size of the file system specified at the time of purchase remains unchanged until the capacity is expanded.
### Capacity remaining
As the number of files written to the file system and the directories created in the file system increase, the remaining capacity decreases, and you should expand the capacity in a timely manner when the remaining capacity is insufficient to the level required by the business.
### Mount point
Access the entry to the file system, and the mount operation can be completed by performing the mount operation on the host that needs to access the file system through the IP provided by the mount point. The mount point is within a VPC and allows hosts within the VPC to access the specified file system by selecting the specified VPC when you create the mount point.
### Mount address
The IP address provided by the mount point for the mount operation from which mount operations are performed within the host.
### Expansion
If the remaining capacity of the file system cannot meet the service availability requirements, you can resize the file system without affecting the applications using the file system without downtime or unmounting.
### Mount and unmount
The mount operation is the operation of executing the mount command on the host to display an instance of the UFS file system as an intra-host disk partition, and you can mount the file system to a specified directory mount point on the host.

The unmount operation detaches an instance of the file system from the host, and the operation that occurred in the previous directory mount point after the unmount will not be reflected on the UFS file system.
### Soft quotas
When the usage of the file system exceeds the purchased capacity, we allow the file system to have a certain proportion of additional capacity to cope with the ongoing IO, so as to avoid untimely expansion operations that cause the business to be unable to write. This additional space is scaled at 4% of purchased capacity and is capped at 1TB. We strongly recommend that you enable alerts for each file system instance and expand the capacity in a timely manner.

## Supported regions

### Capacity-based file storage

| Region | Supported or not | Protocol type | Remarks |
| --- | --- | --- | --- |
| North China One | Supported | NFSv4 |
| Shanghai II | Supported | NFSv4 |
| Guangzhou | Supported | NFSv4 |
| Fujian | Supported | NFSv4 | Linkage technology supports the activation of permissions |
| Hong Kong | Supported | NFSv4 |
| Taipei | Supported | NFSv4 | Linkage technology supports the activation of permissions |

### Performance-oriented file storage

| Region | Supported or not | Protocol type | Remarks |
| --- | --- | --- | --- |
| North China One | Supported | NFSv4 |
| North China II | Supported | NFSv4 | Linkage technology supports the activation of permissions |
| Shanghai II | Supported | NFSv4 |
| Guangzhou | Supported | NFSv4 |
| Fujian | Supported | NFSv4 | Linkage technology supports the activation of permissions |
| Lagos | Supported | NFSv4 | Linkage technology supports the activation of permissions |

*Note : Regions not listed in the table indicate that this type of file storage is not supported.*

## Product Restrictions
### Single-account quotas
There is a limit of 100 file system instances that a single account can create.

File system limits: 
- The file system supports only the NFSv4.0 protocol in TCP mode
- Currently, the maximum purchase capacity of the capacity type is `100TB`, and the maximum purchase capacity of the performance type is `20TB`
- The maximum size per file is limited to `1TB`
- The number of single file system nodes is limited to `1.000.000.000`
- There is no limit to the number of file system instances that can be attached to a single ECM
- There is no limit to the number of hosts that a single file system can be mounted
- The number of mount points for a single file system is limited to 5, note that the limit on the number of mount points is not the limit on the number of hosts that can mount a file system, as long as hosts in the VPC allowed by the mount point can access the specified file system
- Capacity single-file system throughput grows with capacity, with a single TB throughput of `120MBps` and a maximum of `20GBps`
- The maximum throughput of a single file system during the beta period is `1GBps`, please contact Technical Support for higher throughput
- Currently, the file system supports only the scale up operation, but does not support the scale in operation
- The NFSv4 file system supports cross-account and cross-region access based on VPC
  
Protocol Restrictions

| Protocol type | Unsupported protocols and semantics |
| -- | -- |
| NFSv4.0 | Unsupported attributes include: `ACL, WINDOWS_FILE_ATTRIBUTES (HIDDEN, ARCHIVE, SYSTEM), MIMETYPE, QUOTA_AVAIL_HARD, QUOTA_AVAIL_SOFT, QUOTA_USED, TIME_BACKUP, TIME_CREATE; UNSUPPORTED OPS INCLUDE: DELEGPURGE, DELEGRETURN, LOOKUPP, OPENATTR`; Delegation is not supported |

Billing policies

On-demand usage is not supported, and you can currently only use file storage by purchasing capacity upfront.

