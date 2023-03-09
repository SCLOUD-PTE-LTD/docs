---
layout: default
title: UES
parent: Big Data
grand_parent: Public Cloud
permalink: /public-cloud/big-data/ues/
nav_order: 3
---

# UES - ElasticSearch
## Product Introduction
Built on the open source Elasticsearch, UES is a distributed, multi-user full-text search engine service. UES provides services by creating clusters, which can quickly implement cluster deployment and provide customers with the ability to retrieve and analyze massive stored data in real time.
### Product characteristics
#### Massive data storage, retrieval and analysis
Near real-time processing, full-text search, structured retrieval, and data analysis of massive data.
#### Rapid deployment and ease of use
Deploy and initialize the service cluster in minutes, and the service has built-in commonly used word segmentation, document parsing, and SQL plug-ins, and supports operations such as linear cluster expansion.
#### Superior performance, safe and reliable
The underlying node uses high-performance SSD disks, and the cluster operation has the most basic and efficient support for data backup, data migration, and fault recovery. The service cluster runs on the private network to ensure the security of data writing and retrieval.
#### Performance monitoring and visual management
The service supports rich performance indicator monitoring and built-in visual management platform Kibana with permission verification to help customers better manage and maintain their service clusters.
#### Rich node configuration and flexible billing
Supports annual, monthly, and hourly billing, and you can select appropriate node configurations and billing cycles based on business needs to effectively control costs.
### Product usage scenarios
#### Log management analysis
Analyze unstructured and semi-structured logs such as URLs, mobile devices, and servers for troubleshooting scenarios, application monitoring, and fraud detection
#### Full-text search
Search and navigation services for e-commerce, O2O, enterprise and other industries
#### Document storage
Friendly support for storage analysis of Json's documents
#### Data storage (database function)
Starting a new project, consider using ES as the only data store to help keep the design as simple as possible. This scenario does not support operations that include frequent updates and transactions.
#### Traditional database data analysis
For a complex system that is running, you need to add a retrieval service to the existing system. A relatively safe way is to add ES as a new component to an existing system.

## Version management
UES provides different product versions, and new versions will be released regularly according to the official website update. Users can choose the version to be used according to actual needs and usage habits, and it is recommended to use the latest version.

The versions that are now available are:
- V5.5.1 * V6.2.1 * V6.5.4 * V7.1.1
  
Version attributes
- V6.0 version highlights
  
### Use serial numbers for faster restarts and restores

The biggest new feature in the 6.0 release is sequence IDs, which allow operation-based shard recovery. Previously, if a node was disconnected from the cluster due to network issues or node reboots, each partition on the node had to be resynchronized by comparing the segment file to the primary shard and replicating any different segments. This can be a long and expensive process and even make the rolling restart of the node very slow. Using sequence IDs, each shard will only replay operations that are missing in that shard, making the recovery process more efficient.

### Use sorted indexes to query faster

With index sorting, the search can be terminated as long as enough hits are collected. It allows for more efficient searches when sorting low-cardinal fields (e.g. age, gender, is_published) that are commonly used as filters because all potentially matching documents are grouped together.

### Sparse area improvements

Previously, a storage space was reserved for each field in each column. If only a few documents have many fields, it can lead to a huge waste of disk space. Now, you pay for what you use. Dense fields will use the same amount of space as before, but sparse fields will be significantly reduced. This not only reduces disk space usage, but also reduces merge time and improves query throughput because file system caching is better utilized.
### Type removal
In version 6.0, an index no longer supports multiple types, type-based parent-child relationships will be implemented through a separate join field, and type will be completely removed in version 7.0.
### Index version inheritance
Version 6.0 will avoid conflicts caused by merging all matching index templates, and only one match will be matched, which will be verified when the index is created.
### Load-based request routing
Previously, search requests were rounded by full nodes, and nodes with slow performance tended to cause overall latency. The 6.0 implementation will automatically adjust the queue length based on the time spent by the queue, and both search and indexing will be based on this mechanism.