---
layout: default
title: VPC
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/vpc/
nav_order: 7
---
# Private Network
## Introduction to VPC
### Overview
SCloudStack virtualizes traditional data center physical networks through software-defined networking (SDN), using OVS as virtual switches, VXLAN tunnels as OverLay network isolation means, and Layer 2 protocols encapsulated through Layer 3 protocols to define the encapsulation and forwarding of packets between virtual VPCs and different virtual machine IP addresses.

A private network (VPC (Virtual Private Cloud) is a logically isolated Layer 2 network broadcast domain environment belonging to users. Within a VPC, users can build and manage multiple three-layer networks, namely subnets, including network topology, IP network segments, IP addresses, gateways, and other virtual resources as network communication carriers for tenant virtual machine services.

VPC is the core of the virtualized network, providing internal network services for cloud platform virtual machines, including network broadcast domains, subnets (IP network segments), IP addresses, etc., which are the foundation of all NFV virtual network functions. A VPC is a container of subnets, which are absolutely isolated between different VPCs to ensure network isolation and security.

Virtual resources such as virtual machines, load balancers, ENIs, and NAT gateways can be added to the subnets of a VPC, providing functions similar to traditional data center switches, enabling custom network planning, and using security groups to protect traffic between VPCs of virtual resources.

You can use IPSecVPN, leased line, and external IP access to form a customized hybrid cloud network environment with other cloud platforms or IDC data centers through IPSecVPN, leased line, and external IP access.

VPC networks have data center attributes, each VPC VPC belongs to only one data center, and resources and networks are completely isolated between data centers, and resources are not connected to the internal network by default. By default, the intra-tenant and inter-tenant VPC networks are not connected, ensuring the isolation of tenant networks and resources from different dimensions.

### VPC Logical Structure
A VPC network consists of VPC CIDR blocks and subnets, as shown in the following figure:

![1](/assets/images/product-functional-architecture-10.jpg)

(1) VPC CIDR block
The CIDR CIDR block to which the VPC network belongs is used as the private CIDR block of the VPC isolated network. For more information about CIDR, see CIDR. To create a VPC network, you need to specify a private CIDR block, and the platform administrator can customize the CIDR block of the VPC VPC through the management console, so that the virtual resources of the tenant communicate only using the IP address of the administrator-defined CIDR block. The following table lists the CIDR CIDR blocks supported by platform VPC by default:

| Network segment  | Mask range | IP address range |
| --- | --- | --- |
| 10.0.0.0/16 | 16 ~ 29 | 10.0.0.0 - 10.0.255.255 |
| 172.16.0.0/16 | 16 ~ 29 | 172.16.0.0 - 172.16.255.255 |
| 192.168.0.0/16 | 16 ~ 29 | 192.168.0.0 - 192.168.255.255 |

Because DHCP and related services need to occupy IP addresses, the CIDR CIDR CIDR block of a VPC does not support a private CIDR block with a 30-bit mask.

By default, the platform occupies or restricts a certain part of the IP CIDR segment, so the unsupported CIDR blocks include `127.0.0.0/8`, `0.0.0.0/8`, `169.254.0.0/16`, and `169.254.0.0/16`.
(2) Subnet
A subnet is the basic network address space of a VPC VPC that is used for intranet connections between virtual resources.
- A VPC consists of at least one subnet, and the CIDR of the subnet must be within the CIDR CIDR block of the VPC.
- Subnets in the same VPC are connected through public gateways, and resources can communicate with each other on the private network by default, and virtual machines, load balancing, NAT gateways, and IPSecVPN gateways can be deployed.
- By default, subnets in the same VPC communicate through a public gateway.
- The minimum number of CIDR CIDR blocks in subnets is 29 bits, and subnet CIDR blocks with 30 and 32-bit masks are not supported.
- In each subnet, the gateway address that uses the first available IP address as the gateway, such as `192.168.1.0/24, is 192.168.1.1`.

When virtual resources exist in a subnet, deleting and destroying VPC and subnet resources is not allowed.

### VPC Connections
By connecting VPC networks with virtual machines, ENIs, public IP addresses, security groups, NAT gateways, load balancers, VPN gateways, MySQL databases, Redis caches, and leased lines, the platform can quickly build and configure complex network environments and hybrid cloud scenarios, as shown in the following figure:

![1](/assets/images/product-functional-architecture-11.jpg)

- The default internal NIC (the virtual NIC that comes with the virtual NIC) of a virtual machine joins the same VPC network to implement network communication between virtual machines, and security groups can be used to ensure the security of east-west traffic of virtual machines.
- The default external network NIC of a virtual machine (the virtual network card that comes with it when it is created) can be directly bound to multiple external IP addresses for Internet access, and the external IP address connected to the IDC physical network can be bound to realize physical network connection, and the security group can control the north-south traffic of the virtual machine while building a secure and reliable hybrid access environment.
- The ENI of a virtual machine is added to different VPC networks and subnets to implement refined network management and inexpensive failover solutions, and the security group is bound to the ENI to ensure the security of VPCs and virtual resources in multiple dimensions through security group rules.
- Virtual machines join the same VPC network as UDB and URedis services to meet the connection scenarios of business applications, databases, and cache services.
- Virtual machines with the same VPC network can access the Internet or IDC data center network through NAT gateway and public IP connection, share the external IP address, and provide external services through DNAT port mapping.
- Virtual machines from the same VPC network are added to the ULB backend service nodes in the private network to provide load balancing services within the VPC network.
- Virtual machines with the same VPC network are added to the ULB backend service node of the Internet, and the Internet IP address associated with the ULB is combined to provide the Internet load balancing service.
- Virtual machines of the same VPC network can be interconnected with virtual machines of different VPC networks on the private network through IPSecVPN Gateway to achieve interconnection between VPCs.
- IPSecVPN Gateway allows virtual machines between two VPCs to communicate directly over the private network.
- IPSecVPN gateway or leased line is used to connect the platform with on-premises IDC data centers and third-party cloud platforms to build a secure and reliable hybrid cloud environment.

The external IP address can be used to open up the physical network of the IDC data center, and the application and virtual machine can directly communicate with the physical machine on the internal network. IPSecVPN Gateway is used to connect virtual networks of third-party cloud platforms or IDC data centers, and is used in scenarios where different cloud platforms can be used to securely connect through VPN.

### Functions and Features
Platform VPC networking provides isolated network environment, custom subnets, subnet communication, and security protection based on tenant consoles and APIs, and can provide high-performance virtual networks based on hardware and technical features such as DPDK.

- Isolated network environment <br/>
The VPC is based on the OVS (Open vSwitch) component, which implements an isolated virtual network through VXLAN tunnel encapsulation technology. Each VPC network corresponds to a VXLAN tunnel number (VNI), which serves as a globally unique network identifier to provide tenants with an independent and completely isolated Layer 2 network that can be used to connect multiple virtual resources by dividing multiple subnets in the VPC as a communication carrier for virtual resources. Different VPC networks are completely isolated from each other and cannot communicate directly.
- Custom subnets<br/>
Supports three-tier network planning within a VPC network, that is, dividing one or more subnets. Provides custom IP segment ranges, available IP segments, and a default gateway to deploy applications and services from virtual machines in subnets. You can add multiple ENIs to a subnet, specify IP addresses in the subnet, and bind them to the virtual machine where the application is deployed to fine-grained network access for application services.
- Subnet communication<br/>
Each subnet belongs to a broadcast domain, and the VPC network provides gateway services by default, and different subnets in the same VPC communicate through the gateway.
- Security<br/>
The cloud platform provides internal security groups and external network firewalls, provides multi-dimensional security access control for virtual resources through protocols and ports, and performs uplink and downlink QoS control based on the network traffic of virtual NICs and virtual instances, improving the security of VPC networks in an all-round way. Security groups are stateful security layers that set security rules in the inbound and outbound directions to control and filter data traffic to and from subnet IPs.
- High-performance virtual networks<br/>
The SDN network is distributed across all compute nodes, communicating between nodes through 20GE redundant links, and providing highly reliable and high-performance virtual networks for cloud platforms through the load of intranet traffic across all compute nodes.

While ensuring network isolation, network scale, network communication, and security, the cloud platform provides tenants and sub-accounts with full lifecycle management of VPC subnet creation, modification, deletion, and operation audit logs. When you create virtual resources such as virtual machines, NAT gateways, load balancers, and VPN gateways, you can specify the VPC networks and subnets to join, and query the number of available IPs for each subnet.

VPC networks have data center attributes and only support specifying virtual resources from the same data center into a VPC network, and the subnet block of each VPC network must be in the CIDR segment of the VPC network. The platform provides default VPC network and subnet resources for each tenant and sub-account through the VPC network configured by the administrator, which is convenient for users to log in to the cloud platform to quickly deploy services.

## Create a VPC
You can specify the VPC name and CIDR CIDR block to add a VPC network with one click to set up network environments for different services. If a VPC network block is created, you must plan the network in advance, such as planning the service IP CIDR block and IP address.

Go to the VPC Network resource list page in the navigation bar to create a VPC network, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-52.png)

Select and configure the name and CIDR block information of the VPC network:
- VPC Name: The name of the VPC network that needs to be created;
- VPC CIDR block: The IP CIDR block included in the VPC network cannot be modified after it is created, and all subnets in the VPC share the IP address of the CIDR block.

When the VPC network is created, the status is "Creating", and when the status changes to "Active", the VPC network is successfully created, and the VPC network can usually be created within 5 seconds.

## Viewing Private Networks
Go to the VPC Network console in the navigation pane to view the list of VPC network resources, and you can view the details of VPC network and subnet resources by using the VPC name on the list.
### List of private networks
On the VPC Network List page, you can view the list and related information of VPC resources under the current account, including name, resource ID, CIDR block, number of subnets, status, creation time, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-53.png)

- Name/ID: The name and globally unique identifier of the VPC VPC.
- CIDR block information: CIDR CIDR block information specified when the current VPC network is created.
- Number of subnets: The number of subnets included in the current VPC network;
- Status: The status of the current VPC network, which is generally active;
- Creation time: The creation time of the current VPC network resource;

The action items on the list can delete a single VPC network, and you can search and filter the VPC list through the search box, supporting approximate search .

In order to facilitate tenants' resource statistics and maintenance, the platform supports downloading the list of all VPC network resources owned by the current user as an Excel table. You can also delete VPC networks in bulk.

### VPC details
On the VPC network list, click the VPC name or ID to go to the Overview page to view the details and subnet information of the current VPC network, and switch to the Operation Log page to view the operation log information of the current VPC network and subnet, as shown on the Overview page:

![1](/assets/images/user-guide/user-guide-54.png)

(1) Basic information

Basic information about the VPC network, including resource ID, resource name, region (data center), CIDR block, creation time, and status.

(2) Subnet management

The VPC Details page displays the list of subnet resources created in the current VPC network, including name, resource ID, CIDR block, status, creation time, and subnet operations, where CIDR block refers to the CIDR block of the current subnet, which is included in the CIDR block of the VPC network.
The action item on the subnet list is that you can delete a single subnet, and only subnet resources that are not used by the resource can be deleted. To facilitate tenants' maintenance of subnet resources, the platform supports batch deletion of subnets.

## Modifying the Name and Remarks
Modify the name and comment of the VPC VPC to perform operations in any state. You can modify it through the Edit button to the right of each VPC name on the VPC VPC list page.

## Delete VPC
Allows users to delete and release VPC networks that are not occupied by any resources with IP addresses. After the VPC network is deleted, it is completely destroyed, and you must ensure that the resources created by the VPC network have been emptied before deletion. The delete operation is shown in the following figure:

![1](/assets/images/user-guide/user-guide-55.png)

## Adding Subnets
Adding subnets refers to adding subnets to a VPC network, that is, three-layer networks, which are used to form private CIDR blocks belonging to users' services, and each CIDR block is an independent broadcast domain. The CIDR CIDR block of a subnet must be within the CIDR CIDR block of the VPC, and resources in the same subnet must be interconnected by default, and all subnets in the same VPC must be interconnected by default.

You can specify the subnet name, subnet CIDR CIDR block, and add one or more subnets to a VPC network to build different service networks on the internal network. Before creating a subnet, you must ensure that there are sufficient IP CIDRs in the CIDR of the VPC network, and you can enter the creation wizard page by using Create Subnet in the subnet list on the VPC network details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-56.png)

- Name/Description: The name and description of the subnet that needs to be created;
- Subnet CIDR block: The CIDR CIDR block of the subnet that needs to be created, which must be within the CIDR CIDR CIDR block of the VPC, which can be the same as the CIDR CIDR CIDR CIDR block of the VPC, which means that the subnet includes all network IP addresses under the VPC.

When the subnet is created, the status is "Creating", and after the subnet is created, the status of the subnet changes to "Valid" and can be used for resource creation.

Note: If the subnet CIDR block is the same as the VPC CIDR block, the VPC currently supports only one subnet.

## Delete a subnet
You can delete the current subnet resources through the Delete function on the subnet list, and the deleted subnets will be directly destroyed. Before deleting a subnet, you must ensure that the resources in the subnet have been emptied, including the resources of the Recycle Bin, otherwise you are not allowed to delete the current subnet, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-57.png)

## Modify the subnet name
Modify the name and comment of the subnet to be performed in any state. You can modify it by clicking the "Edit" button to the right of each subnet name on the subnet list page.
