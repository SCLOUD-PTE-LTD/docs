---
layout: default
title: VPC
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/vpc/
nav_order: 8
---
# 8. Virtual Private Network

## 8.1 Introduction to VPC

### 8.1.1 Overview

Private cloud platforms virtualize traditional data center physical networks through software-defined networking (SDN). They use OVS as a virtual switch and VXLAN tunnels as the OverLay network isolation method. Three-layer protocol encapsulation is used to define virtual private networks (VPCs) and to encapsulate and forward packets between different virtual machine IP addresses.

A VPC (Virtual Private Cloud) is a logical, isolated Layer 2 network broadcast domain environment that belongs to the user. Within a VPC, users can build and manage multiple Layer 3 networks, or subnets, including IP segments, IP addresses, gateways, and other virtual resources as communication carriers for tenant virtual machine services.

The VPC is the core of the virtualized network and provides intranet services for the cloud platform's virtual machines, including network broadcast domains, subnets (IP segments), IP addresses, etc., forming the basis for all NFV virtual network functions. A private network is a container for subnets that are completely isolated between different private networks, ensuring network isolation and security.

Virtual resources such as virtual machines, load balancers, elastic network interfaces, and NAT gateways can be added to the subnet of a private network to provide functionality similar to that of a traditional data center switch. Networks can be custom planned and virtual resource traffic between VPCs can be protected by security groups.

> The cloud platform's private network and virtual resources can be combined with other cloud platforms or IDC data centers to form an on-demand customized hybrid cloud network environment through IPSecVPN, dedicated lines, and external IP access.

VPC networks have data center properties, with each VPC belonging to only one data center. Resources and networks between data centers are completely isolated, and resources are not available on the internal network by default. The VPC networks within and between tenants are isolated from each other, ensuring the isolation and security of tenant networks and resources from different perspectives.

### 8.1.2 VPC Logical Structure

A VPC network is mainly composed of private network segments and subnets, as shown in the following figure:

![vpccom](/assets/images/userguide/vpccom.png)

**（1）Private Network Segments**

The CIDR segment to which the VPC network belongs serves as the private network segment for isolating VPC networks. For related information on CIDR, refer to [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). To create a VPC network, you must specify a private network segment. Platform administrators can customize the VPC private network segment through the management console, allowing tenant virtual resources to communicate only using IP addresses defined by the administrator's network segment. The default range of network segments supported by the platform's VPC private network CIDR is shown in the table below (the 10.0.0.0/8 network segment needs to be configured in the system management-global configuration-product policy):

  |  Segment   | Mask Range  |  IP Address Range   | Default Configuration / Configurable Item  |
  |  --------  | --------  | --------  | --------  |
  | 10.0.0.0/8| 8 ~ 29   | 10.0.0.0 - 10.255.255.255     | Configurable Item |
  | 10.0.0.0/16 | 16 ~ 29  | 10.0.0.0 - 10.10.255.255  | Default Configuration  |
  | 172.16.0.0/16～172.29.0.0/16 |16 ~ 29  | 172.16.0.0 - 172.29.255.255 | Configurable Item |
  | 192.168.0.0/16    | 16 ~ 29  | 192.168.0.0 - 192.168.255.255 | Default Configuration |

> Private network CIDR segments with a `30-bit` mask are not supported because DHCP and related services require IP addresses.

**（2）subnet**

A subnet is a basic network address space of a VPC (Virtual Private Cloud) private network, used for connecting virtual resources within the internal network.

* A private network consists of at least one subnet, and the CIDR of the subnet must be within the CIDR range of the VPC.
* Subnets within the same private network are connected through a `public gateway`, and resources are mutually accessible by default. Virtual machines, load balancers, NAT gateways, IPSec VPN gateways, etc. can be deployed.
* Subnets within the same VPC communicate with each other through a public gateway by default.
* The smallest subnet CIDR block is `/29`, and subnet segments with `/30` or `/32 `masks are not supported.
* In each subnet, the first available IP address is used as the gateway. For example, the gateway address for `192.168.1.0/24` is `192.168.1.1`.

> When there are virtual resources in a subnet, it is not allowed to delete and destroy the private network and subnet resources.

### 8.1.3 VPC Connection

The platform has implemented software-defined and component abstractions for common network devices. By connecting the VPC network with virtual machines, elastic network cards, public IP addresses, security groups, NAT gateways, load balancers, VPN gateways, and other components, complex network environments and hybrid cloud scenarios can be quickly built and configured, as shown in the figure below:

![vpccon.png](/assets/images/userguide/vpccon.png)

* Virtual machines join the same VPC network through their default internal network adapter (i.e., the virtual network card that comes with it when created) to achieve inter-VM communication. The communication can be secured by security groups.
* Virtual machines can directly bind multiple public IP addresses to their default external network adapter (i.e., the virtual network card that comes with it when created) to achieve Internet access. At the same time, they can also bind public IP addresses connected to IDC physical networks to establish communication between the virtual and physical networks. With the help of security groups, both north-south and south-north traffic of virtual machines can be securely controlled, thus building a secure and reliable hybrid access environment.
* Elastic network cards of virtual machines can join different VPC networks and subnets, achieving fine-grained network management and cost-effective fault-tolerant solutions. They are also bound with security groups to provide multi-dimensional security guarantees for the private network and virtual resources.
* Virtual machines on the same VPC network can connect via NAT gateways and public IP addresses to share external network access and IDC data center network access. And they can provide business services externally through DNAT port mapping.
* Virtual machines on the same VPC network can join the backend service nodes of an internal LB (load balancer) to provide load balancing services within the VPC network.
* Virtual machines on the same VPC network can join the backend service nodes of an external LB to provide load balancing services outside the VPC network, combined with public IP addresses associated with the LB.
* Virtual machines on the same VPC network can interconnect within different VPC networks through IPSec VPN gateways.
* Two VPCs can communicate directly with each other via IPSec VPN gateways that bridge their networks.
* By using IPSec VPN gateways or dedicated lines, the platform can connect to local IDC data centers and third-party cloud platforms to build a secure and reliable hybrid cloud environment.

> Public IP addresses can be used to establish communication between virtual machines and physical machines in IDC data centers. IPSec VPN gateways are used to connect virtual networks of third-party cloud providers or IDC data centers, providing a secure connection via VPN between different cloud platforms.

### 8.1.4 Features and Characteristics

The platform's VPC network provides an isolated network environment, customizable subnets, subnet communication and security protection functions via the tenant console and API. It can also provide high-performance virtual networks through hardware and DPDK technology features.

* Isolated Network Environment

  The private network is based on the [OVS](http://www.openvswitch.org/) (Open vSwitch) component and uses [VXLAN](https://datatracker.ietf.org/doc/rfc7348/) tunnel encapsulation technology to achieve an isolated virtual network. Each VPC network corresponds to a unique VXLAN tunnel ID (VNI), which serves as a globally unique network identifier, providing tenants with an independent and completely isolated layer 2 network. Multiple subnets can be created within a private network as communication carriers for virtual resources to connect multiple virtual resources. Different VPC networks are completely isolated and cannot communicate directly.

* Customizable Subnets

  Three-layer network planning can be performed within a VPC network by dividing it into one or more subnets. Users can customize IP address ranges, available IP address ranges and default gateways, and deploy applications and services by creating virtual machines in subnets. Multiple elastic network cards can be added to a subnet, each with its own IP address within the subnet, and bound to the virtual machine that deploys the application, enabling fine-grained management of network access to application services.

* Subnet Communication

  Each subnet belongs to a broadcast domain, and the VPC network provides gateway services by default. Different subnets within the same VPC communicate with each other through a gateway.

* Security Protection

  The cloud platform provides internal security groups and external firewalls to provide multi-dimensional security access control for virtual resources based on protocol and port, while controlling QoS for uplink and downlink network traffic based on virtual network cards and instances. Security groups are stateful security layers that allow different security rules to be set for incoming and outgoing traffic, filtering data flow between subnet IPs.

* High-Performance Virtual Network

  The SDN network is deployed in a distributed manner across all computing nodes, communicating between nodes via redundant 20GE links, and load-balancing internal network traffic through all computing nodes to provide the cloud platform with highly reliable and high-performance virtual networks.

While ensuring network isolation, scalability, communication, and security, the cloud platform provides lifecycle management for VPC subnets, including creation, modification, deletion, and operation audit logs for tenants and sub-accounts. Users can specify the VPC network and subnet that virtual resources such as virtual machines, NAT gateways, load balancers, and VPN gateways should join, and query the available IP address count for each subnet.

The VPC network has data center properties, and virtual resources between different data centers do not communicate by default, nor do different VPCs within the same data center. All subnets and resources of the same VPC communicate by default. Only virtual resources specified for the same data center can be added to a VPC network, and the CIDR block of each subnet must be within the CIDR block of the VPC network.

The platform provides default VPC networks and subnet resources for each tenant and sub-account through administrator-configured VPC networks, making it easy for users to quickly deploy business when logging in to the cloud platform.

## 8.2 Create VPC

Users can add a VPC network with one click by specifying the VPC name and [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) block, which is used to build network environments for different businesses. Once created, the subnet of the VPC cannot be modified, so it is necessary to plan the network in advance, such as planning the business IP subnet and IP addresses.

To create a VPC network, navigate to the "VPC Network" resource list page and follow the steps shown in the figure below:

![createvpc](/assets/images/userguide/createvpc.png)

Select and configure the name and subnet information of the VPC network:
- VPC Name: The name identifier for the VPC network to be created.
- VPC Subnet: The IP subnet included in the VPC network. Once created, it cannot be modified, and all subnets under the VPC share the same IP address range.

When creating a VPC network, the status will be "Creating". When the status changes to "Available", it means that the VPC network has been successfully created. Usually, the creation of a VPC network can be completed within 5 seconds. Users can view the created VPC resources through the VPC list.

## 8.3 View Private Networks

Navigate to the VPC network console through the navigation bar to view the list of VPC network resources. By clicking on a VPC name in the list, users can enter the details page to view the details of the VPC network and subnet resources.

### 8.3.1 VPC Network List

The VPC network list page displays a list of VPC resources and related information for the current account, including name, resource ID, network segment, number of subnets, status, creation time, and actions as shown in the figure below:

![vpclist](/assets/images/userguide/vpclist.png)

- Name/ID: The name and globally unique identifier of the VPC private network;
- Network Segment: The CIDR network segment information specified when the VPC network was created;
- Number of Subnets: The number of subnets included in the current VPC network;
- Status: The status of the current VPC network, usually available;
- Creation Time: The creation time of the current VPC network resource;

The action item on the list allows for deleting individual VPC networks, and the search box can be used to search and filter the VPC list, supporting fuzzy search.

To facilitate tenants' resource statistics and maintenance, the platform supports downloading the Excel table of all VPC network resource lists owned by the current user; it also supports batch deletion of VPC networks.

### 8.3.2 VPC Network Details

On the VPC network list, clicking the VPC name or ID takes you to the overview page where you can view the details and subnet information of the current VPC network. You can also switch to the operation log page to view the operation log information of the current VPC network and subnets, as shown in the figure below:

![vpcdetails](/assets/images/userguide/vpcdetails.png)

**(1) Basic Information**

The basic information of the VPC network includes the resource ID, resource name, region (data center), network segment, creation time, and status.

**(2) Subnet Management**

The VPC details page displays a list of created subnet resources in the current VPC network, including name, resource ID, network segment, status, creation time, and actions for subnets. The network segment refers to the subnet's network segment, which is included in the VPC network's network segment.

The action item on the subnet list allows for deleting individual subnets, only supporting the deletion of unused subnet resources. To facilitate tenants' maintenance of subnet resources, the platform supports batch deletion of subnets.

**(3) Subnet Route Management**

The subnet route page displays a list of routing rules under the current subnet, including destination address, next hop type, next hop, remarks, and actions for routing rules. The next hop type refers to virtual machines, VIPs, and custom types.

The action item on the route list allows for deleting individual routes, and the platform supports batch creation and deletion of routes.

## 8.4 Modify Name and Comment

You can modify the name and comment of a VPC private network at any time, regardless of its status. To do so, click the "Edit" button next to the name of the VPC on the VPC private network list page.

## 8.5 Delete Private Network

Users can delete and release VPC networks that have IP addresses not being used by any resources. Once a VPC network is deleted, it will be permanently destroyed, and all resources created within the VPC network must be cleared before deletion. The following image shows how to perform the deletion operation:

![deletevpc](/assets/images/userguide/deletevpc.png)

## 8.6 Add Subnet

Adding a subnet means adding a layer three network to a VPC, which is used to build a private network segment belonging to the user's business. Each subnet is an independent broadcast domain. The CIDR network segment of the subnet must be within the CIDR network segment of the VPC. Resources within the same subnet are interconnected by default, and all subnets under the same VPC are interconnected by default.

Users can add one or more subnets to a VPC network by specifying a subnet name and CIDR network segment, which are used to build different internal business networks. Before creating a subnet, ensure that there are sufficient IP segments within the VPC network CIDR. You can enter the creation wizard page by clicking "**Create Subnet**" on the subnet list page of the VPC network details page, as shown in the following figure:

![createsubnet](/assets/images/userguide/createsubnet.png)

- Name/Description: The name and description information of the subnet you want to create.
- Subnet Segment: The CIDR network segment of the subnet you want to create, which must be within the CIDR network segment of the VPC. It can be the same as the VPC CIDR network segment, which means that the subnet includes all network IP addresses under the VPC.

The subnet is in the "Creating" state during creation and changes to "Available" once it's successfully created and can be used for resource creation.

> Note: If the subnet segment is the same as the VPC segment, the current private network supports only one subnet.

## 8.7 Delete Subnet

Users can delete the current subnet resource through the "**Delete**" function on the subnet list, and the deleted subnet will be directly destroyed. Ensure that all resources within the subnet have been cleared, including those in the recycle bin, before deleting the current subnet. Otherwise, the current subnet cannot be deleted, as shown in the following figure:

![rmsubnet](/assets/images/userguide/rmsubnet.png)

## 8.8 Modify Subnet Name

You can modify the name and description of a subnet at any time, regardless of its state. To do so, click the "**Edit**" button next to each subnet on the subnet list page.

## 8.9 Add Subnet Route
Routing policies are used to control the direction of outbound traffic from a subnet. Users can create routing policies by clicking the "**Routing Policy**" button on the subnet details page to enter the routing list, specifying the destination address, next hop type, next hop, and description.

![createsubnetroute](/assets/images/userguide/createsubnetroute.png)

- Destination: The destination represents the target network segment to which you want to forward traffic. The destination network segment description only supports network segment format. If you want the destination to be a single IP, you can set the mask to 32 (for example, 172.16.1.1/32).
- Next Hop Type:

| Next Hop Type | Description |
| ------------- | ----------- |
| Local         | Non-editable, providing VPC interconnection capability |
| NAT Gateway   | Non-editable, routes issued by NATGW |
| IPSec VPN     | Non-editable, routes issued by IPSecVPN |
| Public Service| Non-editable |
| VIP           | Editable, VIP |
| Virtual Machine | Editable, virtual machine resources |
| Custom        | Editable, custom address |

- Next Hop: Specifies the specific next hop instance to jump to, such as gateway or cloud server IP.
- Description: You can add descriptive information for routing entries for resource management purposes.

## 8.10 Modify Subnet Route

Users can modify the current routing policies using the "**Modify**" function on the routing list.

![updatesubnetroute](/assets/images/userguide/updatesubnetroute.png)

## 8.11 Delete Subnet Route

Users can delete the current routing policy through the "**Delete**" function on the routing list.

![rmsubnetroute](/assets/images/userguide/rmsubnetroute.png)

## 8.12 Subnet Topology Details

Users can view the subnet usage through the subnet topology page. The following figure shows the thumbnail status.

![rmsubnetroute](/assets/images/userguide/usesub.png)

Users can view detailed subnet usage through the button on the right-hand side list, as shown in the figure below.

![rmsubnetroute](/assets/images/userguide/usesub1.png)

## 8.13 Network Interconnection
The network interconnection function is used to achieve network interconnection between two VPCs of the same tenant. Tenants can use the network interconnection function to establish a connection between two VPCs, so that private IP addresses can be used for communication between the two VPCs, just like two VPCs in the same network.

### 8.13.1 Connect Networks
(1) The premise of connecting networks is to enable the VPC gateway, as shown in the figure below:

![opengateway](/assets/images/userguide/opengateway.png)

(2) The connection page is as follows:

![vpclink1](/assets/images/userguide/vpclink1.png)

- VPCID: ID of the current VPC.
- Connection scenario: "VPC Intercommunication" can be used directly for interconnection between different VPCs of the same tenant; "Dedicated Access" requires the creation of a dedicated line on the management side.
- Peer VPC: The VPC connected to the current VPC also needs to enable VPC gateway.

### 8.13.2 View List

![vpclinklist](/assets/images/userguide/vpclinklist.png)

### 8.13.3 Disconnect Network
Users can disconnect the connection individually by clicking the "Disconnect" button in the operation column, or by selecting to **disconnect connections in batches**.

![disconnect](/assets/images/userguide/disconnect.png)

### 8.13.4 Constraints and Limitations
- When configuring network interconnection, the IP range (CIDR) of the two VPCs cannot overlap, otherwise it may cause routing conflicts and lead to configuration failure.
- Only one VPC connection can be established between two VPCs at the same time.
- The VPC gateway cannot be turned off when there is a connection in the VPC.