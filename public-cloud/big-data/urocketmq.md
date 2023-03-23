---
layout: default
title: URocketMQ
parent: Big Data
grand_parent: Public Cloud
permalink: /public-cloud/big-data/urocketmq/
nav_order: 5
---
# URocketMQ
## Product concept

URocketMQ is a message queue service based on the open-source RocketMQ and compatible with the open-source RocketMQ protocol. Provides one-click instance creation, topic and group management, message query and backtracking, dead-letter queuing and monitoring alarms, supports message types such as normal, sequential, timed, and transaction, and supports broadcast and cluster consumption modes.

The current URocketMQ is based on the open source RocketMQ version `4.7.1`.

## Product architecture
The product architecture is as follows

![1](/docs/assets/images/architecture.jpg)

NameServer: Routing registry, providing broker management and routing information management functions.

Broker: Message processing module, responsible for message storage, delivery, query and other functions, including Broker Master and Broker Slave two roles, a single 

Master node can correspond to multiple Slave nodes.

Producer: Message producer, supports cluster deployment.

Consumer: Message consumer, supports cluster deployment, supports pull and push.

High availability module: SCloud's self-developed Broker availability management module monitors the availability of master-slave broker nodes, and can realize autonomous master-slave switching when the broker master node is abnormal to ensure the availability of the cluster.

## Message type

URocketMQ supports a variety of message types such as normal, partition order, global order, transaction, timing, etc., and the production/consumption code of each type of message may be different, so it is recommended not to mix them.
### Normal messages
Universal no special settings message, highest performance.
### Partition order messages
User-supplied Sharding Key is partitioned, and messages within the same partition are published and consumed in strict FIFO order.
### Global order messages
All messages guarantee FIFO production and consumption, effectively as a queue, with minimal performance.
### Transactional messages
Transactional messages provide distributed transaction functionality similar to X/Open XA, which either succeed or fail at the same time, and the final consistency of distributed transactions can be achieved through transaction messages.
### Timed/delayed messages
While the open-source RocketMQ supports fixed gradient delay messages, URocketMQ supports arbitrary second-level timed/delayed messages.
### Fixed gradient delay messages
Open source RocketMQ supports fixed gradient delay messages, after the message is sent, it will not be consumed immediately, waiting for a specific time to be delivered to the real topic, the default configuration is "1s 5s 10s 30s 1m 2m 3m 4m 5m 6m 7m 8m 9m 10m 20m 30m 1h 2h" 18 levels, each level corresponds to a different delay time, such as 1 corresponds to 1s, 5 corresponds to 1m, as follows:

| Delay level | Delay time |
| --- | --- |
| 1 | 1s |
| 2 | 5s |
| 3 | 10s |
| 4 | 30s |
| 5 | 1m |
| 6 | 2m |
| 7 | 3m |
| 8 | 4m |
| 9 | 5m |
| 10 | 6m |
| 11 | 7m |
| 12 | 8m |
| 13 | 9m |
| 14 | 10m |
| 15 | 20m |
| 16 | 30m |
| 17 | 1h |
| 18 | 2h |

### Delay messages in any second
URocketMQ supports arbitrary second-level delay messages, and users can customize the timing or delay the sending of messages to any second in the future.

Illustrate:

The accuracy of the timing message will have a delay error of 1s~2s.

By default, you can customize second-level delay messages within 2 days.
Set both the fixed gradient delay and the custom delay properties, and only the fixed gradient delay takes effect.

If the delay time exceeds the support range (for example, the default is 2 days), the maximum support time shall prevail.

The actual timestamp is based on the server time, and if the timing timestamp is less than the current server timestamp, it will be sent immediately.

Instances created before November 15, 2021 will be upgraded in batches to support this feature, if the instance is not supported, you can contact your account manager to upgrade the instance.

## Consumption model
URocketMQ supports two consumption models: cluster and broadcast, please select according to the actual situation.

### Cluster mode

In cluster mode, consumer instances in the same consumption group distribute messages equally, and each message can only be consumed by one of the consumers.
### Broadcast mode
Consumers in the same consumer group in broadcast mode will receive all messages, and each message will be consumed by all consumers.
### Notes:
The consumption progress in broadcast mode is maintained on the client, and the probability of repeated consumption is higher than that in cluster mode, and the console does not support message accumulation viewing and related alarms in this mode.

Sequential consumption is not supported in broadcast mode.

Resetting the message bits is not supported in broadcast mode.

Based on the above points, we recommend that you prioritize cluster mode consumption.

## Message backtracking
Users can consume consumed messages or skip some unconsumed messages as needed, and currently support backtracking in the time dimension, because expired messages will be cleaned up, so the required time must be within the validity period of the message.

## Dead-letter queue
When a message is consumed continuously and fails to the maximum number of retries, the message confirms that it cannot be consumed normally, and the message is not immediately deleted, but is delivered to the dead-letter queue. You can use the console to query the dead-letter queue within a specified time range, download or resend it, and perform operations such as downloading.

## Message tracks
Message trajectory refers to the link information of a message passing through all nodes from production to consumption, including production and consumption trajectory information, and users can obtain message-related link information for relevant business analysis according to the message trajectory.
### Basic information
The basic message refers to the message itself, including the topic, key, tag, consumption status and other information.
### Production information
It includes information such as producer address, message sending time, message sending time, message sending status, etc.
### Consumption Information
A single message may be consumed by multiple consumers in multiple consumer groups, including the client address, consumption status, consumption time, and delivery time of each consumer who consumes the message.
### Access method
Clients need to set up message trajectories to use the message trajectories feature.

## Network mode
URocketMQ provides two network modes: intranet and extranet, which users can choose according to their business needs.
### Intranet mode
Users can only access URocketMQ instances on the SCloud user network.
### Internet mode
In the public network mode, the instance provides two access addresses: the private network and the external network:

Internet address: Users can access the URocketMQ instance in any environment that can access the Internet, and will be charged according to the actual Internet usage traffic every day.

Private network address: Users can only access URocketMQ instances on the SCloud user network without paying traffic fees.

## Product advantages
### Compatible with open source RocketMQ
Users can easily migrate to URocketMQ without modifying the code.
### User instance isolation
The underlying resources of each instance are isolated, and users can exclusively enjoy instance resources to ensure that instances do not affect each other.
### ACL security
Each instance is assigned a public and private key, which users can use to ensure the security of instance access.
### The service is highly available
The underlying services of each instance are deployed with multiple replicas to ensure high service availability.
### Master-slave autonomously
URocketMQ provides the master-slave switching function to ensure the availability of the cluster.

