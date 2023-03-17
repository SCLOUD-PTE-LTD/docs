---
layout: default
title: Product Functional Architecture
parent: Whitepaper
grand_parent: Private Cloud
permalink: /private-cloud/whitepaper/product-functional-architecture/
nav_order: 4
---
# Product Functional Architecture
## Core Functional Concepts
### Territory
- Region refers to the geographic area of the SCloudStack cloud platform physical data center, such as Shanghai, Beijing, Hangzhou, etc.
- Different regions are completely physically isolated, and regions cannot be changed after cloud platform resources are created.
- The networks between different regions are completely isolated, and the internal networks of resources cannot be interconnected, and network communication can be carried out through the public network or private lines.
VPC and Server Load Balancer support deployment in the same region.

### Clustering
A cluster is a logical division of SCloudStack physical resources that distinguishes server nodes with different configuration specifications and different storage types. The logical relationships between regions, clusters, and physical servers are as follows:
- A region can contain multiple clusters, and the unified cloud management platform is used for cluster management and operation, and cloud resources can only be scheduled in a single cluster.
- A cluster consists of at least 3 server nodes, and the servers in the cluster must have the same CPU/memory, disk type, and operating system;
  - When the server is a compute-storage fusion node, the nodes of different disk types are divided into a cluster, such as an SSD computing node cluster.
  - When the server is an independent storage node, the nodes of different disk types are divided into a cluster, such as a SATA storage node cluster;
- Usually, a cluster server is recommended to access the same set of access switches, and the service data network is only transmitted within the cluster.
- If you use an independent storage node, you can divide it into a cluster with the compute node for disk mounting.
- Virtual machines only support mounting distributed block storage devices across clusters for data storage.

The cloud platform supports unified management of heterogeneous computing clusters such as x86, ARM, and GPU, and can uniformly manage storage clusters of SSD, STAT, NVME and other architectures. You can deploy virtual resources in different computing clusters and attach block storage devices of different storage clusters to virtual resources. At the same time, cloud platform virtualization can connect with IPSAN commercial storage devices through the ISCSI protocol, providing high-performance block storage services in the cluster for cloud platform virtual machines, and at the same time, it can benefit the centralized storage devices of old enterprise users, saving the total cost of ownership of information transformation.

The administrator console can easily manage and maintain the computing cluster, storage cluster, and external storage cluster of the data center, and the platform can control the permissions of the cluster, which is used to exclusively use some physical resources to one or some tenants, which is suitable for exclusive private cloud scenarios.

#### Compute clusters
A compute cluster is a group of compute nodes (physical machines) configured for the same purpose to deploy and host virtual compute resources running on the platform. A data center can deploy multiple different types of computing clusters, such as x86 clusters, ARM clusters, GPU clusters, etc., different clusters can run different types of virtual machine resources, such as GPU clusters can provide GPU virtual machines for tenants, and ARM clusters can provide tenants with virtual machines based on ARM or domestic OS.

To ensure high availability of virtual machines, the platform provides virtualization intelligent scheduling strategies based on the cluster latitude, including scattered deployment, online migration, and downtime migration, that is, virtual resources can be scheduled, deployed, and migrated across all computing nodes in the cluster to improve service availability.
- Dispersal deployment means that when a platform tenant creates a virtual machine, the created virtual machine is dispersed and deployed on all nodes in the cluster by default to ensure the availability of the tenant's business services under abnormal conditions such as hardware or software failures.
- Online migration refers to manually migrating a virtual machine from one physical machine in a cluster to another physical machine, freeing up the resources of the source physical machine, and supports random allocation and designated physical nodes.
- Downtime migration means that when an exception or failure occurs on the physical machine running a virtual machine, the scheduling system automatically migrates the virtual resources it carries to a healthy physical machine with normal load in the cluster to ensure service availability as much as possible.

Based on the logic of online migration and downtime migration, it is usually recommended to plan a single computing cluster for physical machine nodes with the same CPU and memory configuration in the deployment to avoid virtual machine exceptions or failure to start after migration due to inconsistent CPU architecture or configuration.

By default, the platform sets the cluster name according to the CPU platform architecture, and the administrator can modify the cluster name according to the platform's own usage. At the same time, administrators can manage physical machines and computing instances in a computing cluster. By default, the cluster is open to all tenants, and the platform supports permission control on the computing cluster, which is used to exclusively use some physical computing resources to one or some tenants, which is suitable for exclusive private cloud scenarios. After you modify the cluster permissions, the cluster can only be opened and used by specified tenants, and tenants without permissions cannot view and use the restricted cluster to create virtual resources.

#### Storage Clusters
A storage cluster is a platform distributed block storage cluster, which usually consists of a group of identically configured storage nodes (physical machines) that are used to deploy and host distributed storage resources. A data center can deploy multiple storage clusters of different types, such as SSD clusters, SATA clusters, capacity clusters, and performance clusters, and different clusters can provide different types of cloud disk sources, such as SSD storage clusters, which can provide SSD-type EVS disks for tenants.

The platform provides basic storage resources through distributed storage cluster architecture, supports online horizontal expansion, and integrates technologies such as intelligent storage cluster, multi-copy mechanism, data rebalancing, fault data reconstruction, data cleaning, automatic thin provisioning, QOS and snapshots to provide high performance, high reliability, high scalability, easy management and data security guarantee for virtualized storage, and comprehensively improve the service quality of storage virtualization and cloud platforms.

By default, distributed storage clusters support the three-replica policy, which first writes data to the primary replica when writing data, and the primary replica is responsible for synchronizing data with other replicas, and stores each copy of data on different disks across disks, servers, and cabinets to ensure data security in multiple dimensions. When there are no anomalies such as network outages or disk failures for the storage server nodes in the storage cluster, the replica data is always maintained as 3 replicas, regardless of the primary and standby replicas; 

When the number of abnormal copies of a storage node is less than three, the storage system automatically rebuilds the data copies to ensure that the number of data copies is permanently three, ensuring the security of virtualized storage data.

By default, the platform sets the cluster name according to the storage architecture, and the administrator can modify the cluster name according to the platform's own usage. It also enables administrators to manage storage clusters. By default, the cluster is open to all tenants, and the platform supports permission control on the storage cluster, which is used to exclusively use some physical storage resources to one or some tenants, which is suitable for exclusive private cloud scenarios. After you modify the cluster permissions, the cluster can only be opened and used by specified tenants, and tenants without permissions cannot view and use restricted clusters to create cloud disk resources.

#### External Storage Cluster
By default, the cloud platform provides distributed storage as virtualized back-end storage, providing high-availability, high-performance, highly reliable, and highly secure storage services for cloud platform users. At the same time, cloud platform virtualization supports docking with commercial storage devices, such as IPSAN and other storage arrays, to provide high-performance block storage services in the cluster for cloud platform virtual machines, and at the same time can benefit the centralized storage devices of old enterprise users, saving the total cost of ownership of information transformation as a whole.

External storage service is a commercial storage service provided by the cloud platform for enterprise users, currently supports ISCSI protocol and FC protocol docking commercial storage, uses commercial storage as a virtualized back-end storage pool, provides storage pool management and logical volume allocation, and can be directly used as the system disk and data disk of the virtual machine, that is, as long as the storage devices that support the ISCSI protocol and FC protocol can be used as the back-end storage of platform virtualization, adapting to a variety of application scenarios.

The platform supports the docking and management of storage devices, and supports assigning LUNs in storage devices to tenants, and tenants assigning or mounting LUNs to the system disk or data disk of virtual machines for reading and writing data, with the following specific features:
- It supports the entry management of storage device resource pools, and supports one-click scanning of created lun storage volume information in `ISCSI` devices and fc devices.
- Support for assigning scanned LUN storage volumes to platform tenants, giving tenants permission to use disks as system or data disks for virtual machines.
- Tenants can use the LUN storage volume information of the privileged as the system disk of the virtual machine, so that the virtual machine can run directly into the direct commercial storage to improve performance.
- Enables tenants to use privileged LUN storage volume information as data disks for virtual machines.
- Support for reassigning storage volumes to other tenants of the platform.
- Based on the above features, the platform can support the direct use of commercial storage devices as virtualized back-end storage, providing storage space for virtual machines with traditional commercial storage devices, without affecting other LUNs in commercial storage to provide storage services for other services.

The platform is based on the ISCSI protocol and FC protocol to dock commercial storage, and the LUNs of storage devices need to be mapped to platform compute nodes during the docking, so that virtual machines running on platform computing nodes can directly use the mapped LUNs; At the same time, in order to ensure the high availability of virtual machines, it is necessary to map LUNs to all compute nodes in a cluster at the same time, that is, all compute nodes can mount and use the mapped storage volumes, so as to ensure that the storage volume information can be mounted on each compute node during the outage migration.
- When the compute node on which the virtual machine resides fails, the platform automatically triggers the virtual machine downtime migration, that is, the virtual machine is migrated to the normal computing node in the computing cluster, so that the virtual machine can provide services normally.
- When the virtual machine is migrated to a new node in the cluster, you can directly use the mapped LUN storage to start the system disk or data disk of the virtual machine and mount it to the virtual machine normally to ensure the normal business after the virtual machine is migrated.

The platform only uses commercially stored LUNs as storage volumes, and does not manage the storage volumes themselves, such as LUN creation, mapping, expansion, snapshots, backups, rollbacks, cloning, and so on.

ISCSI protocol and FC protocol have their own emphasis: ISCSI is based on TCP/IP protocol, and the device supports the protocol consistently; The FC protocol is fast and requires the purchase of specialized switches. Users can mix and match according to their needs. Before using external storage, the platform manager or storage device manager needs to connect the external storage with the platform's computing node network, so that the computing nodes can communicate with the storage device directly on the internal network.

After the physical storage device and network are ready, you can connect with the platform and use the external storage services provided by the platform, and the entire docking process requires three roles of storage device administrator, platform administrator and platform tenant to operate, of which the operation related to the platform is the platform administrator and the platform tenant, as shown in the following figure process: