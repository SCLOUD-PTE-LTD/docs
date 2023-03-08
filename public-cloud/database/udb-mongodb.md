---
layout: default
title: MongoDB
parent: Database
grand_parent: Public Cloud
permalink: /public-cloud/database/mongodb/
nav_order: 2
---
# MongoDB UDB
## Introduction
### What is cloud database MongoDB
cloud database for MongoDB is a highly available and high-performance database service based on mature cloud computing technology. It is fully compatible with the MongoDB protocol and supports flexible deployment. In addition to the replica set instance architecture, cloud database for MongoDB also provides a fragmented cluster architecture to meet massive data business scenarios ; At the same time provide a complete set of solutions for disaster recovery, backup, monitoring and alarming.
## Key concepts
### Instance type
MongoDB instance types include: data node, routing node, and configuration node; Data nodes can build replica sets, and sharded clusters can be built by configuring nodes, routing nodes, and replica sets.

### Node
MongoDB is divided into Primary (Shardsvr), Secondary, Arbiter, Configsvr, Mongos and other node types.
Primary: is the primary node of the replica set, and the default replica set is a three-node replica set architecture, which supports changing the configuration of replica set nodes and increasing or decreasing the number of nodes in the replica set.
Secondary node: As a replica set slave node, it can provide read services, and adding a secondary node can improve the read service capability and availability of the replica set.
Arbiter node: is the quorum node of the replica set, does not store data, and is only responsible for voting during failover.
Mongos nodes: As a service proxy, a single cluster instance can support multiple Mongos nodes.
Configsvr node: The configuration node that is required for the cluster.
### Database model
MongoDB instances are available with standard models and SSDs
### Version
MongoDB currently supports MongoDB 2.4, MongoDB 2.6, MongoDB 3.0, MongoDB 3.2, MongoDB 3.4, MongoDB 3.6, and MongoDB 4.0, and users can choose the corresponding cloud database version according to their needs.

### Replica set
By default, a replica set of three nodes is built with one click: Primary node + one secondary node + one Arbiter node. Creating nodes in a replica set supports scaling the replica set of more nodes (for example, five-node, seven-node, or more nodes): It is suitable for business scenarios that have higher read performance requirements for the database, such as read-more-write-less scenarios or bursty business requirements such as active promotions.

### Sharded clusters
The console supports the construction of a sharded cluster, which consists of three copies of Configsvr + N Mongos + data shards (three-node replica set: Primary node + one secondary node + one Arbiter node), and the number of nodes and shards can be configured according to the business data situation. By default, the sharded cluster of the three copies of Configsvr supports versions: MongoDB 3.4, MongoDB 3.6, and MongoDB 4.0. MongoDB 3.2 and below can create self-built versions by configuring nodes and routing nodes

### Memory
The memory size of the cloud database. Users can choose based on their hardware requirements for the cloud database.
### Hard disk
The size of the hard disk of the cloud database for the ECB. Users can choose based on their hardware requirements for the cloud database.
### Payment method
Payment methods are divided into three methods: annual, monthly, and on-demand, and the payment methods are prepaid, that is, the corresponding service cycle is paid in advance.
For detailed billing instructions, please refer to the "Purchase and Billing" document.
### Quantity
By default, the number of CVDBS that you need to apply for is `1`, and you can select multiple databases to create them in batches.
### Configuration file
Configuration files include various configuration parameters for the operation of cloud database for ECSD, which can be customized and modified as needed, and default profiles are provided for different cloud database versions.
Profiles include default profiles and custom profiles, which are created and imported by users.
### Administrator
By default, the super administrator (`root`) privilege is provided, allowing users to customize the administrator password.
### Instance name
You can customize the name of the cloud database instance.
### Resource ID
After you create an cloud database instance, the system automatically generates a resource ID, which is globally unique.
### IP and port
The IP address is the private network address of the user accessing the CVD, which is automatically generated after the CVD is created, and the external IP address is not currently provided.
The default port for MongoDB is `27017`.
### Backup
The backup stores all the data of the cloud database at a certain point in time.  cloud database provides automatic backup and manual backup to prevent data loss and avoid risks caused by mis-operation.
### Log
A log is a log file used to record the operation events of a cloud database. Includes binary logs, slow query logs, error logs, operation logs.
## Product advantages
### Excellent performance
In view of the high-performance requirements of the database, the high-end high-performance hardware configuration is adopted, and the database performance parameters are specially optimized.
### Flexible backups
Automatic backup and manual backup are provided to prevent data loss and accidental deletion and ensure the security and reliability of user data.
### Data security
Ensure data security in multiple aspects, including data access, network isolation, and data disaster recovery.
### Easy to manage
Simply select the appropriate instance according to the required data space and performance requirements, and you can use it directly in a few minutes.
### Strong compatibility
Seamlessly compatible with MongoDB, you can migrate applications to the cloud without any changes.
### Elastic scaling
Real-time instance upgrades are performed based on the actual load of the database to obtain larger database space and stronger database performance.
## Specification version
### Product version
MongoDB replica sets and sharded clusters support MongoDB 2.4, MongoDB 2.6, MongoDB 3.0, MongoDB 3.2, MongoDB 3.4, MongoDB 3.6, and MongoDB 4.0.

Users can select the appropriate cloud database version according to their needs.
### Introduction of new features in each version
- MongoDB version 3.4:

When you initialize synchronization, all indexes are created synchronously.
Supports configuring the time when Primary chases data.
A large number of aggregation operators have been added, which is more powerful, such as $graphLookup, which can support more complex relational operations.

- MongoDB version 3.6:
  
Version 3.6 updates the authentication mechanism `MONOGDB-CR` to `SCRAM` for enhanced security.

Added more aggregate operators, such as: `$arrayToObject`, `$objectToArray`, `$mergeObjects`, etc.

With the addition of the `replSetResizeOplog` command, the WiredTiger storage engine can dynamically resize the oplog.

- MongoDB version 4.0:

Introduce multi-document transactions to support multi-document transactions across one or more collections within a replica set, ensuring the atomicity of updates for multiple documents.

The data node of the wt engine is not allowed to set `storge.journal.endbled` to `false`.
The read snapshot solves the problem that the application of oplog from the library and global lock causes the service read and application oplog to block each other.