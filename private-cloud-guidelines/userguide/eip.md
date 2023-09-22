---
layout: default
title: Elastic IP
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/eip/
nav_order: 9
---
# 9. Elastic IP on Public Network

## 9.1 Introduction to EIP

### 9.1.1 Overview

Elastic IP Address (EIP) is a public IP address provided by the platform for virtual resources such as virtual machines, NAT gateways, VPN gateways, and load balancers. It provides these virtual resources with network access outside of the platform's VPC network, such as the physical network of the Internet or IDC data centers. External networks can also directly access virtual resources within the platform's VPC network via EIP addresses.

EIP resources can be independently applied for and owned. Users can apply for IP addresses in the IP segment resource pool through the console or API and bind them to virtual machines, NAT gateways, load balancers, and VPN gateways to provide external network service channels for their business.

### 9.1.2 Physical Architecture

In private cloud platforms, platform administrators are allowed to customize the platform's public network IP resource pool, which means that the platform's access to the external network is customized by the platform administrator. Before adding the public network IP segment resource pool to the cloud platform, it needs to be deployed to the switch port connected to the computing node through the physical network device.

![EIPtopology](/assets/images/userguide/EIPtopology.png)

As shown in the physical architecture diagram above, all computing nodes need to connect to the external network access switch of the physical network with a network cable, and configure the connected port to allow VLAN network access through the interactive switch of the physical network, so that virtual machines running on the computing nodes can communicate directly with the external network through the external network physical network card:

* If accessing the Internet through an external network IP, the custom external network IP segment needs to be configured as either directly accessible or NATed to the Internet on the physical network device;
* If accessing the physical network of the IDC data center through an external network IP, the custom external network IP segment needs to be configured as able to communicate with the IDC data center network on the physical network device, such as through the same VLAN or opening up VLANs.

> The physical network architecture is a high-availability diagram, and the actual production environment architecture can be adjusted, such as merging the internal and external network access switches into a group of high-availability access switches and distinguishing them through different VLANs.

### 9.1.3 Logical Architecture

After confirming the physical network architecture and configuration, it is necessary to add the Internet IP subnet and IDC physical network subnet to the resource pool of the cloud platform at the platform level. Tenants can apply for EIP addresses from different subnets and bind EIP addresses leading to different networks to the virtual machine's default external network card so that the virtual machine can simultaneously access the Internet and the IDC data center physical network through an external IP address.

![eiparch](/assets/images/userguide/eiparch.png)

As shown in the logical architecture diagram, users add VLAN200 for Internet and VLAN100 for the IDC physical network to the cloud platform separately. The network segments are as follows:

* The subnet for VLAN200 is `106.75.236.0/25`, and the default route is configured to be sent. Therefore, the EIP bound by the virtual machine subnet will automatically send a default route with a destination address of 0.0.0.0/0.
* The subnet for VLAN100 is `192.168.1.0/24`, and only the current subnet route is sent. Therefore, the EIP bound by the virtual machine subnet will only send specified routes with a destination address of 192.168.1.0/24.

Tenants can apply for EIP addresses for VLAN200 and VLAN100 separately and can bind both EIPs to the virtual machine simultaneously. The platform configures the EIP address and sends the route directly to the virtual machine's external network card, and sends the flow table to the OVS on the physical machine where the virtual machine is located via the SDN controller. The OVS on the physical machine is interconnected with the physical machine's external network interface and switch and communicates with the Internet or IDC physical network through the switch device.

When the virtual machine needs to access the Internet or the physical network, the data is directly transmitted through the virtual machine's external network card to the physical machine's OVS virtual switch, and the request is forwarded to the physical machine's external network card and physical switch through the OVS flow table. The data packet is then forwarded to the Internet or IDC physical network area via the Vlan or routing configuration of the physical switch, completing the communication.

As shown in the above diagram, the virtual machine of the VPC1 network is bound with EIP addresses for VLAN100 and VLAN200 simultaneously. The EIP for VLAN100 is `192.168.1.2`, and the EIP for VLAN200 is `106.75.236.2`. The platform directly configures both IP addresses to the virtual machine's external network card, and the EIP address configured on the external network card can be viewed directly through the virtual machine's operating system. At the same time, the routes that need to be sent for the two IP address subnets are automatically sent to the virtual machine's operating system; the next hop for the virtual machine's default route is specified as the gateway of the VLAN200 Internet subnet, allowing the virtual machine to communicate with the Internet via the `106.75.236.2` IP address and perform intranet communication with the Oracle and HPC high-performance servers in the physical network area via the `192.168.1.2` IP address.

The entire communication process is carried out directly through the physical network card of the physical machine where the virtual machine is located. Under the guarantee of the performance of the physical network card and physical switch, the best forwarding performance of the physical network hardware can be achieved, improving the forwarding capability of the virtual machine for external communication. At the same time, all external IP traffic can be controlled by the platform security group within the platform to ensure the security of virtual machine access to external networks.

### 9.1.4 Feature Highlights

EIP is a floating IP that can be automatically drifted to healthy nodes when a failure occurs in the virtual machine, continuing to provide external network access services for the virtual machine or other virtual resources.

![eip](/assets/images/userguide/eip.png)

When a physical host that a virtual machine is on fails, the intelligent scheduling system will automatically perform a live migration operation on the virtual machine on the failed host, i.e., the failed virtual machine will be restarted on another healthy host and provide normal business services. If the virtual machine is bound to an external network IP, the intelligent scheduling system will also drift the external network IP address and related flow table information to the physical host where the virtual machine is migrated, ensuring network communication is reachable.

* Support platform administrators to customize the external network IP resource pool, i.e., customize the external network IP segment, and support configuring the routing policy of the segment. When tenants apply for external network IPs bound to virtual resources, the traffic of the destination routing address is automatically routed through the bound external network IP as the network egress.
* The external network IP segment supports issuing default routes and specified routes. Issuing a default route represents all traffic being routed through the bound external network IP, and a specified route is where an administrator specifies the traffic of the destination address to be routed through the bound external network IP.
* **Provide IPv4/IPv6 dual-stack capability. Administrators can customize and manage IPv4 and IPv6 network segment resource pools, and support binding both IPv4/IPv6 addresses to virtual machines simultaneously to provide dual-stack network communication services for virtual machines.**

- Support permission control for external network IP segments, which can be specified for all tenants or some tenants to use. Tenants who have not been specified are not authorized to apply for and use EIP for network segments.
- EIP has the feature of elastic binding, supporting binding to virtual machine resources such as NAT gateways, load balancers, VPN gateways, and can be unbound and bound to other resources at any time.
- Virtual machines support binding 50 external network IPv4 and 10 external network IPv6 addresses, with the first default-routed external network IP as the virtual machine's default network egress.
- Provide external network IP segment acquisition service, support tenants manually specifying IP addresses to apply for EIP, and provide IP address conflict detection to facilitate user business network address planning.
- Platform administrators can customize the bandwidth specifications of the external network IP segment, and tenants can configure the bandwidth upper limit of the external network IP within the range of the bandwidth specifications.
- Currently, only QEMU-Agent machines support binding direct mode elastic external network IPv6.

External network IPs have data center properties and only support binding to virtual resources in the same data center. Users can apply for EIP through the platform, and perform binding, unbinding, and adjusting the bandwidth on the EIP.

## 9.2 Applying for External Network IP

Applying for EIP refers to tenants applying for an IPv4 or IPv6 external network IP address from the administrator's custom external network IP segment through the console, and binding the IP address to virtual machines, load balancers, NAT gateways, VPN gateways, MySQL and Redis resources to provide external network access capability for virtual resources.

When applying for EIP, the IP version, subnet, IP address, resource name, and bandwidth upper limit need to be specified. You can enter the "External Network IP" resource console through the navigation bar and enter the wizard page by clicking "Apply for External Network IP", as shown in the figure below:

![createEIP](/assets/images/userguide/createEIP.png)

1. Select and configure the basic settings and management information for the applied external network IP:

* Name/Description: The name and description of the external network IP to be applied for, and a name must be specified during the application process.

- Billing Method: The billing method for the resource, which currently only supports bandwidth billing, where the bandwidth is used as the billing object and the upper limit of the egress, with no restrictions on traffic.
- IP Version: The IP version of the external network IP address, supporting IPv4 and IPv6.
  - When selecting IPv4, only the IPv4 segment will be displayed;
  - When selecting IPv6, only the IPv6 segment will be displayed. If the platform administrator has not defined an IPv6 segment, the IP version will only support IPv4.
- Network Segment: The network segment to which the applied external network IP belongs, which is customized by the platform administrator and displays the IP segments of the network segment. Manually specified IP addresses must be within the IP address range of the network segment.
- IP Address: Manually specify the IP address to apply for EIP, and the specified IP address must be within the IP range of the selected network segment.
- Bandwidth: The upper limit of the egress bandwidth for the applied EIP resources, with specifications determined by the platform administrator, and the unit is Mbps.

2. Select the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to apply for and create EIP.

- Purchase Quantity: Create multiple EIP addresses in batches according to the selected configuration and parameters, currently supporting the creation of up to 10 EIPs in batches.
- Payment Method: Choose the billing method for the EIP, supporting three different methods: hourly, yearly, and monthly, and select the appropriate payment method according to your needs.
- Total Cost: Display the cost of the EIP resources according to the selected payment method.
- Buy Now: After clicking "Buy Now", the EIP resource list page will be returned, where you can view the application process for the EIP. Typically, the "applying" status will be displayed first and then transition to the "Not bound" status within a few seconds, indicating that the application was successful.

## 9.3 View external IP

By accessing the external IP console through the navigation bar, you can view the resource list of external IPs and enter the details page to view detailed information and operation logs of external IPs by their name and ID.

### 9.3.1 External IP List

The external IP list displays a list of all EIP resources under the current account, including their name, resource ID, IP address, IP version, bandwidth, bound resources, routing type, billing method, creation time, expiration time, status, and operation options, as shown in the figure below:

![eiplist](/assets/images/userguide/eiplist.png)

- Name/ID: The name and unique identifier of the EIP resource.
- IP Address: The IP address and subnet name of the EIP resource. If the IP version is IPv6, it will be displayed as an IPv6 address.
- IP Version: The IP version of the EIP address, such as IPv4 or IPv6 .
- Bandwidth: The maximum outbound bandwidth specified when applying for the EIP resource.
- Bound Resources: The name and ID of the resources that the EIP has been bound to; the resource type can be a virtual machine, NAT gateway, load balancer, or VPN gateway.
- Routing Type: The routing type defined by the network segment to which the EIP address belongs, including default routing and non-default routing (specified or unspecified routing).
  - Default routing is bound to a virtual resource and automatically issues a route with a destination address of 0.0.0.0/0, that is, the default route;
  - Non-default routing is bound to a virtual resource and only issues routes with user-specified destination addresses.
- Billing Method: The payment method for the EIP address, including hourly, yearly, and monthly.
- Creation Time/Expiration Time: The creation time and fee expiration time of the EIP resource.
- Status: The status of the EIP resource, including being created, unbound, binding, bound, unbinding, modifying bandwidth, deleting, etc.

The operation options on the list refer to the operations on a single external IP address, including binding, unbinding, modifying bandwidth, and deleting. You can search and filter the external IP list through the search box, supporting fuzzy search.

For the convenience of tenants to manage their resources, the platform supports downloading all external IP resource lists owned by the current user as an Excel spreadsheet; at the same time, it supports batch unbinding and deletion of external IPs.

### 9.3.2 Details of External IP

On the External IP resource list, clicking on the name or ID will take you to the overview page where you can view detailed information about the current External IP. You can also switch to the operation log page to view the operation log for the current External IP, as shown on the Overview page:

![eipdetails](/assets/images/userguide/eipdetails.png)

- Basic Information: This section provides basic information about the External IP address, including its name, ID, IP address, IP version, bandwidth, bound resources, status, creation time, and alarm template information.

  - You can click on the button to the right of the name to modify the name and remark of the External IP;
  - You can click on the button to the right of the alarm template to modify the alarm template associated with the External IP. By default, it is bound to the Default alarm template.

  > The alarm template can only be modified when the External IP is bound to a virtual machine resource.

- Monitoring Information: This section displays monitoring information for the current External IP address, including network card outbound bandwidth utilization, outbound/inbound bandwidth, and outbound/inbound packet volume.

- Operation Log: The Operation Log page displays the operation log for the current External IP. It provides custom time-level log display and allows for fuzzy searching of logs. By default, it provides operation logs within the past two weeks and allows you to switch date periods to view operation logs for different time periods.

## 9.4 Binding Public IP

Binding public IP refers to binding EIP addresses to virtual machines, NAT gateways, load balancers, and VPN gateways, providing external network service capabilities for virtual resources.

- Virtual machines can bind up to 50 IPv4 and 10 IPv6 public IP addresses. The first IP address with a default route is used as the default network egress of the virtual machine.
- IPv6 public IP addresses can only be bound to virtual machines and cannot be bound to other resources.
- After binding a public IP address to a virtual machine, the system will configure the external IP address and its subnet's routing directly to the virtual machine's default external network card. All bound public IP addresses and related routing information can be viewed directly through the virtual machine's operating system.
- NAT gateways support binding one IPv4 address with a default route, but do not support binding IPv6 public IP addresses.
- VPN gateways support binding one IPv4 address with a default route, but do not support binding IPv6 public IP addresses.
- Load balancers support binding one IPv4 address with a default route, but do not support binding IPv6 public IP addresses.

A public IP can only be bound to one virtual resource at a time. Only unbound public IP addresses can be bound, and the bound resource must be in a running, valid, or shutdown state. Users can enter the public IP binding wizard page by selecting "Bind" from the external IP resource list operation options, as shown in the following figure:

![eiplink](/assets/images/userguide/eiplink.png)

When binding, you need to select the type of resource to be bound and the bound resource object:

(1) Resource type: refers to the type of the bound object, supporting binding to virtual machines, load balancers, NAT gateways, and VPN gateways.

(2) Resource object: refers to the bound resource object. Different types of resources have different selectable resource objects.
- Virtual machine: you can select the virtual machine resource to be bound based on the virtual machine's name and internal IP address. Virtual machines that have already bound 50 IPv4 or 10 IPv6 addresses cannot be selected.
- Load balancer: you can select the load balancer resource to be bound based on its name and ID. Only unbound external type load balancing instances can be selected, and binding IPv6 public IP to load balancers is not supported.
- NAT gateway: you can select the NAT gateway resource to be bound based on its name and ID. Only unbound external IP address NAT gateways can be selected, and binding IPv6 public IP to NAT gateways is not supported.
- VPN gateway: you can select the VPN gateway resource to be bound based on its name and ID. Only unbound external IP address VPN gateways can be selected, and binding IPv6 public IP to VPN gateways is not supported.

During the binding process, the status of the public IP address will be "**Binding**". When the status changes to "**Already bound**", it means that the binding is successful. Users can also view the information of the bound public IP address through the bound resource. Usually, the binding will be completed immediately, and you can test whether the binding is effective by `pinging the public IP` or related network tools. If the network is not reachable, check whether the security group rules of the bound resource allow network access, and for virtual machines, check the rule policies of both the internal and external security groups according to the scenario.

## 9.5 Unbinding Public IP

Unbinding public IP refers to separating an EIP address from a virtual resource and re-binding it to other virtual resources. Only bound external IP resources can be unbound. Users can perform the unbinding operation of public IP through the operation options in the public IP list, as shown in the following figure:

![eipunlink](/assets/images/userguide/eipunlink.png)

When unbinding, the status of the public IP will change to "Unbinding", and when the status of the public IP address changes to "Unbound", it means that the unbinding is successful. The network or service of the resource that has been unbound may be affected.

- After the external IP address of a virtual machine is unbound, it will not affect the internal network communication of the virtual machine itself. If the unbound external IP address is the default network egress of the virtual machine, the system will automatically select the next external IP address with a default route as the default network egress of the virtual machine.
- If the NAT gateway only binds one external IP address, after the external IP address is unbound, it will affect the network services of the NAT gateway, and all SNAT and DNAT services will fail, which means that resources associated with SNAT and DNAT rules cannot access the external network or provide port mapping services to the outside world through the NAT gateway. You need to rebind a public IP address to make it effective.
- After the external IP address of a load balancer is unbound, it will affect the network services of the load balancer, and users cannot access the services deployed in the service nodes through the original external IP address.
- After the external IP address of a VPN gateway is unbound, it will affect the network services of the VPN gateway, and the two ends of IPSecVPN cannot communicate with each other. You need to rebind the external IP address and modify the IP address of the peer gateway at the peer platform or data center VPN gateway to the newly bound EIP before connecting normally.

## 9.6 Adjusting Bandwidth

Adjusting bandwidth refers to upgrading or downgrading the maximum bandwidth limit of a public IP to meet different business needs for bandwidth. The adjustable bandwidth specifications are customized by cloud platform administrators on the management console, and different public IP resource pools support different bandwidth specification configurations.

Bandwidth can be adjusted online or offline, which means that the bandwidth of a public IP can be adjusted in real-time without affecting the network communication of bound resources. Depending on the payment method, bandwidth adjustments may affect costs and effective times.

- For pay-per-hour elastic IPs, upgrading or downgrading bandwidth will take effect from the next billing cycle;
- For annual or monthly pay elastic IPs, upgrading bandwidth takes effect immediately and the price difference is automatically made up;
- For annual or monthly pay elastic IPs, downgrading bandwidth is allowed only until the last day of the current billing cycle, and it takes effect from the next billing cycle.

Users can enter the modification wizard page to adjust bandwidth by selecting the "**Adjust Bandwidth**" option in the public IP resource list, as shown in the following figure:

![upbandwidth](/assets/images/userguide/upbandwidth.png)

When the bandwidth is being modified, the EIP status changes to "Adjusting Bandwidth", and after successful adjustment, it changes to "Not bind" or "Already bound" status. In a private cloud environment, a public IP address can be simulated by an "internal IP address", that is, the administrator assigns a NAT internal IP address segment to the public IP segment issued by the cloud platform on the physical network layer, so the actual bandwidth of the public IP address is controlled at the physical network layer.

The platform's bandwidth adjustment acts only as an upper limit for the bandwidth that an IP address can communicate with. If the public IP address segment is used for pure internal network communication with an IDC data center physical network, the bandwidth specification can be set to the maximum internal network bandwidth, such as 10000Mbps.

## 9.7 Modifying Alarm Templates

Modifying alarm templates involves configuring alarms for monitoring data of public IPs. By defining metrics and thresholds in the alarm template, alarms can be triggered when public IP-related metric faults or threshold exceedances occur, notifying relevant staff to handle faults and ensuring normal network communication for public IPs.

![eipalarm](/assets/images/userguide/eipalarm.png)

Users can click the button on the right side of the alarm template in the public IP details overview page to modify the alarm template. In the modify alarm template wizard, select a new alarm template for the public IP and click "OK" to take effect immediately.

> Only after a public IP address is bound to a virtual resource can the alarm template be modified.

## 9.8 Modifying Public IP Names

The name and remarks of a public IP resource can be modified at any state. Users can click the "Edit" button on the right side of the public IP resource list name to modify it.

## 9.9 Deleting Public IPs

Users can delete unbound public IP addresses in their accounts through the console, and batch deletion is supported. Only unbound public IP resources can be deleted. The deleted public IPs will automatically enter the "**Recycle Bin**" and can be recovered or permanently destroyed.

Deletion can be done through the "Delete" option in the public IP list, as shown in the following figure:

![eiprm](/assets/images/userguide/eiprm.png)

## 9.10 Renewing Public Network IP

Users can manually renew their public network IP addresses, and the renewal operation is only for the resource itself, not for any additional resources associated with it, such as virtual machines, NAT gateways, load balancers, VPN gateways, etc. When the associated resources expire, they will be automatically unbound. To ensure normal business use, it is necessary to renew the relevant resources in a timely manner.

![reneweip](/assets/images/userguide/reneweip.png)

When renewing a public network IP address, users can change the renewal method, but only from a short period to a long period. For example, a monthly renewal method can be changed to monthly or yearly.

Fees for renewing a public network IP address are charged according to the duration of the renewal, which matches the billing method of the resource. When the billing method of the public network IP is "hourly," the renewal duration is specified as one hour; when the billing method is "monthly," the renewal duration can be selected from one to eleven months; when the billing method is "yearly," the renewal duration is from one to five years.

## 9.11 NAT-EIP

When creating a VPC, enabling the VPC gateway supports the NAT mode EIP function, which consumes 2 cores and 2GB of resources, as shown in the following figure:

![vpc-nat](/assets/images/userguide/vpc-nat.png)

Applying for NAT-EIP is the same as applying for a direct access mode public network IP. Virtual machines created with the VPC gateway enabled support binding to NAT-EIP, as shown below:

![nat-eip](/assets/images/userguide/nat-eip.png)

For existing VPC resources that do not require NAT-EIP, the NAT-EIP function can be disabled by disabling the VPC gateway. For existing VPC gateways, NAT-EIP can also be used by enabling the VPC gateway. Virtual machines can be bound to both direct access mode public network IPs and NAT mode public network IPs at the same time, as shown below:

![vm-eip](/assets/images/userguide/vm-eip.png)
