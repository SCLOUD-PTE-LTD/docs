---
layout: default
title: UHost
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/uhost/
nav_order: 1
---
# UHost - Cloud Host
## Introduction
### What is a cloud host? 
Each cloud host runs as a virtual machine, and the host resources include the most basic computing components such as CPU, memory, and disk.
Cloud host is the most core service of SCloud, some services, such as IP, image, cloud disk, etc. must be combined with the cloud host after use, other services, such as database, cache, object storage, etc. can be combined with the cloud host to build IT architecture.
## Product advantages
### Flexible and resilient
According to the development trend of the business, users can scale cloud resources horizontally and vertically at any time to eliminate waste of resources.

Create or release an ECS at the minute level, with the online hot upgrade capability, that is, upgrade or downgrade the CPU and memory of the host within 5 minutes.

Upgrade or downgrade public bandwidth online, and customize the image feature to easily copy host data and environment.

It also provides open APIs and open source developer tools to meet the needs of batch management and automatic management.
### High availability
Strictly rent or build IDCs in accordance with Tier 3+/GB A standards, and follow IDC standards, server access standards, and O&M standards to ensure the high availability of cloud computing infrastructure frameworks, data reliability, and cloud server high availability.

There are multi-AZ in the regions where UCD is located, and you can use the multi-AZ deployment solution to build primary/standby services or active-active services to improve availability.

For solutions for two regions and three centers in the financial field, higher availability services can be built through multi-region and multi-AZ. 

Smooth switching can be achieved between the provided cloud services, including disaster recovery, backup and other services, and there are very mature solutions.
### Stable and reliable
Commitment to 99.95% service availability, data reliability is not less than 99.9999%.

The local disk of the cloud host adopts RAID for data protection to prevent data loss. 

Cloud disks have three-copy disaster recovery capabilities, I/O isolation, and rapid data migration. Kernel optimization and hot patch technology, uninterrupted online migration technology, provide data snapshots and other functions, failure failure, 100 times compensation.
### Performance
The host CPU and memory performance indicators are excellent, and the unique storage technology (IO acceleration technology) increases the random read and write I/O capabilities of the disk by 10 times that of ordinary SAS disks.

There are also SSD disk cloud hosts, providing ultra-high IOPS performance. Excellent network processing capabilities to meet various business application requirements.
### Security guarantees
100% complete network isolation between tenants to ensure data security.

Provides functions such as network firewalls and security groups to strictly control public network connections. In conjunction with the VPC feature, you can create a private subnet under a single account to support your internal security management requirements. 

Provides a wealth of monitoring and security tools, DDOS protection, intrusion detection, data leakage detection, vulnerability protection and other comprehensive security protection solutions to protect business data security in an all-round way. 

You can create snapshots and back up data through manual and automated snapshot backup policies without worrying about the risk of data loss such as operational errors.
### Data Center
You can launch cloud host products in more than 18 regions around the world to cover all five continents, and provide high-quality cloud computing services with consistent experience for your global business

![1](/assets/images/uhost1.png)

### Seconds Ark backup capability
UDataArk provides continuous data protection (CDP) services for SCloud local disks/EVS disks, and is an automatic data backup system that is completely heterogeneous with local disks/EVS disk storage, which can prevent data deletion or loss caused by mis-operation and hacker attacks to the greatest extent. 

The Ark product capability provided by SCloud has an RPO of seconds, which supports restoring any second within 12 hours by default, and can also customize the second-level backup time according to user business needs. 

The RTO of the data ark can be restored in as little as 5 minutes, and the amount of 1TB data can be restored within 1 hour. 

For the cloud disk backup solution, SCloud launched Disk Snapshot Service (USnap), which provides the ability to create snapshots for all series of cloud disks (normal/SSD/RSSD) based on the data ark CDP technology, in addition to the automatic backup capability of the basic layer ark, you can also manually make snapshots.



### Flexible CPU and memory ratio
SCloud provides 1:8 ratio capability of CPU and memory, flexible configuration, to meet the needs of different customers' business scenarios, and the cost can be optimized.

## Introduction to the feature
All SCloud data center nodes can support the creation of cloud hosts

A variety of host models meet the needs of customers in different business scenarios. The previous generation includes `general-purpose N-type, high-frequency C-type and GPU models`; The new generation of outstanding series products include outstanding O type, outstanding Pro type, outstanding shared type and outstanding memory type

Flexible host specifications, CPU and memory sizes can be customized combinations, CPU memory ratio from `1:1` to `1:8`, can meet the customized needs of different businesses, can maximize the cost of enterprise IT expenditure. You can also create a more flexible CPU memory ratio through the API.

Three disk types: local ordinary disk, local SSD disk, and cloud disk (UDisk), of which EVS disk includes ordinary cloud disk, SSD cloud disk, and RSSD cloud disk, and can be freely combined with different performance, price, availability, and capacity limit;

Support common Linux and Windows images, support custom images;

Internet firewall, providing network security access control function;

Supports two network modes: basic network and VPC;

Multiple monitoring indicators comprehensively monitor the health of the host;

Supports the use of load balancing products (ULB) to build high-availability clusters.

The data ark function can be activated and restored to any second within 12 hours;

Some hosts can enable the hot upgrade function, and the CPU/memory upgrade does not need to be shut down;

Multiple billing models: monthly prepaid annually, and hourly prepaid.
## Cloud Instance Type

UHost are divided into four types according to the application scenarios: Outstanding (O), Standard (N), High Frequency (C).

|         | Features          | Applicable scene |
|:-------------|:------------------|:------|
| Outstanding (O) | The latest generation of cloud host with excellent computing, storage and network performance. Latest Intel Cascade Lake and AMD EPYC2 processor | Database, MMO games, artificial intelligence, etc. |
| Standard (N) | Flexible configuration & Great Choices   | Enterprise-level applications, memory services, data analysis, etc. |
| High Frequency (C) | Adopts 3.2GHz CPU with strong computing performance | High frequency trading, data processing, graphics rendering, etc. |

Summary of basic parameters of cloud instance:

|  | Standard N	| High Frequency C | Outstanding O |
|:-------------|:-------------|:------------------|:------|
| CPU: Memory |	1:1-1:8 |	1:1-1:8 |	1:1-1:8 |
| CPU Type	| IvyBridge/Haswell/Broadwell/Skylake |	Skylake (models with main frequency ≥3.0GHz) | Skylake/Cascadelake
| Hard Disk type	| Cloud disk, Standard local disk, SSD local disk	| Cloud disk, SSD local disk	| RSSD cloud disk, SSD cloud disk
Network	| Network enhancement 1.0/Network enhancement 2.0 (only Skylake and above support) and Hot Scale-Up	| Network enhancement 1.0 and Hot Scale-Up	| Network enhancement 2.0 and Hot Scale-Up

### Standard Instance N

- Scenario: Provide the most flexible and free combination of CPU, memory, and disk. Suitable for balanced scenarios such as computing, storage, and networking.
- CPU platform support: IvyBridge/Haswell/Broadwell/Skylake
- CPU memory combination (support ratio 1:1-1:8):

| CPU | RAM |
| --- | --- |
| 1 core | 1G, 2G, 4G, 8G |
| 2 core | 2G, 4G, 6G, 8G, 12G, 16G |
| 4 core | 4G, 6G, 8G, 12G, 16G, 32G |
| 8 core | 8G, 12G, 16G, 24G, 32G, 48G, 64G |
| 16 core | 16G, 24G, 32G, 48G, 64G, 128G |
| 24 core | 24G, 32G, 64G, 96G, 192G |
| 32 core | 32G, 64G, 96G, 128G |

- Disk type support: support cloud disk, Standard local disk, SSD local disk

Specific selection range:

| System Disk | Data Disk |
| --- | --- |
| SSD cloud disk (`20-500GB`) | SSD cloud disk (`20-4000GB`),Standard cloud disk (`20-8000GB`) |
| Standard local disk (`20-100GB`) | Standard local disk (`20-2000GB`) |
| SSD local disk (`20-100GB`) | SSD local disk (`20-1000GB`) |

- Feature support: network enhancement 1.0/network enhancement 2.0 (only Skylake and above support) and Hot Scale-Up

### High Frequency Instance C

- Scenario: Models with CPU frequency `≥3.0GHz`, suitable for computing services, such as high-frequency trading, rendering, artificial intelligence, etc.
- CPU platform support: Skylake
- CPU memory combination (support ratio `1:1 - 1:8`):

| CPU | RAM |
| --- | --- |
| 1 core | 1G, 2G, 4G, 8G |
| 2 core | 2G, 4G, 8G, 16G |
| 4 core | 4G, 8G, 16G, 32G |
| 8 core | 8G, 16G, 32G, 64G |
| 16 core | 16G, 32G, 64G, 128G |
| 32 core | 32G, 64G, 128G |

- Disk type support: support cloud disk, SSD local disk

Specific selection range:

| System Disk | Data Disk |
| --- | --- |
| SSD cloud disk (20-500GB) | SSD cloud disk (20-4000GB),Standard cloud disk (20-8000GB) |
| SSD local disk (20-100GB) | SSD local disk (20-1000GB) |

- Feature support: network enhancement 1.0 and Hot Scale-Up.

#### Outstanding Instance O

The latest generation of cloud hosts with excellent computing, storage and network performance. Suitable for all type of demanding scenarios.

- CPU platform support: Skylake/Cascadelake
- CPU memory combination (support ratio 1:1-1:8):

| CPU | RAM |
| --- | --- |
| 4 core | 4G, 8G, 16G, 32G |
| 8 core | 8G, 16G, 32G, 64G |
| 16 core | 16G, 32G, 64G, 128G |
| 32 core | 32G, 64G, 128G, 256G |
| 64 core | 64G, 128G, 256G |

- Disk type support: support RSSD cloud disk, SSD cloud disk.

- Specific selection range:

| System disk | Data disk |
| --- | --- |
| SSD cloud disk (20-500GB) | RSSD cloud disk (20-32000GB) |

- Feature support: network enhancement 2.0 and Hot Scale-Up.

## UDisk performance

### Performance indicators
There are three important metrics to evaluate EVS disk performance:
- IOPS: The number of reads and writes per second.
- Throughput: Read and write IO traffic per second.
- IO delay: The time between IO submission and completion of IO.

In theory, the larger the IOPS and throughput, the better, and the lower the latency, the better.

#### IOPS

IOPS (Input/Output Operations Per Second) is a measurement method used for performance testing of computer storage devices such as hard disk drives (HDDs), solid-state drives (SSDs) or storage area networks (SANs), which can be regarded as the number of reads and writes per second. IOPS mainly includes four types of IOPS indicators according to the different test tendencies: random read IOPS, random write IOPS, sequential read IOPS, and sequential write IOPS.

| IOPS type | Illustrate |
| --- | --- |
| Random read IOPS | The average number of random reads per second |
| Write IOPS randomly | The average number of random writes per second |
| Read IOPS sequentially | The average number of sequential reads per second |
| Write IOPS sequentially | The average number of sequential writes per second |

#### Throughput

Throughput is the average amount of data that a disk can successfully deliver per unit of time. Throughput is usually expressed in megabytes per second (MB/s or MBps).

#### IO latency

IO latency refers to the time it takes for an IO request to be made and the request to be completed.

### Performance comparison
UDisk mainly includes 3 types of products: RSSD UDisk, SSD UDisk and Standard UDisk.

- RSSD: The bottom layer uses NVMe SSD as the storage medium, and the network transmission uses `RDMA` (Remote Direct Memory Access). 
- SSD: The bottom layer uses NVMe SSD as the storage medium.
- Standard: The bottom layer uses HDD mechanical disk as the storage medium.

The following table shows the performance of cloud disks for the three products:

| Parameter | RSSD UDisk | SSD UDisk | Standard UDisk |
| --- | --- | --- | --- |
| Single-disk IOPS |min {1800+50*capacity, 1200000} |  min {1200+30*capacity, 24000} | 1000(peak value) |
| Throughput per disk | min {120+0.5*capacity,4800}MBps | min {80+0.5*capacity, 260}MBps | 100MB/s (maximum). |
| Average latency | 0.1-0.2ms | 0.5-3ms | 10ms |

### RSSD performance and instance performance relationship
The IO performance of a VM instance is proportional to its CPU configuration, and the higher the number of VM cores, the higher the storage IOPS and throughput.
- If the performance of an RSSD disk does not exceed the IO storage capacity of the instance, the actual storage performance is subject to the performance of the RSSD disk
- If the performance of an RSSD disk exceeds the IO storage capacity of the instance, the actual storage performance is subject to the storage performance of the instance
- If the number of cores of the instance is not in the table below, the instance performance is the maximum performance that does not exceed the number of cores, for example, if the number of CPU cores is 50, its storage IO performance is the same as that of 48 cores

| vCPU (core) | Storage IOPS | Storage throughput (MBps) |
| --- | --- | --- |
| 1 | 18.000 | 75 |
| 2 | 38.000 | 150 |
| 4 | 75.000 | 300 |
| 8 | 150.000 | 600 |
| 12 | 225.000 | 900 |
| 16 | 300.000 | 1200 |
| 24 | 450.000 | 1800 |
| 32 | 600.000 | 2400 |
| 48 | 900.000 | 3600 |
| 64 | 1.200.000 | 4800 |