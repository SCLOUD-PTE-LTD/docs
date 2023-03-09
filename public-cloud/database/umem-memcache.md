---
layout: default
title: Memcache
parent: Database
grand_parent: Public Cloud
permalink: /public-cloud/database/memcache/
nav_order: 4
---
# Memcache UMem
## Product Introduction
Cloud Memory Memcache is an memory-based cache service that supports high-speed access to massive and small data. Cloud memory Memcache can greatly relieve the pressure on back-end storage and improve the response speed of websites or applications. Cloud memory Memcache supports Key-Value data structure, and clients compatible with the Memcached protocol can communicate with cloud memory Memcache.

The same as the local self-built MemCache is that the cloud memory Memcache is also compatible with the Memcached protocol, compatible with the user environment, and can be used directly. The difference is that the hardware and data are deployed in the cloud, with complete infrastructure, network security, and system maintenance services. All of these services require no investment and only pay for what you use.

### Product features
1. Hotspot data access
Cache hot data can be used to greatly improve the response speed of applications and greatly relieve the pressure on back-end storage.
2. Compatible with Memcached protocol
The data structure that supports Key-Value and is compatible with the Memcached protocol can use cloud memory Memcache.
3. Real-time monitoring
Provide real-time monitoring and historical monitoring of multiple statistics.
4. Online capacity adjustment
Support online capacity adjustment, lifting configuration is completed instantly.
### Product advantages
1. Superior performance
Cache data is stored in memory, and data access returns quickly.
2. Reliable service
When a server is down, the system can quickly restore the service, and the user's current client can automatically reconnect to the service.
3. Security guarantee
Cloud memory Memcache only supports intranet access and is completely isolated from the external network.
4. Auto scaling

When the service scale changes, you can modify the configuration of the instance at any time as needed.
### Stand-alone memcache features
The stand-alone version of memcache supports all Memcache protocols and stats commands; Supports reboot and LRU retirement strategies; Low latency per request; High availability is supported, but no data is available after disaster recovery switchover. There are clear limits on the size of key and value, which are `250 bytes` and `1M` respectively; Single-node service QPS generally does not exceed `10W`; The maximum capacity supports `32G`, and when the capacity is expanded, Memcache will be restarted and the data will be cleared.

## Explanation of terms
### Instance name
The name of the user-defined cloud memory Memcache instance.
### Protocol
Cloud memory Memcache instances support the Memcache protocol.
### Resource ID
After you create a cloud memory Memcache instance, the system automatically generates a resource ID, which is globally unique.
### IP and port
The IP address is the private network address of the cloud memory storage instance, which is automatically generated after the cloud memory memcache instance is created.

The default port is `11211`.
### Expansion
If the configuration of the cloud memory Memcache instance cannot support services, you can expand and upgrade the cloud memory Memcache instance.

## Product advantages
### Excellent performance
Notable features with high concurrency and high performance read and write.
### Quick Create
Select the required memory capacity according to your business needs, create it with one click, and use it immediately.
### The service is highly available
Supports high availability and automatic failover without being affected by a single point of failure, but no data after switchover.
### Strong compatibility
Seamlessly compatible with the Memcache protocol, the original service can be migrated directly without any modification.
### One-click capacity expansion
Depending on the actual load and memory usage, you can easily expand the capacity on the console with one click to obtain higher storage space and performance. The maximum capacity supports `32G`, and when the capacity is expanded, Memcache will be restarted and the data will be cleared.
## Application scenarios
### Frequently accessed services
Such as social networks, e-commerce, games, advertising, etc., can store very frequently accessed data in the cloud memory Memcache
### Large-scale promotional business
Large-scale promotional flash sale system, the overall access pressure of the system is very large, the general database can not bear such access pressure at all, you can choose cloud memory Memcache storage.
### Inventory system with counter
Cloud memory Memcache is used with cloud database for UDB, UDB stores specific data information, database fields store specific count information, cloud memory Memcache reads the count, and UDB stores count information.
### Select Recommendations
- Stand-alone Memcache is suitable for cache-only business scenarios and can accept data emptying.
- Stand-alone Memcache is suitable for scenarios where QPS is less than 10w.
- Stand-alone Memcache is suitable for latency-sensitive scenarios.
  
*If the service has been launched stably and self-built Memcache is used, it is recommended to use a stand-alone Memcache.*
