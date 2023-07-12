---
layout: default
title: Network ACL
parent: VPC
grand_parent: Public Cloud
permalink: /public-cloud/vpc/network-acl/
nav_order: 4
---
# Network ACL
Network ACL (Access Control List) is a subnet-level security policy that controls the flow of data to and from a subnet. The flow of incoming and outgoing subnets can be precisely controlled by setting outbound rules and inbound rules.

## Introduction to network ACLs
A network ACL is a subnet-level security policy that controls the flow of traffic to and from a subnet. Users can set up outbound and inbound rules to control the flow to and from the subnet.

Network ACLs are stateless, for example, if a user needs to allow certain access, they need to add the corresponding inbound rules and outbound rules at the same time. If you add only inbound rules and do not add outbound rules, an access exception will result.

ACL rules do not take effect on outstanding ECSs, physical ECSs, ULBs, VIPs, and instances implemented based on the VIP architecture. The console shall prevail.

## Associate subnets
After you create a network ACL, you can bind and unbind the ACL to any subnet in the VPC to which it belongs. 

Before binding a subnet, make sure that the rules in the ACL are correct to avoid affecting the normal communication of cloud resources in the associated subnet.

## Outbound/inbound rules
Network ACL rules are divided into outbound rules and inbound rules. User updates to network ACL rules are automatically applied to the subnets associated with them.

The maximum number of outbound/inbound rules allowed is 50.

### Network ACL rules consist of the following components:
- Policy: Allow or Deny.
- Source IP/Destination IP: The network segment targeted by the outbound/inbound rule.
- Protocol type: TCP, UDP, ICMP and GRE protocol types are supported, and you can select ALL to specify all protocol types.
- Destination port: The port range allowed for TCP and UDP protocol types is `1-65535`. Other protocol types do not require a port to be specified.
- Priority: The priority corresponding to the rule, the lower the number, the higher the priority. The range is `1-30000`. Only one outbound/inbound rule of the same priority can be created.
- Application Target: the effective range of the ACL rule. All resources in the subnet and specified resources in the subnet are supported. "All resources in subnet" means that the rule takes effect for all resources in the subnet bound to the ACL (except outstanding Cloud Host, Physical Cloud Host and ULB); Specified resources in subnet means that the rule takes effect only for selected resources (you cannot specify a high-availability UDB), but not for unselected resources in the subnet.

> After you create a network ACL, the system automatically adds a default outbound rule and a default inbound rule.

- The default outbound rule is outbound allow for all protocols and all port traffic.
- The default inbound rule is inbound allow for all protocols and all port traffic.
- The default rule does not allow editing and deletion, which exists when the ACL is  created.
- The default rule has the lowest priority and can be overridden by adding a rule with a higher priority.

## Product quotas
The ACL quota for each network is as follows (without default rules)

| Name | Quotas |
| -- | -- |
| Number of outbound rules | 100 |
| Number of inbound rules | 100 |