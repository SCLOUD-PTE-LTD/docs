---
layout: default
title: UKafka
parent: Big Data
grand_parent: Public Cloud
permalink: /public-cloud/big-data/ukafka/
nav_order: 4
---
# UKafka - Kafka message queue
## Product Introduction
### About UKafka
UKafka is a distributed messaging product in the SCloud platform dedicated to processing streaming data. By creating UKafka by creating a cluster, Kafka and the services it depends on can be quickly deployed, providing users with a fast deployment, easy-to-manage, and elastically scalable streaming data processing system.

**Rapid deployment** is created in minutes, and node allocation, service deployment, and performance optimization are fully automated

Easy to manage 
Combined hardware and software deployment without worrying about the maintenance of the entire service

High performance 

Inherits the performance advantages of cloud hosts and optimizes each configuration of Kafka services to improve the overall performance of the service, which is much higher than that of self-built clusters

Auto scaling 

You can adjust the configuration of the entire cluster by adjusting the node configuration according to performance and capacity, supporting dynamic scaling and effectively avoiding waste of resources

Security and stability 

The cluster mode is deployed on multiple independent physical servers, ensuring no resource preemption and high service availability

Fully compatible 

Fully compatible with the Kafka version of the open source community, users can directly use standard APIs for development without changing the code

### UKafka's service architecture
The UKafka service master mainly contains the following components:
- Message Subject (Topic)
- Message Producer
- Message Consumer
- Message broker

Its structure is shown in the figure:

| Term | Meaning | Illustrate |
| --- | --- | --- |
| Topic | Message subject | That is, a specific message flow or class message fleet. Where the message is a valid abridge, and the subject is the genus of the message or the source of the message |
| Producer | Message Producer | That is, any service or system capable of issuing any message |
| Consumer | Message consumers | A message receiver or handler who subscribes to a single or multiple messages and obtains the published message data from the broker |
| Broker | Message node point | A group of servers that can save messages is the service node in the UKafka cluster |
| Partition | Message partitioning | That is, the topic partition, which can distribute a message subject among multiple brokers to achieve distributed and high availability of services |

### UKafka usage scenarios

As a distributed message processing system, UKafka is mainly responsible for processing all action flow data related to message publishing and subscribing in a high-throughput environment.

Basic monitoring: Use the basic resources as the message publisher, send monitoring data indicators related to the system and applications, and then collect and process these data and collect them through self-built systems to monitor the basic resources.

Message push: As a messaging system for messages and signaling between applications, it performs unified message publishing and subscription management to provide search management or content management.

Real-time data analysis: UKafka can provide synchronous analysis of data to analyze user behavior in real time and improve user experience by tracking user website whereabouts. And the ability to generate data storage into a UHadoop cluster for asynchronous processing of data.

Log collection: It can be used as a log system for distributed applications or platforms to provide core data for analytical databases for unified collection and processing of service logs

### Create a cluster

Click "All Products" in the upper-left corner of the console and select "Kafka Message Queuing UKafka", or you can lock it to the left menu bar.

Enter the UKafka operation interface and click "Create Cluster" to create a UKafka cluster.

Select the Kafka version, service requirements, and node model and number, with a minimum of 3 nodes. After you select it, go to the Cluster Settings page to configure cluster parameters.

For more information about the models that can be selected for clustering, see Model List

The configurable parameters of the cluster are shown in the following table (the cluster has a default configuration, please change the following configuration after confirming the actual requirements):

| Parameter name | The parameter type | Parameter range | Paraphrase |
| --- | --- | --- | --- |
| auto.create.topics.enable | bool | true/false | Whether to allow automatic creation of topics |
| auto.leader.rebalance.enable | bool | true/false | Whether to enable data balancing between nodes |
| default.replication.factor | int | 1-10 | The number of replication factors by default |
| delete.topic.enable | bool | true/false | Whether to allow deletion of topic information |
| log.cleaner.delete.retention.ms | int | 1-2147483646 | Compress logs for the longest retention time |
| log.cleaner.enable | bool | true/false | Whether to enable compressed log cleanup |
| log.cleanup.policy | string | delete/compact | Log cleanup policy, pruning/compressing |
| log.retention.hours | int | 1-10000 | Minimum log retention |
| log.segment.bytes | int | 1-2147483646 | The size of each segment file |
| message.max.bytes | int | 1-2147483646 | The maximum size of the message body |
| num.io.threads | int | 1-32 | The number of threads of the processing disk IO |
| num.network.threads | int | 1-32 | The number of threads requested by the processing network |
| num.partitions | int | 1-1000 | Number of partitions |
| replica.fetch.max.bytes | int | 1-2147483646 | The maximum size of the message when the replica is synchronized |
| zookeeper.session.timeout.ms | int | 1-2147483646 | zk session timeout time |
| zookeeper.connection.timeout.ms | int | 1-2147483646 | The customer terminal is connected to the ZK timeout timeout |
| zookeeper.sync.time.ms | int | 1-2147483646 | ZK information synchronization interval |

#### Notes:

`replica.fetch.max.bytes` must be greater than or equal to `message.max.bytes`;
`num.io.threads` and `num.network.threads` control node load balancing and cluster performance.

#### Adjust cluster parameters

After the cluster is created, you can also adjust the configuration parameters of the Kafka service. The parameters are detailed in the preceding table.