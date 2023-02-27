---
layout: default
title: UHost
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/uhost/
---

# 1.1 UHost Cloud Compute Instance

SCloud UHost is based on mature cloud computing technology, high-performance infrastructure, high-quality network bandwidth, high-quality data center and other resources. It provides safe and stable, fast deployment, flexible expansion, and convenient management computing unit.

## 1.1.1 Cloud Instance Type

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

### 1.1.1.1 Standard Instance N

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
| SSD cloud disk (20-500GB) | SSD cloud disk (20-4000GB),Standard cloud disk (20-8000GB) |
| Standard local disk (20-100GB) | Standard local disk (20-2000GB) |
| SSD local disk (20-100GB) | SSD local disk (20-1000GB) |

- Feature support: network enhancement 1.0/network enhancement 2.0 (only Skylake and above support) and Hot Scale-Up

### 1.1.1.2 High Frequency Instance C

- Scenario: Models with CPU frequency ≥3.0GHz, suitable for computing services, such as high-frequency trading, rendering, artificial intelligence, etc.
- CPU platform support: Skylake
- CPU memory combination (support ratio 1:1-1:8):

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

#### 1.1.1.2.1 Outstanding Instance O

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


## 1.1.2 Product Advantages

- Elastic and Flexible

You can easily scale cloud resources horizontally and vertically at any time based on your business demands to prevent resource waste. Create or release cloud hosts in minutes; upgrade or downgrade host CPU and memory within 5 minutes; upgrade or downgrade public network bandwidth online, and easily copy host data and environment with custom image. It also provides open APIs to meet the needs of batch management and automated management.

- Stable and Reliable

SCloud promises 99.95% service availability and data durability no less than 99.9999%. The local disk of the cloud host uses RAID for data protection to prevent data loss. UHost has industry-leading kernel optimization and hot patch technology, non-stop online migration technology, and also provide data snapshots capability.

- High Performance

The performance indicators of the host CPU and memory are industry-leading, and the unique storage technology increases the random read and write I/O capacity of the disk by 10 times that of the Standard SAS disk. There is also an SSD disk cloud host that provides ultra-high IOPS performance. Excellent network processing capabilities to meet various business application requirements.

UHost can support up to Intel Cascadelake CPU, self-developed network enhancement 2.0 technology, disk Binlog technology, full NVMe disk RSSD cloud disk, etc., can achieve up to 1.2 million IOPS IO performance and 10 million PPS network performance

- Secure

UHost has 100% complete network isolation between users to ensure data security. We also provide network firewall function to strictly control access to the public network. In conjunction with the VPC function, a private subnet under a single account can be established to support your internal security management needs. And provide a wealth of monitoring and security tools.

- Data Center

SCloud is located in global Tier-3 data centers, relying on high-quality hardware resources and infrastructure to provide users with high-quality BGP, and international bandwidth resources. According to the needs of the business, you can select a matching data center for the target user area that needs to be covered.

- Wide Coverage with 31 availability zones Globally

You can launch cloud host products in more than 31 availability zones around the world to cover all five continents, and provide high-quality cloud computing services with consistent experience for your global business