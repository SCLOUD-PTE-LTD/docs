---
layout: default
title: Core Functionality
has_children: false
parent: Private Cloud
permalink: /private-cloud/core-functionality
nav_order: 3
---

# Core Functionality

The overall product architecture of SCloudStack platform consists of basic hardware facilities, virtual core engine, intelligent scheduling system, core product resources, unified cloud management platform and operation and maintenance management platform, providing cloud platform management and services for platform tenants, administrators and operation personnel.

### Infrastructure: Servers, switches, and storage devices used to host the SCloudStack platform.
The platform supports and is compatible with general-purpose x86, ARM and MIPS architecture hardware servers, regardless of server and hardware brand;

Supports disk storage such as SSD, `SATA`, SAS, etc., as well as computing and storage hyper-converged nodes and interconnected disk array devices, without vendor lock-in.

Supports access to network devices such as Huawei, Cisco, and H3C routers, and all network functions are defined through SDN software, requiring only physical switches to support features such as VLANs, trunks, IPv6, port aggregation, and stacking.

Supports hybrid cloud access and adapts to existing hardware resources, making full use of resources and seamlessly connecting with existing resource services.

### Virtual Core Engine: The operating system core, virtualized computing, storage, and network implementation and logic that carry the platform core.
Kernel modules: server operating systems and kernel modules that carry the cloud platform, and reuse the deeply optimized Linux kernel of the public cloud; At the same time, it is compatible with ARM ecosystem UOS, Galaxy Kirin and other server operating systems and cores;

Virtualized computing: Computing virtualization is implemented through `KVM, Libvirt, and Qemu`, supports standard virtualization architecture, provides full lifecycle management of virtual machines, is compatible with x86 and ARM architecture architectures, supports hot upgrade, system reinstallation, CPU over-resolution, GPU transparent transmission, online migration, downtime migration, anti-affinity deployment, and supports importing and exporting virtual machine images to meet the requirements of business migration to the cloud.

Distributed network SDN: Implement virtual networks through `OVS` + `VXLAN`, pure software-defined distributed networks, improve network forwarding performance and virtualize traditional data center physical networks, provide cloud platform resources with VPC isolated network environment, elastic network cards, external IP addresses,  NAT gateways, load balancing, firewalls, VPN connections, hybrid cloud access, network topology and other network functions, and support IPv4 & IPv6 dual-stack.

Distributed Storage SDS: Distributed, high-performance storage is implemented based on Ceph, provides block storage services for the platform, and supports online disk expansion, cloning, snapshot, and rollback functions. At the same time, multiple copies of the underlying data are stored and data re-balancing and fault reconstruction capabilities are supported to ensure performance and data security.

### Intelligent scheduling system
Supports anti-affinity scheduling and deployment strategies to ensure high availability and reliability of services.

Support online migration technology to perceive the status and load information of physical machines in real time;

When the physical host fails or exceeds the load, automatically migrate the virtual machine to a low-load physical host;

When you create a virtual machine, automatically start the virtual machine to a physical host with low load and health according to the service scheduling policy.

Support computing quota allocation and resource preemption to effectively share physical resources under the premise of ensuring fairness;

Supports the control and delivery of network flow tables of platform virtual resources to ensure the performance and availability of distributed network architectures.

### Core product resources

- Region (Data Center): Data center refers to the classification of the physical location of resource deployment, and the data centers are independent of each other, such as Singapore data center and Tokyo data center. The platform supports multi-data center management, and uses a set of management platforms to manage private cloud platforms located in various data centers;
- Cluster: It is used to distinguish the distribution of different resources under a data center, such as x86 computing cluster, ARM computing cluster, SSD storage cluster and `SATA` storage cluster, and multiple clusters can be deployed in one data center;
- Multi-tenancy: The platform supports multi-tenancy mode and provides functions such as tenant isolation, sub-accounts, permission control, quota configuration, and price configuration.
- Sub-accounts and permissions: Multiple sub-accounts in a tenant are supported, resource isolation is supported, and resource management permissions can be controlled for sub-accounts.
- Metered billing: supports on-demand, monthly, and annual billing methods, supports expired renewal and recovery policies, and provides complete billing orders and consumption details.
- Elastic computing: Virtual machines running on physical hosts support the full life cycle functions of virtual machines such as image creation, restart/shutdown/startup, deletion, VNC login, system reinstallation, password reset, hot upgrade, binding external IP addresses and security groups, mounting data disks, and anti-affinity  policy deployment.
- GPU virtual machine: The platform provides GPU device transmission capability, allowing users to create and run GPU virtual machines on the platform, allowing virtual machines to have high-performance computing and graphics processing capabilities.
- Auto scaling: Supports auto scaling, allowing users to define auto scaling policies to automatically increase computing resources (virtual machines) when business needs grow to ensure computing power. Automatically reduce compute resources to save costs when business needs drop. Based on the load balancing and health check mechanisms, it can be applied to both business scenarios where the request volume fluctuates and the service volume is stable.
- Image: The operating system required for the virtual machine to run, providing common basic operating system images such as CentOS, Windows, and Ubuntu; Supports exporting virtual machines as images and rebuilding virtual machines through self-made images; At the same time, it supports the import and export of images, which is convenient for users to customize images;
- EVS disk: A block device that provides persistent storage space for virtual machines based on a distributed storage system. It has an independent life cycle, supports binding/unbinding to multiple virtual machines at will, distributes access based on the network, and supports capacity expansion, cloning, snapshots and other characteristics, providing highly secure, reliable, high-performance, and scalable disks for virtual resources.
- Snapshots: Provides disk snapshots and snapshot rollback capabilities, which can be applied to business scenarios such as disaster recovery backup and version rollback, reducing the risk of data loss caused by mis-operation and version upgrades.
- VPC network: Software-defined virtual VPC for data isolation between tenants, providing custom VPC network, subnet planning, and network topology.
External IP: Used for external IP access of resources such as virtual machines, load balancers, NAT gateways, and VPN gateways, and used to connect with external networks, such as virtual machines accessing the Internet or accessing the physical network of IDC data centers. Supports binding multiple public IP addresses to virtual resources at the same time and provides IPv6 network connection services.
- Security group: a virtual firewall that provides access control rules for inbound and outbound traffic, defines which networks or protocols can access resources, restricts network access traffic to virtual resources, supports TCP, UDP, ICMP and multiple application protocols, and provides necessary security guarantees for cloud platforms.
- ENI: An elastic network interface that can be attached to a virtual machine at any time, supports binding and unbinding, and can be flexibly migrated between multiple virtual machines, providing high-availability cluster construction capabilities for virtual machines, and implementing refined network management and inexpensive failover solutions.
- NAT gateway: An enterprise-level VPC gateway that provides `SNAT` and `DNAT` proxies for cloud platform resources, supports automatic and whitelist network egress modes, and provides port mapping proxy services for VPC networks, enabling external networks to access virtual machines and MySQL through NAT gateways.
- Load balancing: A control service that automatically distributes network access traffic among multiple virtual machines based on the `TCP/UDP/HTTP/HTTPS` protocol, similar to the hardware load balancer of the traditional physical network, which is used to achieve traffic load and high availability among multiple virtual machines, and provides Layer 4 and Layer 7 monitoring and health check services for internal and external networks.
- IPSecVPN: Provides IPSecVPN gateway service, which connects SCloudStack with the internal network of SCloud public cloud, IDC data center, and third-party public cloud through IPSec protocol encryption tunneling technology, provides a secure channel for the two private networks on the Internet, and ensures the security of the connection through encryption. At the same time, the IPSecVPN service can also serve as a bridge between VPCs of the SCloudStack platform.
- Monitoring alarms: Supports monitoring data collection and display in various dimensions such as virtual machines, auto scaling, disks, elastic IPs, NAT gateways, load balancers, IPSecVPN, MySQL, and Redis, and can quickly configure alarm policies and notification rules for resource monitoring metrics through alarm templates.
- Operation log: The operation and audit logs of all resources of the cloud platform and the cloud platform itself support multi-time log collection and display, and provide the cause of operation failure.
- Recycle bin: The location where resources are temporarily stored after they are deleted, and operations such as reclaiming resources, restoring resources, and completely deleting resources are supported.
- Timer: Provides a timer task execution function, which can be used to execute a series of tasks on a regular basis, support the creation of snapshots on a regular basis, and can be executed repeatedly or only once in a specified cycle, and each task supports multiple resource batch operations.

### Unified cloud management platform
The SCloudStack platform provides two ways to access and manage the cloud platform in two ways: web console and API interface.

Through the web console, users can quickly use and manage cloud platform resources, such as virtual machines, elastic ips, load balancing, billing, etc.

Developers can custom-build cloud platform resources through APIs to support seamless migration to the cloud.

### O&M management platform

An O&M operation management platform provided for cloud platform administrators, including tenant management, resource management, accounting management, monitoring and alarming, log auditing, system management, and deployment and upgrade.

Tenant management: It is used to manage the tenant and account information of the entire cloud platform, provide the functions of creating/freezing tenants and recharging, view the resource information, order records, transaction records, and quota prices owned by tenants, and modify the resource quota and product prices of tenants.

Resource management: View and manage all physical and virtual resources of the platform;

- Physical resources include physical data centers, clusters, host resources, storage resources, network IP block resource pools, and mirror resource pools.

- Virtual resources include resources owned by all tenants and sub-accounts, including virtual machines, VPCs, Server Load Balancer, Internet IP addresses, ENIs, Auto Scaling, NAT gateways, MySQL, Redis, IPSecVPN, monitoring alarms, security groups, and recycle bins.

Accounting management: support to view all order records, transaction records, recharge records and global product prices of the platform, support the configuration of the overall product price of the platform, and support the export of financial statements;

Platform monitoring alarm: provides monitoring data of SCloudStack's own physical devices, components and all virtual resources, and supports custom monitoring alarms and notifications;

Log events: Provides operation logs and audit information of all tenants, sub-accounts, and administrators of the platform, and can be filtered and searched in multiple dimensions.

System management: Provides global configuration, specification configuration, and quota management functions of the cloud platform.

- Global configuration includes mailbox settings, recycling policies, network settings, billing, resource management, quota settings, login status, console and website settings, etc.

- Specification configuration supports custom configuration of CPU memory specifications, disk capacity range, Internet IP bandwidth, and MySQL/Redis memory specifications of virtual machines.
- Global quotas can view and modify the quota values of virtual resources in each region.

Basic monitoring service: The peripheral monitoring service of the basic hardware resources of the cloud platform, including the operation status and performance indicators of all network devices, servers, disk arrays and other hardware devices connected to the cloud platform, can also monitor and alarm common services such as MySQL, Redis, and MongoDB in the cluster.