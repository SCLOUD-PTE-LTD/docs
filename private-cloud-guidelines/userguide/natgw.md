---
layout: default
title: NAT Gateway
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/nat-gateway/
nav_order: 12
---
# 12. NAT Gateway

## 12.1 Introduction to NAT Gateway

### 12.1.1 Overview

NAT Gateway is a VPC gateway similar to the [NAT](https://zh.wikipedia.org/wiki/网络地址转换) protocol. It provides SNAT and DNAT proxy for cloud platform resources, supporting internet or physical network address translation capabilities. The NAT Gateway service of the platform realizes the SNAT forwarding and DNAT port mapping functions of virtual resources in the VPC through SNAT and DNAT rules.

* SNAT rule: SNAT capability is implemented at VPC level, subnet level, and virtual resource instance level through the SNAT rule, enabling resources of different dimensions to access the external network through the NAT Gateway.
* DNAT rule: With DNAT rules, port forwarding based on TCP and UDP protocols can be configured to map the internal network ports of cloud resources in the VPC to the external IP bound to the NAT Gateway, providing services to the internet or IDC data center networks.

As a virtual gateway device, NAT Gateway needs to bind an external IP as the SNAT rule exit and DNAT rule entry. NAT Gateway has regional (data center) properties and only supports SNAT and DNAT forwarding services for the same VPC virtual resources under the same data center.

The network that virtual machines can access through NAT Gateway depends on the configuration of the network segment to which the bound external IP belongs in the physical network. If the bound external IP can reach the internet, virtual machines can access the internet through NAT Gateway; if the bound external IP can reach the physical network of the IDC data center, virtual machines access the physical network of the IDC data center through NAT Gateway.

### 12.1.2 Use Cases

When users deploy application services using virtual machines on the platform, there are scenarios where they need to access the external network or access virtual machines through the external network. Usually, we bind an external IP to each virtual machine for communication with the internet or IDC data center network. In real environments and cases, it may not be possible to allocate sufficient public IPs, and even if there are enough public IPs, it is not necessary to bind an external IP address to every virtual machine that needs to access the external network.

- Share EIP: Multiple internal virtual machines in VPC share external IP addresses to access the internet or the physical network of the IDC data center through SNAT proxy.
- Shield real IP: Through SNAT proxy, multiple internal virtual machines in VPC communicate using a proxy IP address, automatically shielding the real internal network address.
- Internal virtual machines in VPC provide external network services: Configure IP and port forwarding through DNAT proxy to provide business services to the network of the internet or IDC data center.

### 12.1.3 Architecture Principles

The underlying resources of the platform's product services are unified, and the NAT gateway instances are deployed in a primary/standby high-availability cluster architecture, which can achieve automatic failover in case of NAT gateway failures, improving the availability of SNAT and DNAT services. At the same time, combined with external IP addresses, SNAT and DNAT proxy services are provided to tenant virtual resources according to SNAT and DNAT rules.

At the product level, tenants apply for a NAT gateway and specify the VPC network that the NAT gateway can allow communication with. By binding an external IP address, virtual machines under multiple subnets can communicate with the internet or IDC data center physical networks. The specific logical architecture diagram is shown below:

![natgw](/assets/images/userguide/natgw.png)

- The platform supports using NAT gateways to access the internet or IDC data center networks for virtual machines under multiple subnets of the same VPC.
- When virtual machines in multiple subnets that are not bound with an external IP address are associated with a NAT gateway, the platform will automatically issue routes for accessing the internet in these virtual machines.
- The virtual machine transmits the data accessing the internet through the NAT gateway by the issued route to the bound external IP address.
- The data transmitted to the external IP is sent to the physical switch via the platform OVS and physical network card to complete the communication of SNAT.
- When the internet needs to access virtual machines in the VPC, it can use port forwarding on the NAT gateway to enable the physical network of the internet or IDC to access the internal network service of the VPC through the IP address and port bound to the NAT gateway.

### 12.1.4 Functional Features

The cloud platform provides high-availability NAT gateway services and supports full lifecycle management of the gateway, including external IP addresses, SNAT rules, DNAT port forwarding, monitoring, and alarms. At the same time, it provides network and resource isolation security guarantees for NAT gateways.

A VPC can create up to 20 NAT gateways, and the SNAT rules of all NAT gateways in the same VPC cannot be repeated, that is, the SNAT rules of the 20 NAT gateways cannot be repeated. Examples are as follows:

* When a subnet's (192.168.0.1/24) SNAT rule is created in NATGW (VPC: 192.168.0.0/16), the SNAT rule of the same subnet (192.168.0.1/24) as the source address cannot be created for NATGWs in the same VPC. The subnet rule can only be created after deleting it from NATGW01.
* When a VPC-level rule is created in NATGW (VPC: 192.168.0.0/16), a VPC-level rule cannot be created in the same VPC.
* When a virtual machine's (192.168.1.2) SNAT rule is created in NATGW (VPC: 192.168.0.0/16), the SNAT rule of the same virtual machine (192.168.1.2) as the source address cannot be created for NATGWs in the same VPC.

#### 12.1.4.1 SNAT Rule

The NAT gateway supports the SNAT (Source Network Address Translation) feature through SNAT rules, where each rule consists of a source address and a destination address. This means that the source address is translated into the destination address for network access. The platform's SNAT rules support a variety of scenarios for outbound connections, where the source address can be one of three types: VPC, subnet, or virtual machine.

- VPC level: All virtual machines in the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.
- Subnet level: All virtual machines in the designated subnet to which the NAT gateway belongs in the VPC can access the Internet through the NAT gateway.
- Virtual machine level: Only the designated virtual machines in the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.

The target address of the rule is the external IP address bound to the NAT gateway, allowing the source address of the virtual machine's IP address to be translated into the external IP address of the gateway for network communication. This enables virtual machines to communicate with the external network without binding an external IP address, such as accessing IDC data center networks or the internet.

SNAT rules have different priorities based on the type of source address, with higher priority given to rules with VPC source addresses:

**(1) Source address is VPC**

- All virtual machines in the VPC to which the NAT gateway belongs can access the Internet through the NAT gateway.

**(2) Source address is subnet CIDR**

- Only one SNAT rule can be created per subnet and duplicates are not allowed.
- Supports configuring SNAT rules for individual virtual machines under the subnet, which take precedence over SNAT rules with VPC source addresses.

**(3) Source address is virtual machine IP**

- Virtual machines can access the Internet through the NAT gateway.
- Only one SNAT rule can be created per virtual machine IP and duplicates are not allowed.
- SNAT rules with virtual machine IP source addresses have higher priority than those with subnet source addresses.

> A NAT gateway can create up to 100 SNAT rules by default.

After configuring SNAT rules, the NAT gateway automatically issues a default route to the virtual machine that matches the source address, allowing the virtual machine to access the Internet through the external IP address specified in the SNAT rule. The communication logic is as follows:

- If the virtual machine is not bound to an IPv4 external IP address, it will automatically use the NAT gateway to access the Internet.
- If the virtual machine is bound to an IPv4 external IP address and has a default network egress, it will use the virtual machine's default network egress to access the Internet.
- If the virtual machine is bound to an IPv4 external IP address but has no default network egress, it will use the NAT gateway to access the Internet.

When a virtual machine accesses the Internet through the NAT gateway, the external IP address used depends on the configuration of the SNAT rule and is used as the virtual machine's egress.

#### 12.1.4.2 DNAT Rules

The NAT gateway supports DNAT (Destination Network Address Translation), also known as port forwarding or port mapping, which translates the IP address of the external network into the IP address of the VPC subnet to provide network services.

- Supports port forwarding for both TCP and UDP protocols and supports lifecycle management of port forwarding rules.
- It supports batch configuration of multi-port forwarding rules, i.e., it supports mapping port segments, such as TCP:1024~TCP:1030.
- When the NAT gateway is bound to an external IP, the port forwarding rule provides Internet extranet services for the virtual machines in the VPC subnet, and the services of the virtual machines in the subnet can be accessed through the external network.

#### 12.1.4.3 Monitoring and Alerting

The platform supports collecting and displaying monitoring data of NAT gateways, displaying the indicator data of each NAT gateway through monitoring data, and supporting setting threshold alarms and notification policies for each monitoring indicator. The supported monitoring metrics include network out/bandwidth, network out/packet volume, and connections.

Support to view monitoring data of a NAT gateway in multiple time dimensions, including 1 hour, 6 hours, 12 hours, 1 day, 7 days, 15 days and custom time monitoring data. The default query number is 1 hour of data, and you can view up to 1 month of monitoring data.

#### 12.1.4.4 NAT Gateway High Availability

NAT gateway instances support a high availability architecture, i.e., they are built from at least two virtual machine instances and support dual hot standby. When a NAT gateway instance fails, it supports automatic online switchover to another virtual machine instance to ensure normal NAT proxy service. It also supports the availability of SNAT gateway egress and DNAT entry when the physical machine is down based on the drift feature of the external IP address.

### 12.1.5 NAT gateway security

The network access control of the NAT gateway can be associated with security groups to give security assurance. The rules of the security groups can control the inbound and outbound traffic reaching the external IP bound to the NAT gateway, and support filtering and control of TCP, UDP, ICMP, GRE and other protocol packets.

Security groups and rules for security groups support restricting traffic to NAT gateways that have security groups associated with them, allowing only the traffic within the security group rules to pass through the security group to its destination. To ensure the resource and network security of NAT gateways, the platform provides resource isolation and network isolation mechanisms for NAT gateways:

* Resource isolation
  * NAT gateways have data center attributes, and NAT gateway resources are physically isolated between different data centers;
  * NAT gateway resources are isolated from each other among tenants, and tenants can view and manage all NAT gateway resources under their accounts and sub-accounts; * NAT gateway resources within a tenant are isolated from each other;
  * NAT gateway resources within a tenant only support binding VPC subnet resources of the same data center within the tenant;
  * NAT gateway resources within a tenant only support binding to the external IP resources of the same data center within the tenant; * NAT gateway resources within a tenant only support binding to the external IP resources of the same data center within the tenant;
  * A NAT gateway resource within a tenant supports binding only the security group resources of the same data center within the tenant.
  
* Network Isolation
  * Physical isolation of NAT gateway resource networks between different data centers;
  * NAT gateway networks in the same data center are isolated by VPCs, and NAT gateway resources of different VPCs cannot communicate with each other;
  * The isolation of the external IP network bound by the NAT gateway depends on the configuration of the user's physical network, such as different Vlan, etc.

## 12.2 Usage Procedure

Before using the NAT gateway service, you need to plan the VPC network and external IP network of the NAT gateway according to the business requirements, and configure the SNAT and DNAT rules according to the business requirements. The specific process is as follows:

1. the tenant creates VPCs and subnets according to the requirements, and creates virtual machines in multiple subnets;
2. the tenant creates an external IP address according to the requirement and specifies the network type, associated subnet and bound egress IP address through API or console to create a NAT gateway;
3. add SNAT rules for VPC, subnet or VM type through SNAT rules, then the associated VMs can access the external network through the NAT gateway;
4. configure rules for virtual machines that need to be served externally through DNAT rules, so that the external network can access resources in the VPC network that are not bound to an external IP;
5. if you need to restrict the incoming and outgoing traffic of the NAT gateway, you can configure it through the security group bound to the NAT gateway;
You can bind the external IP address for the NAT gateway and configure SNAT rules and DNAT rules for the external IP.

## 12.3 Creating a NAT Gateway

To create a NAT gateway in the platform, users need to specify the model, VPC network, subnet, external IP, security group, NAT gateway name and comment information. A VPC is allowed to create 20 NAT gateways, and the SNAT rules in all NAT gateways under the same VPC cannot be duplicated, that is, the SNAT rules in 20 NAT gateways are not allowed to be duplicated.

Users can enter the [NAT Gateway] resource console through the navigation bar, and enter the creation wizard page through "Create NAT Gateway", as shown in the following figure:

![createnat](/assets/images/userguide/createnat.png) 

1. Select and configure the NAT gateway base configuration and network settings information: 1:

- Cluster: The cluster type of the node where the NAT gateway instance is located is customized by the platform administrator, such as x86 machine and ARM machine, and the instance created by ARM machine is the ARM version of NAT gateway instance, which has been adapted to domestic chips, servers and operating systems.
- Name/Remarks: The name of the NAT gateway and the notes information.
- Model: NAT gateway supports standalone version and master/backup version.
- VPC Network: The VPC network served by the NAT gateway, that is, the NAT gateway only provides SNAT and DNAT services for the resources in the selected VPC, and only supports adding the resources of the VPC network it belongs to as the source address of SNAT rules and the destination address of DNAT rules.
- Subnet: The subnet where the NAT gateway instance is located. It is recommended to select a subnet with sufficient number of available IPs.
- External IP: The external IP address used for the NAT gateway address. All resources bound in the VPC network access the Internet or the IDC physical network through the external IP address bound by the NAT gateway, and only the external IP address bound with the default route is supported.
- Security Group: The security group used by the external IP address of the NAT gateway to control the traffic that can enter the NAT gateway.
- Project group: Set the project to which the instance belongs, default is default.
- Label: Select the corresponding resource label for easy management. 

2. After selecting and configuring the above information, you can select the purchase quantity and payment method, confirm the order amount and click "Buy Now" to create the NAT gateway:

- Purchase quantity: Create NAT gateway instances in batch according to the selected configuration and parameters, only 1 NAT gateway instance can be created at a time.
- Payment method: Select the billing method of the NAT gateway, which supports hourly, yearly and monthly, you can choose the suitable payment method according to your needs.
- Total cost: The user selects the NAT gateway resources to display the cost according to the payment method.

After confirming the order is correct, click Buy Now. After clicking Buy Now, it will return to the NAT gateway resource list page, in which you can view the creation process of NAT gateway.

> Allow you to create multiple NAT gateways under one VPC and add the virtual machines under the VPC to multiple NAT gateways in batches to achieve NAT gateway triage, which can cope with the scenario that a large number of virtual machines share the external IP address to access the external network.

## 12.4 Viewing NAT Gateways

Enter the NAT gateway resource console through the navigation bar, you can view the NAT gateway resource list, and enter the detail page by the name and ID on the list to view the overview and monitoring information of NAT gateways, and switch to the whitelist tab to manage the whitelist of NAT gateways.

### 12.4.1 NAT Gateway List

The NAT gateway list can view the resource information of all NAT gateways under the current account, including name, resource ID, VPC, subnet, security group, external IP, creation time, expiration time, billing method, status and operation items, as shown in the following figure:

![natlist](/assets/images/userguide/natlist.png)

- Name/ID: The name and globally unique identifier of the NAT gateway.
- External IP: The IP address of the external network to which the NAT gateway is bound.
- VPC Network: The VPC network served by the NAT gateway, i.e., the NAT gateway only provides SNAT and DNAT services for the resources in the VPC, and only supports adding the resources of the VPC network it belongs to as the source address of the SNAT rule and the destination address of the DNAT rule.
- Subnet: Only represents the subnet where the NAT gateway instance is located
- Status: The operational status of the NAT gateway, including being created, running, deleted, etc.
- Creation Time/Expiration Time: The creation time and cost expiration time of the current NAT gateway.
- Billing method: The billing method specified when the current NAT gateway was created.

Operation item on the list refers to the operation of a single NAT gateway instance, including deletion and modification of security groups, etc. The NAT gateway resource list can be searched and filtered by the search box, and fuzzy search is supported.

To facilitate tenants' statistics and maintenance of resources, the platform supports downloading all the NAT gateway resource list information owned by the current user as an Excel table; it also supports batch deletion operations for NAT gateways.

### 12.4.2 NAT Gateway Details

On the NAT gateway resource list, click "**Name**" to enter the overview page to view the detailed information of the current NAT gateway instance, and switch to the SNAT rule, DNAT rule and external IP management page to manage the SNAT rule, DNAT rule and bound external IP management of the current NAT gateway respectively, as shown in the overview page: ### 12.4.2 NAT gateway details IP management, as shown in the overview page:

![natdetails](/assets/images/userguide/natdetails.png)

**(1) Basic information**

Basic information of the NAT gateway, including name, ID, VPC network, subnet, external IP, security group, status, billing method, creation time, expiration time and alarm template information, you can click the button on the right of the alarm template to modify the alarm template associated with the NAT gateway.

**(2) Monitoring Information**

Monitoring charts and information related to NAT gateway instance, including NIC in/out bandwidth, NIC in/out packet volume and connection number, supports viewing monitoring data of 1 hour, 6 hours, 12 hours, 1 day and custom time.

**(3) SNAT Rules**

SNAT rule management of NAT gateway, i.e., virtual resources and egress IP management for accessing external network through NAT gateway, including SNAT rule adding, viewing, modifying and deleting operations, see [SNAT Rules](#_1210-SNAT-Rules) for details.

**(4) DNAT Rules**

DNAT rule management of NAT gateway, i.e., port mapping management for accessing virtual resources in VPC through external IP of NAT gateway, including adding, viewing, modifying and deleting DNAT rules, see [DNAT Rules](#_1211-DNAT-Rules) for details.

**(5) External IP Management**

The external IP management of NAT gateway, i.e. the external IP address management of the bound to NAT gateway, including the external IP view, binding and unbinding operations, see [External IP Management](#_1212-external-IP-management) for details.

## 12.5 Modify Alarm Template

Modify Alarm Template is the configuration of alarming the monitoring data of NAT gateway. Through the indicators and thresholds defined in the alarm template, an alarm can be triggered when the relevant indicators of NAT gateway fail or exceed the indicator thresholds to notify relevant personnel for troubleshooting and ensure the network communication of NAT gateway and services.

Users can modify the alarm template through the operation item on the NAT gateway details overview page, and select the new NAT gateway alarm template for modification in the Modify Alarm Template wizard.

## 12.6 Deleting NAT gateways

Users can delete unwanted NAT gateway instances through the console or API, and the bound external IP addresses will be automatically unbundled and the added SNAT/DNAT rules and routing policies of the NAT gateway will be cleared.

![rmnatgw](/assets/images/userguide/rmnatgw.png)

The NAT gateway is destroyed directly after it is deleted. Please make sure that there is no service traffic accessing the external network for the NAT gateway before deleting it, otherwise it may affect the service access.

## 12.7 Modify Name and Remarks

Modifying the name and comments of a NAT gateway resource can be done in any state. You can modify them by clicking the "Edit" button to the right of each NAT gateway name on the NAT gateway resource list page.

## 12.8 Modifying Security Groups

The security group policy bound to a NAT gateway applies to the external IP of the NAT gateway egress and is used to restrict traffic through the NAT gateway egress. Users can modify the security group of the NAT gateway through "**Modify Security Group**" in the operation item of the NAT gateway list, as shown in the following figure:

![natgwbindsg](/assets/images/userguide/natgwbindsg.png)

A NAT gateway only supports binding one security group, and the modified security group takes effect immediately. The platform will restrict the traffic to and from the NAT gateway with the new security group policy, and users can view the modified security group information through the NAT gateway list and details.

## 12.9 NAT Gateway Renewal

The user can renew the NAT gateway manually, and the renewal operation is only for the resource itself, not for the additional resources associated with the resource, such as the bound external IP resources. After the expiration of the additional resources, they will be unbundled from the NAT gateway automatically. To ensure the normal use of the service, you need to renew the relevant resources in time.

![renewnatgw](/assets/images/userguide/renewnatgw.png)

The renewal method of NAT gateway can be changed from short period to long period only, for example, the monthly renewal method can be changed to monthly or yearly.

When the billing method of NAT gateway is [Hourly], the renewal time is specified as 1 hour; when the billing method of NAT gateway is [Monthly], the renewal time can be selected from 1 to 11 months; when the billing method of NAT gateway is [Yearly], the renewal time is from 1 to 5 years.

## 12.10 SNAT-Rules

The NAT gateway supports SNAT (Source Network Address Translation) capability through SNAT rules, each rule consists of source address and destination address, which translates the source address into the destination address for network access.

The platform SNAT rules support various scenarios of outbound network scenarios, that is, the source address includes three types of VPCs, subnets and virtual machines. Usually, you only need to specify a SANT rule of VPC type to realize the ability of all virtual machines under the VPC network to which the NAT gateway belongs to access the external network.

The SNAT rule only supports SNAT capability and does not restrict the DNAT port forwarding capability, that is, the virtual machine added to the SNAT rule can access the external network or the physical IDC network through the NAT gateway; if the virtual machine needs to provide service services to the external network at the same time, the DNAT rule can be configured for the virtual machine at the same time.

### 12.10.1 Create SNAT rule

The user creates a SNAT rule to specify the IP address of the specified virtual resource to access the external network. The source resource in the rule must be in the same VPC network as the NAT gateway. Users can access the rule addition wizard page through "Create SNAT Rule" in the "SNAT Rule" console on the NAT gateway details page, as shown in the following figure:

![adsnat](/assets/images/userguide/addsnat.png)

In the wizard page, users need to specify the source address type, subnet, virtual machine and extranet IP of the SNAT rule. The source address is the type of resource that needs to access the extranet through the NAT gateway; the subnet is the subnet CIDR that accesses the extranet through the NAT gateway; the virtual machine is the IP address of the virtual machine that accesses the extranet through the NAT gateway; the extranet IP is the egress IP address when the resource accesses the extranet.

**(1) Source Address Type**: Specify the source address type of the SNAT rule, including VPC level, subnet level, and VM level, and a rule supports only one type of rule.

* VPC level: All virtual machines under the VPC to which the current NAT gateway belongs can access the external network through the NAT gateway, and all NAT gateways under a VPC support only one VPC level SNAT rule.
* Subnet level: All virtual machines under the currently specified subnet can access the external network through the NAT gateway, and the subnet SNAT rule has a higher priority than the rule whose source address type is VPC.
* Virtual machine level: means that the currently specified virtual machines can access the external network through the NAT gateway, and the SNAT rules of the virtual machine type have higher priority than the SNAT rules of the VPC and subnet types.

**(2) Subnet**: Specified only when the source address type is subnet, you can specify the subnet of the VPC to which the NAT gateway belongs, and only one SNAT rule can be created for each subnet, such as VPC CIDR is 192.168.0.0/16, the subnet can be specified 192.168.1.1/24.

**(3) Virtual Machine**: Specified only when the source address type is a virtual machine, you can specify the virtual machine of the VPC to which the NAT gateway belongs, and only one SNAT rule can be created for each virtual machine IP, such as VPC CIDR is 192.168.0.0/16, the virtual machine can be specified 192.168.1.2.

**(4) External IP**: The IP address of the exit specified when the source address of the current SNAT rule accesses the external network, and only the external IP bound to the NAT gateway is supported.

After the SNAT rule is successfully added, the platform will automatically send the default route to the virtual machine specified in the rule, so that the virtual machine can access the Internet or IDC data center network through the NAT gateway.

> After the SNAT rule is successfully added, the platform will only issue default routes to virtual machines without IPv4 default routes, and virtual machines with default routes will automatically access the external network through the virtual machines' own default routes.

### 12.10.2 Viewing SNAT Rules

Users can view the list and information of SNAT rules added to the current gateway through "SNAT Rules" on the NAT gateway details page, including resource ID, source address type, source address, external IP, status, creation time and operation items, as shown in the following figure:

![snatlist](/assets/images/userguide/snatlist.png)

- Resource ID: The globally unique identifier of the added SNAT rule.
- Source Address Type: The source address type of the current SNAT rule, such as subnet level, virtual machine level.
- Source Address: The CIDR or IP address of the resource that is currently the source address of the SNAT rule:
  - When the source address type is VPC, the source address is the CIDR segment and name of the VPC;
  - When the source address type is Subnet, the source address is the CIDR segment and name of the specified subnet;
  - When the source address type is virtual machine, the source address is the specified intranet IP and name of the virtual machine.
- Extranet IP: The target extranet IP address of the current SNAT rule.
- Status: The status of the current SNAT rule, including being created, available, and deleted.
- Creation Time: The creation time of the current SNAT rule.

The operation item on the list refers to the operation of a single SNAT rule, including modification and deletion, and the SNAT rule can be searched and filtered by the search box, which supports fuzzy search. It also supports bulk deletion of SNAT rules for the convenience of tenant maintenance of resources.

### 12.10.3 Modify SNAT Rules

The user modifies the external IP address of an SNAT rule, and the rule takes effect immediately after modification, and the source address of the current rule will use the new external IP address as the outlet when accessing the external network. As shown in the following figure:

![updatesnat](/assets/images/userguide/updatesnat.png)
When modifying the external IP of the SNAT rule, only the external IP address that has been bound to the NAT gateway is supported.

### 12.10.4 Delete SNAT rule

The rule will be destroyed immediately after deletion, and the SNAT capability associated with the rule will be invalidated immediately. SNAT rules can be deleted from the console SNAT rule list, and bulk deletion is supported, as shown in the following figure:

![rmsnat](/assets/images/userguide/rmsnat.png)

During the deletion process, the status of the SNAT rule is [Deleting], and the deletion is successful when the SNAT rule is cleared from the list. The deletion of SNAT rules does not affect the normal operation of the virtual machine itself, and the automatically issued routes will be cleared, that is, you cannot access the external network through the NAT gateway, you can access the external network by adding SNAT rules again or binding the external IP address.

## 12.11 DNAT-Rules

DNAT rule is the entry point for NAT gateway to provide DNAT service, which supports both TCP and UDP forwarding protocols. Users can configure port mapping for the NAT gateway through port forwarding to map the internal ports of virtual machines in the VPC subnet to the external IPs of the NAT gateway so that virtual machines can provide services to the external network.

Each rule consists of five elements: protocol, source IP (external IP), port, destination IP (virtual machine IP), and destination port, which means that the port request of the source IP is forwarded to the port of the destination IP, so that users can directly access the services provided by the virtual machines on the VPC intranet through the source IP address.

### 12.11.1 Adding DNAT Rules

The user adds a forwarding rule for a NAT gateway to support DNAT proxy. After the forwarding rule is successfully added, if the service is running normally on the destination IP virtual machine, the user can access the application services provided by the destination IP virtual machine through the external IP bound to the NAT gateway.

To add a forwarding rule, users need to specify the NAT gateway name, protocol, source IP, source port, destination IP, destination port and multi-port, and can add DNAT rules through the [DNAT Rules] list in the NAT gateway details, as shown in the following figure:

![adddnat](/assets/images/userguide/adddnat.png)

* Protocol: The forwarding protocol of the DNAT port forwarding rule, supports TCP and UDP, must be specified when creating, default is TCP.
* Source IP: The source IP address of the DNAT port forwarding rule, i.e. the external IP bound by the NAT gateway, a rule only supports one external IP.
* Source Port: The source port of the DNAT port forwarding rule, i.e. the port exposed by the external IP bound by the NAT gateway.
  * You must specify the source port when creating, and the port range is 1~65535.
  * Only the uncreated source port is supported. Duplicate source port rules are not supported under the same protocol.
* Destination IP: The destination IP of the DNAT port forwarding rule, i.e., the intranet IP address of the virtual machine in the VPC network to which the NAT gateway belongs.
  * The destination IP address must be specified when creating, and only the IP address of the virtual machine under the VPC to which the NAT gateway belongs is supported.
  * The destination IP address is not restricted by SNAT rules, that is, a virtual machine can add both SNAT rules and DNAT rules.
* Destination Port: The destination port of the DNAT port forwarding rule, i.e. the port on which the destination IP virtual machine provides services to the outside world.
  * The destination port must be specified when creating, and the port range is 1~65535.
  * The destination port can be the same as or different from the source port, for example, the source port is TCP:80 and the destination port is TCP:8080, which means the TCP 80 port traffic of the source IP address will be forwarded to the destination IP address TCP 8080 port.
  * Support creating duplicate destination ports with the same destination IP, such as forwarding both port 80 of the source address to the same port of the same destination address for business data processing.

* Port range: DNAT rules also support multi-port mapping rules, that is, they support specifying the source port as a continuous range, such as 1024~1030; when specifying the port range, the number of destination port ranges must be the same as the source port.

In the same protocol case, duplicate source port rules are not supported. After the DNAT rule is successfully added, users can access the application services provided by the destination virtual machine through the source IP address.

### 12.11.2 Viewing DNAT Rules

Users can view the information of the added port forwarding rule list, including the forwarding rule protocol, source IP, source port, destination IP, destination port, status and action items, as shown in the following figure:

![dnatlist](/assets/images/userguide/dnatlist.png)

* Protocol: The protocol of the current DNAT rule, such as TCP or UDP.
* Source IP: The source IP address of the current DNAT rule, i.e. the IP of the external network specified by the current rule.
* Source Port: The source port of the source IP address of the DNAT rule.
* Destination IP: The destination IP address of the DNAT rule port forwarding, that is, the IP of the virtual machine specified by the current rule.
* Destination Port: The destination port of the destination IP address of the DNAT rule, i.e. the port where the traffic is finally processed.
* Status: The status of the current DNAT rule, including being created, available, and being deleted.

The operation item on the list refers to the operation of a single DNAT rule, including modification and deletion; meanwhile, the bulk deletion operation of DNAT rules is supported for the convenience of the tenant's maintenance of resources.

### 12.11.3 Modify DNAT Rule

Users modify the added DNAT rules, including protocol, source IP, source port, destination IP and destination port, as shown in the following figure:

![updatedant](/assets/images/userguide/updatednat.png)

The user must modify the source IP to be the bound external IP address in the NAT gateway, and the modification will take effect immediately.

### 12.11.4 Deleting DNAT Rules

The user can delete one or more DNAT forwarding rules, and the deletion takes effect immediately, and the service services of the target IP address cannot be accessed through the external IP address specified by the DNAT rule. As shown in the following figure:

![rmdnat](/assets/images/userguide/rmdnat.png)

Support deleting multiple forwarding rules in batch, the rules will be destroyed after being deleted, so you need to operate carefully before deleting.

## 12.12 external-IP-management

NAT gateway supports binding 50 IPv4 external IP addresses of default route type to provide a shared pool of external IP resources for virtual resources of specified subnets of NAT gateway to provide more flexible and convenient SNAT and DNAT capabilities.

Users can view the bound external IP addresses and information of the NAT gateway through the external IP management, and support binding and unbinding operations on the external IP of the NAT gateway.

### 12.12.1 View Network

Users can view the network information of the NAT Gateway, including the associated VPC, subnet, attached security group, as well as the resource ID, IP address, bandwidth, status and operation options of the bound internal and external IPs, on the [Network] tab of the NAT Gateway details page, as shown in the following figure:

![nateiplist](/assets/images/userguide/nateiplist.png)

* Resource ID: The globally unique identifier of the EIP that is currently bound to the NAT gateway.
* IP Address: The IP address.
* Bandwidth: The current bandwidth of the IP address in Mb.
* Status: The current status of the IP address, including Already bound, binding, and not bind.

The operation item on the list refers to the unbinding operation of a single external IP address, and supports the batch unbinding operation of bound external IP addresses for the convenience of the tenant's maintenance of resources.

### 12.12.2 Binding External IPs

Support users to bind 50 IPv4 external IP addresses of default route type for the NAT gateway to provide a shared external IP pool for the specified virtual resources of the NAT gateway, and provide flexible and convenient SNAT and DNAT capabilities. The specific binding operation is shown in the following figure:

![natbondeip](/assets/images/userguide/natbondeip.png)

After successful binding, when you add SNAT and DNAT rules, you can select the binding IP address as the external IP of SNAT or the source IP address of DNAT. **Note: Binding IPv6 and non-default route type IPv4 external IP address is not supported.** 

### 12.12.3 Unbind External IP

After unbinding, the associated SNAT rules and DNAT rules will be disabled.

- After unbundling, the external IP will be automatically cleared from the external IP pool of SNAT rules.
- After unbundling, the external IPs are automatically cleared from the source IPs of the DNAT rule.

![natunbondeip](/assets/images/userguide/natunbondeip.png)

Users can set the new egress IP and ingress source IP address by modifying the SNAT and DNAT rules respectively.


### 12.13 Upgrade Models

Support users to upgrade the standalone NAT gateway to the main backup version.

![updatenatmachine](/assets/images/userguide/updatenatmachine.png)

### 12.14 Modify Tags

Support users to modify NAT gateway labels.

![updatenattag](/assets/images/userguide/updatenettag.png)



