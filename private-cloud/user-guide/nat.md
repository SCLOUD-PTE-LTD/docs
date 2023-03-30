---
layout: default
title: NAT Gateway
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/nat-gateway/
nav_order: 10
---
# NAT Gateway
## Introduction to NAT Gateways
### Overview
NAT Gateway is a VPC gateway similar to NAT network address translation protocol, which provides SNAT and DNAT proxies for cloud platform resources and supports Internet or physical network address translation capabilities. 

The SNAT and DNAT rules of the platform NAT gateway service implement SNAT forwarding and DNAT port mapping for virtual resources in a VPC, respectively.

- SNAT rules: SNAT rules enable SNAT capabilities at the VPC level, subnet level, and virtual resource instance level, so that resources in different dimensions can access the Internet through NAT gateways.
- DNAT rules: DNAT rules allow you to configure port forwarding based on TCP and UDP protocols to map the private network ports of cloud resources in a VPC to the public IP addresses bound by the NAT gateway to provide services to the Internet or IDC data center networks.

As a virtual network gateway device, you need to bind an Internet IP address as the exit of SNAT rules and DNAT rules for the NAT gateway. NAT gateways have region (data center) attributes and only support SNAT and DNAT forwarding services for VPC virtual resources in the same data center.

The network that a virtual machine can access through a NAT gateway depends on the configuration of the CIDR block to which the bound public IP belongs on the physical network. If the bound public IP address can access the physical network of the IDC data center, the virtual machine accesses the physical network of the IDC data center through a NAT gateway.

### Application scenarios
When users use virtual machines to deploy application services on the platform, there are scenarios where you can access the external network or access the virtual machine through the external network, usually we bind an external IP address to each virtual machine for communication with the Internet or IDC data center network. In real environments and scenarios, it may not be possible to allocate enough public IP addresses, and even if the public IP addresses are sufficient, there is no need to bind a public IP address to each virtual machine that needs to access the Internet.

- Shared EIP: Allows multiple VPC intranet virtual machines to share one or more external IP addresses to access the Internet or the physical network of an IDC data center through SNAT proxy.
- Mask real IP addresses: With SNAT proxy, multiple VPC intranet virtual machines use proxy IP addresses to communicate and automatically mask real IP addresses.
- VPC intranet virtual machines provide Internet services: configure IP and port forwarding through DNAT proxy to provide service services to the Internet or IDC data center networks.

### Architecture Principles
The underlying resources of the platform products and services are unified, and the NAT gateway instance is the primary and standby high-availability cluster architecture, which can automatically fail over the NAT gateway and improve the availability of SNAT and DNAT services. At the same time, combined with the public IP address, SNAT and DNAT proxies are provided according to the NAT configuration for tenant virtual resources.

At the product level, tenants apply for a NAT gateway, specify the subnets that the NAT gateway can allow communication on, and bind [Internet IP] to enable virtual machines under multiple subnets to communicate with the Internet or the physical network of IDC data centers, as follows:

![1](/assets/images/product-functional-architecture-14.jpg)

- The platform supports using a NAT gateway for multi-subnet VMs with the same VPC to access the internet or IDC data center network.
- When a virtual machine in multiple subnets that is not bound to a public IP address is associated with a NAT gateway, the platform automatically issues routes to the Internet in the virtual machine.
- The virtual machine transmits data accessing the Internet to the bound Internet IP address through the NAT gateway through the routed route.
- The data transmitted to the external IP address sends packets to the physical switch through the platform OVS and physical NIC to complete the data SNAT communication.
- When the external network needs to access virtual machine services in a VPC, you can use NAT gateway port forwarding to enable the Internet or IDC physical network to access VPC intranet services through the IP + port bound to the NAT gateway.

### Features
The cloud platform provides highly available NAT gateway services and supports gateway lifecycle management, including multi-public IP addresses, SNAT rules, DNAT port forwarding and monitoring alarms, and provides network and resource isolation security for NAT gateways.

A VPC allows you to create 20 NAT gateways, and SNAT rules in all NAT gateways under the same VPC are not repeatable, that is, SNAT rules in 20 NAT gateways are not allowed. Scenario example:
- When an SNAT rule for the subnet (`192.168.0.1/24`) is created in NATGW (VPC:`192.168.0.0/16`), NATGW cannot create an SNAT rule with subnet (`192.168.0.1/24`) as the source address in the same VPC, and the subnet rule in NATGW01 can be deleted.
- When you create a VPC-level rule in NATGW (VPC:`192.168.0.0/16`), you cannot create a VPC-level rule under the same VPC.
- When you create an SNAT rule for a virtual machine (`192.168.1.2`) in NATGW (VPC:`192.168.0.0/16`), NATGW cannot create an SNAT rule with the source address of the virtual machine (`192.168.1.2`) in the same VPC.

#### Multi-public IP support
NAT gateways support binding multiple public IP addresses to enable resources in SNAT rules to access the Internet through multiple public IP addresses, and virtual resources in DNAT port forwarding rules can access VPC intranet services through specified public IP addresses.

A NAT gateway supports binding IPv4 public IP addresses of 50 default route types, providing a shared public IP resource pool for virtual resources in the specified subnet of the NAT gateway, providing more flexible and convenient SNAT and DNAT capabilities.

You can view all external IP addresses that have been bound to a NAT gateway, and unbind external IP addresses, after which the associated SNAT rules and DNAT rules will be invalidated. You can modify SNAT and DNAT rules to set new egress IP addresses and ingress source IP addresses, respectively.

#### SNAT rules
NAT gateways support SNAT (Source Network Address Translation) capabilities through SNAT rules, each rule consists of a source address and a destination address, that is, the source address is translated to the destination address for network access. Platform SNAT rules support outbound network scenarios in multiple scenarios, that is, the source address includes three types: VPC, subnet, and virtual machine:

- VPC level: All virtual machines under the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.
- Subnet level: All virtual machines in the specified subnet under the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.
- VM Level: Only virtual machines specified in the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.

The destination address of the rule is the public IP address bound to the NAT gateway, and the source address of the VPC, subnet, and virtual machine can be converted to the external IP of the gateway-bound network for network communication through the rule policy, that is, the virtual machine can communicate with the external network of the platform without binding the external IP of the SNAT rule, such as accessing the IDC data center network or the Internet.

The rule priority of different source address types in SNAT rules is different, and the rule with the highest priority prevails:

(1) The source address is VPC
- All virtual machines in the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.
- A NAT gateway allows only one SNAT rule with a source address of ALL.
- Rules with a source address of ALL have the lowest priority, and when no precise rule is matched, the rule with a source address of ALL accesses the Internet.

(2) The source address is the subnet CIDR
- Virtual machines under the subnet can access the Internet through a NAT gateway, and the SNAT rule of the subnet takes precedence over the rule with a source address of ALL.
- Only one SNAT rule can be created per subnet, and duplicates are not allowed.
- You can configure SNAT rules separately for virtual machines under a subnet, with a higher priority than SNAT rules with a source address of the subnet.

(3) The source address is the IP address of the virtual machine
- Virtual machines can access the Internet through a NAT gateway.
- Only one SNAT rule can be created per virtual machine IP, and duplicates are not allowed.
- SNAT rules with a source address of the virtual machine IP take precedence over SNAT rules with a source address of ALL and Subnet.

The destination address of an SNAT rule can be one or more public IP addresses bound to the NAT gateway, and when the destination public IP address is ALL, the source address resource randomly selects an IP from all public IP pools on the gateway to access the Internet.

By default, a NAT gateway can create 100 SNAT rules.

After you configure SNAT rules, NAT Gateway automatically routes default routes to VMs with matching source addresses, enabling VMs to access the Internet through the external IP addresses of SNAT rules. The specific communication logic is as follows:
- If the virtual machine is not bound to an IPv4 public IP address, it accesses the Internet through a NAT gateway by default.
- If the virtual machine is bound to an IPv4 public IP address and a default network egress exists, the virtual machine's default network egress accesses the external network.
- If the virtual machine is bound to an IPv4 public IP address and has no default network egress, it accesses the Internet through a NAT gateway.

When a virtual machine accesses the Internet through a NAT gateway, the external IP address used depends on the SNAT rule configuration.

#### DNAT Rules
NAT gateway supports DNAT (Destination Network Address Translation), also known as port forwarding or port mapping, which translates an external IP address into an IP address of a VPC subnet to provide network services.
- It supports port forwarding for TCP and UDP protocols, and supports lifecycle management of port forwarding rules.
- Supports batch multi-port forwarding rule configuration, that is, port segments are mapped, such as TCP:1024~TCP:1030.
- When a NAT gateway is bound to a public IP address, the port forwarding rule provides Internet services to virtual machines in the VPC subnet, and the virtual machine services in the subnet can be accessed through the Internet.

#### Monitoring alarms
The platform supports the collection and display of NAT gateway monitoring data, displays the metric data of each NAT gateway through monitoring data, and supports setting threshold alarms and notification policies for each monitoring metric. Supported monitoring metrics include network egress/bandwidth, network egress/packet volume, and number of connections.

You can view the monitoring data of a NAT gateway in multiple time dimensions, including 1-hour, 6-hour, 12-hour, 1-day, 7-day, 15-day, and custom time monitoring data. The default query count is 1 hour of data, and you can view up to 1 month of monitoring data.

#### NAT gateway is highly available
NAT gateway instances support a highly available architecture, that is, at least two virtual machine instances are built, and hot standby is supported. When an instance of a NAT gateway fails, you can automatically switch to another virtual machine instance online to ensure the normal operation of the NAT agent. At the same time, based on the drift characteristics of external IP addresses, it supports the availability of SNAT gateway egress and DNAT ingress when the physical machine is down.

### NAT Gateway Security
The network access control of the NAT gateway can be associated with a security group to provide security, and the inbound traffic and outbound traffic to the external IP address bound to the NAT gateway can be controlled through the rules of the security group, and the filtering and control of TCP, UDP, ICMP, GRE and other protocol packets can be supported.

Security groups and security group rules can restrict traffic to the NAT gateway of the security group to allow traffic within the security group rules to reach the destination through the security group. To ensure the resource and network security of the NAT gateway, the platform provides resource isolation and network isolation mechanisms for the NAT gateway:

(1) Resource isolation
- NAT gateways have data center attributes, and NAT gateway resources are physically isolated between different data centers;
- NAT gateway resources are isolated from each other, and tenants can view and manage all NAT gateway resources under accounts and sub-accounts.
- NAT gateway resources in a tenant can only bind VPC subnet resources in the same data center in the tenant.
- NAT gateway resources in a tenant can only bind external IP resources in the same data center within the tenant.
- A NAT gateway resource in a tenant can only bind a security group resource in the same data center in the tenant.

(2) Network isolation
- The NAT gateway resource network between different data centers is physically isolated from each other.
- NAT gateway networks in the same data center are isolated by VPCs, and NAT gateway resources in different VPCs cannot communicate with each other.
- The isolation of the public IP network bound by the NAT gateway depends on the configuration of the user's physical network, such as different VLANs.

## Usage Process
Before you use the NAT gateway service, you need to plan the VPC network and public IP network of the NAT gateway based on your business requirements, and configure SNAT and DNAT rules based on your business requirements. The specific process is as follows:
- Tenants create VPCs and subnets based on demand, and create virtual machines in multiple subnets;
- Tenants create an Internet IP address based on their requirements, specify the network type, associated subnet, and bound egress IP address through APIs or consoles, and create a NAT gateway.
- If you add SNAT rules for VPCs, subnets, or VM types through SNAT rules, the associated VMs can access the Internet through a NAT gateway.
- If you configure rules for virtual machines that need to provide services through DNAT rules, the public network can access resources in the VPC network that are not bound to external IP addresses.
- If you need to restrict the inbound and outbound traffic of the NAT gateway, you can configure the security group bound to the NAT gateway.
You can bind multiple public IP addresses to a NAT gateway, and configure SNAT rules and DNAT rules for multiple public IP addresses.
## Create a NAT gateway
To create a NAT gateway on the platform, you must specify the model, VPC network, subnet, public IP address, security group, NAT gateway name, and remarks. A VPC allows you to create 20 NAT gateways, and SNAT rules in all NAT gateways under the same VPC are not repeatable, that is, SNAT rules in 20 NAT gateways are not allowed.

You can enter the NAT Gateway resource console through the navigation bar, and use Create NAT Gateway to enter the creation wizard page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-92.png)

(1) Select and configure the NAT gateway base configuration and network setup information:
- Model: The cluster type of the host where the NAT gateway instance is located, customized by the platform administrator, such as x86 models and ARM models, and the instance created by the ARM model is an ARM NAT gateway instance, which is adapted to domestic chips, servers, and operating systems.
- Name/Comment: The name and remark information of the NAT gateway.
- VPC network: The VPC network served by the NAT gateway, that is, the NAT gateway provides SNAT and DNAT services only for the resources in the selected VPC, and only supports adding resources belonging to the VPC network as the source address of the SNAT rule and the destination address of the DNAT rule.
- Subnet: The subnet where the NAT gateway instance is located, it is generally recommended to choose a subnet with a sufficient number of available IPs.
- Internet IP: The public IP address used by the NAT gateway address, and the resources bound to the VPC network access the Internet or the IDC physical network through the external IP address bound to the NAT gateway.
- Security Group: The security group used by the public IP address of the NAT gateway to control the traffic that can enter the NAT gateway.
(2) After selecting and configuring the above information, you can select the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to create a NAT gateway:
- Purchase quantity: Create NAT gateway instances in batches according to the selected configuration and parameters, and only one NAT gateway instance can be created at a time.
- Payment method: Select the billing method of the NAT gateway, which supports hourly, annual, and monthly billing methods, and you can choose the appropriate payment method according to your needs.
- Total cost: The user selects a NAT gateway resource to display the cost according to the paid method.

## Viewing NAT Gateways
Enter the NAT Gateway resource console through the navigation bar to view the NAT gateway resource list, enter the details page through the name and ID on the list to view the overview and monitoring information of the NAT gateway, and switch to the Whitelist tab to manage the whitelist of the NAT gateway.
### List of NAT gateways
The NAT gateway list can view the resource information of all NAT gateways under the current account, including name, resource ID, VPC, subnet, security group, public IP address, creation time, expiration time, billing method, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-93.png)

- Name/ID: The name and globally unique identifier of the NAT gateway.
- Internet IP: The public IP address bound to the NAT gateway, and if the NAT gateway is bound to multiple public IP addresses, multiple public IP addresses are displayed.
- VPC network: The VPC network served by the NAT gateway, that is, the NAT gateway only provides SNAT and DNAT services for resources in the VPC, and only supports adding resources belonging to the VPC network as the source address of the SNAT rule and the destination address of the DNAT rule.
- Subnet: Represents only the subnet where the NAT gateway instance resides
- Status: The running status of the NAT gateway, including Creating, Running, and Deleting Status.
- Creation time/expiration time: refers to the creation time and fee expiration time of the current NAT gateway.
- Billing method: refers to the billing method specified when the current NAT gateway is created.

The operations on the list refer to operations on a single NAT gateway instance, including deleting and modifying security groups, and you can search and filter the NAT gateway resource list through the search box, supporting fuzzy search.

To facilitate tenants' resource statistics and maintenance, the platform supports downloading the list of all NAT gateway resources owned by the current user as an Excel table. You can also delete NAT gateways in batches.

### NAT Gateway Details
On the NAT gateway resource list, click Name to go to the overview page to view the details of the current NAT gateway instance, and switch to the SNAT rules, DNAT rules, and public IP management pages to manage the SNAT rules, DNAT rules, and bound public IP addresses of the current NAT gateway, as shown on the overview page:

![1](/assets/images/user-guide/user-guide-94.png)

(1) Basic information

For the basic information of the NAT gateway, including name, ID, VPC network, subnet, public IP address, security group, status, billing method, creation time, expiration time, and alarm template information, you can click the button on the right side of the alarm template to modify the alarm template associated with the NAT gateway.

(2) Monitoring information

The monitoring charts and information related to the NAT gateway instance, including the bandwidth of the network card in/out, the number of packets in/out of the network card, and the number of connections, can view the monitoring data of 1 hour, 6 hours, 12 hours, 1 day, and custom time.

(3) SNAT rules

SNAT rule management of NAT gateway, you can access virtual resources and egress IP management of the Internet through NAT gateway, including adding, viewing, modifying, and deleting SNAT rules, see SNAT rules.

(4) DNAT rules

DNAT rule management of NAT gateway, port mapping management of virtual resources in VPC through the external IP address of the NAT gateway, including adding, viewing, modifying, and deleting DNAT rules, see DNAT rules.

(5) External IP management

For details about the management of the public IP address of the NAT gateway, including the viewing, binding, and unbinding of the external IP address, see  Internet IP Management.

## Modifying the Alert Template
Modifying the alarm template configures the monitoring data of the NAT gateway, and triggers alarms when the NAT gateway-related indicators fail or exceed the metric thresholds, and notifies relevant personnel to handle the fault to ensure the network communication between the NAT gateway and services.

You can modify the alarm template through the action items on the NAT gateway details overview page, and select a new NAT gateway alarm template in the Modify Alarm Template wizard to modify it.

## Deleting a NAT Gateway
You can delete unwanted NAT gateway instances through the console or APIs, automatically unbind the bound public IP addresses, and clear the SNAT/DNAT rules and routing policies added by the NAT gateway.

![1](/assets/images/user-guide/user-guide-95.png)

After the NAT gateway is deleted, it is directly destroyed, so make sure that no traffic on the NAT gateway accesses the Internet, otherwise service access may be affected.

## Modifying Names and Remarks
Modify the name and comment of the NAT gateway resource to operate in any state. You can modify this by clicking the Edit button to the right of each NAT gateway name on the NAT gateway resource list page.

### Modifying Security Groups
The security group policy bound to the NAT gateway acts on the public IP address egressed by the NAT gateway and is used to restrict egress traffic through the NAT gateway. You can modify the security group of a NAT gateway, and you can modify it by using Modify Security Group in the NAT gateway list action item, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-75.png)

A NAT gateway can only bind one security group, and the modified security group takes effect immediately, and the platform restricts the traffic to and from the NAT gateway with a new security group policy, and users can view the modified security group information through the NAT gateway list and details.

## NAT Gateway Renewal
You can manually renew a NAT gateway, and the renewal operation applies only to the resource itself, and does not renew additional resources associated with the resource, such as bound public IP resources. After the additional associated resources expire, they are automatically unbound from the NAT gateway, and you need to renew the relevant resources in a timely manner to ensure normal service use.

When the NAT gateway is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the NAT gateway billing method is Hour, the renewal period can be selected from 1 to 24 hours. If the billing method of the NAT gateway is Monthly, you can select the renewal period from January to November. If the NAT gateway is billed as Annual, the renewal period is 1 to 5 years.

### Creating SNAT Rules
You create an SNAT rule to specify the IP address of the specified virtual resource to access the Internet, and the source address resource in the rule must be in the same VPC network as the NAT gateway. You can go to the Create SNAT Rule page in the SNAT Rules console on the NAT gateway details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-96.png)

On the wizard page, you need to specify the source address type, subnet, virtual machine, and public IP address of the SNAT rule, which is the type of resource that needs to be accessed through the NAT gateway. Subnet refers to the CIDR of the subnet that accesses the Internet through a NAT gateway; A virtual machine refers to the IP address of a virtual machine that accesses the public network through a NAT gateway; The public IP address refers to the egress IP address of the resource when accessing the Internet.

(1) Source address type: Specify the source address type of SNAT rules, including VPC level, subnet level, and virtual machine level.

- VPC level: All virtual machines under the VPC to which the current NAT gateway belongs can access the Internet through the NAT gateway, and all NAT gateways in a VPC can only specify one VPC-level SNAT rule.
- Subnet level: All virtual machines in the specified subnet can access the Internet through the NAT gateway, and the SNAT rule of the subnet has a higher priority than the rule that the source address type is VPC.
- VM Level: This means that the specified virtual machine can access the Internet through a NAT gateway, and the SNAT rules of the VM type have a higher priority than SNAT rules of the VPC and subnet type.

(2) Subnet: Only if the source address type is subnet, you can specify the subnet of the VPC to which the NAT gateway belongs, and only one SNAT rule can be created per subnet, such as VPC CIDR is `192.168.0.0/16`, and the subnet can be specified as `192.168.1.1/24`.

(3) Virtual machine: Only if the source address type is virtual machine, you can specify the virtual machine of the VPC to which the NAT gateway belongs, and only one SNAT rule can be created per virtual machine IP, such as VPC CIDR is `192.168.0.0/16`, and virtual machine can be specified as `192.168.1.2`.

(4) Internet IP: refers to the egress IP address specified when the source address of the current SNAT rule accesses the Internet, and only supports selecting the external IP bound to the NAT gateway; At the same time, you can specify the Internet IP address to ALL, which randomly selects an IP address from all external IP pools bound to the gateway to access the Internet on behalf of the source address resource.

After you add an SNAT rule, the platform automatically delivers the default route to the virtual machine specified in the rule, so that the virtual machine can access the Internet or IDC data center network through the NAT gateway, and you can view the route information automatically delivered by the NAT gateway in the Linux virtual machine through the `netstat -rn` command, and detect the connectivity with the external network in the virtual machine.

After an SNAT rule is added, the platform will only issue default routes to VMs without IPv4 default routes, and VMs with default routes will automatically access the Internet through the VM's own default routes.

### Viewing SNAT Rules
You can use SNAT Rules on the NAT gateway details page to view the list and information of SNAT rules that have been added to the current gateway, including resource ID, source address type, source address, public IP address, status, creation time, and action items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-97.png)

- Resource ID: The globally unique identifier for the SNAT rule that was added.
- Source Address Type: refers to the source address type of the current SNAT rule, such as subnet level and virtual machine level.
- Source address: The CIDR or IP address of the source address resource that currently refers to the SNAT rule:
  - If the source address type is VPC, the source address is the CIDR CIDR block and name of the VPC.
  - If the source address type is Subnet, the source address is the CIDR CIDR block and name of the specified subnet;
  - If the source address type is Virtual Machine, the source address is the specified virtual machine private IP address and name.
- Internet IP: The target public IP address of the current SNAT rule, if the Internet IP address is ALL, the destination egress of the current SNAT rule is all the public IP addresses bound to the current gateway.
- Status: refers to the status of the current SNAT rule, including Creating, Active, and Deleted.
- Creation Time: refers to the creation time of the current SNAT rule.

The action items on the list refer to the operations on a single SNAT rule, including modification and deletion, and the SNAT rule can be searched and filtered through the search box, supporting fuzzy search. At the same time, to facilitate the maintenance of resources by tenants, SNAT rules can be deleted in batches.

### Modifying SNAT Rules
If a user modifies the public IP address of an SNAT rule, the rule takes effect immediately after the rule is modified, and the source address of the current rule uses the new public IP address as an egress to access the Internet. As shown in the following figure:

![1](/assets/images/user-guide/user-guide-98.png)

When you modify the public IP address of an SNAT rule, you can select only the public IP address that is bound to the NAT gateway.

### Deleting SNAT Rules
You can delete SNAT rules, which are destroyed immediately after the rules are deleted, and the SNAT capability associated with the rules is immediately invalidated. You can delete SNAT rules on the console SNAT rule list and support batch deletion, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-99.png)

During the deletion process, the status of the SNAT rule is Deleting and the SNAT rule is cleared on the list. Deleting an SNAT rule does not affect the normal operation of the virtual machine itself, and the automatically delivered route is cleared, that is, the Internet cannot be accessed through the NAT gateway, and the Internet can be accessed by adding SNAT rules again or binding a public IP address.