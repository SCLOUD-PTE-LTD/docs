---
layout: default
title: Route Table
parent: VPC
grand_parent: Public Cloud
permalink: /public-cloud/vpc/route-table/
nav_order: 3
---
# Route Table

## Explanation of terms

- Default route table: When you create a VPC, the system tacitly considers the route table created by the VPC. The newly created subnet will be bound by default to the default routing table. The default routing table is not allowed to be deleted or edited, and the rules in the default routing table areal systems unified routing.

- Custom routing table: A route table created by users and custom routing rules. Custom routing tables can be created, deleted, and edited. Note that the system routes in the custom routing table are only allowed to be viewed, not edited.

- System-based routing: The routing rules that the system-based tacitly believes are added to the routing table. System wise routing allows only viewing.

- Custom route: User-defined routing rules. Routing rules include destination address, next-hop type, and next-hop. Custom routes can be added, edited, and deleted. The granularity of the route table is subnet. Each subnet must be bound and can only be bound to one route table, and a route table can be tied to multiple subnets.

## Routing Rule Type: the next hop type of a routing table rule is as follows:

| Next-hop type | Next hop | Illustrate |
| --- | --- | --- |
| LOCAL | LOCAL | A local route is added to each CIDR block in a VPC to indicate that the VPC is accessible by default |
| VPC interconnection | vnet id | System-based routing is added by default after the VPC is opened |
| Internet gateway | Internet gateway | Systematized route, which specifies the exit of the Internet. If the cloud resource is bound to the EIP, it is tacitly exported to the EIP; Otherwise, if the subnet is bound to a NAT gateway, the default egress is NAT Gateway. |
| Public Service | Public Service | System-based routing points to the DNS and YUM sources provided by SCloud, as well as ULB's proxy address and health check address |
| CUSTOM | CUSTOM | System routing, SCloud internal special services, such as UAEK and other services |
| IPSecVPN | vpngw id | Custom route: After using IPSec VPN products, add routes to IPSecVPN gateways in a VPC. At present, the system will be added automatically, and customers are not allowed to add it for the time being |
| Intranet VIP | vip id | Customize the route, and the next hop is VIP |
| Examples | uhost id(post id) | Customize the route, and the next hop is the cloud host or the physical cloud host |

## Usage restrictions:

1. The destination CIDR blocks in routing rules can overlap, but they cannot be identical. If the destination address in multiple routing rules matches at the same time, the priority is determined according to the algorithm of longest suffix matching.

2. The next hop types supported by custom routing rules are intranet VIP and actual examples (cloud host, physical cloud host).

3. The default routing table cannot add custom routing rules, if custom rules are required, custom definitions can be created route table and add custom routing rules.

4. Each routing table supports up to 50 custom routing rules.