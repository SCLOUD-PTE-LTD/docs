---
layout: default
title: Elastic Network Interface
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/eni/
nav_order: 4
---
# ENIs
Elastic Network Interface (ENI) is an elastic network interface that can be attached to virtual machines at any time, supports binding and unbinding, can be flexibly migrated between multiple virtual machines, provides high-availability cluster construction capabilities for virtual machines, and can achieve refined network management and cheap failover solutions.

ENIs and the default NICs (one internal NIC and one external NIC) that come with a virtual machine are virtual network devices that provide network transmission for virtual machines, which are divided into two types: internal NICs and external NICs, and assign IP addresses, gateways, subnet masks, and routing-related network information from the network to which they belong.
- The ENI of the private network type belongs to a VPC and a subnet, and an IP address is automatically or manually assigned from the VPC.
- The network to which the EIP belongs is an external CIDR block, and an IP address is automatically or manually assigned from the external CIDR segment, and the assigned IP address is consistent with the lifecycle of the ENI, and can only be released when the ENI is destroyed.
- If the NIC type is Internet, the NIC is billed based on the bandwidth specifications of the selected Internet IP, and you can select the appropriate payment method and purchase duration according to your business needs.

The default NIC of a virtual machine belongs to the VPC and subnet specified when the virtual machine is created, and an ENI of a different VPC is bound to the virtual machine, so that the virtual machine can communicate with the virtual machine of a different VPC network.

ENIs have an independent lifecycle, support binding and unbinding management, and can be freely migrated between multiple virtual machines. When a virtual machine is destroyed, the ENI is automatically de-bound and bound to another virtual machine.

ENIs have the region (data center) attribute and can only bind virtual machines to the same data center. An ENI can only be bound to one VM, an x86 VM can bind up to 6 ENIs, and an ARM VM can bind up to 3 NICs. After the external ENI is bound to a virtual machine, the default network egress policy of the virtual machine is not affected, including the external IP address bound by the ENI on the virtual machine, the first IP with a default route is used as the default network egress of the virtual machine, and you can set a public IP address with a default route as the default network egress of the virtual machine.

Each ENI can be assigned only one IP address, and a security group can be bound to the ENI as needed to control traffic to and from the ENI to achieve refined network security control. If you do not need to control the traffic of the ENI, you can empty the security group of the ENI.

Users can custom-create network cards through the platform, bind, unbind, and modify security groups on the network cards, and perform the Adjust Bandwidth operation for external ENIs to adjust the bandwidth limit of external IP addresses on external ENIs.

## Create an ENI
Cloud platform users can create an ENI through the API interface or console to expand the network interface of a virtual machine. Before you create an ENI, you must ensure that your account has at least one VPC network, subnet, or public CIDR block. Enter the virtual machine console through the navigation bar, switch to the ENI NIC management page, and click the "Create NIC" button to enter the ENI creation window, as shown below the schematic diagram of creating an internal network type and an external network type:

![1](/assets/images/user-guide/user-guide-46.png)

- Name: The name and identity of the ENI to be created.
- NIC type: The type of ENI, including internal NICs and external NICs, is assigned IP addresses from VPCs and external CIDR blocks.
- Network: The network to which the ENI belongs must be specified when you create it.
  - The internal EIP belongs to a VPC and a subnet, and an IP address is automatically or manually assigned from the VPC.
  - The network to which the EIP belongs is an external CIDR block, and an IP address is automatically or manually assigned from the external CIDR block, specifying an external CIDR block with a sufficient number of available IP addresses and configuring the bandwidth limit of the external IP address.
- IP address: The IP address of the current NIC will be automatically assigned an IP address from the IP address segment of the network to which it belongs by default.
- Security group: The security group that the current NIC needs to bind to control network traffic to and from the ENI. Do not bind to a security group, that is, the current NIC is not bound to a security group.

The security group bound to an ENI and the internal/external network security group bound to a virtual machine do not affect each other, and the security group bound to an ENI only securely controls the traffic of the associated ENI.

When you create an ENI, you are billed based on the bandwidth specifications of the Internet IP address, and you can select the appropriate payment method and purchase duration based on your needs. When the ENI is created, the status changes to Unbound, the NIC has been successfully created, and you can bind a virtual machine and modify the security group of the ENI.

## View the network card
Go to the VM console in the navigation pane and switch to the NIC Management page to view the list and related information of ENI resources, including the name, resource ID, NIC type, network, IP address, bound resource, security group, status, creation time, and operation items of the NIC, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-47.png)

- Name/Resource ID: The name and globally unique identifier of the ENI.
- NIC type: The type of ENI, including internal NIC and external NIC, corresponding to VPC network and external network network, respectively.
- Network: The network to which the ENI belongs is specified, the VPC and subnet to which the network belongs is the specified network in the internal network type, and the specified external network network and CIDR block for the external network type.
- IP address: The IP address assigned by the current ENI from the network to which the ENI belongs, and the IP address configured on the ENI after binding to the virtual machine. If the ENI is an external network type, the bandwidth limit of the IP address is displayed after the IP address.
- Bind resource: The name and ID of the VM resource bound to the ENI, or empty if not specified.
- Security group: The name or ID of the security group bound to the ENI is blank, and you can modify the security group to bind the security group.
- Creation Time: The creation time of the ENI.
- Status: The current status of the ENI, including created, unbound, bound, bound, bound, unbound, and deleted.

![1](/assets/images/product-functional-architecture-5.jpg)

The operation items on the list refer to operations on a single ENI, including binding, unbinding, modifying security groups, adjusting bandwidth, and deleting, and you can search and filter the ENI list through the search box, supporting fuzzy search.

To facilitate tenants' statistics and maintenance of ENI resources, the platform supports downloading the list of all ENI resources owned by the current user as an Excel table. You can also unbind and delete ENIs in batches.

## Bind the NIC
Bonding a NIC refers to binding an ENI to a virtual machine to expand the network interface of the virtual machine.

- An ENI can only be bound to one virtual machine, and only virtual machines bound to the same data center and in the shutdown or running state can be supported.
- X86 virtual machines can bind up to 6 ENIs, and ARM VMs can bind up to 3 ENIs.

You can bind a virtual machine by using the Bind button of the ENI resource list operation item, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-48.png)

During the binding process, the status of the ENI is "binding", and when the status is changed to "bound", the binding is successful, and the user can also view the bound NIC resources and information through the network information of the virtual machine. After the binding is successful, a NIC is added to the operating system of the virtual machine, the IP address and related information assigned on the ENI are configured, and the routing information of the network to which it belongs is distributed in the operating system.

Scenario: If a VM has no default network egress by default, and if you bind an ENI with a default route to a VM, the system automatically issues a default route to the VM because the external IP address on the NIC becomes the first external IP address with a default route for the VM, and all requests in the VM will use the ENI as the default network egress by default.

## Unbind the network card
Unbinding an ENI detaches an ENI from a virtual machine and rebinds it to other VMs. You can use the ENI list or the VM Details page to unbind an ENI, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-23.png)

When unbound, the state of the virtual machine must be powered off or running. During the unbind operation, the status of the ENI changes to Unbound, and the status of the ENI changes to Unbound, which means that the unbinding is successful. After the ENI is unbound, the IP address and security group information of the ENI remain unchanged, and the NIC can be bound to other virtual machines.

If the ENI before unbinding is the default network exit of the current virtual machine, the virtual machine automatically selects a public IP address with a default route from the public IP address bound to the virtual machine as the default network egress of the virtual machine.

## Modify security groups
You can modify the security group of an ENI from the perspective of an ENI, and configure Do Not Bind to unbind a security group. If an ENI is bound to a virtual machine, the security group policy of the ENI only restricts the traffic access of the current NIC, and does not affect the traffic access of the default NIC of the VM and other ENIs.

You can modify a security group on the ENI console, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-35.png)

Only one security group can be bound to an NIC, and you can view the modified security group information through the ENI list information.

Only if the ENI is bound to a security group, you can unbind the bound security group by using Do Not Bind Yet.

## Remove the NIC
You can delete ENI resources in the unbound state, that is, you can delete only ENIs in the Unbound state. After you delete an ENI, the security group associated with it is automatically unbound. You can delete ENIs from the ENI list, which supports batch deletion.

![1](/assets/images/user-guide/user-guide-49.png)

## Modifying the Name and Remarks
Change the name and remarks of the ENI to perform operations in any state. You can modify it by clicking the Edit button to the right of each NIC name on the ENI List page.

## Adjust bandwidth
You can adjust the IP bandwidth of an ENI on the Internet by using Adjust Bandwidth in the ENI list, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-50.png)

Only external network ENIs can be used to adjust bandwidth, and billing is based on IP bandwidth, so ensure that the account balance is sufficient.
