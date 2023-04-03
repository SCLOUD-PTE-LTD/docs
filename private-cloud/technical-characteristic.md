---
layout: default
title: Technical Characteristics
has_children: false
parent: Private Cloud
permalink: /private-cloud/technical-characteristics
nav_order: 7
---

# Technical Characteristics
### Idempotent API
Idempotent means that one and multiple requests for a resource should have the same side effects, ensuring that the result of the resource request is always consistent no matter how many times it is called. If you call the API request to update a virtual machine multiple times, the results returned are consistent.

SCloudStack ensures the idempotence of platform resource APIs through technical means such as distributed locks, unique constraints on business fields, and unique constraints on tokens. Repeated submission of operation requests (except creation requests) for resources such as VMs, EVS disks, VPCs, and Load Balancing ensures that the results returned by multiple calls to the same API request are consistent, and the problem of repeated operations is prevented from being obtained due to network interruptions.

### Fully asynchronous architecture
The cloud platform uses the message bus for service communication connection, and when calling the service API, the source service sends a message to the destination service, registers a callback function, and then returns immediately. Once the destination service completes the task, it triggers a callback function to reply to the task result.

Asynchronous call methods are adopted between cloud platform services and internal services, communication is carried out through asynchronous messages, and asynchronous HTTP call mechanism is combined to ensure that all components of the platform realize asynchronous operations;

Based on the asynchronous architecture mechanism, the cloud platform can manage more than hundreds of thousands of virtual machines and virtual resources at the same time, and the back-end system can process tens of thousands of API requests per second.
SCloudStack adopts the plug-in mechanism, each plug-in sets the corresponding agent, and at the same time sets the callback URL for each request in the HTTP packet header, and the agent sends a URL to the caller after the plug-in task is completed.

### Distributed
#### (1) Distributed underlying system: 
The SCloudStack core module provides distributed underlying support such as computing, storage and scheduling, and is used for functional modules such as intelligent scheduling, resource management, security management, cluster deployment and cluster monitoring.

Intelligent scheduling: Provides intelligent scheduling modules for tenants based on distributed service calls and remote service calls. The intelligent scheduling module monitors the status and load of the cluster and all service nodes in real time, and when a cluster expands, server failure, network failure and configuration changes, the intelligent scheduling module will automatically migrate the virtual resources of the changed cluster to healthy server nodes to ensure the high reliability and high availability of the cloud platform.

Resource management: Through the distributed resource management module, it is responsible for the allocation and management of cluster computing, storage, network and other resources, providing cloud platform tenants with resource quotas, resource applications, resource scheduling, resource occupation and access control, and improving the resource utilization of the entire cluster.

Security management: The distributed underlying system provides a security management module to provide tenants with functions such as identity authentication, authorization mechanism, and access control. Conduct inter-service calls and user authentication through API key pairs, usernames and passwords; Control user access to resources through the role permission mechanism; Access control of resource networks through VPC isolation mechanism and security groups ensures the security of the platform.

Cluster deployment: The distributed underlying system provides modules for automatic deployment of cluster nodes for the cloud platform, provides functions such as cluster deployment, configuration management, cluster management, cluster expansion, online migration, and offline service nodes for O&M personnel, and provides automatic deployment channels for platform managers.

Cluster monitoring: The monitoring module is mainly responsible for the collection, monitoring, and alarm of the platform's physical and virtual resource information. The monitoring module deploys agents on physical machines and virtual resources, obtains the running status information of resources, and displays the information to users in an indexed manner. At the same time, the monitoring module provides monitoring alarm rules, monitors and alarms the status events of the cluster by configuring alarm rules, and effectively stores monitoring alarm history.

#### (2)Distributed storage system:
SCloudStack adopts a highly reliable, highly secure, highly expandable, and high-performance distributed storage system to provide block storage services to ensure the security and reliability of local data.

Software-defined distributed storage aggregates the disk storage resources of a large number of general-purpose machines and adopts common storage system standards to uniformly manage all storage in the data center;

The distributed storage system adopts a multi-copy data backup mechanism, which first writes data to the primary replica when writing data, and the primary replica is responsible for synchronizing data to other replicas, and storing copies of each copy of data on different disks across disks, servers, cabinets, and data centers to ensure data security in multiple dimensions.

The multi-copy mechanism stores data, automatically shields software and hardware failures, disk damage, and software failures, and the system automatically detects and automatically performs copy data backup and migration to ensure data security without affecting business data storage and use.

The distributed storage service supports horizontal expansion, incremental expansion, and automatic data balancing to ensure high scalability of the storage system.
Support petabyte-level storage capacity, and the total number of files can support hundreds of millions;

Support uninterrupted data storage and access services with an SLA of 99.95% to ensure high availability of the storage system;
Supports high-performance EVS disks, and IOPS and throughput grow linearly with storage capacity to ensure response latency.

In terms of deployment, the SSD disks that come with compute nodes are built as high-performance storage pools, and the ``SATA`/SAS` disks that come with compute nodes are built as normal performance storage pools. The distributed storage system uniformly abstracts disk devices into elastic block storage, which can be directly mounted and used by virtual machines, and ensures data security and availability to the greatest extent through measures such as three copies, write confirmation mechanism, and replica distribution policy when data is written. In the event of data loss or corruption, local business data, including database data, application data, and file directory data, can be quickly restored by backup, which can be achieved in minutes.

#### (3) Distributed network architecture: 
Distributed overlay network is used to provide VPC private network, NAT gateway, load balancing, security group, public IP and other network functions.
The SCloudStack cloud platform overlay network runs distributed across all computing nodes;
- The management service is only used as an administrative role, and does not undertake the deployment of network components and the transmission of production networks;
- The virtual network flow table distribution service is a high-availability architecture, which only distributes flow tables and does not transmit production network transmission.
- All production networks are transmitted only on compute nodes and do not need to be forwarded through the management service or the streaming table distribution service;
- The failure of the management service and the flow table distribution service does not affect the operation and communication of the deployed virtual resources.

Hyper-converged computing nodes or independent storage nodes are divided into different clusters (Sets) based on disks and services.
- Up to 45 nodes per cluster to control the cluster size;
- The business data network transmits only in a single cluster, that is, in a single set of switches.

Distributed storage is mounted directly through the physical network, eliminating the need for mounting and transmission through the overlay network;
- Distributed storage RBD and QEMU are fused through libvirt, and QEMU operates distributed storage through `Librbd`;
- The virtualization process and the distributed storage process communicate natively & across the physical intranet;
- At least 10 Gigabit switches and port aggregation are used in the intranet of the cloud platform to meet the performance requirements of virtual machines and distributed storage.

The distributed network architecture distributes service data transmission to each computing node, except for the northbound traffic such as business logic that requires management services, and the southbound traffic such as the service implementation of all virtualized resources is distributed on the computing node or storage node, that is, the platform service expansion is not limited by the number of management nodes.

### High availability
SCloudStack enterprise proprietary cloud architecture platform provides high-availability technical solutions from hardware facilities, network devices, server nodes, virtualization components, and distributed storage to ensure the uninterrupted operation of the entire cloud platform business:

- Data center cabinet-level redundancy design, all equipment is symmetrically deployed in the cabinet, and the power loss or failure of a single cabinet does not affect the service.
- Network service area isolation design, internal network services and external network services are completely isolated on physical devices, avoiding mutual influence between internal and external network services.
- Network equipment scalability design, all network equipment is divided into core and access two-layer architecture, one set of core can horizontally expand dozens of access devices;
- Network equipment redundancy design, all network equipment is a group of two stacked to avoid single point of failure of the switch;
- Switch downlink access redundancy design, all server dual-uplink switch interfaces are LACP port aggregation to avoid a single point of failure;
- Server network access redundancy design, all server nodes are dual-network card binding, respectively connected to the internal network and external network, to avoid single point of failure;
- Management node redundancy and scalability design, multiple management nodes are HA deployment, support horizontal expansion, avoid single point of failure of management nodes;
- The intelligent scheduling system distributes virtual machines in a balanced manner among the compute nodes, which can horizontally expand the number of compute nodes.
- Distributed storage redundancy design, balanced storage of data on all disks, and three copies, write confirmation mechanism, and copy distribution policy to ensure data security;
- When expanding server nodes and storage, only the corresponding number of hardware devices needs to be added, and the resource scheduling management system needs to be configured accordingly.
- Each component in the cloud platform adopts a high-availability architecture design, such as management services, scheduling services, and network flow table distribution services, to ensure high availability of the platform.
- The products and services provided by the cloud platform, such as load balancing, NAT gateway, database service, and cache service, are built with a high-availability architecture to ensure the reliability of the cloud platform.

### Business separation is achieved
The SCloudStack cloud platform architecture is divided into northbound interface and southbound interface from the business logic, separating the business logic and business implementation of the cloud platform, and when the business management logic is not available, it does not affect the normal operation of virtual resources, and improves the service availability and reliability of the cloud platform as a whole.
- Northbound interface: Only defines business logic, provides service interfaces, and is responsible for northbound data implementation. Service interfaces include service interfaces such as account authentication, resource scheduling, monitoring, billing, api gateway, and web console.
- Southbound interface: only defines the service implementation, and is responsible for converting the services of the northbound interface into implementations, such as virtual machine running, VPC network construction, distributed storage data storage, etc.

After the separation of services, when a failure occurs on the service side of the cloud platform (such as the web console), it does not affect the virtual machines that are already running on the cloud platform and the services running in the virtual machines, ensuring high service availability to a certain extent.

### Componentization
SCloudStack componentizes all virtual resources of the cloud platform, supporting hot plugging, orchestration composition, and scale-out.

Componentization includes virtual machines, disks, NICs, IP addresses, routers, switches, security groups, etc.

Each component supports hot plugging, such as binding an IP to a running virtual machine;

Each component supports scale-out, such as scaling out a virtual machine's disks, to improve the robustness of the overall cloud platform.