---
layout: default
title: External IP
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/eip/
nav_order: 7
---
# External IP
## Introduction to EIP
### Overview
Elastic IP Address (EIP) is the external IP address provided by the platform for virtual resources such as virtual machines, NAT gateways, VPN gateways, and load balancers, providing virtual resources with network access capabilities outside the platform VPC network, such as the Internet or the physical network of IDC data centers, and external networks can also directly access virtual resources in the platform VPC network through EIP addresses.

EIP resources can be independently applied for and owned, and users can apply for IP addresses in IP CIDR segment resource pools through the console or APIs, and bind EIPs to virtual machines, NAT gateways, load balancers, and VPN gateways to provide Internet service channels for services.

### Physical Architecture
In the private cloud platform, platform administrators are allowed to customize the platform public IP resource pool, that is, the platform administrator can customize the way the platform accesses the Internet, and the external IP block resource pool needs to be delivered to the switch port connected to the computing node through the physical network device before being added to the cloud platform.

![1](/assets/images/SCloudStack_Network_Topology.jpg)

As shown in the physical architecture diagram above, all compute nodes need to connect network cables to the external network access switch of the physical network, and configure the connected ports on the interactive machines of the physical network to allow transparent transmission of VLAN network access mode, so that virtual machines running on computing nodes can directly communicate with the external network through external physical NICs:
- If you access the Internet through an external IP, you need to configure the custom External IP block on the physical network device to be pass-through or NAT to the Internet.
- If you access the physical network of an IDC data center through an external IP address, you need to configure a custom external IP block on the physical network device to communicate with the IDC data center network, such as the same VLAN or inter-VLAN connection.

The physical network architecture is a high-availability schematic, and the actual production environment architecture can be adjusted, for example, internal and external network access switches can be merged into a set of high-availability access switches, and internal and external networks can be distinguished by different VLANs.

### Logical Architecture
After the physical network architecture and configuration are confirmed, the External IP block and the IDC physical CIDR block need to be added to the cloud IP block resource pool at the platform level, and tenants can apply for EIP addresses of different CIDR blocks and bind EIP addresses to the virtual machine's default external network cards, so that the virtual machine can access the Internet and the IDC data center physical network at the same time through the external IP address.

![1](/assets/images/product-functional-architecture-12.jpg)

As shown in the logical architecture diagram, users add network blocks to the Internet (VLAN200) and IDC physical network (VLAN100) to the cloud platform, respectively. Examples of CIDR blocks are as follows:
- The CIDR block of VLAN200 is `106.75.236.0/25`, and the default route is configured, that is, the EIP bound to the CIDR block of the virtual machine will automatically deliver the default route with the destination address `0.0.0.0/0`.
- The CIDR block of VLAN100 is `192.168.1.0/24`, and only the current CIDR block route is delivered, that is, the EIP bound to the CIDR block of the VM is only routed with the specified destination address `192.168.1.0/24`.

Tenants can request EIP addresses for VLAN200 and VLAN100 respectively, and can bind both EIPs to virtual machines at the same time. The platform configures the EIP address and delivery route directly to the external network card of the virtual machine, and delivers the flow table to the OVS of the physical machine where the virtual machine resides through the SDN controller, and the physical OVS communicates with the Internet or the IDC physical network through the switch device through the interconnection with the external network card interface and switch of the physical machine.

When a virtual machine needs to access the Internet or a physical network, the data is directly transmitted to the OVS virtual switch of the physical machine through the external NIC of the virtual machine, and the request is forwarded to the external NIC and physical switch of the physical machine through the OVS flow table, and the packet is forwarded to the Internet or the IDC physical network area through the VLAN or routing configuration of the physical switch to complete the communication.

As shown in the preceding figure, a virtual machine in the VPC1 network is bound to the EIP addresses of both VLAN100 and VLAN200 CIDRs, with the Vlan100 EIP being `192.168.1.2` and the VLAN200 EIP being `106.75.236.2`. The platform directly configures the two IP addresses to the external network card of the virtual machine, and the EIP address configured to the external network card can be directly viewed through the virtual machine operating system. The default route of the virtual machine specifies that the next hop is the gateway of the VLAN200 Internet segment, so that the virtual machine can communicate with the Internet through the `106.75.236.2` IP address and communicate with Oracle and HPC high-performance servers in the physical network area through `192.168.1.2`.

The entire communication process is directly communicated through the physical NIC of the physical machine where the virtual machine is located, and the best forwarding performance of the physical network hardware can be given full play to improve the forwarding capability of the virtual machine's external communication under the premise of ensuring the performance of the physical NIC and the physical switch. At the same time, all external IP traffic can be controlled within the platform through the platform security group to ensure the security of virtual machines accessing the external network of the platform.

### Features
EIPs are floating IPs that drift to healthy nodes as the failed VM recovers and continue to provide Internet access services for VMs or other virtual resources.

![1](/assets/images/product-functional-architecture-13.jpg)

When the physical host where a virtual machine resides fails, the intelligent scheduling system will automatically perform downtime migration operations on the virtual machine on the failed host, that is, the failed virtual machine will be re-pulled up on other healthy hosts and provide normal business services. If the virtual machine is bound to an external IP address, the intelligent scheduling system will drift the external IP address and related flow table information to the physical host where the virtual migration is located to ensure network communication.

- Platform administrators can customize the public IP resource pool, that is, customize the External IP CIDR block, and configure the routing policy of the CIDR block. After the tenant applies for the public IP address of the CIDR block to be bound to the virtual resource, the bound External IP address is automatically used as the network egress for traffic delivered to the destination routing address.
- The public IP block supports the issuance of default routes and specified routes, which means that all traffic is egressed by default, and the bound public IP address is used as the egress of traffic specified by the administrator.
- Provides IPv4/IPv6 dual-stack capabilities, administrators can customize and manage IPv4 and IPv6 network segment resource pools, and support simultaneous binding of IPv4/IPv6 addresses to virtual machines to provide dual-stack network communication services for virtual machines.
- Supports permission management and control of public IP CIDR blocks, which can be used by all tenants or some tenants, but unspecified tenants do not have permission to apply for and use EIPs for CIDR blocks.
- EIP has the feature of elastic binding, which can be bound to virtual machine resources such as virtual machines, NAT gateways, load balancers, and VPN gateways at any time, and can be unbound to other resources at any time.
- Virtual machines can bind 50 public IPv4 and 10 External IPv6 addresses, and use the first public IP address with a default route as the default network egress of the virtual machine.
- Provides the External IP CIDR block acquisition service, supports tenants to manually specify IP addresses to apply for EIPs, and provides IP address conflict detection to facilitate user service network address planning.
- Platform administrators can customize the bandwidth specifications of the public IP CIDR segments, and tenants can configure the bandwidth limit of the External IP addresses within the bandwidth specifications.
- External IP addresses have data center attributes and can only bind virtual resources to the same data center. 

Users can apply for EIPs through the platform, and perform operations such as binding, unbinding, and adjusting bandwidth of EIPs.

## Request External IP
EIP application means that a tenant can apply for an IPv4 or IPv6 public IP address from the public IP block defined by the administrator through the console, and bind the IP address to resources such as VMs, Server Load Balancer, NAT gateways, VPN gateways, MySQL, and Redis to provide Internet access capabilities for virtual resources.

When applying for an EIP, you need to specify the IP version, CIDR block, IP address, resource name, bandwidth limit, etc., and you can enter the External IP resource console through the navigation bar, and enter the wizard page through "Request External IP", as shown in the following figure:

![1](/assets/images/user-guide/user-guide-58.png)

(1) Select and configure the basic configuration and management settings of the applied public IP address:

- Name/Description: The name and description of the external IP address that must be specified when applying.
- Billing method: Currently, only bandwidth billing is supported, that is, bandwidth is used as the billing object and egress limit, and traffic is not restricted.
- IP version: The IP version of the public IP address, which supports IPv4 and IPv6.
  - When IPv4 is selected, only IPv4 CIDR blocks are displayed.
  - When IPv6 is selected, only IPv6 CIDR blocks are displayed, and if the platform administrator does not define IPv6 CIDR blocks, the IP version supports only IPv4.
- CIDR Block: The CIDR block to which the requested external IP address belongs is customized by the platform administrator, and the IP CIDR block of the CIDR block is displayed, and the manually specified IP address must be within the IP address range of the CIDR segment.
- IP address: The user manually specifies the IP address to apply for EIP, and the specified IP address must be within the IP range of the selected CIDR segment.
- Bandwidth: The upper limit of the bandwidth egress of the requested EIP resource, and the specification range is customized by the platform administrator in Mbps.

(2) Select the purchase quantity and payment method, confirm the order amount and click "Buy Now" to apply for and create an EIP.

- Purchase quantity: Create multiple EIP addresses in batches according to the selected configuration and parameters, and currently support batch creation of 10 EIPs;
- Payment method: Select the billing method of EIP, which supports three modes: hourly, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: The user selects the EIP resource to display the fee according to the payment method.
- Purchase Now: After clicking Buy Now, you will be returned to the EIP resource list page, where you can view the application process of the EIP, usually displaying the status of "Requesting" first, and switching to the "Unbound" status within a few seconds, which means that the application is successful.

## View the external IP address
Go to the External IP console through the navigation pane to view the list of External IP resources, and you can use the name and ID on the list to enter the details page to view the details and operation logs of the External IP.

### List of External IPs

The External IP List can view the list information of all EIP resources under the current account, including name, resource ID, IP, IP version, bandwidth, bound resource, route type, billing method, creation time, expiration time, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-59.png)

- Name/ID: The name and globally unique identifier of the EIP resource.
- IP address: The IP address and CIDR segment name of the EIP resource, or the IPv6 address if the IP version is IPv6.
- IP version: The IP version of the EIP address, such as IPv4 or IPv6.
- Bandwidth: The upper limit of bandwidth egress specified when EIP resources are requested.
- Bind resource: The resource name and resource ID bound by the EIP, and the resource type can be virtual machine, NAT gateway, load balancer, and VPN gateway.
- Route Type: The route type defined by the CIDR block to which the EIP address belongs, including default route and non-default route (specified route or unspecified route).
  - The default route is bound to the virtual resource, and the route with the destination address 0.0.0.0/0 is automatically delivered, which is the default route.
  - Non-default routes are bound to virtual resources, and only routes with user-specified destination addresses are delivered.
- Billing method: The billing method for EIP addresses, including hourly, annual, and monthly.
- Created/Expired: The creation time and expense expiration time of the EIP resource.
- Status: The status of the EIP resource, including requesting, unbound, bound, bound, unbound, modified bandwidth, and deleted.

The operation items on the list refer to operations on a single public IP address, including binding, unbinding, modifying bandwidth, and deleting, and can be searched and filtered through the search box to support fuzzy search.

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all external IP resources owned by the current user as an Excel table. At the same time, you can unbind and delete External IP addresses in batches.

### External IP Details
On the list of public IP resources, click the name or ID to view the details of the current public IP address, and switch to the Operation Log page to view the operation log of the current public IP address, as shown on the Overview page:

![1](/assets/images/user-guide/user-guide-60.png)

- Basic Information: The basic information of the External IP address, including name, ID, IP address, IP version, bandwidth, bound resources, status, creation time, and alarm template information.
  - You can click the button to the right of the name to modify the name and remarks of the public IP address;
  - You can click the button on the right side of the alarm template to modify the alarm template associated with the public IP address, and the default alarm template will be bound by default.
  - You can modify the alarm template only if the external IP address is bound to a virtual machine resource.
- Monitoring Information: The monitoring information of the current public IP address, including the outbound bandwidth usage, ingress/outbound bandwidth, and outbound/inbound packet volume of the NIC.
- Operation Log: The Operation Log page displays the operation log of the current External IP address. It can display logs at a custom time level, perform fuzzy search on logs, provide operation logs within two weeks by default, and view operation logs of different time periods by switching the date period.

## Bind a public IP address
Binding an external IP address refers to binding an EIP address to a virtual machine, NAT gateway, Server Load Balancer, and VPN gateway to provide Internet service capabilities for virtual resources.
- A virtual machine supports binding 50 IPv4 and 10 IPv6 External IP addresses, and the first IP address with a default route is used as the default network egress of the virtual machine.
- IPv6 public IP addresses can only be bound to virtual machines, not to other resources.
- After a virtual machine is bound to a public IP address, the system configures the external IP address and the delivery route of the CIDR block directly to the default external network card that comes with the virtual machine, and you can directly view all the external IP addresses and related routing information bound to the virtual machine through the virtual machine's operating system.
- NAT gateways can only bind an IPv4 External IPv4 address with a default route, and cannot bind an IPv6 External IP address.
- VPN Gateway can only bind a public network with a default route IPv4 address, but does not support binding an IPv6 public IP address.
- Server Load Balancer can bind only one External IPv4 with a default routed IPv4 address, but does not support binding an IPv6 External IP address.
- A public IP address can bind only one virtual resource, and only unbound public 
 
IP addresses can be bound to it, and the bound resource must be running, valid, or shut down. You can use Bind to the External IP Resource List to enter the External IP Binding wizard page and perform resource binding operations, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-61.png)

When binding, select the type of resource to be bound and the binding resource object:

(1) Resource type: refers to the resource type of the bound object, which supports binding to virtual machines, load balancing, NAT gateways, and VPN gateways.

(2) Resource object: refers to the bound resource object, different resource types can choose different resource objects.
- Virtual machine: You can select the virtual machine resources to be bound based on the virtual machine name and private IP address, but you cannot select a virtual machine with 50 IPv4 or 10 IPv6 addresses bound.
- Server Load Balancing: You can select the Server Load Balancer resource to be bound based on the name and ID, and you can select a Server Load Balancer instance that is not bound to an External IP address and is an Internet network IP, and you cannot bind an IPv6 External IP address for Server Load Balancer.
- NAT gateway: You can select the NAT gateway resource to be bound based on the name and ID, only you can select a NAT gateway that does not have a public IP address bound to it, and you cannot bind an IPv6 public IP address to a NAT gateway.
- VPN gateway: You can select the VPN gateway resource to be bound based on the name and ID, and you can select only VPN gateways that do not have an External IP address bound to it, and you cannot bind IPv6 public IP addresses to VPN gateways.

During the binding process, the status of the public IP address is "binding", and if the status is changed to "bound", the binding is successful, and the user can also view the information of the bound external IP address through the bound resource. Usually, the binding is completed instantly, and you can use ping the External IP or related network tools to test whether the binding takes effect. If the network is not reachable, check whether the rules of the security group bound to the resource allow network access, and check the rules and policies of the private network security group and the public security group for the virtual machine respectively.

## Unbind external IP addresses
Unbinding an external IP address separates an EIP address from a virtual resource and rebinds it to other virtual resources. You can only unbind bound public IP resources in the bound state. You can use the External IP list action item to unbind a public IP address, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-62.png)

## Adjust bandwidth
Adjusting bandwidth refers to upgrading or downgrading the bandwidth limit of an external IP address to meet the different bandwidth requirements of services. The adjustable bandwidth specifications are customized by the cloud platform administrator on the management console, and different public IP resource pools support different bandwidth configurations.

You can adjust the bandwidth online or offline, so that you can adjust the bandwidth of the External IP address in real time without stopping the service, without affecting the network communication of the bound resources. Depending on the payment method, bandwidth adjustments may affect the cost and effective time.

- Hourly elastic IP with bandwidth increase, effective in the next payment cycle;
- Elastic IP with annual and monthly payment, bandwidth upgrades, effective immediately, and automatic price difference;
- Elastic IPs that are paid annually and monthly, and bandwidth downgrades are not allowed until the last day of the current billing cycle, and the next billing cycle takes effect.

You can use the Adjust Bandwidth item in the External IP resource list to enter the modification wizard page and adjust the bandwidth, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-63.png)

The EIP status in the modified bandwidth is converted to "Adjusting Bandwidth", and after success, it changes to the "Unbound" or "Bounded" state. In a private cloud environment, the external IP address can be simulated by the "private IP address", that is, the external IP block issued by the administrator to the cloud platform on the physical network is a NAT internal IP address block, and the real bandwidth of the external IP address is controlled at the physical network level.

If the IP address block of the external IP address is used as a pure intranet communication with the IDC data center physical network, the bandwidth specification can be set to the maximum bandwidth of the internal network, such as 10000 Mbps.

## Modify the alarm template
Modify the alarm template to configure alarms on the monitoring data of the external IP address, and trigger alarms when the external IP-related indicators fail or exceed the indicator thresholds through the indicators and thresholds defined by the alarm template, notify relevant personnel to handle the fault, and ensure normal communication of the external IP network.

![1](/assets/images/user-guide/user-guide-64.png)

You can click the button on the right side of the alarm template on the External IP Details Overview page to modify the alarm template, select a new public IP alarm template in the Modify Alarm Template wizard, and click OK to take effect immediately.

You can modify the alarm template only after the public IP address is bound to a virtual resource.

## Modify the public IP address
You can modify the name and remarks of the External IP resource in any state. You can modify it by clicking the Edit button to the right of the name of the External IP resource list.

## Delete the External IP
You can delete the external IP addresses of unbound virtual resources in the account in the console, and you can delete them in batches. You can delete unbound public IP resources. Deleted public IP addresses are automatically recycled and can be restored and completely destroyed.

You can use Delete in the External IP list action item, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-65.png)

## Renewing External IP
You can manually renew external IP addresses, and the renewal operation is only for the resource itself, and does not renew additional resources associated with the resource, such as virtual machines, NAT gateways, load balancers, and VPN gateways. After the additional associated resources expire, they are automatically unbound, and you need to renew the relevant resources in a timely manner to ensure normal service use.

When the external IP address is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the billing method of the external IP is Hour, the renewal period can be selected from 1 to 24 hours; If the billing method of the public IP address is Monthly, the renewal period can be from January to November. If the billing method of the public IP address is Annual, the renewal period is 1 to 5 years.
