---
layout: default
title: UHost Cloud Compute Instance
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

1. Scenario: Provide the most flexible and free combination of CPU, memory, and disk. Suitable for balanced scenarios such as computing, storage, and networking.
2. CPU platform support: IvyBridge/Haswell/Broadwell/Skylake
3. CPU memory combination (support ratio 1:1-1:8):

| CPU | RAM |
| --- | --- |
| 1-core | 1G, 2G, 4G, 8G |
| 2-core | 2G, 4G, 6G, 8G, 12G, 16G |
| 4-core | 4G, 6G, 8G, 12G, 16G, 32G |
| 8-core | 8G, 12G, 16G, 24G, 32G, 48G, 64G |
| 16-core | 16G, 24G, 32G, 48G, 64G, 128G |
| 24-core | 24G, 32G, 64G, 96G, 192G |
| 32-core | 32G, 64G, 96G, 128G |

4. Disk type support: support cloud disk, Standard local disk, SSD local disk

Specific selection range:

| System Disk | Data Disk |
| --- | --- |
| SSD cloud disk (20-500GB) | SSD cloud disk (20-4000GB),Standard cloud disk (20-8000GB) |
| Standard local disk (20-100GB) | Standard local disk (20-2000GB) |
| SSD local disk (20-100GB) | SSD local disk (20-1000GB) |

5. Feature support: network enhancement 1.0/network enhancement 2.0 (only Skylake and above support) and Hot Scale-Up