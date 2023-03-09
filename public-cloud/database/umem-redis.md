---
layout: default
title: Redis
parent: Database
grand_parent: Public Cloud
permalink: /public-cloud/database/redis/
nav_order: 5
---
# UMem Redis
## Introduction
### What is Cloud Memory Redis
Cloud Memory Redis is a Key-Value type online storage service compatible with the open-source Redis protocol. It supports a variety of data types such as String, Linked List, Set, SortedSet, Hash Table, and advanced functions such as Transactions, message subscription and publishing (Pub/Sub).

SCloud Cloud Memory Redis provides two architectures, the primary and standby Redis and the distributed Redis, based on the highly reliable dual-machine hot standby architecture and the smoothly scalable cluster architecture to meet the business needs of high read and write performance scenarios and elastic scaling.

With its rich data structure and functions, excellent performance of a single core and perfect software ecosystem, Redis has gradually become a mainstream solution for in-memory storage in Internet applications in recent years. In response to the pain points of native Redis Cluster with high requirements for clients and inconvenient expansion, SCloud's distributed Redis product has developed a distributed product based on proxy implementation, and has been deeply cultivating the product for many years, pursuing the ultimate in higher performance, larger capacity, data security, etc., and constantly providing customers with the ultimate distributed cache service.

## Key concepts

SCloud cloud memory Redis products mainly use the following terms, in order to facilitate and quickly use, do the following analysis:

| Term | Definement/parsing |
| --- | --- |
|Example name|The name of the user-defined cloud memory Redis instance. |
|Protocol|The cloud memory Redis example supports the Redis protocol. |
|Models|The cloud memory Redis example is the primary and standby version and the distributed version according to the model. |
|Example of the main and backup editions|Refers to an example of Redis with a primary-standby architecture.  The double-node primary and standby versions support capacity expansion, and the expanded capacity and performance are limited. |
|Example of distributed version|Refers to an instance of Redis with a scalable sharded cluster. Distributed cluster examples have better scalability and performance, but also have certain limitations in functionality. |
|Resource ID|After the user creates a cloud memory Redis instance, the system will automatically generate the resource ID and capital The source ID is globally unique. |
|IP and port|The IP address is the private network address of the user accessing the cloud memory Redis instance, and it automatically starts after the cloud memory Redis instance is successfully created Generate. The default port is `6379`. |
|Expand|If the configuration of the cloud memory  Redis instance cannot support the  service, you can expand the cloud memory Redis instance Upgrade. |
|Password|The main and backup versions support password access authentication to further ensure the security of real examples. |
|Configure step lifting|The primary and standby editions support self-expansion and scale-down|
|From the library|The master and  backup versions support the creation of read-only examples, and a master-standby Redis example supports up to five read-only slaves The library has two nodes of one master and one backup to ensure that only read from the library is highly available|
|High availability across zones|Redis for  active and standby supports high-availability editions across zones, and one primary and one standby are deployed in different zones in the same region to achieve disaster recovery at the zone level|
|Backup Zone|The cross-zone deployment feature is enabled for the zones deployed in the secondary database of the primary and standby Redis editions, and users can choose to deploy the backup in other zones in the same region|
|Attribute|Divided into master, slave;  The attribute of the master-standby redis example is master, and the slave database of the active-standby redis example is slave|
|Capacity|The size of the user's capacity to create a Redis instance|

## Product advantages
### High availability
The default setting is highly available to avoid the impact of downtime and other failures on cloud memory services. When the master node fails, the slave node is quickly promoted to the new master node and continues to provide services.
Active and standby Redis provides the cross-zone high availability feature, and users can enable the "cross-zone high availability" feature according to business needs, thereby improving service availability and achieving cross-data center level disaster recovery. However, due to Redis' asynchronous replication technology, when the business pressure is high, a small amount of data of the slave node may lag behind its master node, and a small amount of data inconsistency may occur when the master-slave node is switched.
### Rich architecture
Redis provides two architectures: primary, standby and distributed.

- Primary/standby architecture: When the system is running, the standby node (Replica) synchronizes the master node data in real time, and the system automatically switches over the system in seconds when the primary node fails, and the standby node takes over the business, which is automatic and has no impact on the business, and the primary/standby architecture ensures high availability of services.

- Distributed architecture: Distributed instances adopt a distributed cluster architecture, and each node adopts a highly available primary/standby architecture of one master and one slave, which can perform automatic disaster recovery switchover and failover migration. Different capacity configurations of the distributed version can be applied to services under different pressures, and the performance of distributed Redis can be extended as needed.
### Full monitoring
Provide users with multiple types of monitoring, including usage, number of connections, QPS, number of keys, etc.
### Online scaling
Users can directly visualize the expansion of Redis in the console, and the whole process is transparent to users and effectively meets the needs of business growth. The primary and standby Redis versions support capacity expansion. Different capacity configurations of the distributed version can be applied to services under different pressures, and the performance of distributed Redis can be extended as needed.
### Low O&M costs
Effectively reduce O&M costs, users can create instances required for upgrade according to business needs, eliminating the need to purchase high-cost hardware in the early stage of business, effectively reducing initial capital investment and idle waste of resources. Convenient and fast deployment management can reduce the O&M cost of deployment and maintenance.
### Data security
Data persistent storage adopts the hybrid storage mode of memory + hard disk to meet the data persistence requirements while providing high-speed data reading and writing capabilities.
Backup and recovery, automatic data backup every day, support manual backup, strong data disaster recovery ability, free saving of 3 copies, support one-click backup recovery and backup download, effectively prevent data mis-operation, and minimize possible business losses.
Internal network isolation security protection, the main and standby versions support setting password access authentication to ensure secure and reliable access.
## Product architecture
### Primary and standby Redis architecture
The master-replica architecture is used in which the primary node provides daily service access and the standby node ensures high availability.
#### The service is highly available
The dual-server primary/standby architecture is adopted, and the primary node provides external access, and users can add, delete, modify, and check data through the Redis command line and the general client. When the primary node fails, the primary and standby switchover is automatically performed to ensure smooth service operation.
#### High data reliability
By default, the data persistence feature is enabled, and all data is stored. The data backup function is supported, and users can restore from the backup creation instance, effectively solving problems such as data mis-operation. It also supports the deployment of primary/standby nodes across zones to achieve cross-zone disaster recovery.
#### Compatibility
The primary and standby versions of Redis support Redis version 4.0/5.0 and are compatible with Redis protocol commands. Self-built Redis can be smoothly migrated to the primary and standby Redis versions.
### Distributed Redis architecture
The distributed Redis adopts Redis sharding + Proxy architecture, and Redis sharding is based on the resource pool of the primary and standby Redis versions, which easily breaks through the single-thread bottleneck of Redis itself and supports online expansion, which can greatly meet the business needs of Redis for large capacity or high performance.
#### Distributed Redis architecture - Redis sharding
By default, the distributed version of Redis provides an access IP, and users can access the IP address for normal Redis access and data operations.
Redis sharding: Each sharded server is an active and standby Redis high-availability architecture, and after a primary node fails, the system automatically switches between the primary and standby servers to ensure high service availability. 

The minimum capacity of a single shard is `4G`, and a default alarm is set for the memory usage of a single shard, and an alarm is triggered when the memory usage of a shard reaches the alarm threshold. In the actual production environment, if a shard has a high load or high memory usage, you can scale out the shard separately. In addition, it is recommended that you try to balance the load of each shard as much as possible to avoid waste of resources caused by one shard being very idle and serious load on another shard.
#### Distributed Redis architecture – proxy
Proxy Agent: In a dual-master configuration, there will be multiple Proxy components in the distributed architecture, and the system will automatically load balance and fail over.
### Distributed Redis Cluster architecture
The distributed Redis cluster adopts a multi-Redis node architecture, and a single node is based on the primary and standby Redis resource pools, and the standby node maintains high availability, easily breaking through the single-thread bottleneck of Redis itself, and supporting online expansion, which can greatly meet the business needs of Redis for large-capacity or high-performance.
Distributed high-performance udredis versus cluster mode
#### Same point
In terms of operation, except for the agent, the two operations are the same, and you can expand or shrink the shard capacity on the console shard management, or you can split the node. Both nodes provide primary and standby nodes to ensure high availability of nodes.
#### Differences
Cluster supports all Redis native commands (except disabled commands), but does not support inter-node command operations such as `mget`, `keys`, etc. In terms of performance, cluster command requests are sent directly to the backend redis, and udredis needs to be forwarded to the backend redis through the proxy, and the performance of cluster will be better than `udredis`.
Version upgrade
In April 2022, the cluster version was adjusted and upgraded.
1. All nodes are not created at the beginning of `udrediscluster-` but synchronized to the beginning of `udredis-` .
2. Route multiplexing native `clster`, `0-16383`, can currently distinguish the backend as a cluster cluster or a proxy shard through routing.
3. When all clusters are created, proxies will be created simultaneously, and customers can use the backend cluster directly or use the cluster through proxy connection.
4. If the instance of the cluster cluster (`udrediscluster` - beginning) created earlier needs a proxy, we can add a proxy to the backend non-standard.

### Performance-enhanced primary-standby Redis
The performance-enhancing active-standby Redis improves the performance of Redis version 6.0 and adjusts the number of threads according to the capacity to provide high performance.
#### Capacity
The performance-enhanced primary/standby Redis supports `4, 6, 8, 12, 16, 24, 32, 40, 52, and 64` capacity specifications.
#### The service is highly available
The dual-server primary/standby architecture is adopted, and the primary node provides external access, and users can add, delete, modify, and check data through the Redis command line and the general client. When the primary node fails, the primary and standby switchover is automatically performed to ensure smooth service operation.
#### High data reliability
By default, the data persistence feature is enabled, and all data is stored. Support data backup function, users can restore from backup creation instances, effectively solve data mis-operation and other problems.
#### Compatibility
Performance enhancement supports version 6.0 and is compatible with Redis protocol commands.
#### Capacity memory and io_thread relationship

| Capacity | `io_thread` |
| --- | --- |
| 4G, 6G, 8G, 12G, 16G, 24G, 32G | 4 |
| 40G, 52G | 6 |
| 64G | 8 |

#### Notes:
Performance-enhanced instances do not support creating slave libraries.
It is only supported by outstanding computer room.