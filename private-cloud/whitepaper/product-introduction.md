---
layout: default
title: Product Introduction
parent: Whitepaper
grand_parent: Private Cloud
permalink: /private-cloud/whitepaper/product-introduction/
nav_order: 1
---
# Product Introduction
## Product Overview

SCloudStack private cloud platform provides unified management, resource scheduling, monitoring logs and operation and maintenance of core services such as virtualization, SDN network, distributed storage, container and security. A complete set of cloud resource management capabilities to facilitate the digital transformation of government and enterprises.

![1](/assets/images/scloud-stack.png)

Based on the SCloud public cloud infrastructure, the platform reuses the core and core virtualization components, and privatizes the public cloud architecture. It has the characteristics of independent controllability, stability and reliability, continuous evolution, and open compatibility. The console or APIs can quickly build resources and services, support seamless connection with the public cloud, flexibly call public cloud capabilities, and help governments and enterprises quickly build a safe and reliable business architecture.

SCloudStack is positioned as a lightweight delivery, 3 nodes can build a production environment and can be smoothly expanded, without binding hardware and brands, compatible with X86 and ARM architecture, and provides unified resource scheduling and management, It supports multiple delivery modes of pure software, hyper-converged all-in-one machine and all-in-one cabinet, effectively reducing user management and maintenance costs, and providing users with a safe, reliable, self-controllable cloud service platform.

SCloudStack is positioned as a lightweight delivery, 3 nodes can build a production environment and can be smoothly expanded, without binding hardware and brands, compatible with X86 and ARM architecture, and provides unified resource scheduling and management, It supports multiple delivery modes of pure software, hyper-converged all-in-one machine and all-in-one cabinet, effectively reducing user management and maintenance costs, and providing users with a safe, reliable, self-controllable cloud service platform.

## Product Architecture

The overall product architecture of the SCloudStack platform consists of basic hardware facilities, virtual core engine, intelligent scheduling system, core product resources, unified cloud management platform and operation and maintenance management platform. and operators to provide cloud platform management and services.

### Infrastructure
Servers, switches, and storage devices used to host the SCloudStack platform.

- The platform supports and is compatible with general-purpose `X86, ARM` and `MIPS` architecture hardware servers, with no restrictions on server and hardware brands;
- Support `SSD, `SATA`, SAS` and other disk storage, and support computing and storage hyper-converged nodes and docking disk array devices, without vendor lock-in;
- Support Huawei, Cisco, H3C and other general switches, router network equipment access, all network functions are defined by SDN software, only physical switches are required to support `Vlan, Trunk, IPv6`, port aggregation, stacking, etc. characteristics;
- Support hybrid cloud access and adapt to customers' existing hardware resources, and seamlessly connect existing resource services while making full use of resources.

### Virtual Core Engine
The implementation and logic of the operating system kernel, virtualized computing, storage, and network that carry the core of the platform.

- Kernel module: host the server operating system and kernel module running on the cloud platform, and reuse the deeply optimized Linux kernel of the public cloud; at the same time, it is compatible with server operating systems such as ARM ecological UOS and Galaxy Kylin system and kernel;
- Virtualized computing: Realize computing virtualization through `KVM, Libvirt` and `Qemu`, support standard virtualization architecture, provide full life cycle management of virtual machines, compatible with X86 and ARM architecture systems, support hot upgrade, reinstallation Features such as system installation, CPU over-scaling, GPU transparent transmission, online migration, downtime migration, anti- affinity deployment, etc., and support for importing and exporting virtual machine images to meet the needs of business migration to the cloud;
- Distributed network SDN: Realize virtual network through `OVS` + `VXLAN`, pure software -defined distributed network, improve network forwarding performance and virtualize traditional data center physical network at the same time, for the cloud Platform resources provide network functions such as VPC isolated network environment, elastic network card, external network IP, NAT gateway, load balancing, firewall, VPN connection, hybrid cloud access and network topology, and support IPv4&IPv6 dual stack;
- Distributed storage SDS: Realize distributed high-performance storage based on Ceph, provide block storage services for the platform, support cloud disk online expansion, cloning, snapshot and rollback functions; at the same time, multiple copies of the underlying data are stored It also supports data re-balancing and fault reconstruction capabilities to ensure performance and data security.

### Intelligent Dispatch System
- Supports anti- affinity scheduling deployment strategies to ensure high availability and reliability of services;
- Support online migration technology, real-time perception of physical machine status and load information;
- the physical host fails or exceeds the load, the virtual machine is automatically migrated to the low- load physical host;
- When creating a virtual machine, according to the business scheduling policy, automatically start the virtual machine to a healthy physical host with low load;
- Support computing quota allocation and resource preemption, and effectively share physical resources under the premise of ensuring fairness;
- flow table control and delivery of platform virtual resources to ensure the performance and availability of the distributed network architecture.

### Core Product Resources

- Region (data center): Data center refers to the physical location classification of resource deployment, and data centers are independent of each other, such as Ho Chi Minh data center and Singapore data center. The platform supports multi-data center management, using a management platform to manage private cloud platforms spread across data centers;
- Cluster: used to distinguish the distribution of different resources in a data center, such as x86 computing clusters, ARM computing clusters, and SSD storage clusters And `SATA` storage clusters, one data center can deploy multiple clusters;
- Multi- tenant: The platform supports multi- tenant mode, providing functions such as tenant isolation, sub -account, authority control, quota configuration and price configuration;
- Sub- accounts and permissions: support a tenant to have multiple sub- accounts, support resource isolation and control the permissions of sub- accounts for resource management;
- Metering and billing: Supports three billing methods: on-demand, monthly, and yearly, supports overdue renewal and recycling strategies, and provides complete billing orders and consumption details;
- Elastic Computing: Virtual machines running on physical hosts support image creation, restart / shutdown / startup, deletion, VNC login, system reinstallation, password reset, hot upgrade, Bind external network IP and security group, mount data disk and anti -affinity policy deployment and other virtual machine life cycle functions, and support the virtual mechanism as a mirror image and disk snapshot capabilities, providing fast business deployment and backup capabilities;
- GPU virtual machine: The platform provides GPU device transparent transmission capabilities, supports users to create and run GPU virtual machines on the platform, and enables virtual machines to have high-performance computing and graphics processing capabilities;
- Elastic scaling: supports elastic scaling, users can define elastic scaling policies to automatically increase computing resources ( virtual machines) to ensure computing power when business needs grow; Automatically reduce computing resources to save costs when business demands drop. Based on the load balancing and health check mechanism, it can be applied to business scenarios with fluctuating request volume and stable business volume;
- Image: the operating system required for the virtual machine to run, providing common basic operating system images such as CentOS, Windows, Ubuntu, etc.; supporting the export of the virtual machine as an image, and rebuilding the virtual machine through self-made images machine; at the same time, it supports the import and export of images, which is convenient for users to customize images;
- Cloud Disk: A block device that provides persistent storage space for virtual machines based on a distributed storage system. With an independent life cycle, it supports binding a separately created cloud hard disk to a virtual machine, or unbinding a bound cloud hard disk, and then binding it to other virtual machines; based on network distribution access, and supports features such as capacity expansion, cloning, and snapshots, providing virtual resources with high-security, high-reliability, high-performance, and scalable disks;
- Snapshot: Provides disk snapshot and snapshot rollback capabilities, which can be applied to business scenarios such as disaster recovery backup and version rollback, reducing the risk of data loss caused by mis-operation and version upgrade;
- VPC network: software- defined virtual private network, used for data isolation between tenants, providing custom VPC network, subnet planning and network topology;
- External network IP: used for external network IP access of resources such as virtual machines, load balancing, NAT gateways, and VPN gateways, used to connect to the external network of the platform, such as virtual machines accessing the Internet or accessing The physical machine network of the IDC data center; supports simultaneous binding of multiple external network IPs to virtual resources, and provides IPv6 network connection services;
- Security group: virtual firewall, which provides inbound and outbound traffic access control rules, defines which networks or protocols can access resources, and is used to limit network access traffic to virtual resources, supports TCP, UDP, ICMP and multiple An application protocol to provide the necessary security guarantee for the cloud platform;
- Elastic network card: An elastic network interface that can be attached to a virtual machine at any time, supports binding and unbinding, and can be flexibly migrated among multiple virtual machines, providing high-availability cluster building capabilities for virtual machines. It can realize refined network management and cheap failover scheme at any time;
- NAT gateway: enterprise -level VPC gateway, which provides SNAT and DNAT agents for cloud platform resources, supports two network egress modes, automatic and whitelist, and provides port mapping agent services for VPC networks, making external networks Access the virtual machine through the NAT gateway.
- Load balancing: A control service that automatically distributes network access traffic among multiple virtual machines based on the TCP/UDP/HTTP/HTTPS protocol, similar to a traditional physical network hardware load balancer, used for multiple virtual machines Realize traffic load and high availability between virtual machines, and provide Layer 4 and Layer 7 monitoring and health check services for internal and external networks;
- IPSecVPN: Provide IPSecVPN gateway service, connect SCloudStack with SCloud public cloud, IDC data center, and third-party public cloud intranet through IPSec protocol encrypted tunnel technology, and provide security for two private networks on the Internet The channel ensures the security of the connection through encryption; at the same time, the IPSecVPN service can also be used as a communication bridge between VPCs on the SCloudStack platform;
- Bastion machine: Provides the ability to manage and monitor the operation and access of operation and maintenance personnel, notify and warn high-risk behaviors, and improve the operation and maintenance audit mechanism;
- Vulnerability scanning: Provides complete, fast, stable and efficient detection of WEB and host vulnerabilities, relying on the research accumulation of the professional SCloud Security Center to improve security self-inspection capabilities;
- Database audit: Provide monitoring of database status and communication content, accurately assess the risks faced by the database, provide reliable evidence through complete log records, and improve the tracing mechanism;
- Web Application Firewall ( WAF ): Provides security protection capabilities for the business layer, maximizes business security protection, complements the 7- layer protection capability, and builds a complete boundary protection capability;
- Monitoring and alarming: Supports the collection and display of monitoring data in various dimensions of virtual machines, elastic scaling, disks, elastic IPs, NAT gateways, load balancing, IPSecVPN and other resources, and can be quickly configured through alarm templates Alarm policies and notification rules for resource monitoring indicators;
- Operation logs: all resources of the cloud platform and the operation and audit logs of the cloud platform itself, support multi- time span log collection and display, and provide reasons for operation failures;
- Recycle bin: the location where resources are temporarily stored after deletion, and supports operations such as recycling resources, restoring resources, and completely deleting resources;
- Timer: Provides timer task execution function, which can be used to execute a series of tasks regularly, supports timing creation of snapshots, It can be executed repeatedly in a specified period, or only once, and each task supports multiple resource batch operations.

### Unified cloud management platform

- SCloudStack platform provides web console Access and manage the cloud platform in two ways: and API interface;
- Through the WEB console, users can quickly use and manage cloud platform resources, such as virtual machines, elastic IPs, and load balancing, billing, etc.;
- Developers can customize and build cloud platform resources through APIs to support seamless migration to the cloud.

### Operation and maintenance management platform

An operation and maintenance operation and management platform for cloud platform administrators, including tenant management, resource management, account management, monitoring and alarming, log auditing, system management, deployment and upgrading and other functional modules.

- Tenant management: used to manage the tenant and account information of the entire cloud platform, providing the function of creating / freezing tenants and recharging, and supporting viewing of resource information, order records, transaction records and quota prices owned by tenants and other information, while supporting the modification of tenant resource quotas and product prices;
- Resource management: support viewing and managing all physical and virtual resources of the platform;
 -Physical resources include physical data centers, clusters, host resources, storage resources, network IP segment resource pools, and mirror resource pools;
 -Virtual resources include resources owned by all tenants and sub- accounts, including virtual machines, VPC, load balancing, external network IP, elastic network card, elastic scaling, NAT gateway, IPSecVPN, monitoring alarms, security group, recycle bin, etc.;
- Account management: support viewing all order records, transaction records, recharge records and global product prices on the platform, support configuration of overall product prices on the platform, and support financial report export;
- Platform monitoring alarm: Provide monitoring data of SCloudStack's own physical equipment, components and all virtual resources, and support custom monitoring alarms and notifications;
- Log events: provide operation logs and audit information of all tenants, sub- accounts and administrators of the platform, which can be screened and searched in multiple dimensions;
- System management: Provide cloud platform global configuration, specification configuration and quota management functions.
 -Global configuration includes mailbox settings, recycling policies, network settings, billing, resource management, quota settings, login status, console and website settings, etc.;
 -Specification configuration supports custom configuration of the virtual machine's CPU memory specification, disk capacity range, and external network IP bandwidth;
 -Global quota supports viewing and modifying the global quota value of each regional virtual resource.
- Installation and deployment: Provide a private cloud deployment system, support graphical interface, configure servers, networks and other infrastructure, automate machine installation, and deploy private clouds.

## Product Features
### Autonomous and controllable
Based on the public cloud architecture, the multiplexing core virtualization components are independently developed, with high controllability and reliability verified by tens of thousands of enterprises.
### Stable and reliable
platform services, intelligent scheduling of virtual resources, multiple copies of data storage, self-healing distributed network, escorting business.
### Easy to use
environment is built with 3 nodes, which is light in scale and can be expanded horizontally, supports smooth business migration, and helps government and enterprises to easily migrate to the cloud.
### Open and compatible
Not bound to hardware brands, compatible with X86 and ARM architecture and ecological adaptation, heterogeneous equipment construction and unified management.
## Technical Architecture Features
### API idempotence
Idempotency means that one-time and multiple requests for a certain resource should have the same side effects, ensuring that resource requests always get the same results no matter how many times they are called. If the API request for updating the virtual machine is called multiple times, the returned results are consistent.

SCloudStack ensures the idempotence of platform resource APIs through technical means such as distributed locks, unique constraints on business fields, and unique constraints on Tokens. Operation requests (except creation requests) for resources such as virtual machines, cloud disks, VPCs, and load balancing support repeated submissions, and ensure the consistency of results returned by multiple calls to the same API request, and at the same time avoid the problem of repeated operations caused by the failure of the API to obtain exact results due to network interruption;

### Fully asynchronous architecture

- The cloud platform uses the message bus for service communication connection. When calling the service API, the source service sends a message to the destination service, registers a callback function, and returns immediately; once the destination service completes the task, That is, the callback function is triggered to reply to the task result;
- Asynchronous call method is adopted between cloud platform services and within the service, and communication is carried out through asynchronous messages, combined with the asynchronous HTTP call mechanism, to ensure that all components of the platform realize asynchronous operations;
- Based on the asynchronous architecture mechanism, the cloud platform can manage more than hundreds of thousands of virtual machines and virtual resources at the same time, and the back-end system can concurrently process tens of thousands of API requests per second;
- The plug-in mechanism adopted by SCloudStack, each plug-in sets a corresponding proxy program, and at the same time sets a callback URL for each request in the HTTP header. After the plug-in task is completed, the proxy program sends a response to the calling the URL of the author;

### Distributed

(1) Distributed underlying system: SCloudStack core module provides distributed underlying support such as computing, storage, and scheduling for intelligent scheduling, resource management, security management, cluster deployment, and cluster monitoring, etc. function module.

- Intelligent scheduling: Provide tenants with intelligent scheduling modules based on distributed service calls and remote service calls. The intelligent scheduling module monitors the status and load of the cluster and all service nodes in real time. When a cluster expands, a server fails, a network fails, or configuration changes occur, the intelligent scheduling module will automatically Migrate the virtual resources of the changed cluster to healthy server nodes to ensure the high reliability and availability of the cloud platform;
- Resource management: Through the distributed resource management module, it is responsible for the allocation and management of resources such as cluster computing, storage, and network, and provides resource quotas, resource applications, and resource management for cloud platform tenants. Scheduling, resource occupation and access control to improve the resource utilization of the entire cluster;
- Security management: The distributed underlying system provides a security management module to provide tenants with functions such as identity authentication, authorization mechanism, and access control. Inter-service calls and user identity authentication are performed through various methods such as API key pairs and usernames and passwords; user access to resources is controlled through the role authority mechanism; isolation is achieved through VPC The mechanism and security group control access to the resource network to ensure the security of the platform;
- Cluster deployment: The distributed underlying system provides modules for automatic deployment of cluster nodes for the cloud platform, and provides cluster deployment, configuration management, cluster management, cluster expansion, online migration and service nodes for operation and maintenance personnel. Click offline and other functions to provide automatic deployment channels for platform managers;
- Cluster monitoring: The monitoring module is mainly responsible for information collection, monitoring and alarming of platform physical resources and virtual resources. The monitoring module deploys Agents on physical machines and virtual resources, obtains the running status information of resources, and displays the information indicators to users; at the same time, the monitoring module provides monitoring alarm rules, through By configuring the alarm rules, monitor and alarm the status events of the cluster, and effectively store the monitoring and alarm history records;
  
(2) Distributed storage system: SCloudStack adopts a highly reliable, highly secure, highly scalable, and high-performance distributed storage system to provide block storage services to ensure the security and reliability of local data.

- Software -defined distributed storage aggregates the disk storage resources of a large number of general-purpose machines, adopts common storage system standards, and manages all storage in the data center in a unified manner;
- The distributed storage system adopts a multi-copy data backup mechanism. When writing data, it first writes data to the master copy, and the master copy is responsible for synchronizing data to other copies, and copies each copy of data Servers, across cabinets, and across data centers are stored on different disks, ensuring data security in multiple dimensions;
- The multi-copy mechanism stores data, which will automatically shield software and hardware failures, disk damage and software failures. The system automatically detects and automatically performs copy data backup and migration to ensure data security. Affect business data storage and use;
- Distributed storage services support horizontal expansion, incremental expansion and automatic data balance to ensure high scalability of the storage system;
- Support PB -level storage capacity, and the total number of files can support hundreds of millions;
- Support uninterrupted data storage and access services, with an SLA of 99.95%, ensuring high availability of the storage system;
- Support high-performance cloud disk, IOPS and Throughput increases linearly with storage capacity scale, guaranteeing response delay;

In terms of deployment, the SSD disk built into the computing node is built as a high-performance storage pool, and the ``SATA`/SAS` disk built into the computing node is built into a common performance storage pool. The distributed storage system builds block devices as elastic block storage, which can be directly mounted and used by virtual machines. When data is written, it uses three copies, a write confirmation mechanism, and copy distribution strategies. Maximize data security and availability. The snapshot technology can be used locally to back up the local data regularly. When the data is lost or damaged, the snapshot can be used to quickly restore the local business data.

(3) Distributed network architecture: Distributed Overlay network is adopted to provide network functions such as VPC, NAT gateway, load balancing, security group, and external network IP.

- The Overlay network of the SCloudStack cloud platform is distributed and runs on all computing nodes;
 -The management service is only used as a management role, and does not undertake the deployment of network components and production network transmission;
 -The virtual network flow table distribution service is a high-availability architecture, only for flow table distribution and not transparently transmitted to the production network;
 -All production networks are only transmitted on computing nodes, without forwarding through management services or flow table distribution services;
 -the management service and the flow table distribution service will not affect the operation and communication of the deployed virtual resources.
- Hyper-converged computing nodes or independent storage nodes are divided into different clusters ( Set ) according to disks and services;
 -Each cluster has a maximum of 45 nodes to control the cluster size;
 -The business data network is only transmitted in a single cluster, that is, in a single group of switches.
- Distributed storage is mounted directly through the physical network, without the need for mounting and transmission through the overlay network;
 -Integrate distributed storage rbd and qemu through libvirt, qemu operates distributed storage through librbd;
 -The virtualization process and the distributed storage process communicate through the local & cross-physical machine intranet;
 -The intranet of the cloud platform uses at least 10 Gigabit switches and port aggregation, which can meet the performance requirements of virtual machines and distributed storage;

The distributed network architecture disperses business data transmission to each computing node. Except for northbound traffic such as business logic that requires management services, all southbound traffic such as business implementation of virtualized resources is distributed on computing nodes or On storage nodes, that is, platform business expansion is not limited by the number of management nodes.

### High availability
The SCloudStack private cloud platform architecture provides high-availability technical solutions from hardware facilities, network devices, server nodes, virtualization components, and distributed storage to ensure the uninterrupted operation of the entire cloud platform business:

- Data center cabinet -level redundancy design, all equipment is symmetrically deployed in the cabinet, power failure or failure of a single cabinet will not affect the business;
- Network service area isolation design, internal network business and external network business are completely isolated on physical equipment, to avoid mutual influence between internal and external network services;
- Network equipment scalability design, all network equipment is divided into core and access two-layer architecture, a set of core can be horizontally expanded to dozens of sets of access equipment;
- Network Equipment Redundancy Design, all network devices are stacked in a group of two to avoid a single point of failure of the switch;
- Redundancy design for the downlink access of the switch, and the interfaces of all servers with dual uplink switches are aggregated with `LACP` ports to avoid single point of failure;
- Server network access redundancy design, all server nodes are bound with dual network cards, respectively connected to the internal network and external network, to avoid single point of failure;
- Management node redundancy and scalability design, multiple management nodes are deployed in HA, and support horizontal expansion to avoid single point of failure of management nodes;
- The virtual machine is evenly deployed on the computing nodes through the intelligent scheduling system, and the number of computing nodes can be expanded horizontally;
- Distributed storage redundancy design, evenly storing data on all disks, and three copies, write confirmation mechanism and copy distribution strategy to ensure data security;
- When expanding server nodes and storage, you only need to increase the corresponding number of hardware devices, and configure the resource scheduling management system accordingly;
- All components in the cloud platform are designed with a high-availability architecture, such as management services, scheduling services, network flow table distribution services, etc., to ensure high availability of the platform;
- The products and services provided by the cloud platform, such as load balancing, NAT gateway, database service, and cache service, are all built with a high-availability architecture to ensure the reliability of the services provided by the cloud platform.

### Separation of business implementation

The SCloudStack cloud platform architecture is divided into northbound interface and southbound interface in terms of business logic, which separates the business logic of the cloud platform from business implementation. When the business management logic is unavailable, it does not affect the normal operation of virtual resources and improves the overall cloud Platform business availability and reliability.

- Northbound interface: only defines business logic, provides business interface, and is responsible for northbound data landing. Business interfaces include account authentication, resource scheduling, monitoring, billing, API gateway and WEB console and other business service interfaces;
- Southbound interface: only defines the business implementation, and is responsible for converting the business of the northbound interface into realization, such as virtual machine operation, VPC network construction, distributed storage data storage, etc.;
  
the business is separated, when the cloud platform business end (such as the WEB console) fails, it will not affect the virtual machine already running on the cloud platform and the business running in the virtual machine, ensuring business to a certain extent High availability.

### Componentization

SCloudStack componentizes all virtual resources of the cloud platform, and supports hot-swapping, orchestration and horizontal expansion.

- Componentization includes virtual machines, disks, network cards, IP, routers, switches, security groups, etc.;
- Each component supports hot plugging, such as binding an IP to a running virtual machine;
- Each component supports horizontal expansion, such as increasing the disk of the virtual machine horizontally to improve the robustness of the overall cloud platform.

## Customer Pain Points
### Pain points of self-built private cloud
- Poor controllability

Components and services packaged based on the open source architecture come from the community, with poor controllability and unproven reliability. The upgrade of platform features is limited by the community and requires specialized operation and maintenance personnel. At the same time, the open source framework Complicated, the deployment and implementation process is complicated, and the implementation is difficult.
- High input cost

Directly deployed by the OEM public cloud requires an independent server cluster for all services. The initial deployment scale is large and usually restricts the hardware architecture and brand. Deployment and implementation require a lot of infrastructure and manpower Resources; In terms of operation and maintenance, hosting operation and maintenance is usually required, and the construction cost is relatively high.
- Complex operation and maintenance

Self-built data center and general virtualization system, for a series of applications such as database, cache, and load balancing required for business construction, you need to build and maintain them through virtual machines. Consider cluster deployment, monitoring, logging, backup, disaster recovery, reliability and availability of services, etc.

- Poor compatibility

General data centers and virtualization systems have poor adaptation and compatibility with localized hardware, operating systems, and middleware.

### Solution
- Controllable and reliable
  
SCloudStack adopts non-open source architecture, based on independent research and development of public cloud, reuses kernel and core virtualization components, and reconstructs the scale of public cloud deployment into operable, O& M, fast delivery and privatized delivery The cloud platform is highly controllable and its reliability has been verified by tens of thousands of enterprises.
- Lightweight build

All products and services of the platform use a unified underlying resource pool. All products do not need to prepare dedicated server clusters. 3 nodes can lightly build a production environment, which is lightweight and horizontally scalable, supports automated one- click deployment and provides smooth upgrade capabilities, and can be easily managed by an operation and maintenance personnel.
- Rich components

Self- service platform with consistent user experience in the public cloud. In addition to basic IaaS products, it provides government and enterprise users with highly available, highly reliable, and self- service load balancing, NAT gateway, IPSecVPN, and database services And cache services and other PaaS products and services.
- High compatibility

The platform itself is not bound to the hardware architecture and brand, and is compatible with mainstream architectures such as X86, ARM, and MIPS. It can be built heterogeneously and perform unified resource management; at the same time, it has been adapted to the hardware, operating system, and middleware. Such as Huawei, UOS and NTU General Database, etc.

## Application scenarios
### Virtualization & Cloudification
By deploying business systems and internal applications to the SCloudStack platform, it can provide users with a set of private cloud platforms integrating virtualization, distributed storage, and SDN networks. The platform supports multi-data center management, and can deploy services to multiple data centers to build disaster recovery clouds or edge computing. At the same time, it supports seamless connection with public clouds, flexibly calls public cloud capabilities, and helps governments and enterprises quickly build safe and reliable services. architecture.

### Rapid service delivery
Platform service, and you can deploy and manage the infrastructure and middleware required for business delivery with one click through the self- service cloud management platform, including online expansion, load distribution, and database caching and monitoring logs and other application basic environment service capabilities; at the same time, the platform supports image import and export, which can conveniently and quickly migrate business systems to the cloud platform, and can manage resources of all business systems Unified management.

### Hyper-converged all-in-one machine
The platform provides an all-in-one machine delivery model. Various models are used in different business scenarios. It integrates the SCloudStack private cloud platform. It is pre-installed and ready to use out of the box. A series of cloud service capabilities such as network, storage, database, cache and cloud management; at the same time, it can build a hybrid cloud solution by interconnecting with the IDC data center.

### Private Cloud for Government and Enterprise
SCloudStack provides tenant console and administrator console, supports multi- tenant, account registration, metering and billing and other features, and provides cloud platform managers with operation and maintenance management functions, including resource management, tenant Management, price configuration, resource specification configuration, deployment upgrade and monitoring logs and other services, providing industry- specific cloud solutions for government and enterprises.

SCloudStack lightweight private cloud is an IaaS+PaaS composite product, and can be equipped with public cloud products such as big data, safe house, and AI on demand. It is suitable for business applications that require cloudification and private deployment for customers in all industries. Using cloud scenarios, typical industries are as follows:
- Government, state-owned enterprises, military industry, transportation, manufacturing enterprises
Industry customers who undertake public service responsibilities externally, and require rapid delivery, resource sharing, intelligent scheduling, and unified management of internal and external business application systems and commercial software.
- B2B e- commerce, big data, education and other enterprises
customers who need to build an industry- specific cloud and combine their own SaaS business to provide overall solutions for their users.
- Artificial intelligence and scientific research laboratory industry
Industry customers who need a large number of virtualized environments that can be delivered quickly and privately deployed, for rapid deployment and management of scientific research projects and training systems.

## Delivery Mode
SCloudStack is positioned as lightweight delivery, 3 nodes can build a production environment and can be smoothly expanded, and provides unified resource scheduling and management, supporting pure software, hyper-converged all-in-one machines and hyper-converged cabinets The delivery mode can effectively reduce the cost of user management and maintenance, and provide users with a safe, reliable, independent and controllable cloud service platform.

- Pure software delivery <br/>
The customer provides the hardware server, network equipment and related infrastructure for the operation of the cloud platform, and SCloud provides SCloudStack lightweight private cloud software; usually in the case of complete infrastructure network facilities, SCloudStack software can be deployed and delivered within 2 hours.
