---
layout: default
title: UVPC
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/uvpc/
nav_order: 3
---

# UVPC 
## Introduction to VPCs
A VPC (Virtual Private Cloud) is a logically isolated network environment that belongs to users. In a VPC, you can create a VPC with a specified CIDR block, create subnets in the VPC, and manage cloud resources independently, while using network ACLs to protect security. VPC provides users with the following capabilities:
- Custom CIDR blocks: The three CIDR blocks can be freely combined, and VPC CIDR blocks can be added or decreased at any time.
- Complete network management: Gateway NAT, custom route table, network ACL, private virtual IP, and virtual NIC, users can freely choose the configuration and set rules according to their requirements.
- Subnets across zones: Subnets can cover any zone in a region to achieve disaster recovery across zones.
- High scalability: Cross-region VPCs can be connected through Express Connect (UDPN) to achieve stable intranet transmission. Combined with private line access, it realizes a hybrid cloud architecture with single-point access and global interconnection.

If your business may involve multiple networks, we recommend that you plan the network in advance.

## VPC components
A VPC consists of VPCs, subnets, NAT gateways, and network ACLs:

- VPC: A VPC is a logically isolated network environment that belongs to users. In a VPC, you can create a VPC with a specified CIDR block, create subnets in the VPC, and manage cloud resources independently.
- Subnet: To scientifically and effectively divide the address space within a VPC, divide it into more fine-grained network segments. These independent network segments are called subnets.
- NAT gateway: NAT gateway is an enterprise-level VPC public network gateway that allows cloud resources in subnets that are not bound to an elastic IP address to access the Internet, and can also configure port forwarding rules to enable cloud resources to provide services to the outside world.
- Network ACL: is a subnet-level security policy that controls the flow of traffic to and from a subnet. Users can set up outbound and inbound rules to control the flow to and from the subnet.

## Introduction to VPCs and subnets
### VPC
A VPC (Virtual Private Cloud) is a logically isolated network environment that belongs to users. In a VPC, you can create a VPC with a specified CIDR block, create subnets in the VPC, and manage cloud resources independently. When you create a VPC, you can independently plan CIDR blocks and flexibly specify CIDR blocks for the VPC. 

Currently, VPC supports the following CIDR blocks:

- 10.0.0.0/8 (10.0.0.0-10.255.255.255) mask range: maximum /8 mask, minimum /29 mask.
- 172.16.0.0/12 (172.16.0.0-172.31.255.255) mask range: maximum /12 mask, minimum /29 mask.
- 192.168.0.0/16 (192.168.0.0-192.168.255.255) mask range: maximum /16 mask, minimum /29 mask.

You can no longer provide default VPC details.

### Subnets

To scientifically and effectively divide the address space in a VPC, divide it into more fine-grained network segments. These independent network segments are called subnets.

Cloud resources in the subnet support cross-zone deployment, providing strong guarantees for cross-zone disaster recovery.

The minimum mask of the subnet CIDR block is a /29 mask. When there are cloud resources in a subnet, the subnet is not allowed to be deleted.

### The VPC network is interoperable

By default, networks in the same VPC communicate with each other, but networks in different VPCs are not connected by default. VPC network interconnection enables network interconnection between VPCs in different projects and regions.

No overlapping of CIDR blocks between interconnected VPCs is allowed. For more information about VPC network interconnection capabilities, see VPC connection rules.

### Product quotas
The default quota for each account is as follows:

| Name | Quota |
| --- | --- |
| The number of VPCs in each region | 100 |
| The number of subnets per VPC | 200 |

## NAT gateway introduction
NAT gateway is an enterprise-level VPC public network gateway that allows cloud resources in subnets that are not bound to elastic IP addresses You can configure port forwarding rules to enable cloud resources to provide services to the outside world. 

### Mode setting
NAT gateways can be selected from normal mode or whitelist mode. 
In  normal mode, all cloud resources in the specified subnet that are not bound to elastic IP addresses can pass through the NAT The gateway accesses the Internet. 
In the whitelist mode, only cloud resources in the subnet specified by the NAT gateway and defined in the whitelist can pass through the NAT gateway to go out of the external network. 

Before you change the NAT network to the white list mode, you can configure the white list in advance to ensure that you switch to the white list mode Does not affect normal business. In normal mode, the whitelist is configurable but does not take effect. 
### Port forwarding
Users can configure port forwarding to map the internal network ports of cloud resources in a  VPC  to a NAT gateway to make cloud resources matchable Outside to provide services. 

Cloud resources in the specified subnet of the NAT gateway that have elastic IP addresses bound will not appear in the port forwarding configuration list. 
### Network export
You can set a cloud resource in a subnet to access the Internet through a specified EIP. 

Provide default egress rules to support resources in the subnet to access the Internet through the responsible balancing mode or designated EIP. 

### Quota limits
| Name | Quotas |
| --- | --- |
| The number of EIPs that can be tied to NAT | 64 |
| Number of egress rules | 100 |
| Number of port forwardings | 100 |

### Products supported by NAT
Currently, the following types of sources are supported for whitelisting, egress rules, and port forwarding: 
| Name | Whitelist | Export rules | Port forwarding |
| --- | --- | --- | --- |
| UHOST cloud host | ✓ | ✓ | ✓ |
| UPHOST physical cloud host | ✓ | ✓ | ✓ |
| UDHost Private Zone | ✓ | — | ✓ |
| UHybrid hosts the cloud | — | — | — |
| UNI virtual NIC | ✓ | ✓ | ✓ |
| UAuditHost fort machine | ✓ | ✓ | ✓ |

## Intranet VIP
Intranet virtual IP (VIP) is a driftable intranet portal for user-defined high-availability services provided by SCloud cloud platform for cloud hosts and physical cloud hosts. When using VIP, users need to combine VIP with highly available services to drift the service entrance when the service fails, so as to achieve high availability of the service.
### Product quotas
We recommend a maximum of 1,000 VIPs in the same VPC. If drift is required (i.e. high availability), a maximum of 100 is recommended.
| Name | Quota |
| -- | -- |
| Number of VIPs supported in a single region | 100 |

## Introduction to network ACLs

A network ACL is a subnet-level security policy that controls the flow of traffic to and from a subnet. Users can set up outbound and inbound rules to control the flow to and from the subnet.

Network ACLs are stateless, for example, if a user needs to allow certain access, they need to add the corresponding inbound rules and outbound rules at the same time. If you add only inbound rules and do not add outbound rules, an access exception will result.

ACL rules do not take effect on ECSs, physical ECSs, ULBs, VIPs, and instances implemented based on the VIP architecture.

### Associate subnets
After you create a network ACL, you can bind and unbind the ACL to any subnet in the VPC to which it belongs. Before binding a subnet, make sure that the rules in the ACL are correct to avoid affecting the normal communication of cloud resources in the associated subnet.

### Outbound/inbound rules
Network ACL rules are divided into outbound rules and inbound rules. User updates to network ACL rules are automatically applied to the subnets associated with them.

The maximum number of outbound/inbound rules allowed is 50.

Network ACL rules consist of the following components:

- Policy: Allow or Deny.
- Source IP/Destination IP: The network segment targeted by the outbound/inbound rule.
- Protocol type: TCP, UDP, ICMP and GRE protocol types are supported, and you can select ALL to specify all protocol types.
- Destination port: The port range allowed for TCP and UDP protocol types is 1-65535. Other protocol types do not require a port to be specified.
- Priority: The priority corresponding to the rule, the lower the number, the higher the priority. The range is 1-30000. Only one outbound/inbound rule of the same priority can be created.
- Application Target: The effective range of the ACL rule. All resources in the subnet and specified resources in the subnet are supported. "All resources in subnet" means that the rule takes effect for all resources in the subnet bound to the ACL (except Cloud Host, Physical Cloud Host and ULB); Specified resources in subnet means that the rule takes effect only for selected resources (you cannot specify a high-availability UDB), but not for unselected resources in the subnet.

Note: After you create a network ACL, the system automatically adds a default outbound rule and a default inbound rule.

- The default outbound rule is outbound allow for all protocols and all port traffic.
- The default inbound rule is inbound allow for all protocols and all port traffic.
- The default rule does not allow editing and deletion, which exists when the ACL is created.
- The default rule has the lowest priority and can be overridden by adding a rule with a higher priority.

### Product quotas
The ACL quota for each network is as follows (without default rules)
| Name | Quota |
| -- | -- |
| Number of outbound rules | 100 |
| Number of inbound rules | 100 |

## Introduction to security groups
### Overview of security groups
A security group is a stateful virtual firewall with packet filtering capabilities, which can be used for network access control between different VPCs and different instances in the same VPC.
Currently supported instances: Cloud hosts and virtual NICs;
### Security group initiation rule description
If you enable a security group but do not configure any security group rules, the inbound traffic of the instances in the security group is restricted and all outbound traffic is allowed.
### Security group rule execution order
Match according to priority, first match the priority between security groups, and then match the priority of rules in the security group (the lower the number, the higher the priority);

Inter-group priority: the priority between different security groups under the same instance, the smaller the better, the more repeatable, the value range is 1-5;

Rule priority: the smaller the priority, the better, the more repeatable, and the value range is 1-200.
### Security group rule template
By default, a general WEB server template is provided: the instances in the security group allow TCP `22`, `3389`, `80`, and `443` ports and ICMP protocols to be allowed in the inbound direction, allowing all internal network blocks and allowing all outbound traffic.

### Security group operation process
The following figure shows the security group operation process:
- Step 1: Create security group.
- Step 2: Add security group rule(s).
- Step 3: Binding instance.
- Step 4 (optional): Manage security group.
- Step 5 (optional): Manage security group rule(s).

### Security groups and external firewalls can coexist at the same time
If the security group and the external firewall coexist, traffic accessing the instances in the security group from the external network is first processed by the external firewall rules, and the allowed traffic is processed by the security group rules before reaching the security group instances.

Conversely, traffic that an instance in a security group accesses the Internet is processed by the security group rules first, and then passes through the firewall rules of the Internet after it is allowed to access the Internet.
### Security group usage limits
1. Supported model restrictions: Outstanding cloud host (virtual machine), virtual network card
2. Restrictions on use:
   - Bind up to five security groups to the same instance;
   - A maximum of 50 outbound and inbound rules under a security group;
   - A maximum of 100 instances can be bound to a security group;
   - Up to 50 security groups can be created under a single company ID;

### Additional usage notes for security groups
1. A security group can only bind instances under the same VPC as itself.
2. Different NICs on the same cloud host belong to completely independent different instances, and different security groups can be bound;
3. It is recommended that VIP drift only between instances bound to the same security group;
4. If the security rules are changed to security groups for the cloud host that was originally configured with an external network firewall, the security group rules will not take effect until the host migration is completed, and the security group and firewall rules will coexist on the cloud host.
5. Security groups do not support IPv6 at the moment;

## Introduction to routing tables
### Explanation of terms
Default route table: When you create a VPC, the system defaults to the route table created by the VPC. The newly created subnet is bound to the default route table by default. The default route table does not allow deletion or editing, and the rules in the default route table are system routes.

Custom route table: You can create a route table and add custom routing rules. Custom route tables can be created, deleted, and edited. Note that system routes in the custom routing table are only allowed to be viewed, not edited.

System Routing: The routing rule added by default to the route table. System routes are only allowed to be viewed.

Custom routes: User-defined routing rules. Routing rules include destination address, next-hop type, and next-hop. 

Custom routes can be added, edited, and deleted. The granularity of the route table is subnet. Each subnet must be bound and can only be bound to one route table, and a route table can be bound by multiple subnets.

### Routing rule types 
The next-hop types of routing table rules are enumerated as follows:

| Next-hop type | Next hop | Illustrate |
| --- | --- | --- |
| LOCAL | LOCAL | A local route is added to each CIDR block in a VPC to indicate that the VPC is accessible by default |
| VPC interconnection | VNET id | System-based routing is added by default after the VPC is opened |
| Internet gateway | Internet gateway | Systematized route, which specifies the exit of the Internet. If the cloud resource is bound to the EIP, it is tacitly exported to the EIP; Otherwise, if the subnet is bound to a NAT gateway, the default egress is NAT Gateway. |
| Public Service | Public Service | System-based routing points to the DNS and YUM sources provided by SCloud, as well as ULB's proxy address and health check address |
| CUSTOM | CUSTOM | System routing, SCloud internal special services, such as UAEK and other services |
| IPSecVPN | vpngw id | Custom route: After using IPSec VPN products, add routes to IPSecVPN gateways in a VPC. At present, the system will be added automatically, and customers are not allowed to add it for the time being |
| Intranet VIP | vip id | Customize the route, and the next hop is VIP |
| Examples | uhost id(post id) | Customize the route, and the next hop is the cloud host or the physical cloud host |


### Restrictions on use:
1. The destination CIDR blocks in routing rules can overlap, but they cannot be identical. If the destination address in multiple routing rules is matched at the same time, the priority is determined according to the algorithm of the longest prefix match.
2. The next hop types that can be added by custom routing rules are intranet VIP and instance (cloud host, physical cloud host).
3. Custom routing rules cannot be added to the default route table, if you need custom rules, you can create a custom route table and add custom routing rules.
4. Each routing table supports up to 50 custom routing rules.