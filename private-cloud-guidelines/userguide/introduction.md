---
layout: default
title: Product Introduction
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/introduction/
nav_order: 1
---
# 1. Product Introduction

## 1.1 Product Overview

Enterprise private cloud platform, providing unified management, resource scheduling, monitoring logs and operation and maintenance of a whole set of cloud resource management capabilities such as virtualization, SDN network, distributed storage, etc., to help enterprises digital transformation.

The platform is based on a public cloud infrastructure, reusing the kernel and core virtualization components, privatizing the public cloud architecture, and featuring autonomous control, stability and reliability, continuous evolution, and open compatibility. Enterprises can quickly build resources and businesses through the console or APIs, support seamless connection with the public cloud, flexibly call public cloud capabilities, and help enterprises quickly build a secure and reliable business architecture.

Private cloud is positioned as a lightweight delivery, with 3 nodes to build a production environment and can be smoothly expanded, not forcibly binding hardware and brands, compatible with X86 and ARM architectures, and providing unified resource scheduling and management, supporting pure software, hyper-converged integrated machines and integrated machine cabinets for various delivery modes, effectively reducing user management and maintenance costs, providing users with a secure, reliable and autonomous controllable cloud service platform.

## 1.2 Core Advantages

* Self-control & Autonomy<br/>— Based on public cloud architecture, core virtualization components are reused and independently developed, with high controllability and reliability verified by tens of thousands of enterprises.

* Stable and reliable<br/>— High availability of platform services, intelligent scheduling of virtual resources, multiple copies of data storage, self-healing distributed network, to escort the business.

* Simple and easy to use<br/>— 3 node builds a production environment, the scale is lightweight and can be horizontally expanded, supports business smooth migration, and helps enterprises easily go to the cloud.

* Open compatibility<br/>— Not bound to hardware brands, compatible with X86 and ARM architectures and ecological adaptation, heterogeneous devices for unified management.

## 1.3 Product Architecture

![Product Architecture](/assets/images/userguide/arch.png)

The overall product architecture of the private cloud platform consists of basic hardware infrastructure, virtual core engine, intelligent scheduling system, core product resources, unified cloud management platform and operation and maintenance management platform, providing cloud platform management and services for platform tenants, administrators and operators.

- **Infrastructure**: Servers, switches, and storage devices used to host the private cloud platform.

  - The platform supports and is compatible with general X86, ARM and MIPS architecture hardware servers, without limiting the server and hardware brands.
  - Support for SSD, SATA, SAS and other disk storage, as well as support for computing storage hyper-converged nodes and disk array devices, without vendor lock-in.
  - Support for general switches, routers and other network devices from Huawei, Cisco, H3C, etc., all network functions are defined by SDN software, only physical switches need to support Vlan, Trunk, IPv6, port aggregation, stacking and other features.
  - Support hybrid cloud access and adapt to customers' existing hardware resources, while fully utilizing resources, seamlessly connecting to existing resources services.
- **Virtual Core Engine**: The implementation and logic of the operating system kernel, virtualized computing, storage, and network that carries the core of the platform.
  - Kernel Module: Server operating system and kernel module that carries the cloud platform, reusing the Linux kernel that has been deeply optimized in public cloud; At the same time, it is compatible with ARM ecology UOS, Galaxy Kirin and other server operating systems and kernel modules.
  - Virtualization Computing: Computing virtualization is achieved through KVM, Libvirt and Qemu, supporting standard virtualization architecture, providing full life cycle management of virtual machines, compatible with X86 and ARM architectures, supporting hot upgrade, system reinstallation, CPU over-division, GPU pass-through, online migration, crash migration, anti-affinity deployment, etc., and supporting import and export of virtual machine images to meet business migration and cloud requirements.
  - Distributed Network SDN: Virtual networks are realized through OVS + VXLAN, pure software-defined distributed networks, which can improve the forwarding performance of the network while virtualizing the traditional data center physical network, providing VPC isolation network environment, elastic network card, external IP, NAT gateway, load balancing, firewall, VPN connection, high availability VIP and other network functions for cloud platform resources, and supporting IPv4 & IPv6 dual stacks.
  - Distributed Storage SDS: Based on Ceph to implement distributed high-performance storage, providing block storage services to the platform, supporting online expansion, cloning, snapshot and rollback functions of cloud disk; meanwhile, the underlying data is stored in multiple copies and supports data rebalancing and fault reconstruction capabilities to ensure performance and data security.
- **Intelligent Scheduling System**
  - Support anti-affinity scheduling deployment policies to ensure high availability and reliability of the business.
  - Support online migration technology, real-time sensing of physical machine status and load information.
  - Automatically migrate virtual machines to low-load physical hosts when physical hosts fail or exceed the load.
  - When creating a virtual machine, automatically start the virtual machine to a low-load healthy physical host according to the business scheduling policy.
  - Support the allocation of computing quotas and resource grabbing, under the premise of fairness, to effectively share physical resources.
  - Network flow table control and distribution to support platform virtual resources, ensuring performance and availability of distributed network architecture.
- **Core Product Resources**
  - **Region (Data Center)**: Data centers refer to the physical location classification of resource deployment. Data centers are independent of each other, such as Wuxi Data Center, Shanghai Data Center, etc. The platform supports multi-data center mana
  - **Cluster**: Used to differentiate the distribution of different resources under a data center, such as x86 computing clusters, ARM computing clusters, SSD storage clusters and SATA storage clusters, a data center can deploy multiple clusters.
  - **Multi-Tenancy**: The platform supports multi-tenancy mode, providing tenant isolation, sub-accounts, permission control, quota configuration, and price configuration.
  - **Sub-accounts and permissions**: Support for a tenant to have multiple sub-accounts, support for resource isolation and permission control for resource management of sub-accounts.
  - **Metering and Billing**: Supports three billing methods: on-demand, monthly, and yearly, supports expiration renewal and recycling strategies, and provides complete billing orders and consumption details.
  - **Virtual Machine**: Virtual machines running on physical hosts, supporting full life cycle functions of virtual machines such as creating from images, restarting/shutting down/starting up, deleting, VNC login, reinstalling the system, resetting passwords, hot upgrades, binding external IPs and security groups, mounting data disks and anti-affinity deployment, etc., while also supporting the ability to make virtual machines as images and disk snapshots, providing quick business deployment and backup capabilities.
  - **GPU Virtual Machine**: The platform provides GPU device pass-through capability, supporting users to create and run GPU virtual machines on the platform, enabling virtual machines to have high-performance computing and graphics processing capabilities.
  - **Elastic Scaling:** Supports elastic scaling, allowing users to define elastic scaling policies to automatically increase computing resources (virtual machines) to ensure computing power when business demand increases, and automatically reduce computing resources to save costs when business demand decreases. Based on load balancing and health check mechanisms, it can be applied to both scenarios with fluctuating requests and stable business volumes.
  - **Translation: Mirror**: The operating system required for virtual machine running provides commonly used basic operating system mirrors such as CentOS, Windows, Ubuntu, etc.; supports exporting virtual machines as mirrors, rebuilding virtual machines through homemade mirrors; at the same time, it supports the import and export of mirrors, which is convenient for users to customize mirrors.
  - **Cloud disk**: A block device that provides persistent storage space for virtual machines based on a distributed storage system. It has an independent life cycle, supports binding/unbinding to multiple virtual machines at will, is based on network distributed access, and supports capacity expansion, cloning, snapshot and other features, providing high security, high reliability, high performance and scalable disks for virtual resources.
  - **Snapshot**: Provide disk snapshot and snapshot rollback capabilities, which can be applied to disaster recovery backup and version rollback scenarios, reducing the risk of data loss caused by misoperation, version upgrade, etc.
  - **External storage**: External storage services are commercial storage services provided by cloud platforms for enterprise users. By connecting commercial storage through ISCSI protocol and FC protocol, commercial storage is used as a virtualized backend storage pool to provide storage pool management and logical volume allocation. It can be used directly as the system disk and data disk of virtual machines, that is, any storage device that supports ISCSI protocol and FC protocol can be used as the backend storage of the platform virtualization, which is suitable for various application scenarios.
  - **VPC Network**: Software-defined Virtual Private Network for tenant data isolation, providing custom VPC network, subnet planning and network topology.
  - **External IP**: Used for external IP access of virtual machines, load balancers, NAT gateways, and VPN gateways to connect with the external network of the platform, such as virtual machines accessing the Internet or physical machines accessing the IDC data center network; supports binding multiple external IPs to virtual resources at the same time, and provides IPv6 network connection services.
  - **Security Group**: Virtual Firewall, providing two-way traffic access control rules, defining which networks or protocols can access resources, used to limit network access traffic of virtual resources, supporting TCP, UDP, ICMP and multiple application protocols, providing necessary security protection for the cloud platform.
  - **Elastic Network Interface**: An elastic network interface that can be attached to a virtual machine at any time, supports binding and unbinding, can be flexibly migrated between multiple virtual machines, provides high availability cluster building capability for virtual machines, and at the same time can realize fine-grained network management and low-cost fault transfer schemes.
  - **NAT Gateway**: An enterprise-level VPC gateway that provides SNAT and DNAT proxy for cloud platform resources, supports both automatic and whitelist network exit modes, and provides port mapping proxy services for VPC networks, allowing external networks to access VMs and MySQL through the NAT gateway.
  - **Load balancing**: A control service that automatically distributes network access traffic between multiple virtual machines based on TCP/UDP/HTTP/HTTPS protocols, similar to a traditional physical network hardware load balancer, used to achieve traffic load and high availability between multiple virtual machines, providing internal and external networks 4-layer and 7-layer listening and health check services.
  - **HAvip**：Provide high availability VIP services, HAvip is a migratable private IP belonging to a subnet within VPC, users can combine VIP with high availability services to migrate the service entrance when the service fails, to achieve high availability of services.
  - **IPSecVPN**：We provide IPSecVPN gateway services, which use IPSec protocol tunnel technology to connect private cloud with public cloud, IDC data center and third-party public cloud's intranet, and provide a secure channel for two private networks on the Internet, and ensure the security of the connection through encryption; At the same time, IPSecVPN services can also serve as a bridge for communication between VPCs of private cloud platform.
  - **Monitoring and Alarm**: Supports the collection and display of monitoring data from various dimensions of resources such as virtual machines, elastic scaling, disks, elastic IPs, NAT gateways, load balancers, IPSecVPNs, etc., and can quickly configure the alarm policy and notification rules of the resource monitoring index through the alarm template.
  - **Operation Log**: Logs of all resources and operations and audits of the cloud platform itself, supporting log collection and display across multiple time spans, providing reasons for operation failure.
  - **Recycle Bin**: A location where resources are temporarily stored after deletion, supporting operations such as recycling resources, restoring resources, and completely deleting resources.
  - Scheduler: Provides scheduler task execution functions, can be used to regularly execute a series of tasks, supports timed snapshot creation, can be repeated at a specified period, can also be executed only once, and each task supports batch operations on multiple resources.
  - **App Store**: Supports administrators to specify tenants to install applications, instances belong to the specified tenant, and supports the installation and deployment of multiple secure cloud applications.
  - **File Storage**: Administrators can specify tenants to create file storage instances in the console, install file storage clients in virtual machine instances, and use standard mounting commands to mount the created file system, allowing files to be shared between multiple instances.
  - **Object Storage**: Object Storage Service (OSS) compatible with Amazon Cloud's S3 API (interface protocol). By simply creating an object storage instance on the private cloud platform, you can store and access any type of data at any time and in any place through any application. Object storage is designed for cloud native, and can efficiently use CPU and memory resources even under high load, suitable for private cloud scenarios.
  - **Mysql**：MySQL is a database service provided by the platform, supporting both single-machine and master-slave models, and providing functions such as backup, upgrade models, monitoring, etc.
  - **Redis**：Redis is a database service provided by the platform, which supports both single-machine and master-slave models, and provides functions such as backup and monitoring to meet the performance scenarios of high read and write.
  - **Isolation Group**: Isolation Group is a simple scheduling policy for virtual machine resources, which supports the dispersion of instances within or between groups to different physical machines, in order to ensure high availability of services.
  - **Multicast**：As a communication method alongside unicast and broadcast, multicast technology can effectively solve the problem of single-point sending and multi-point receiving, thus realizing efficient data transmission from point to multi-point in the network, which can save network bandwidth and reduce network load for enterprises. Create multicast products on private cloud platform and support multicast communication within VPC.
- **Unified Cloud Management Platform**
  - The private cloud platform provides two ways to access and manage the cloud platform, the Web console and the API interface.
  - Through the WEB console, users can quickly use and manage cloud platform resources such as virtual machines, elastic IPs, load balancers, billing, etc.
  - Developers can customize cloud platform resources through APIs to support seamless migration to the cloud.
- **Operation and Maintenance Management Platform**: Operation and operation management platform provided for cloud platform administrators, including tenant management, resource management, accounting management, monitoring and alarm, log audit, system management and deployment upgrade and other functional modules.
  - **Tenant Management**: Used to manage the tenants and account information of the entire cloud platform, providing functions of creating/freezing tenants and recharging, supporting to view the tenant's resource information, order records, transaction records and quota prices, etc., and also supporting to modify the tenant's resource quota and product prices.
  - **Resource Management**: Supports viewing and managing all physical and virtual resources on the platform.
    - Physical resources include physical data centers, clusters, node resources, storage resources, network IP segment resource pools, and image resource pools.
    - Virtual resources include all resources owned by tenants and sub-accounts, including virtual machines, VPCs, load balancers, public IPs, elastic network cards, elastic scaling, NAT gateways, high availability VIPs, IPSecVPNs, monitoring alarms, security groups, and recycling bins.
  - **Accounting Management**: Supports viewing all order records, transaction records, recharge records and global product prices of the platform, supports configuring the overall product prices of the platform, and also supports exporting financial statements.
  - **Platform Monitoring Alarm**: Provide monitoring data of physical devices, components and all virtual resources of the private cloud itself, and support custom monitoring alarm and notification.
  - **Log Events**: Provide operation logs and audit information for all tenants, sub-accounts and administrators of the platform, with multi-dimensional filtering and searching.
  - **System Administration**: Provides global configuration, specification configuration and quota management functions for the cloud platform.
    - Global configuration includes email settings, recycling policies, network settings, billing, resource management, quota settings, login status, console and website settings, etc.
    - The specification configuration supports custom configuration of CPU memory specifications, disk capacity range, and external IP bandwidth for virtual machines.
    - Global quota support allows you to view and modify the quota values for virtual resources in each region globally.
  - **Deployment Upgrade**: The platform supports automated script installation of physical server nodes, including operating system, cloud platform components and management services.
- **Basic Monitoring Service**: Peripheral monitoring service of the basic hardware resources of the cloud platform, including the running status and performance indicators of all network devices, servers, disk arrays and other hardware devices accessed by the cloud platform for monitoring and alarm.

