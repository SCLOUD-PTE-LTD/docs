---
layout: default
title: UDisk
parent: Storage
grand_parent: Public Cloud
permalink: /public-cloud/storage/udisk/
nav_order: 4
---
# Block storage (UDisk)
## Product Overview
Cloud Disk (UDisk), as a foundational block storage product in cloud computing scenarios, provides persistent block device storage for cloud hosts. It has an independent lifecycle, supports distributed access over the network, and offers cloud hosts a storage solution that is high-capacity, highly reliable, scalable, easy to use, and cost-effective.

UDisk is available in three types: Basic Cloud Disk, SSD Cloud Disk, and RSSD Cloud Disk. The Basic Cloud Disk uses SATA media, while the SSD and RSSD Cloud Disks use SSD media.

UDisk is a flexible and advanced cloud disk device offered by SCloud, capable of being created and managed easily. It uses a multi-copy, cross-rack physical machine backup mechanism within an availability zone. The Basic Cloud Disk guarantees `99.99999%` data durability, while SSD and RSSD Cloud Disks guarantee `99.999999%` durability.
To further enhance data durability, enabling the snapshot service (regular snapshot creation + UDataArk protection) is an economical and convenient way to ensure long-term data persistence.

UDisk is both powerful and easy to use. Users can attach a created cloud disk to any cloud host, and expand disk capacity when space is insufficient. The snapshot and cloning features also make it easier to back up, restore, and replicate data, improving data availability.
The maximum capacity currently supported:
- Basic Cloud Disk: 8,000 GB
- SSD Cloud Disk: 8,000 GB
- RSSD Cloud Disk: 32,000 GB

This large capacity support meets user needs without requiring them to manually configure LVM.

>Note: For security reasons, each UDisk cloud disk can currently only be mounted to one cloud host at a time.

## Key concepts
#### Disk Name
The custom name assigned by the user to the cloud disk.
#### Disk Capacity
The size of the cloud disk.
#### Mount Point
The location where the cloud disk is mounted on the cloud host.
#### Resource ID
After a cloud disk is created, the system automatically generates a globally unique Resource ID.
#### Expansion
When the capacity of a cloud disk is no longer sufficient for business needs, it can be expanded or upgraded.
#### Mount and Unmount
The actions of attaching a cloud disk to a cloud host and detaching it from the host.
#### Snapshot
A snapshot is an effective disk management feature to prevent data loss and ensure data integrity.
Snapshots can be created in seconds and capture the state of the disk at a specific point in time, allowing users to restore the disk from a snapshot when needed.
#### Dismount (Windows)
The dismount operation for a cloud disk in Windows environments.

## Product advantages
### Reliability
UDisk cloud disks use a multi-replica backup mechanism across physical machines in different racks within the same availability zone, with real-time synchronization. This ensures fault tolerance and prevents service interruption due to single-machine failures.
- Basic Cloud Disks guarantee 99.99999% data durability
- SSD and RSSD Cloud Disks guarantee 99.999999% data durability

### Capacity and Scalability
UDisk allows flexible configuration of storage capacity and can be expanded at any time.
- RSSD Cloud Disks currently support up to 32,000 GB per disk.
- A single cloud host can attach multiple cloud disks, enabling virtually unlimited storage expansion for the host.

### Ease of Use
UDisk supports fast operations such as create, attach, detach, delete, and resize, making it easy to deploy and manage without requiring server reboots.
### Backup and Recovery
UDisk supports two types of backup: manual snapshots and UDataArk.
- Manual Snapshots: Enable data disk backups, allowing you to restore or create new UDisks from snapshots.
- UDataArk: In addition to manual snapshots, it supports real-time backup of cloud disks and allows recovery or creation of new UHost instances from any point-in-time backup.

Enabling UDataArk or scheduling regular snapshots is a cost-effective and convenient way to enhance long-term data durability. In the event of a disk failure, data can be quickly restored or migrated using the latest snapshot or UDataArk backup.

> Notes:

- Enabling snapshot service gives access to manual snapshots and includes free UDataArk.
- UDataArk supports real-time backup.
- RSSD snapshot service is currently available in select data centers: Hong Kong B and Washington.

## Usage Restrictions
### Supported Regions

| Region                  | RSSD Cloud Disk | SSD Cloud Disk | Basic Cloud Disk |
|-------------------------|------------------|----------------|------------------|
| Hong Kong Zone A        | Not Supported    | Supported      | Supported        |
| Hong Kong Zone B        | Supported        | Supported      | Supported        |
| Washington Zone A       | Not Supported    | Supported      | Supported        |
| Singapore Zone A        | Not Supported    | Supported      | Supported        |
| Singapore Zone B        | Supported        | Supported      | Not Supported    |
| Taipei Zone A           | Supported        | Supported      | Supported        |
| Jakarta Zone A          | Supported        | Supported      | Supported        |
| Seoul Zone A            | Supported        | Supported      | Supported        |
| Los Angeles Zone A      | Supported        | Supported      | Supported        |
| Ho Chi Minh City Zone A | Supported        | Supported      | Supported        |
| Tokyo Zone A            | Supported        | Supported      | Not Supported    |
| Frankfurt Zone A        | Not Supported    | Supported      | Not Supported    |
| Bangkok Zone B          | Supported        | Supported      | Not Supported    |
| Mumbai Zone A           | Not Supported    | Supported      | Not Supported    |
| Lagos Zone A            | Supported        | Supported      | Not Supported    |
| Manila Zone A           | Supported        | Not Supported  | Not Supported    |
| London Zone A           | Supported        | Supported      | Not Supported    |

> Note:
> - SSD Cloud Disk supports up to 8 TB
> - Basic Cloud Disk supports up to 8 TB
> - RSSD Cloud Disk supports up to 32 TB

### Quota Per Account

- Maximum of 1,000 cloud disks

### Cloud Disk Limit Per Host

- A single cloud host can attach up to 26 cloud disks

## Usage scenarios
RSSD Cloud Disks are suitable for:
- High-performance databases
- I/O-intensive applications that require low latency, such as Elasticsearch search engines

SSD Cloud Disks are suitable for:
- I/O-intensive applications
- Medium to large-scale relational databases
- NoSQL databases

Basic Cloud Disks are suitable for:
- Backup and log storage scenarios
- Large file sequential read/write workloads with high capacity requirements (e.g., Hadoop offline data analysis)
- Small relational databases and development/test environments that require a certain level of data reliability

## Performance
### Performance Metrics

There are three key metrics for evaluating cloud disk performance:

- IOPS: Input/Output operations per second.
- Throughput: The amount of IO data read/written per second.
- IO Latency: The time from IO submission to completion.

In general, higher IOPS and throughput are better, and lower latency is better.

#### IOPS

IOPS (Input/Output Operations Per Second) is a metric used to measure the performance of storage devices such as HDDs, SSDs, or SANs. It indicates how many read/write operations are performed per second.

There are four main types of IOPS based on test scenarios:

| IOPS Type     | Description                        |
|---------------|------------------------------------|
| Random Read   | Average number of random reads per second |
| Random Write  | Average number of random writes per second |
| Sequential Read | Average number of sequential reads per second |
| Sequential Write| Average number of sequential writes per second |

#### Throughput

Throughput refers to the average amount of data successfully transferred by the disk per unit time. It is typically measured in MB/s (megabytes per second).

### IO Latency

IO latency refers to the time taken from when an IO request is sent to when it is completed.

### Performance Comparison

UDisk offers three types of cloud disks: RSSD, SSD, and Basic Disk.

- RSSD Cloud Disk: Uses NVMe SSD and RDMA network transport
- SSD Cloud Disk: Uses NVMe-based solid-state drives
- Basic Cloud Disk: Uses traditional mechanical HDDs

| Metric         | RSSD Cloud Disk                           | SSD Cloud Disk                          | Basic Cloud Disk       |
|----------------|--------------------------------------------|------------------------------------------|-------------------------|
| IOPS per disk  | `min{1800 + 50 * capacity, 1,200,000}`     | `min{1200 + 30 * capacity, 24,000}`      | 1000 (peak)             |
| Throughput     | `min{120 + 0.5 * capacity, 4800}` MB/s     | `min{80 + 0.5 * capacity, 260}` MB/s     | 100 MB/s (max)          |
| Avg Latency    | 0.1 – 0.2 ms                               | 0.5 – 3 ms                               | 10 ms                   |

#### Relationship Between RSSD Performance and Instance Performance

The IO performance of a virtual machine (instance) increases linearly with the number of vCPUs. More CPU cores → higher IOPS and throughput.

- If the RSSD disk’s performance is less than or equal to the instance’s IO capability, actual performance is based on the RSSD disk’s limits.
- If the RSSD disk’s performance exceeds the instance’s IO capability, actual performance is limited by the instance’s max IO limits.
- If the number of CPU cores is not listed in the table below, performance is based on the maximum matching lower vCPU value.  
  For example, an instance with 50 cores will match the performance of a 48-core instance.

| vCPU Cores | Max Storage IOPS | Max Storage Throughput (MB/s) |
|------------|-------------------------|-------------------------------|
| 1          | 18.000                     | 75                            |
| 2          | 38.000                     | 150                           |
| 4          | 75.000                     | 300                           |
| 8          | 150.000                      | 600                           |
| 12         | 225.000                    | 900                           |
| 16         | 300.000                      | 1200                          |
| 24         | 450.000                      | 1800                          |
| 32         | 600.000                      | 2400                          |
| 48         | 900.000                      | 3600                          |
| 64         | 1.200.000                     | 4800                          |