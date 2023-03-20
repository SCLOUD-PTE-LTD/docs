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
- Region refers to the geographic area of the SCloudStack cloud platform physical data center, such as Singapore, Tokyo, Seoul, etc.
- Different regions are completely physically isolated, and regions cannot be changed after cloud platform resources are created.
- The networks between different regions are completely isolated, and the internal networks of resources cannot be interconnected, and network communication can be carried out through the public network or private lines.
VPC and Server Load Balancer support deployment in the same region.

### Clustering
A cluster is a logical division of SCloudStack physical resources that distinguishes server nodes with different configuration specifications and different storage types. The logical relationships between regions, clusters, and physical servers are as follows:

![1](/docs/assets/images/product-functional-architecture-1.png)

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

![1](/docs/assets/images/product-functional-architecture-2.jpg)

- Storage device administrators manage storage volumes<br/>
All storage volume management is handled by the storage device administrator on the commercial storage management system, including the creation and mapping of storage volumes (LUNs), as well as the related lifecycle management of storage volume expansion, snapshots, backups, and deletions.
- The storage device administrator maps storage volumes to cluster compute nodes<br/>
The created LUN is mapped to all compute nodes on the storage device by the storage device administrator (if new compute nodes are added, they need to be mapped again), and multipath mapping is also possible.
- The platform administrator logs in and manages storage devices<br/>
  ISCSI storage:<br/>
After the storage volume LUN mapping is successful, the platform administrator enters the ISCSI storage pool or storage device in the management console "External Storage - ISCSI", and you need to specify the ISCSI address of the storage device, such as 172.18.12.8:8080 when entering.<br/>
FC storage: No device address entry required.
- The platform administrator scans the mapped LUN information<br/>
ISCSI storage:<br/>
After the storage device is entered, the platform administrator scans the storage device and information that have been mapped to the cluster node in the storage device with one click.<br/>
FC Storage:<br/>
After the storage volume LUN mapping is successful, the Storage Devices present in the system can be added to the platform by scanning by the Platform Administrator in the Management Console External Storage - FC SAN.
- The platform administrator assigns LUN devices to the tenant<br/>
The LUN storage volume device that successfully scans is assigned to the tenant by the Platform Administrator, and a storage volume can only be assigned to one tenant at a time, after allocation, the tenant can query the allocated storage volume device in the external storage device, and create virtual machines or mount virtual machines.
- Platform tenants use LUN storage volume devices<br/>
Platform tenants can directly query the allocated storage volumes through the console external storage, and specify the system disk type as external storage when creating a virtual machine, or they can directly attach LUN storage volumes to existing virtual machines and use them as data disks of virtual machines.<br/>
The premise for platform tenants to use external storage services is that storage volumes are mapped and assigned to tenants, and tenants only need simple binding to easily use the external storage devices provided by the platform, and can be elastically bound and unbound.

## Virtual Machines
Virtual machine is the core service of SCloudStack cloud platform, providing scalable computing power services at any time, including the most basic computing components such as CPU, memory, operating system, etc., and combining with network, disk, security and other services to provide a complete computing environment. Build IT architecture by combining with services such as load balancing, databases, caching, object storage, and more.

The SCloudStack cloud platform virtualizes the computing resources of physical servers through KVM (Kernel-based Virtual Machine) to provide computing resources for virtual machines.

The computing resources of a virtual machine can only be located on one physical server, and when the physical server load is high or fails, it will be automatically migrated to other healthy physical servers.
Virtual machine computing power is expressed by virtual CPU (vCPU) and virtual memory, and storage capacity is reflected in cloud storage capacity and performance;

The hypervisor controls the QoS of vCPUs, memory, and disks to support virtual machine resource isolation to ensure that multiple virtual machines do not affect each other on the same physical server.

Virtual machines are the basic environment for cloud platform users to deploy and run application services, which is used in the same way as physical computers, providing complete life cycle functions such as creation, shutdown, power off, power on, password reset, system reinstallation, and upgrading. Support Linux, Windows and other different operating systems, and can be accessed and managed through VNC, SSH, etc., and have full control of virtual machines. The resources involved in virtual machine operation and their association relationships are as follows:

![1](/docs/assets/images/product-functional-architecture-3.jpg)

As shown in the figure, the instance type, image, and VPC network are the basic resources that must be specified to run the virtual machine, that is, the CPU memory, operating system, virtual NIC, and IP information of the virtual machine. On top of the virtual machine, you can bind EVS disks, EIPs, and security groups to provide data disks, public IP addresses, and network firewalls for virtual machines to ensure data storage and network security of virtual machine applications.

In terms of virtualization computing capabilities, the platform provides GPU device transparent transmission capabilities, allowing users to create and run GPU virtual machines on the platform, allowing virtual machines to have high-performance computing and graphics processing capabilities. Devices that support transparent transmission include `NVIDIA's K80, P40, V100, 2080, 2080Ti, T4, and Huawei Atlas300`.

### Instance Specifications
An instance profile is a configuration definition of a virtual machine's CPU memory that provides computing power to the virtual machine. CPU and memory are the basic attributes of a virtual machine, and you need to cooperate with images, VPC networks, EVS disks, security groups, and keys to provide a fully capable virtual machine.

- By default, instance types such as 1C2G, 2C4G, 4C8G, 8C16G, and 16C32G are provided.
- Supports custom instance types and provides multiple CPU memory combinations to meet the load requirements of different application scales and scenarios.
- Support for upgrading and downgrading virtual machine CPU and memory configurations, which can be adjusted by changing the instance type;
- After the instance type is changed, you need to restart the virtual machine to take effect.
- Instance types are consistent with the lifecycle of virtual machines, and are released when a virtual machine is destroyed.

Create VM specifications support creating different specifications for different clusters, which can create different specifications for different models, and create virtual machines with different specifications when tenants create virtual machines to adapt to application scenarios where the hardware configuration of different clusters is inconsistent. CPU and memory can be defined separately:

- CPU specification support (C): Increase in multiples of 2 except 1, such as 1C, 2C, 4C, 6C, and the maximum value is 240C.
- Memory specification support (G): Increase in multiples of 2 except 1, such as 1G, 2G, 4G, 6G, and the maximum value is 1024G.

The created specifications can be seen and used by all tenants, and different specifications can be created in different clusters according to business needs.

### Mirroring
An image is a template for the running environment of a virtual machine instance, usually including the operating system, preinstalled applications, and related configurations. The hypervisor uses the specified image template as the system disk of the boot instance, and the life cycle is the same as that of the virtual machine, and the system disk is destroyed when the virtual machine is destroyed. Platform virtual machine images are divided into base images and homemade images.
#### Base image
The base image is officially provided by SCloudStack and includes multiple distributions of native operating systems such as Centos, Ubuntu and Windows.

- The base image is available to all tenants by default, and the default images include `Centos 6.5 64, Centos 7.4 64, Windows 2008r2 64, Windows 2012r2 64, Ubuntu 14.04 64, Ubuntu 16.04 64`.
- The basic image has been systematically tested and regularly updated and maintained to ensure the safe and stable operation and use of the image.
- The base image is an image provided by default by the system, which only supports viewing and running virtual machines through the image, and does not support modification, creation, and deletion.
- The default system disk for Linux images is 40 GB and the default system disk for Windows images is 40 GB.
- Supports management of copying the image made or imported by the tenant as the base image and sharing it with all tenants of the platform as the default base image; At the same time, administrators can modify the name, remarks, and delete the base image.
- 
Support reinstalling the system, that is, replacing the virtual machine image, Linux virtual machine only supports replacing Centos and Ubuntu operating systems, Windows virtual machine only supports replacing other versions of Windows operating systems;

Windows operating system images are officially provided by Microsoft and need to purchase license activation by yourself.
#### Self-managed images
Custom images are self-owned images exported or custom-imported by tenants or administrators through virtual machines, which can be used to create virtual machines, and only the account itself has permission to view and manage them except platform administrators.

- Support management will import custom images for tenants, and administrators can export tenants' virtual machines as homemade images; At the same time, the administrator can download all the self-made images in the image repository.
- Administrators can create virtual machines, delete homemade images, and modify the name of homemade images from homemade images.

In order to facilitate the sharing of platform image template files, the platform supports administrators to copy a self-made image as a base image, so that a tenant's self-made image can be shared with all tenants, which is suitable for scenarios where the operation and maintenance department makes a template image, such as after the vulnerability repair or upgrade of the self-made image operating system, make a self-made image and copy the base image, so that all tenants can use the new image file to upgrade the virtual machine system.

#### Mirror storage
By default, both the base image and the user-made image are stored in the distributed storage system, ensuring performance and ensuring data security through three copies.

- The image supports `QCOW2` format, which can convert RAW, VMDK and other format images into `QCOW2` format files for V2V migration scenarios.
- All images are stored in a distributed storage system, that is, the image files are distributed on the disks of the underlying computing storage `hyperconverged` node.
- If it is an isolated storage node, it is distributed across all disks of the isolated storage node;
- Only virtual machines in the local domain can be created for an image in a region, and you cannot create virtual machines across region images.

### Virtual NIC
A virtual NIC (Virtual NIC) is a virtual network device that communicates with the outside world and is created by default with the VPC network when the virtual machine is created. The virtual NIC is the same as the life cycle of the virtual machine, and cannot be detached, and the virtual NIC is destroyed when the virtual machine is destroyed. For more information about VPC networks, see VPC Networks .

The virtual NIC is implemented based on Virtio, and QEMU provides a set of Tun/Tap emulation devices through APIs to bridge the network of virtual machines to the host NIC and communicate with other virtual networks through OVS.
- By default, each virtual machine generates two virtual NICs to carry the internal and external network communication of the virtual machine.
- When the virtual machine starts, a DHCP request is automatically initiated to obtain the private IP address based on the selected VPC subnet, and the network information is configured on a virtual NIC to provide private network access for the virtual machine.
- After the virtual machine is started, you can apply for a public IP address (external IP) to be bound to the virtual machine to provide Internet access services
  - The bound public IP address automatically configures the public IP information on another virtual NIC to provide external network access for the virtual machine.
  - A virtual machine supports binding 50 public IPv4 and 10 IPv6 addresses.
- Modifying the IP address of a vNIC is not supported, and manually modified IP addresses will not take effect.
- Each vNIC can be bound to a security group to provide NIC-level security control.
- Supports virtual NIC QoS control and provides custom settings for the ingress/ingress bandwidth of the vNIC.

By default, the platform provides two virtual NICs, and if your service has two NICs, you can bind an ENI to provide multi-network services for virtual machines.

### ENIs
Elastic Network Interface (ENI) is an elastic network interface that can be attached to virtual machines at any time, supports binding and unbinding, can be flexibly migrated between multiple virtual machines, provides high-availability cluster construction capabilities for virtual machines, and can achieve refined network management and cheap failover solutions.

ENIs and the default NICs (one internal NIC and one external NIC) that come with a virtual machine are virtual network devices that provide network transmission for virtual machines, which are divided into two types: internal NICs and external NICs, and assign IP addresses, gateways, subnet masks, and routing-related network information from the network to which they belong.

- The ENI of the private network type belongs to a VPC and a subnet, and an IP address is automatically or manually assigned from the VPC.
- The network to which the EIP belongs is an external CIDR block, and an IP address is automatically or manually assigned from the external CIDR segment, and the assigned IP address is consistent with the lifecycle of the ENI, and can only be released when the ENI is destroyed.
- If the NIC type is Internet, the NIC is billed based on the bandwidth specifications of the selected Internet IP, and you can select the appropriate payment method and purchase duration according to your business needs.

The default NIC of a virtual machine belongs to the VPC and subnet specified when the virtual machine is created, and an ENI of a different VPC is bound to the virtual machine, so that the virtual machine can communicate with the virtual machine of a different VPC network.

ENIs have an independent lifecycle, support binding and unbinding management, and can be freely migrated between multiple virtual machines. When a virtual machine is destroyed, the ENI is automatically debound and bound to another virtual machine.

ENIs have the region (data center) attribute and can only bind virtual machines to the same data center. An ENI can only be bound to one VM, an x86 VM can bind up to 6 ENIs, and an ARM VM can bind up to 3 NICs. After the external ENI is bound to a virtual machine, the default network egress policy of the virtual machine is not affected, including the external IP address bound by the ENI on the virtual machine, the first IP with a default route is used as the default network egress of the virtual machine, and you can set a public IP address with a default route as the default network egress of the virtual machine.

Each ENI can be assigned only one IP address, and a security group can be bound to the ENI as needed to control traffic to and from the ENI to achieve refined network security control. If you do not need to control the traffic of the ENI, you can empty the security group of the ENI.

Users can custom-create network cards through the platform, bind, unbind, and modify security groups on the network cards, and perform the Adjust Bandwidth operation for external ENIs to adjust the bandwidth limit of external IP addresses on external ENIs.

ENIs have attributes such as region, NIC type, VPC, subnet, public CIDR block, public IP bandwidth, IP address, and security group, and can be managed during the life cycle of creating, binding, unbinding, binding, unbind, and deleting ENIs.

- Region: ENIs can only be bound to virtual machines in the same region.
- NIC type: The network access type of the ENI supports VPC intranet and EIP external network type.
- VPC/subnet: An intranet ENI can only be added to a VPC and subnet, and you cannot modify the VPC or subnet after it is created.
- Internet CIDR block: An Internet ENI can only assign an IP address from an external CIDR block, and cannot modify it after it is created.
- Internet IP bandwidth: The bandwidth of the IP address assigned by the external network card.
- IP address: You can manually specify and automatically obtain the IP address of an ENI in a subnet or public CIDR segment.
- Security group: Each ENI can be bound to a security group to provide NIC-level security control, see Security groups for details.
- MAC address: Each ENI has a globally unique MAC address.

The entire life cycle of an ENI includes states such as creating, unbound, bound, bound, bound, unbound, and deleted.