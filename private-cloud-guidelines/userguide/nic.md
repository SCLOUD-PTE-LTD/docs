---
layout: default
title: Elastic Network Interface
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/nic/
nav_order: 5
---
# 5. Elastic Network Interface

Elastic Network Interface (ENI) is an elastic network interface that can be attached to a virtual machine at any time, supporting binding and unbinding, flexible migration between multiple virtual machines, providing high availability cluster building capabilities for virtual machines, and enabling fine-grained network management and inexpensive failover solutions.

Both the elastic network interface and the default network interface (one internal network interface and one external network interface) provided by the virtual machine are virtual network devices for network transmission. They are divided into two types: internal network interface and external network interface, and both will allocate IP addresses, gateways, subnet masks, and routing-related network information from the corresponding networks they belong to.

* Elastic network interfaces of the internal network type belong to VPC and subnets, and IP addresses are automatically or manually allocated from the VPC.
* Elastic network interfaces of the external network type belong to the external network segment and will automatically or manually allocate IP addresses from the external network segment. The allocated IP address is consistent with the lifecycle of the elastic network interface and will only be released when the elastic network interface is destroyed.
* When the network interface type is external, the network interface will be billed based on the bandwidth specifications of the selected external IP. Users can choose the appropriate payment method and purchase duration according to their business needs.

ENI has a regional (data center) property and only supports binding to virtual machines in the same data center. **An ENI can only be bound to one virtual machine. x86 architecture virtual machines support binding up to 6 ENIs, while ARM architecture virtual machines support binding up to 3 network interfaces.** After an ENI is bound to a virtual machine on the public network, it does not affect the default network egress policy of the virtual machine, including the external IP address bound to the ENI on the virtual machine. The first IP address with a default route will be used as the default network egress for the virtual machine. Users can set a specific external IP address with a default route as the default network egress for the virtual machine.

Each ENI supports assigning only one IP address and can be bound to a security group as needed to control traffic in and out of the ENI, achieving fine-grained network security control. If traffic control for the ENI is not needed, the security group for the ENI can be left empty.

Users can create network interfaces and perform related operations such as binding, unbinding, and modifying security groups through the platform. For ENIs on the public network, users can also adjust the bandwidth limit of the external IP address on the ENI.

## 5.1 create ENI

Cloud platform users can create an ENI through API interfaces or the console to expand the network interface of a virtual machine. Before creating an ENI, the account must have at least one VPC network and subnet or public network segment. Enter the virtual machine console through the navigation bar, switch to the "Elastic Network Interface" network interface management page, and click the "**Create Network interface**" button to enter the ENI creation wizard dialog box. The following are schematic diagrams for creating internal and external network interfaces:

![createnic](/assets/images/userguide/createnic.png)

![createnic2](/assets/images/userguide/createnic2.png)

* Name: The name and identification of the ENI to be created.
* Network interface Type: The type of ENI, including internal network interfaces and external network interfaces, which assign IP addresses from VPC networks and public network segments, respectively.
* Associated Network: The network to which the ENI belongs must be specified when it is created.
  * The ENI of the internal network type belongs to the VPC and subnet, and the IP address is automatically or manually assigned from the VPC. When created, a subnet with a sufficient number of available IP addresses should be specified.
  * The ENI of the external network type belongs to the public network segment and will automatically or manually assign an IP address from the public network segment. When created, a public network segment with a sufficient number of available IP addresses should be specified, and the bandwidth limit of the external IP address should be set up.
* IP Address: The IP address of the current network interface is automatically assigned from the IP address range of the belonging network by default. If you need to customize the IP address, you can enter the specified IP address in the IP Address field.
* Security Group: The security group that the current network interface needs to bind to controls the network traffic in and out of the ENI. The option to not bind a security group is also supported.
  
> The security group bound to the ENI does not affect the internal/external network security group bound to the virtual machine. The security group bound to the ENI only controls the network traffic associated with the ENI.

When creating an external network ENI, it will be billed according to the bandwidth specification of the external IP address. Users can choose the appropriate payment method and purchase duration as needed. The status of the ENI when created is "Creating." When the status changes to "Unbound," it means the network interface has been created successfully and can be bound to a virtual machine, and the security group of the ENI can be modified.

## 5.2 Viewing Network Interface Cards (NICs)

You can view a list of Elastic Network Interfaces (ENIs) and related information, including the name, resource ID, status, type, associated network, IP address, bound resources, security groups, project groups, creation time, and operations, by navigating to the NIC management page in the virtual machine console, as shown in the following figure:

![niclist](/assets/images/userguide/niclist.png)

* Name/Resource ID: The name and globally unique identifier of the ENI.
* Type: The type of ENI, which can be either an internal or external network interface, corresponding to VPC networks and public networks, respectively.
* Associated Network: The network to which the ENI belongs. For internal ENIs, the associated network is the specified VPC and subnet; for external ENIs, the associated network is the specified public network and subnet.
* IP Address: The IP address assigned to the ENI from the associated network, which is also the IP address configured on the ENI after it is attached to a virtual machine. If the ENI is an external network interface, the maximum bandwidth for that IP address is also displayed.
* Bound Resources: The names and IDs of the virtual machine resources that the ENI is bound to, if specified.
* Security Groups: The names or IDs of the security groups that the ENI is bound to. If not specified, this field is empty, and you can modify the bound security groups.
* Creation Time: The creation time of the current ENI.
* Project Group: The project group information of the current ENI.
* Status: The current status of the ENI, including creating, unbound, bound, and deleting.

The operations on each individual ENI in the list include binding, unbinding, modifying security groups, adjusting bandwidth, and deleting. You can search and filter the ENI list using the search box, which supports fuzzy searching.

To facilitate tenants in maintaining and summarizing their ENI resources, the platform allows you to download a list of all ENIs owned by the current user as an Excel table. It also supports batch unbinding and deleting operations on ENIs.

## 5.3 Bind NIC

Binding NIC refers to binding an elastic network interface card to a virtual machine for expanding its network interface.

* An elastic network interface card can only be bound to one virtual machine and only supports binding to virtual machines that are in the same data center and in a shutdown or running state;
* **X86 architecture virtual machines can bind up to 6 elastic network interface cards, while ARM architecture virtual machines support binding up to 3 elastic network interface cards;**
* 
You can perform virtual machine binding operations by clicking the "**Bind**" button in the elastic network interface card resource list operation item, as shown in the following figure:

![niclink](/assets/images/userguide/niclink.png)

When binding, you need to select the virtual machine to which you want to bind the network interface card. When the status changes to "**Already bound**", it means that the binding is successful. Users can also view the bound network interface card resources and information through the virtual machine's network information. After the binding is successful, a network interface card will be added to the virtual machine's operating system, and the IP address and related information allocated on the elastic network interface card will be configured. At the same time, the routing information of the associated network will be issued in the operating system.

Scenario example: A virtual machine has no default network egress by default. After binding an external network type elastic network interface card with a default route, since the external IP on the card becomes the first external IP with a default route in the virtual machine, the system will automatically issue the default route in the virtual machine. All requests in the virtual machine will default to use the elastic network interface card as the default network egress.

## 5.4 Unbind NIC

Unbinding network interface card refers to separating the elastic network interface card from the virtual machine and re-binding it to other virtual machines. Only bound elastic network interface card resources can be unbound. Users can perform the unbinding operation of elastic network interface cards through the elastic network interface card list or the network page of the bound virtual machine details, as shown in the following figure:

![nicunlink](/assets/images/userguide/nicunlink.png)

When unbinding, the virtual machine's status must be in a shutdown or running state. The network card status changes to "Unbound", which means that the unbinding is successful. After unbinding, the IP address and security group information of the elastic network interface card remain unchanged, and the network card can be bound to other virtual machines.

After the unbinding is successful, the original elastic network interface card information in the virtual machine's operating system will be automatically cleared. If the elastic network interface card before unbinding is the default network egress of the current virtual machine, the virtual machine will automatically select an external IP with a default route from the virtual machine's bound external IPs as the default network egress after unbinding.

## 5.5 Modify Security Group

Support modifying the security group of the elastic network interface card from the perspective of the elastic network interface card, and support configuring "No Security Group" for unbinding the security group. The minimum unit of the security group is the network card. If the elastic network interface card is bound to a virtual machine, the security group policy of the elastic network interface card only restricts the traffic in and out of the current network card and does not affect the traffic in and out of the virtual machine's default network card and other elastic network interface cards.

Users can modify the security group of the elastic network interface card through the "Modify Security Group" on the elastic network interface card management console list, as shown in the following figure:

A network card only supports binding to one security group. After the modification is successful, users can view the modified security group information through the elastic network interface card list information.

> Only bound security groups can be unbound by selecting "No Security Group".

## 5.6 Delete NIC

Support deleting unbound elastic network interface card resources, that is, only unbound elastic network interface cards can be deleted. After deleting the elastic network interface card, the associated security group will be automatically unbound. Users can perform the operation of deleting elastic network interface cards through the elastic network interface card list, and support batch deletion.

![rmnic](/assets/images/userguide/rmnic.png)

## 5.7 Modify Name and Remarks

Modify the name and remarks of the elastic network interface card, which can be performed in any state. You can click the "Edit" button on the right side of each network card name on the elastic network interface card list page to modify it.

## 5.8 Adjust IP bandwidth

Users can adjust the bandwidth of the Elastic Network Interface (ENI) for external use. This can be done through the "Adjust Bandwidth" option in the ENI list operation, as shown in the figure below:

![ModifyEIPBandwidth](/assets/images/userguide/ModifyEIPBandwidth.png)

Only ENIs of the external network type support bandwidth adjustment operations, and billing will be based on the IP bandwidth. Please ensure that there is sufficient balance in the account.
