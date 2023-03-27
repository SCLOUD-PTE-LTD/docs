---
layout: default
title: UDNS
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/udns/
nav_order: 5
---
# What is UDNS

UDNS service is a highly available and scalable domain name resolution service system provided by SCloud. Currently, you can configure internal domain name resolution and external domain name recursion resolution, and more functions will be available soon.
## Intranet parsing

The intranet resolution service provided by UDNS can be used to implement scenarios such as intranet service discovery, load balancing, and high service availability. By setting up intranet domain name resolution, you can use domain name records to manage cloud resources in your VPC, including cloud hosts and load balancers.
## Technical architecture
### Introduction to DNS services

DNS (Domain Name System) is a service that the Internet relies on, as a system that maps domain names and IP addresses to each other, so that people and businesses can access Internet resources more conveniently. DNS uses TCP and UDP port 53 by default. In fact, DNS queries generally use the UDP protocol, and the use of TCP is very rare. The UDNS system provides UDP services.
### Introduction to the schema

SCloud intranet DNS cluster uses `BGP+ECMP` to achieve high availability and scalability. The architecture is as follows:

![1](/assets/images/DNS1.png)

As shown in the figure above, the UDNS service provided by SCloud is a region-level product. A DNS cluster can be thought of as a stateless service, and each server can provide services independently. 

The DNS server declares the same VIP to the upstream switch via BGP. When an exception occurs in the DNS service, BGP is interrupted, so that traffic is switched to other servers to achieve high availability.

### Application scenarios

![1](/assets/images/DNS2.png)
The core function of DNS is to convert the domain name into an IP address, so as to get rid of the service's dependence on the IP address, and the following functions can be achieved through the operation of the domain name:
#### Service discovery

![1](/assets/images/DNS3.png)
As shown in the preceding figure, the client uses the domain name as the service request object. The server registers the corresponding domain name with the UDNS system separately, so that the client can access the server through the domain name. The server IP address is migrated, and the configuration of the client does not need to be changed, and the DNS resolution record in UDNS can be modified to seamlessly switch the service address.
#### Load balancing

![1](/assets/images/DNS4.png)
As shown in the figure above, UDNS supports the "random answer" mode, and users can configure the A record and AAAA record in this mode. UDNS can randomly return the corresponding value based on the configured record value and weight. According to the weight, the client's requests are distributed to each server.
#### High availability

As shown in the figure above, customers can use preemptive host records to achieve high availability of services.

### Usage Restrictions
#### Quota limits

| Name | Restriction | Description |
| -- | -- | -- |
| Domain Name Instances | 5 | You can add up to five domain name instances per account and region |
|  Associated VPC | 100 | A maximum of 100 VPCs can be bound to a single domain name instance  |
|  Number of records | 1,000 | You can add up to 1,000 resolution records to a domain name instance  |
|  Requests per second | 5,000 | The maximum number of requests per second in a VPC is 5,000 |