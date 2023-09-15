---
layout: default
title: Anycast EIP
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/anycast-eip/
nav_order: 7
---
# AnycastEIP
## Product introduction
AnycastEIP uses SCloud's global BGP (Border Gateway Protocol) announcement capabilities, more than ten overseas nodes covering the world, and dedicated line resources between nodes to provide global network acceleration. 

Through IP transmission quality optimization and multi-entrance nearby access, the impact of public network delay jitter is reduced and the global use experience of EIP is improved.

## Product Features
### Access to multiple Region entrances nearby
The same Anycast EIP is announced by multiple overseas nodes at the same time, and is connected to the SCloud backbone network based on the source region of the access traffic.

After the access traffic is received, it is sent to the server for processing through SCloud dedicated line resources, ensuring global-level network transmission quality. 

When a single regional backbone network fails, AnycastEIP's multi-Region portal access capability is used to ensure normal and stable business.

### Multiple Region service nodes are processed nearby
The same Anycast EIP can be bound to servers in multiple regions at the same time, and services can be deployed on multiple overseas nodes. Access traffic chooses the nearest optimal path to accelerate global business.

### Health-check
Check the health status of backend nodes and automatically block unhealthy service nodes. For different types of service nodes, health check methods are different. 

For cloud hosts, the running status is checked by default, and `running` is considered healthy, otherwise it is unhealthy; for load balancing ULB, if all VServer health checks fail, the node is unhealthy, otherwise it is considered healthy.

![1](/assets/images/anycast-eip.jpg)

## Technology Architecture
### What is Anycast

|  | Scenario | Message replication | Receiver |
| -- | -- | -- | -- |
| Unicast | Unicast | A single source communicates to a single receiver | No replication required |
| Mutilcast | Multicast | A single source communicates to several recipients | Multiple copies |
| Broadcast | Broadcast | A single source communicates to all resources within its subnet | Multiple copies |
| Anycast |  Anycast | A single source communicates to a set of receivers, but the actual receiver remains one, determined by routing protocol selection | No replication required |

### Differences from ordinary EIP
- Ordinary EIP: Single Region declaration, global players across regions need to access through the public network. The delay is high and the jitter is large.
- AnycastEIP: Multi-Region announcement, players and users around the world can access the SCloud backbone network nearby and complete transmission through the SCloud dedicated line. Low latency and small jitter.

### Global acceleration
#### Outbound traffic
- Transmit directly to the public network through the source SCloud node.

#### Inbound traffic

- Cross-computer room back-to-origin: The public network route is routed through BGP, and the appropriate computer room entrance traffic is selected based on the proximity principle and sent to the corresponding POP point of SCloud, and the return-to-origin is completed through a dedicated line.
- Back to the source in the local computer room: The public network is routed to the POP point in the local computer room, and then transmitted back to the source through `UVER`.

## Usage restrictions
The current resource types that can be bound are cloud hosts, physical cloud hosts, and load balancing.

Announced regions: Washington, Los Angeles, Frankfurt, Tokyo, Taipei, Singapore, Mumbai.

Regions where services can be deployed: Singapore, Frankfurt, Washington, Los Angeles, London, Taipei, Bangkok, Tokyo, and Dubai.

The purchased bandwidth is the egress bandwidth. When the purchased egress bandwidth is less than `50Mbps`, the ingress bandwidth is equal to `50Mbps`. When the purchased egress bandwidth is greater than or equal to `50Mbps`, the ingress bandwidth is equal to the egress bandwidth. When multiple regions are bound to the same AnycastEIP at the same time, the bandwidth value is shared.

Product quota: The default quota of AnycastEIP under each account is `10`.

## Product Pricing
AnycastEIP is a prepaid model.

### AnycastEIP address cost
Anycast EIP address fee is free.

### AnycastEIP bandwidth cost
Fixed bandwidth billing, the monthly unit price of bandwidth is `200 Credit/Mbps`. Supports purchase methods such as timely, monthly, and yearly.

### Billing example
To purchase an Anycast EIP and set the bandwidth to `10Mbps`, the monthly unit price is `10Mbps*200` Credit/Mbps=2,000 Credit.

## Application scenarios
According to the functions provided by AnycastEIP, the following demand scenarios can be met.

### Game acceleration
Game user requests reach the game server nearby, shortening the public network path and reducing game experience problems caused by public network delay, jitter, etc. 

There is no need to care about geographical attributes, AnycastEIP automatically optimizes lines and simplifies the service deployment process. 

Only inbound traffic is accelerated, and outbound traffic is still sent from the public network.

### Distributed services
Binding the same Anycast EIP to multiple service nodes in different regions can build a global-level distributed service (similar to Google `8.8.8.8` DNS service) or edge computing service.