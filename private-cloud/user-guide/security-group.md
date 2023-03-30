---
layout: default
title: Security Group
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/security-group/
nav_order: 8
---
# Security Groups
## Introduction to Security Groups
### Overview
Security Group is a virtual firewall similar to IPTABLES, which provides access control rules for inbound and outbound traffic, defines which networks or protocols can access resources, restricts network access traffic to virtual resources, supports IPv4 and IPv6 dual-stack restrictions, and provides necessary security guarantees for cloud platforms.

#### Implementation Mechanism

The platform security group is based on the Linux net-filter subsystem, which is implemented by adding flow table rules to the OVS flow table, and the host IPv4 and IPv6 packet forwarding functions need to be enabled. Each additional access control rule generates a flow table rule based on the NIC as the matching condition to control the traffic entering OVS and ensure the network security of virtual resources.
Security groups can only be applied to virtual machines, ENIs, load balancers, NAT gateways, and bastion hosts with the same security requirements in the same data center, as shown in the following figure:

Security groups have an independent lifecycle and can be bound to virtual machines, ENIs, Server Load Balancer, and NAT gateways to provide secure access control, and are automatically unbound after the virtual resources bound to them are destroyed.

![1](/assets/images/product-functional-architecture-6.jpg)

- The security protection of a security group for a virtual machine is for a network card, that is, the security group is bound to the default virtual network card or elastic network card of the virtual machine, and access control rules are set separately to restrict the network traffic of each network card.
- As shown in the schematic diagram of the security group, the security group is bound to a virtual external network card that provides external IP services, and the inbound and outbound rules are added to filter the north-south traffic (virtual machine extranet) access traffic.
- Security groups are bound to virtual NICs or ENIs that provide VPC services, and inbound and outbound rules are added to control east-west network access (between virtual machines and ENIs).
- Security groups are associated with the load balancer of the Internet type, and by adding inbound and outbound rules, you can restrict and filter the Internet IP traffic entering and leaving the Internet load balancer to ensure the security of the traffic of the Internet load balancer.
- The security group is bound to the NAT gateway, and by adding inbound and outbound rules, you can restrict the traffic entering the NAT gateway to ensure the reliability and security of the NAT gateway.
- A security group can be bound to multiple virtual machines, ENIs, NAT gateways, and Internet Server Load Balancer instances at the same time.
- A virtual machine supports binding an internal network security group and an external network security group, corresponding to the default internal network card and external network card of the virtual machine, respectively, and the external security group takes effect for all external IP addresses bound to the virtual machine.
- ENI can bind only one security group, which is independent of the security group bound to the default NIC of a virtual machine, and the traffic of the corresponding NIC is restricted.
- You can bind only one security group to a Server Load Balancer and NAT gateway instance, and you can change the security group to apply different network access rules.

When you create a virtual machine, you must specify a public network security group, and you can modify the inbound and outbound rules of the security group at any time, and the new rules take effect immediately when they are generated, and you can adjust the rules in the inbound and outbound direction of the security group according to your needs. Supports full lifecycle management of security groups, including the creation, modification, and deletion of security groups, and the creation, modification, and deletion of security group rules.

#### Security Group Rules
Security group rules can control the inbound and outbound traffic allowed to reach the resources associated with the security group, provide dual-stack control capabilities, and support effective filtering and control of TCP, UPD, ICMP, GRE and other protocol packets of IPv4/IPv6 addresses.

Each security group supports configuring 200 security group rules, which take effect for resource access according to priority. When the rule is empty, the security group denies all traffic by default; When the rule is not empty, other access traffic is denied by default except for the generated rule.

Supports stateful security group rules, and you can set inbound and outbound rules separately to control and restrict the inbound and outbound traffic of bound resources. Each security group rule consists of six elements: protocol, port, address, action, priority, direction, and description:
- Protocol: Supports TCP, UDP, ICMPv4, ICMPv6 packet filtering
  - ALL stands for all protocols and ports, ALL TCP stands for all TCP ports, and ALL UDP stands for all UDP ports;
  - Support shortcut protocol designation, such as `FTP, HTTP, HTTPS, PING, OpenVPN, PPTP, RDP, SSH`, etc.;
  - ICMPv4 refers to traffic traffic for IPv4 versions of the network; ICMPv6 refers to traffic traffic for the IPv6 version of the network.
- Port: The local virtual resource accessed by the source address or the TCP/IP port of the destination address accessed by the local virtual resource.
  - The port range of TCP and UDP protocols is `1~65535`;
  - ICMPv4 and ICMPv6 do not support configuration ports.
- Address: The source address of the network packet that accesses the security group-bound resource or the destination address of the security group-bound virtual resource.
  - When the direction of the rule is an inbound rule, the address represents the source IP address segment that accesses the bound virtual resource, supporting both IPv4 and IPv6 address segments;
  - When the direction of the rule is an outbound rule, the address represents the bound virtual resource to access the destination IP address segment, which supports both IPv4 and IPv6 address segments;
  - IP addresses and CIDR blocks that support CIDR notation, such as 120.132.69.216, 0.0.0.0/0, or ::/0.
- Action: When the security group takes effect, the processing policy for the packet, including "accept" and "reject" actions.
- Priority: The order in which the rules in the security group take effect, including high, medium, and low rules.
  - Security groups take effect in order of priority, and rules with high priority take precedence.
- Direction: The traffic direction corresponding to the security group rule, including outbound traffic and inbound traffic.
- Description: A description of each security group rule that identifies the role of the rule.

Security groups support data flow table status, and when a rule allows a request to communicate, the return data flow is automatically allowed without being affected by any rule. That is, security group rules take effect only for newly created connections, and two-way communication is allowed by default for established links. If an inbound rule allows any address to access port 80 of the external IP address of a virtual machine over the Internet, the return traffic (outbound traffic) that accesses port 80 of the virtual machine is automatically allowed without adding an outbound allow rule for the request.

*Note: It is generally recommended to set up concise security group rules to effectively reduce network failures.*

## Security Group Management
### Creating Security Groups
If the security group provided by the system cannot meet the requirements, you can specify the security group name and add relevant security group rules to quickly create a security group that belongs to the user, which can be associated or bound to related resources, and provide internal or external network access control for related resources to ensure the security of network access.

You can enter the Security Groups resource console through the navigation bar, and you can enter the Security Group creation wizard page through Create Security Group, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-66.png)

You can select and configure the security group name as prompted on the wizard page, and configure security group rules, including protocol type, port, address, action, priority, direction, and description.

The security group name refers to the name of the security group that needs to be created. Adding rules refers to adding traffic rules corresponding to the incoming and outgoing security groups, which can be added in batches or added after the security group is created.
- Protocol: A rule supports only one protocol, you can choose ALL or ALL TCP, ALL UDP, and so on.
- Port: A port can specify a single port number or a range of ports, in the format of 56-60.
- Address: The address bar supports entering IPv4 and IPv6 addresses, and supports batch input of multiple IP addresses, separated by commas.
- Action: If the protocol, port, address, and direction of the rule are the same, you cannot configure accept and reject actions at the same time.
- Direction: The traffic direction of the rule, including inbound and outbound, and only one direction can be selected for a rule.
Description: refers to the description of the current security group rule.

After clicking OK, you will automatically return to the security group list page, where you can view the creation process of the new security group.

### Viewing Security Groups
Go to the Security Group Resource console in the navigation bar, where you can view the resource list of the current account security group, and enter the details page through the security group name on the list to view the basic information of the security group, security group rules, and bound resources.

#### List of security groups

On the Security Groups page, you can view the list of security group resources and related information in the current account, including name, ID, number of rules, number of bound resources, creation time, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-67.png)

- Name/ID: The name and globally unique identifier of the security group;
- Number of rules: The number of security group rules added to the security group, expressed as a number;
Number of bound resources: The number of resources bound to the security group, expressed as a number, displayed as 0 when unbound;
Creation time: The creation time of the security group;
Status: The running status of the security group, including active, creating, and deleting.
The operation items on the list can delete a single security group, support batch deletion of security groups, and search and filter the list of security groups through the search box, supporting fuzzy search.

#### Security Group Details
On the security group resource list, click the name of the security group to view the details of the current security group and the security group rule information, and switch to the Resources page to view the resource information bound to the current security group, as shown on the overview page of the following figure:

![1](/assets/images/user-guide/user-guide-68.png)

- Basic Information: The basic information of the current security group, including the name, ID, number of rules, number of bound resources, and creation time.
- Security group rule management: For details about how to manage access control rules of the current security group, including adding, viewing, editing, and deleting, see Security group rule management.
- Bound Resources: For more information about the resources bound to the current security group, see Bound Resources.

#### Bound Resources
Bound resources refer to the list information of the resources bound to a security group, and you can view the virtual resources that have been bound or associated with the current security group. You can go to the Resources sub-page on the Security Group Details page to view the bound resource information.

![1](/assets/images/user-guide/user-guide-69.png)

As shown in the preceding figure, the list information of bound resources includes information such as resource name, resource type, and resource ID, including virtual machine, ENI, NAT gateway, and load balancing.

### Modify the name of a security group
You can modify the names and notes of security group resources in any state. You can modify it by clicking the Edit button to the right of each security group name on the security group resource list page.

### Delete a security group
You can delete security group resources that are not bound to any resource. After a security group is deleted, it is completely destroyed, and you must ensure that the security group is not bound or associated with any resources before deletion. You can delete a security group by deleting it in the action item on the security group list page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-70.png)

## Security Group Rule Management
### Creating a New Rule
The main means of providing network security access control for bound resources is to formulate reasonable security group rules, each security group supports the configuration of multiple rules, and the access to resources takes effect sequentially according to priority. When the rule is empty, the security group denies all traffic by default; When the rule is not empty, other access traffic is denied by default except for the generated rule.

You can specify the protocol type, port, address, action, priority, direction, and description of the rule to add rules, and you can enter the New Rule wizard page through "New Rule" on the security group details page.

### Viewing Rules
On the Security Group Details page, you can view the rules generated by the current security group and edit and delete existing rules through the action items in the list. The rule list information includes the protocol type, port, address, action, priority, direction, description, creation time, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-71.png)

### Editing Rules
If an existing security group rule cannot meet your business requirements, you can modify and change the parameters in the operation item of the security group rule list, which is the same as the parameters specified when creating a rule.
- When the protocol type is ALL or ICMPv4/ICMPv6, the port is not selectable and is displayed as "/";
- The address supports IP address and CIDR IP segment format, and can be configured as 0.0.0.0/0 or ::/0 if specified.

After the rule is edited, it takes effect immediately and will affect the network access of the bound resource.

### Deleting Rules
If you want to delete an existing security group rule, you can delete the deleted rule by using Delete in the security group rule list. To avoid business impact, we recommend that you check whether it is necessary to delete a security group rule before deleting it. After you delete a security group rule, the number of rules in the security group information is recounted and the latest number of rules is displayed.