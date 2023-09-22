---
layout: default
title: Security Group
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/nsg/
nav_order: 10
---
# 10. Security Group

## 10.1 Introduction to Security Groups

### 10.1.1 Overview

A security group is like a virtual firewall similar to [IPTABLES](https://en.wikipedia.org/wiki/Iptables), which provides access control rules for incoming and outgoing traffic, defining which networks or protocols can access resources. It is used to restrict network access traffic of the virtual resources and supports IPv4 and IPv6 dual stack restrictions. Security groups provide necessary security assurance for cloud platforms.

The platform security group is based on the Linux Netfilter subsystem and implemented by adding flow table rules to the [OVS](http://www.openvswitch.org/) table. The compute node IPv4 and IPv6 packet forwarding function needs to be enabled. For each added access control rule, a flow table rule is generated based on the network card as a matching condition, which is used to control the traffic entering the OVS to ensure the network security of virtual resources.

Security groups only apply to virtual machines, elastic network cards, load balancers, and NAT gateways with the same security requirements within the **same data center**. Its working principle is shown in the following figure:

![Security_group](/assets/images/userguide/Security_group.png)

Security groups have an independent lifecycle and can be bound together with virtual machines, elastic network cards, load balancers, and NAT gateways to provide secure access control. When the virtual resources bound with it are destroyed, the security group will automatically unbind.

- The security protection of virtual machines provided by security groups is targeted at a network card, that is, the security group is bound with the default virtual network card or elastic network card of the virtual machine and sets access control rules respectively to limit the incoming and outgoing network traffic of each network card.
- As shown in the security group principle diagram, the security group is bound with the virtual external network card that provides external network IP services, and adds inbound and outbound rules to filter the north-south (virtual machine external network) access traffic.
- Security groups are bound with virtual network cards or elastic network cards that provide private network services and control east-west (between virtual machines and elastic network cards) network access by adding inbound and outbound rules.
- Security groups are associated with external network load balancers, and by adding inbound and outbound rules, they can restrict and filter the inbound and outbound IP traffic of the external network load balancer to ensure the flow security of the external network load balancer.
- Security groups are bound with NAT gateways, and by adding inbound and outbound rules, they can restrict the traffic entering the NAT gateway to ensure the reliability and security of the NAT gateway.
- A security group can be bound to multiple virtual machines, elastic network cards, NAT gateways, and external network load balancing instances simultaneously.
- Virtual machines support binding one internal network security group and one external network security group, corresponding to the default internal network card and external network card of the virtual machine respectively. The external network security group is effective for all external IP addresses bound to the virtual machine.
- An elastic network card can only be bound with one security group, which is independent of the security group bound with the default network card of the virtual machine and restricts the traffic of the corresponding network card respectively.
- External network load balancers and NAT gateway instances only support binding one security group and can switch security groups to apply different network access rules.

To create a virtual machine, an external network security group must be specified. The access control rules of the security group can be modified at any time and the new rules take effect immediately. The rules for inbound and outbound directions of the security group can be adjusted according to requirements. The management of the security group's entire lifecycle is supported, including creating, modifying, and deleting security groups, as well as creating, modifying, and deleting security group rules.

### 10.1.2 Security Group Rules

Security group rules can control inbound and outbound traffic that is allowed to reach security-group associated resources, providing dual-stack control capability and supporting effective filtering and control of TCP, UPD, ICMP, GRE and other protocol packets with IPv4/IPv6 addresses.

Each security group supports configuring multiple rules, which take effect on resource access in sequence according to priority. **When there are no rules, the security group will default to rejecting all traffic; when there are rules, all access traffic is rejected by default except for the generated rules.**

**Supports stateful security group rules, which can separately set inbound and outbound rules to control and restrict inbound and outbound traffic of bound resources.** Each security group rule consists of six elements: protocol, port, address, action, priority, and direction:

- Protocol: Supports TCP, UDP, ICMPv4, and ICMPv6 packet filtering for four protocol types.
  - ALL represents all protocols and ports, ALL TCP represents all TCP ports, and ALL UDP represents all UDP ports.
  - Supports shortcut protocol specification, such as FTP, HTTP, HTTPS, PING, OpenVPN, PPTP, RDP, SSH, etc.
  - ICMPv4 refers to the communication traffic of the IPv4 version network; ICMPv6 refers to the communication traffic of the IPv6 version network.
- Port: The TCP/IP port for local virtual resources visited by the source address or the target address visited by local virtual resources.
  - The port range for TCP and UDP protocols is from 1 to 65535.
  - ICMPv4 and ICMPv6 do not support port configuration.
- Address: The network packet source address accessing the security-group bound resources or the target address visited by the security-group bound virtual resources.
  - When the direction of the rule is an inbound rule, the address represents the source IP address range that accesses the bound virtual resource, supporting IPv4 and IPv6 address ranges.
  - When the direction of the rule is an outbound rule, the address represents the target IP address range visited by the bound virtual resource, supporting IPv4 and IPv6 address ranges.
  - Supports IP addresses and network segments represented in CIDR notation, such as `120.132.69.216`, `0.0.0.0/0`, or `::/0`.
- Action: The packet processing policy when the security group takes effect, including "Accept" and "Reject" actions.
- Priority: The effective order of rules inside the security group, including high, medium, and low three-grade rules.
  - Security groups take effect from high to low priority, with high priority rules taking precedence.
  - For rules with the same priority, more precise rules take precedence.
- Direction: The traffic direction corresponding to the security group rule, including outbound traffic and inbound traffic.

**Security groups support data flow table states. When a rule allows certain request communication, the returned data flow is automatically allowed without being affected by any rules. That is, security group rules only apply to new connections and allow bidirectional communication for established connections by default.** For example, if an inbound rule allows any address to access port 80 of the virtual machine external network IP via the Internet, the returned data flow (outbound traffic) for accessing the virtual machine's 80 port will be automatically allowed without needing to add an outbound allowing rule for this request.

> Note: It is recommended to set concise security group rules to effectively reduce network failures.

## 10.2 Security Group Management

### 10.2.1 Creating-Security-Group

When the default security groups provided by the system cannot meet your needs, you can specify a security group name and add relevant security group rules to quickly create a user-independent security group that can be associated or bound to relevant resources to provide access control for internal or external network resources and ensure network access security.

Users can enter the "security group" resource console through the navigation bar, and enter the security group creation wizard page by clicking on "**Create Security Group**", as shown in the figure below:

![createsg](/assets/images/userguide/createsg.png)

You can select and configure the security group name according to the prompts on the wizard page, and configure security group rules based on your needs, including protocol type, port, address, action, priority, direction, and description.

The security group name refers to the name identification of the security group currently being created. Adding rules means adding corresponding inbound and outbound traffic rules for the security group, which can be added in bulk or added after the security group is created.

- Protocol: A rule only supports one protocol, and you can choose ALL or ALL TCP, ALL UDP, etc.
- Port: Ports support selecting custom ports or port groups. The port group list displays the port groups created under the current tenant, and you can go to the port group page to create them.
- Address: The address field supports selecting custom IP addresses or IP address groups. The IP address group list displays the IP groups created under the current tenant, and you can go to the IP group page to create them.
- Action: When the protocol, port, address, and direction of the rule are the same, it does not support configuring both Accept and Reject actions at the same time.
- Direction: The traffic direction of the rule, including inbound and outbound, and only one direction can be selected for each rule.
- Description: The description of the current security group rule.

After clicking "Confirm", you will automatically return to the security group list page. On the list page, you can view the creation process of the newly created security group. When the status of the security group changes from "Creating" to "Available", it means that the creation is successful.

### 10.2.2 Viewing Security Groups

You can enter the security group resource console through the navigation bar to view the current account's security group resource list. You can click on the security group name in the list to enter the details page to view basic information about the security group, security group rules, and bound resources.
  
#### 10.2.2.1 Security Group List

The security group list page displays the current account's security group resource list and related information, including name, ID, rule count, bound resource count, creation time, status, and operation options, as shown in the figure below:

![sglist](/assets/images/userguide/sglist.png)

- Name/ID: The name and globally unique identifier of the security group.
- Rule Count: The number of security group rules that have been added to the security group, represented by a number.
- Bound Resource Count: The number of resources that have been bound to the security group, represented by a number. If no resources have been bound, it displays "0".
- Creation Time: The time when the security group was created.
- Status: The running status of the security group, including Available, Creating, Deleting, etc.

The operation options on the list allow you to delete individual security groups, and support batch deletion of security groups. You can use the search box to search and filter the security group list, and support fuzzy search.

#### 10.2.2.2 Security Group Details

On the security group resource list, you can click on the security group name to view the details and security group rule information of the current security group. You can also switch to the resource page to view the information of the resources that are currently bound to the security group, as shown in the overview page below:

![sgdetails](/assets/images/userguide/sgdetails.png)

- Basic Information: The basic information of the current security group, including the name, ID, rule count, bound resource count, creation time, and other information.
- Security Group Rule Management: The access control rule management of the current security group, including adding, viewing, editing, deleting, etc., see [Security Group Rule Management](#_103-Security-group-rule-management) for details.
- Bound Resources: The list of resources that are currently bound to the security group, see [Bound Resources](#_10223-Bound-resources) for details.

#### 10.2.2.3 Bound-resources

Bound resources refer to the list of resources that have been associated with a security group. Users can view information about the virtual resources that are currently bound or associated with the security group by accessing the "**Resources**" tab on the security group details page.

![sgresource](/assets/images/userguide/sgresource.png)

As shown in the list in the above figure, the list of bound resources includes the resource name, type, ID, and other information. The resource types include virtual machines, elastic network interfaces, NAT gateways, and load balancers.

### 10.2.3 Binding Virtual Resources in Batches

Users can bind security groups to virtual resources in batches, and the bound resources can be viewed on the "**Resources**" tab of the security group details page. Users can access the security group binding wizard page by clicking the "**Bind**" button in the security group list operation bar.

* Support binding of internal and external security groups to virtual machines as shown in the following figure:

![vm_bindsg](/assets/images/userguide/vm_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Bound Network Types: the types of networks that the security group supports binding virtual machines to, including external and internal networks;
- Resource Information: the resource information list displays a list of virtual machine resources and allows for multiple selections.

* Support binding of security groups to load balancers as shown in the following figure:

![lb_bindsg](/assets/images/userguide/lb_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Resource Information: the resource information list displays a list of load balancing resources and allows for multiple selections.

* Support binding of security groups to NAT gateways as shown in the following figure:

![natgw_bindsg](/assets/images/userguide/natgw_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Resource Information: the resource information list displays a list of NAT gateway resources and allows for multiple selections.

* Support binding of security groups to network cards as shown in the following figure:

![nic_bindsg](/assets/images/userguide/nic_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Resource Information: the resource information list displays a list of network card resources and allows for multiple selections.

* Support binding of security groups to object storage as shown in the following figure:

![oss_bindsg](/assets/images/userguide/oss_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Resource Information: the resource information list displays a list of object storage resources and allows for multiple selections.

* Support binding of security groups to file storage as shown in the following figure:

![fs_bindsg](/assets/images/userguide/fs_bindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource Name: the name of the security group;
- Bound Resource Types: the types of resources that the security group supports binding, including virtual machines, load balancers, NAT gateways, network cards, object storage, and file storage;
- Resource Information: the resource information list displays a list of file storage resources and allows for multiple selections.

### 10.2.4 Batch Unbinding of Resources

Users are supported to unbind security groups in batches. The "**Unbind**" button in the operation column of the security group list can be clicked to enter the unbinding security group wizard page, as shown below:

![unbindsg](/assets/images/userguide/unbindsg.png)

- Resource ID: the globally unique identifier of the security group;
- Resource name: the name of the security group;
- Unbinding resources: displays the list of resources that can be unbound and allows for multiple selections.

### 10.2.5 Modify Security Group Name

The name and remark of the security group resource can be modified at any state. Click the "Edit" button on the right side of each security group name on the security group resource list page to modify it.

### 10.2.6 Delete Security Group

Users are supported to delete security group resources that are not bound to any resources. After the security group is deleted, it will be completely destroyed. Before deleting it, make sure that the security group is not bound or associated with any resources. The security group can be deleted through the "Delete" option in the operation items on the security group list page, as shown below:

![sgrm](/assets/images/userguide/sgrm.png)

## 10.3 Security-group-rule-management

### 10.3.1 Create Rule

To provide network security access control for bound resources, the main means is to formulate reasonable security group rules. Each security group supports configuring multiple rules, and resource access is effective according to priority. **When there are no rules, the security group will default to rejecting all traffic; when there are rules, in addition to the generated rules, it defaults to rejecting other access traffic.**

Users can specify the protocol type, port, address, action, priority, direction, description and other information to add rules. The "**Create Rule**" on the security group details page can be clicked to enter the create rule wizard page. The specific operation is the same as adding rules in [Creating Security Group](#_1021-Creating-Security-Group), and security group rules can be created according to specific business network security control requirements.

### 10.3.2 View Rules

The list of rules generated for the current security group can be viewed through the rule list on the security group details page, and existing rules can be edited or deleted using the options in the list. The rule list includes protocol type, port, address, action, priority, direction, description, creation time, and operation options, as shown in the figure below:

![rulelist](/assets/images/userguide/rulelist.png)

### 10.3.3 Edit Rules

When the existing security group rules do not meet business requirements, modifications and changes can be made through the "**Edit**" option in the security group rule list's operation menu. The modification items are the same as the parameters specified when creating new rules and can be modified according to actual needs.

- When the protocol type is ALL or ICMPv4/ICMPv6, the port cannot be selected and is displayed as `"/"`.
- Addresses support IP addresses and CIDR IP network segment formats. If all IP addresses need to be specified, they can be configured as `0.0.0.0/0` or `::/0`.

> Edited rules take effect immediately and will affect the network access of bound resources. Please proceed with caution.

### 10.3.4 Delete Rules

If an existing security group rule needs to be deleted, it can be done through the "**Delete**" option in the security group rule list's operation menu, and the deleted rule will be destroyed immediately. To avoid affecting business operations, it is recommended to confirm whether the security group rule needs to be deleted before doing so. After deleting a security group rule, the number of rules in the security group information will be re-counted and display the latest number of rules.

## 10.4 IP Groups

### 10.4.1 Create IP Groups

IP groups can be created and configured according to the prompts on the wizard page, including IP group name, IP address, IP group remarks, and project groups.

The IP group name refers to the name identifier of the IP group being created, and multiple IP addresses can be added, as shown in the figure below:

![createipgroup](/assets/images/userguide/createipgroup.png)

- IP Group Name: The name of the IP group.
- IP Address: The address field supports batch input of multiple IP addresses, with multiple IP addresses entered on separate lines. IP addresses support the following formats:
    - Single IP: 10.0.0.1 or FF05::B5
    - Network segment: 10.0.1.0/24 
    - Continuous address range: 10.0.0.1-10.0.0.100
- IP Group Remarks: Remarks for the current IP group.
- Project Group: Can be bound to project groups created under the current tenant.

After clicking "Confirm", it will automatically return to the IP group list page. When the status of the IP group is "available", it indicates that the creation was successful.

### 10.4.2 View IP groups

Users can view the information of created IP groups, including name, resource ID, IP/cidr block, number of bound resources, project group, creation time and operation options, as shown in the figure below:

![ipgrouplist](/assets/images/userguide/ipgrouplist.png)

- Name: The name of the IP group;
- Resource ID: The globally unique identifier of the IP group;
- Status: The status of the IP group, including available and failed;
- IP/CIDR Block: The IP of source data (inbound) or destination data (outbound), which is represented by IP address, CIDR block or continuous address range, such as 10.0.0.1 or 192.168.0.0/16;
- Number of Bound Resources: The number of resources bound to the IP group, and you can hover over it to see the specific security group rule ID list. Click on the corresponding security group ID to jump to the details list of that security group;
- Project Group: The project group bound to the IP group;
- Creation Time: The creation time of the IP group.

The operation options on the list refer to the operations on a single IP group, including editing and deleting. Deletion operation can only be performed when the number of resources bound to the IP group is 0. At the same time, for the convenience of maintaining resources, batch deletion of port groups is supported.

### 10.4.3 Edit IP groups

Users can edit the IP address of the IP group. The name of the IP group cannot be modified. Click the "**Edit**" button in the operation column of the IP group list to modify, as shown in the figure below:

![updateipgroup](/assets/images/userguide/updateipgroup.png)

### 10.4.4 Delete IP groups

Users can delete IP groups in the account through the console, and batch deletion of IP groups is supported. Click "**Delete**" in the operation options of the IP group list to perform the operation, as shown in the figure below:

![ipgrouprm](/assets/images/userguide/ipgrouprm.png)

## 10.5 Port Groups

### 10.5.1 Create Port Groups

You can create and configure port groups based on the prompts on the wizard page, including port group name, protocol ports, project group, etc.

The port group name refers to the name identifier of the port group currently being created, and multiple protocol ports can be added, as shown in the figure below:

![createportgroup](/assets/images/userguide/createportgroup.png)

- Port Group Name: The name of the port group;
- Protocol Ports: Supports inputting multiple protocol ports with line breaks. The format of ports is as follows:
    - Single port: 80
    - Multiple single ports: 80,443
    - Continuous port range: 3306-20000
- Project Group: Can be bound to project groups created under the current tenant.

After clicking "Confirm", it will automatically return to the port group list page. When the status of the port group is "Available", it means that the creation is successful.

### 10.5.2 View Port Groups

Users can view the information of created port groups, including name, resource ID, port, number of bound resources, project group, creation time and operation options, as shown in the figure below:

![portgrouplist](/assets/images/userguide/portgrouplist.png)

- Name: The name of the port group;
- Resource ID: The globally unique identifier of the port group;
- Status: The status of the port group, including available and failed;
- Port: Displayed in the format of protocol: port, indicates the port of source data (inbound) or destination data (outbound), and the value range is 1-65535;
- Number of Bound Resources: The number of resources bound to the port group, and you can hover over it to see the specific security group rule ID list. Click on the corresponding security group ID to jump to the details list of that security group;
- Project Group: The project group bound to the port group;
- Creation Time: The creation time of the port group.

The operation options on the list refer to the operations on a single port group, including editing and deleting. Deletion operation can only be performed when the number of resources bound to the port group is 0. At the same time, for the convenience of maintaining resources, batch deletion of port groups is supported.

### 10.5.3 Edit Port Groups

Users can edit the protocol ports of the port group. The name of the port group cannot be modified. Click the "**Edit**" button in the operation column of the port group list to modify, as shown in the figure below:

![updateportgroup](/assets/images/userguide/updateportgroup.png)

### 10.5.4 Delete Port Groups

Users can delete port groups in the account through the console, and batch deletion of port groups is supported. Click "**Delete**" in the operation options of the port group list to perform the operation, as shown in the figure below:

![portgrouprm](/assets/images/userguide/portgrouprm.png)

