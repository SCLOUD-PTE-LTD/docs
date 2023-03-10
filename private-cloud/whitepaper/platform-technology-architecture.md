---
layout: default
title: Platform Technology Architecture
parent: Whitepaper
grand_parent: Private Cloud
permalink: /private-cloud/whitepaper/platform-technology-architecture/
nav_order: 3
---
# Platform Technology Architecture
Based on the SCloud public cloud platform, SCloudStack platform reuses the core components of the public cloud, has the basic capabilities of computing virtualization, intelligent scheduling, storage virtualization, and network virtualization, provides users with software-defined computing, storage, network and resource management services, and provides unified resource scheduling and management services while ensuring the performance, availability and security of resource services, adapting to various application scenarios of enterprise infrastructure services.

In addition to standardized general product services, the platform provides a multi-core information and innovation government cloud platform for localized scenarios, fully adapting to the government cloud from chip to application. At the same time, it provides a new outstanding private cloud with a coordinated design of software and hardware for high-performance scenarios.

## Compute virtualization
Cloud computing technology is an extension of virtualization technology, computing virtualization is to add a hypervisor on top of the hardware, through which multiple completely isolated hosts and can install different operating systems, carry different applications to run, to the greatest extent to solve the problem of a physical machine being occupied by a system or an application, effectively improve resource utilization.

![1](/docs/assets/images/platform-technology-architecture-1.jpg)

As shown in the preceding figure, there are essential differences between physical machines and virtual machines in terms of application deployment and resource consumption:

### Physical machine environment
- The operating system is directly installed on the physical machine, usually a physical machine only supports the installation of one operating system;
- All applications and services need to be deployed on the physical operating system and share the underlying hardware resources.
- When multiple applications have inconsistent requirements for the underlying operating system and components, the application may not run normally, and the two applications need to be deployed on a single physical machine, and the resource utilization rate is low during non-business peak hours.

### Virtual machine environment
- Add a hypervisor layer on top of the hardware layer and the operating system as the engine of computing virtualization;
- The virtualization engine supports virtualizing the underlying hardware into multiple hosts, that is, virtual machines;
- Each virtual machine has independent hardware facilities, such as CPU, memory, disk, network card, etc.;
- Each virtual machine can be installed and run a different operating system (GuestOS) independently, completely isolated from each other, and not affected by each other;
- Each virtual machine operating system is consistent with the operating system of the physical machine, has independent components and library files, and can run exclusive application services.
- Virtual machines with multiple applications run on a single physical machine without affecting each other, and share the resources of the physical machine, improving the resource utilization and management efficiency of the physical machine.

SCloudStack compute virtualization uses hypervisor components and technologies such as `KVM` and `Qemu` to abstract `x86/ARM` server resources with a common bare metal architecture and present them to users as virtual machines. Virtual machine converts server physical resources such as CPU, memory, I/O, and disk into a set of logical resources that can be managed, scheduled and allocated in a unified manner, and builds multiple simultaneous and isolated virtual machine execution environments on the physical machine based on the virtual machine, which can make full use of hardware-assisted full virtualization technology to achieve high resource utilization while meeting the dynamic resource allocation requirements of more flexible applications, such as rapid deployment, balanced resource deployment, system reset, online configuration change and hot migration. Reduce the operational costs of application services, improve the flexibility of deployment and O&M, and improve the speed of service response.

SCloudStack compute virtualization is implemented through KVM hardware-assisted, all-virtualization technology, so it requires the support of CPU virtualization features, that is, the compute node CPUs are required to support virtualization technologies such as Intel VT and AMD V technology. KVM is a module of the Linux kernel, the virtualization platform can start KVM by loading kernel modules, manage device drivers for virtual hardware, emulate CPU and memory resources, and need to load QEMU modules to simulate I/O devices. KVM VMs include virtual memory, virtual CPUs, and VM I/O devices, where KVM is used for CPU and memory virtualization and QEMU is used for virtualization of I/O devices.

The virtual machine is not directly aware of the physical CPU, and its compute units are rendered through the vCPU and memory abstracted by the Hypervisor abstraction, and the virtual machine system is built in conjunction with GuestOS. Virtualization of I/O devices is the hypervisor multiplexing peripheral resources, presented by software simulation of real hardware, providing GuestOS with peripherals such as network cards, disks, USB devices, and so on.

Compute virtualization is the server virtualization component of SCloudStack's enterprise proprietary cloud platform and is the core component of the entire cloud platform architecture. While providing basic computing resources, it supports features such as CPU over-resolution, `QCOW2` image files, GPU transparent transmission, abnormal restart of virtual machines, and smooth cluster expansion.

### CPU overscore
SCloudStack supports platform physical CPU over-splitting, that is, the number of vCPUs that can be virtualized by the platform can be greater than the number of pCPUs, and when the CPU resources allocated to virtual machines are not fully used, the unused part is shared for other virtual machines to use, further improving the CPU resource utilization of the platform. Take one compute node server with dual CPUs as an example:

Dual-socket CPUs are 2 physical CPUs, each physical CPU is 12 cores, and dual threads are enabled;

Each CPU has 24 cores, and two CPUs have 48 cores to allocate 48 vCPUs;
Under normal circumstances, the virtual machine vCPU that can be provided is 48C;
If the platform administrator enables CPU overscoring and sets the overscore ratio to 1:2, the number of vCPUs that can be used is twice the actual number of CPUs. After the server (48C) is enabled for 2X superscore, it can actually create a virtual machine with a vCPU of 96 to create a 96C virtual machine.

Platform administrators are allowed to set and manage CPU overscores and view the actual CPU usage and vCPU usage of the platform. Since multiple virtual machines may share vCPUs after you enable over-resolution, it is generally recommended to reduce the CPU over-scoring ratio as much as possible, or even not to enable CPU over-scoring in order not to significantly affect the performance and availability of the virtual machine.

If the platform actually has a total of 48 vCPUs, a virtual machine with 96 vCPUs can be created after over-partitioning, which may truly account for the performance of 48 vCPUs at the peak of the virtual machine service, and the performance of the virtual machine running through the over-partitioned resources will drop rapidly, and even affect the normal operation of the virtual machine. The CPU over-score ratio needs to be adjusted through the data of long-term operation operations, which has a strong correlation with the business applications running on the platform virtual machines, and it is necessary to flexibly adjust the amount of CPU resources required by the platform during peak business for a long time.

### RAW image files
The SCloudStack platform uses a RAW-formatted image as a virtual disk file for a virtual machine, or the original image.

The RAW image is directly used as a block device for virtual machines, and the host file system manages the hole in the image file, such as creating a 100GB RAW image file, which actually occupies very little space. When the virtual machine's GuestOS reads and writes to the disk, it is calculated and addressed in the form of CHS variables, and the value is translated into the RAW image-specific format through the KVM driver for IO operations.

The advantage of RAW images is that they are more efficient in starting virtual machines, that is, they start virtual machines faster, which is 25% faster than virtual machines in QCOW2 format images, and RAW images support conversion to QCOW2 format.

RAW images and running virtual machine block devices are stored in a unified distributed storage system to facilitate virtual machine migration and failure recovery.

### GPU transparent transmission
The SCloudStack platform supports GPU device transparent transmission capability, providing GPU virtual machine services for platform users, allowing virtual machines to have high-performance computing and graphics processing capabilities. GPU virtual machines have tens of times higher performance than traditional architecture in scientific computing performance, and can be paired with SSD cloud disks, and IO performance is more than ten times that of ordinary disks, which can effectively improve the computing efficiency of graphics processing and scientific computing and reduce IT cost investment.

GPU virtual machines and standard virtual machines adopt consistent management methods, including internal and external network IP allocation, elastic network cards, subnet and security group management, and can perform full life cycle management of GPU virtual machines, including password reset, configuration change and monitoring, etc., the usage mode is consistent with ordinary virtual machines, support a variety of operating systems, such as CentOS, Ubuntu, Windows, etc., without additional management, to provide tenants with fast GPU computing services.

To get the best performance out of the GPU, the platform defines the combination of GPU, CPU, and memory as follows:

| GPU | CPU | Memory |
| --- | --- | --- |
| 1psc | 4Core | 8G???16G |
| | 8Core | 16G???32G |
| 2psc | 8Core | 16G???32G |
| | 16Core | 32G???64G |
| 4psc | 16Core | 32G???64G |
| | 32Core | 64G???128G |

The platform itself does not limit GPU brands and models, that is, supports transparent transmission of any GPU device, and has been tested and compatible with K80, P40, V100, 2080, 2080Ti, T4 and Huawei Atlas300 with GPU models of NVIDIA.

*The platform does not support GPU virtualization by default, and if you need GPU virtualization capabilities, you need to purchase a GPU virtualization license.*

### Smooth cluster expansion
The SCloudStack platform supports smooth expansion of compute nodes in the cluster, and the new nodes will not affect the operation of existing nodes and virtual resources. By smoothly expanding the platform, the platform administrator can easily solve the resource expansion caused by the platform's business growth, including scenarios such as insufficient hardware resources, high-load host maintenance, and resource expansion of new services.

SCloudStack cluster expansion ensures uninterrupted services and normal operation of virtual resources during node expansion, provides simple and fast deployment operations, and supports one-click deployment of automated scripts. After capacity expansion, the platform supports adding disk functions to nodes online, so that administrators can expand resources horizontally and vertically for the platform without affecting the stable operation of the platform.

After successful smooth expansion, the original virtual resources of the platform will remain in the original state, and when the platform has new virtual resources to run and deploy, the intelligent scheduling platform will schedule the new virtual resources (such as virtual machines) to the nodes of the smooth expansion. If a physical machine fails on the platform, the virtual machines on the original physical machine are migrated to the newly expanded node according to the scheduling policy. Platform administrators can manually migrate a virtual machine to a newly scaled node to balance the overall resource utilization of the platform.

## Intelligent scheduling
Intelligent scheduling is the core of SCloudStack platform virtual machine resource scheduling management, and the [Schedule Manager] module is responsible for the control and management of scheduling tasks, supports anti-affinity deployment strategy, and is used to decide which physical server the virtual machine runs on, and manages the virtual machine status and migration plan to ensure the availability and reliability of the virtual machine.

The intelligent scheduling system monitors the compute, storage, network and other load information of all computing nodes in the cluster in real time, and serves as the data basis for virtual machine scheduling and management. When new virtual resources need to be deployed, the scheduling system preferentially selects low-load nodes for deployment to ensure the load of the entire cluster nodes. As shown in the following figure, the newly created virtual resource is automatically deployed to a low-load Node3 node through scheduling detection.

![1](/docs/assets/images/platform-technology-architecture-2.jpg)

While giving priority to low-load nodes for virtual resource deployment, the scheduling system provides capabilities such as scattered deployment, online migration, and downtime migration to ensure the reliability of the cloud platform as a whole. The SCloudStack cloud platform uses distributed storage to provide storage services, as shown in the figure above, virtual machines run on distributed storage pools, and distributed storage pools can build a unified distributed storage resource pool across multiple physical machines. The system disk, image file and mounted hard disk of the virtual machine are stored in the unified distributed storage pool, and each computing node can register the same virtual machine process through the system disk file and configuration information of the virtual machine in the distributed storage pool, which can be used for online migration or downtime migration tasks.

### Scatter deployments
Dispersed deployment, that is, virtual resource anti-affinity deployment strategy, refers to the scattered and balanced distribution of virtual resources running the same application service on the underlying cluster physical servers according to the scheduling strategy to ensure the high availability of services under abnormal conditions such as hardware or software failures.

SCloudStack cloud platform supports the virtual machine dispersion deployment strategy by default, that is, when a user's virtual machine is created, it will give priority to robust nodes for deployment, and ensure that a user's virtual machine is scattered and deployed to the nodes of the underlying cluster as much as possible to ensure business robustness. At the same time, the platform supports manual dispersal marking, users can distinguish virtual resources of different services through [business groups], and virtual machines of the same business group will be evenly distributed on the clustered physical servers according to the intelligent scheduling policy to ensure high availability of business services.

- When a user identifies two virtual machines as the same business group, the same or related business application services are deployed on behalf of the two virtual machines.
- The platform scheduling policy deploys two virtual machines on different compute nodes based on the business group identity, that is, try to ensure that the two virtual machines are not deployed on the same server node.
- If two virtual machines of the same service are deployed to the same physical machine, if the physical machine fails, both virtual machines will fail and undergo downtime migration at the same time, which will affect the normal service provision of business applications.
- If two virtual machines of the same service are deployed to different physical machines, if one of the virtual machines fails, only one virtual machine will be down and migrated, which will not affect the normal service provision of business applications.

If VM1 and VM5 in the intelligent scheduling diagram belong to the same business group, the system will automatically deploy VM1 to Node1 and VM5 to Node3, when Node1 node fails, VM5 will continue to provide services, and VM1 will automatically migrate to healthy and well-loaded compute nodes, see Downtime Migration for details.

### Online Migration
Online migration (hot migration of virtual machines) is a planned migration operation, that is, online cross-machine migration between different physical machines without downtime of virtual machines. First, a virtual machine process with the same configuration is registered on the target physical machine, and then the virtual machine memory data is synchronized, and finally the service is quickly switched to the target new virtual machine. The entire migration and switching process is very short, which hardly affects or interrupts the user's business running in the virtual machine, which is suitable for scenarios such as dynamic adjustment of cloud platform resources, shutdown and maintenance of physical machines, and optimization of server energy consumption, further enhancing the reliability of the cloud platform.

Due to the use of distributed unified storage, only the running location of [compute] is migrated during online migration of virtual machines, and does not involve the migration of storage (system disks, images, and EVS disks). During migration, you only need to register a virtual machine process with the same configuration and paused status on the destination host through the source virtual machine configuration file in the unified storage, and then repeatedly migrate the memory of the source virtual machine to the destination virtual machine, shut down the source virtual machine and activate the destination virtual machine process after the virtual machine memory synchronization is consistent, and finally perform a network switch and successfully take over the source virtual machine business.

The entire migration task is only interrupted briefly when the target virtual machine is activated and the network is switched, and the source virtual machine service is almost unaware because the activation and switchover time is short, less than the TCP timeout retransmission timeout. At the same time, because there is no need to migrate virtual machine disks and image locations, VM-mounted cloud disks are not affected after migration, providing users with unaware migration services that carry stored data. The specific migration process is as follows:

- Register the target virtual machine
  - The scheduling system registers an identically configured virtual machine process on the target host using the source virtual machine profile within the unified distributed storage;
  - The registered virtual machine process is in the paused state of the non-serviceable and receives migration data by listening on a TCP port;
  - The stage of registering the target virtual machine is instantaneous and usually takes a few milliseconds, when the source virtual machine is in a normal state of service provision.
- Migrate source virtual machine memory
  - When the target virtual machine is registered, the scheduling system immediately migrates the full memory data of the source virtual machine to the destination virtual machine.
  - In order to ensure the consistency of data migration, the memory updates of the source virtual machine also need to be synchronized during the migration process, so the scheduling system migrates the new memory data generated by the source virtual machine to the target end through multiple iterations, which is related to the network bandwidth, performance and memory size of the virtual machine.
  - During memory migration, the source virtual machine provides services normally, and the source virtual machine process is immediately suspended when the memory data is repeatedly iteratively migrated to avoid generating new memory data.
  - After the source virtual machine process is paused, the memory data is synchronized again to ensure the data consistency between the source and target sides.
- Take over the source virtual machine service
  - After completing the memory synchronization, the scheduling system shuts down the process of the source virtual machine and activates the target virtual machine to achieve smooth operation of the virtual machine.
  - When a virtual machine is migrated from the source host to the destination host, the system switches the network of the virtual machine to the target host (delivery flow table), communicates through the vSwitch of the destination host, and successfully takes over the source virtual machine service.
  - If the virtual machine has a bound public IP address, the external IP address automatically drifts to the destination host when the network is switched and communicates through the flow table in OVS.

During the entire migration process, from the pause of the source virtual machine to the activation of the destination virtual machine and the completion of the network switchover to downtime, because the activation of the virtual machine and the network switchover time is very short, usually less than a few hundred milliseconds, less than the TCP timeout retransmission timeout, which is negligible for most application services, so the virtual machine business will hardly perceive the migration downtime. If VM6 in the intelligent scheduling diagram runs on Node1 by default, the administrator can manually migrate VM6 to Node3 through the online migration function:

- After the scheduling system receives the migration instruction, it will immediately register a suspended virtual machine process on the Node3 node using the configuration file of VM6;
- Immediately migrate the full process data of VM6 to VM6' of the Node3 node, and repeatedly migrate and update the memory data;
- The scheduling system pauses VM6 VMs on Node1, migrates memory data again, and shuts down VM6 VMs;
- Activate the VM6' virtual machine process on the Node3 node to complete the network switchover and take over the business services and communication of VM6;
- If VM6 has an EVS disk mounted, after the migration is successful, the mount information and configuration of the EVS disk are not affected, and the EVS disk can be read and written normally

### Downtime Migration
Downtime migration, also known as offline migration or high availability of virtual machines, refers to the fact that when the underlying physical machine of the platform is abnormal or faulty, the scheduling system will automatically migrate the virtual resources it carries to a healthy and well-loaded physical machine to ensure service availability as much as possible. The overall downtime migration does not involve storage and data migration, and the new virtual machine can be quickly run on the new physical machine, and the average migration time is about 90 seconds, which may affect or disrupt the business running in the virtual machine.

Due to the use of distributed unified storage, the system disk of the virtual machine and the data written to the system disk are stored in the underlying distributed storage, and the virtual machine downtime migration only migrates the running location of [compute], does not involve the migration of storage (system disk, image, and EVS disk) location, and only needs to restart the virtual machine on the new physical machine and ensure network communication. The migration mechanism is described as follows:

- The SCloudStack scheduling management system periodically monitors the health status of physical machines at intervals of 1 second;
- When a physical machine is detected, the scheduling system continues to probe for 50 cycles (that is, 50 seconds);
- If the physical machine still fails for 50 cycles, Layer 2 ping detection is triggered at an interval of 400ms for 10 cycles (that is, 4 seconds);
- If Layer 2 detection still fails, the virtual machine migration operation is triggered, that is, the system is scheduled to start the migration task after 54 seconds of continuous failure of the physical machine.
- The scheduling system uses the system disk and data of the failed virtual machine in the distributed storage to restart the virtual machine on the new physical machine, and the startup process and state flow are consistent with the newly created virtual machine, and the average startup time is about 30 seconds.
- After the virtual machine is started on the new physical machine, the virtual machine network is switched to the new physical machine and communicates through the flow table issued in OVS.
- If the virtual machine has a bound public IP address, the public IP address is automatically drifted to the target host after migration and communicates through flow tables in OVS.

The entire migration process, from the detection of failures to the successful migration, averaged about 90 seconds. The virtual machine startup time is related to the components and configurations of the source virtual machine, such as bound EVS disks, external IP addresses, ENIs, and operating systems. At the same time, due to excessive virtual machine specifications, insufficient underlying physical resources, and underlying hardware failures, the migration may fail to occur, and it is recommended to ensure that the underlying physical resources are sufficient as much as possible. If the Node2 node in the intelligent scheduling diagram fails, the intelligent scheduling system automatically migrates VM3 and VM4 to the Node1 and Node3 nodes respectively, as follows:

- After periodic monitoring and Layer 2 detection, the scheduling system determines that the Node2 node is faulty, and the VM3/VM4 virtual machines are unavailable, and downtime migration operations are required.
- According to the collected cluster node information, the scheduling system uses the system disk and data of VM3 in the distributed storage system to start VM3 virtual machines at Node1 node, and re-issues the flow table after startup to switch the network information of VM3 to Node1.
- Use the system disk and data of VM4 in the distributed storage system to start VM4 virtual machines on the Node3 node, and then re-issue the flow table after startup to switch the network information of VM4 to Node3.
- If VM3 or VM4 is bound to a public IP address, the external IP address drifts to the Node1 and Node3 nodes after the virtual machine starts, and communicates through the flow table in the OVS.

The prerequisite for an outage migration is that there are at least two or more physical servers in the cluster, and that the resources and network connectivity of healthy nodes must be ensured during the migration process. Through downtime migration technology, it provides high availability for business systems and greatly reduces the outage time caused by various host physical failures or link failures.

## Storage Virtualization
The cloud computing platform maximizes the efficiency of resource utilization and business operation and maintenance management through hardware-assisted virtualization computing technology, reduces the total cost of ownership of IT infrastructure as a whole, and effectively improves the availability, reliability and stability of business services. While addressing computing resources, enterprises also need to consider the data storage suitable for virtualized computing platforms, including storage security, reliability, scalability, ease of use, performance, and cost.

The virtualized computing KVM platform can connect to various types of storage systems, such as local disks, commercial SAN storage devices, NFS, and distributed storage systems, respectively, to solve the data storage requirements of virtualized computing in different application scenarios.

- Local disk: The local disk on the server, usually RAID striped to ensure disk data security. High performance, poor scalability, difficult migration in virtualized environments, suitable for high-performance business scenarios that basically do not consider data security.
- Commercial storage: that is, disk arrays, usually a single storage that integrates software and hardware, using RAID to ensure data security. High performance, high cost, and virtual migration with shared file systems, which is suitable for large-scale application data storage scenarios such as Oracle Database.
- NFS system: Shared file system, low performance, good ease of use, cannot ensure data security, and is suitable for scenarios where multiple virtual machines share reading and writing
- Distributed storage system: Software-defined storage, which adopts the standard of a general-purpose distributed storage system, aggregates the disk resources of a large number of general-purpose x86 cheap servers to provide unified storage services. It ensures data security through multiple copies, with high reliability, high performance, high security, easy expansion, easy migration, and low cost, and is suitable for storage scenarios such as virtualization, cloud computing, big data, enterprise office, and unstructured data storage.

Each type of storage system has advantages and disadvantages in different storage scenarios, and the virtualization computing platform needs to select the appropriate storage system according to the business characteristics to provide storage virtualization functions, and in some specific business modes, it may be necessary to provide a variety of storage systems for different application services.

In traditional storage architectures, clients communicating with centralized storage components with a single entry point can limit the performance and scalability of the storage system and introduce a single point of failure. SCloudStack platform adopts distributed storage system as virtualization storage for docking KVM virtualization computing and general data storage services, eliminates centralized gateways, enables clients to interact directly with the storage system, and ensures data security and availability with data protection mechanisms such as multi-copy/erasure coding, multi-level fault domains, data re-balancing, and fault data reconstruction.

### Distributed Storage
Based on Ceph distributed storage system adaptation optimization, SCloudStack cloud platform provides a set of pure software-defined, high-performance, highly reliable, highly scalable, high-security, easy-to-manage, and low-cost virtualized storage solutions for virtualized computing platforms that can be deployed on x86 general-purpose servers, while having great scalability. As the core component of the cloud platform, it provides users with a variety of storage services and petabyte-level data storage capabilities, suitable for application scenarios such as virtual machines and databases, meets the storage requirements of key services, and ensures efficient, stable, and reliable business operation.

The distributed storage service integrates the disk storage resources of a large number of x86 general-purpose servers together to build an infinitely scalable unified distributed storage cluster, realize the unified management and scheduling of all storage resources in the data center, and provide [block] storage interfaces to the virtualization computing layer for cloud platform virtual machines or virtual resources to freely allocate and use the storage space in the storage resource pool according to their own needs. At the same time, cloud platform virtualization connects IPSAN commercial storage devices through the iSCSI protocol, uses commercial storage as a virtualized back-end storage pool, provides storage pool management and logical volume allocation, and can be directly used as the system disk and data disk of the virtual machine, that is, as long as the storage device that supports the iSCSI protocol can be used as the back-end storage of platform virtualization, adapting to a variety of application scenarios. It can use the centralized storage devices of old enterprise users and save the total cost of ownership of information transformation as a whole.

Storage function WYSIWYG, users do not need to pay attention to the type and capability of storage devices, you can quickly use virtualized storage services on the cloud platform, such as virtual disk mounting, expansion, incremental snapshots, monitoring, etc., cloud platform users use virtual disks in the same way as using the local hard disk of x86 servers, such as formatting, installing operating systems, reading and writing data, etc. Cloud platform administrators and maintainers can globally and uniformly configure and manage the overall virtualized storage resources of the platform, such as QoS limits, storage pool expansion, storage specifications, and storage policy configurations.

The distributed storage system can provide block storage, file storage, and object storage services, which are suitable for various data storage application scenarios, while ensuring data security and cluster service reliability. File storage and object storage are deployed in a single data center, supporting mixed deployment of mechanical disks and high-performance disks, and can logically divide multiple storage pools, such as high-performance storage pools and capacity-based storage pools. In the deployment of block storage, it is generally recommended to use the same type of disks to build storage clusters, such as hyperconverged computing nodes and independent storage nodes with SSD disks to build high-performance storage clusters. The SATA/SAS disks that come with hyperfinance compute nodes and independent storage nodes are built as normal performance storage clusters.

The distributed storage storage system provides elastic block storage services, object storage, and file storage services for disk devices in the cluster through different storage resource pools built into the OSD, among which the block storage service can be directly mounted and used by virtual machines, and measures such as three copies, write confirmation mechanism, and copy distribution policy are used to ensure data security and availability to the greatest extent when data is written. File storage and object storage can provide multiple protocol interfaces such as NFS, CIFS, and S3 to provide unstructured data storage services for application services, and combine multiple copies and erasure coding data redundancy strategies to meet data storage and processing in various scenarios, and the logical architecture is as follows:

![1](/docs/assets/images/platform-technology-architecture-3.png)

SCloudStack distributed storage system is an indispensable core component of the entire cloud platform architecture, providing basic storage resources through the distributed storage cluster architecture, supporting online horizontal expansion, and integrating intelligent storage cluster, ultra-large-scale expansion, multi-replica and erasure coding redundancy strategy, data rebalancing, fault data reconstruction, data cleaning, thin provisioning and snapshots and other technologies to provide high performance, high reliability, high scalability, easy management and data security guarantee for virtualized storage. Improve the service quality of storage virtualization and cloud platforms in all aspects.

### Intelligent Storage Cluster
A distributed storage cluster can contain thousands of storage nodes and typically requires at least one monitor and multiple OSD daemons for proper operation and data replication. Distributed intelligent storage clusters eliminate centralized control gateways, enable clients to interact directly with the storage unit OSD daemon, and automatically create copies of data on each storage node to ensure data security and availability. The basic concepts included are as follows:

- OSD: Usually an OSD corresponds to a disk, RAID group or physical storage device of the physical machine, which is mainly responsible for data storage, processing data replication, recovery, backfilling and data rebalancing, and reporting detection information to the monitor. A single cluster requires at least two OSDs, and the physical architecture can be divided into multiple fault domains (computer rooms, racks, servers), and multiple replicas are configured to be in different fault domains.
- Monitor Monitor: Implements the status monitoring of the storage cluster, is responsible for maintaining the mapping diagram between the Object, PG and OSD of the storage cluster, provides strong consistency decision-making for data storage, and provides the mapping relationship of data storage for clients.
- Metadata Service MDS: When you implement a file storage service, the Metadata Service (MDS) manages file metadata.
- Client: Deployed on the server, data slicing is implemented, the object location is located through the CRUSH algorithm, and object data is read and written. This typically includes block devices, object storage, and file system clients, and read/write operations are handled by the OSD daemon.
- CRUSH algorithm: A pseudo-random algorithm used to ensure uniform data distribution, and both OSD and client use the CRUSH algorithm to calculate the location information of objects on demand, providing support for dynamic scaling, rebalancing, and self-healing functions of storage clusters.

When storing data, the storage cluster receives data from clients (block devices, object storage, file systems) and shards the data into objects in the storage pool, and each object is stored directly on the OSD's bare storage device, and the OSD process handles read and write operations on the bare device. As shown in the following figure:

![1](/docs/assets/images/platform-technology-architecture-4.png)

The client program obtains the mapping relationship data by interacting with the OSD or monitor, calculates the object storage location locally through the CRUSH algorithm, and directly communicates with the corresponding OSD to complete data reading and writing operations. In order to realize autonomous, intelligent and self-healing access to data in distributed storage clusters, intelligent storage clusters are associated with each other to carry data storage processes through various logical concepts such as CURSH algorithm, storage pool pool, placement group PG and OSD, and the logical architecture diagram is as follows:

![1](/docs/assets/images/platform-technology-architecture-5.png)

- A cluster can be logically divided into multiple pools, Pool is a namespace, and clients need to specify a Pool when storing data;
- A Pool contains several logical PGs (Placement Group), which can define the number of PGs and the number of object replicas in the pool;
- PG is an intermediate logical layering of objects and OSDs, and when writing object data, the PG to be stored for each object is calculated according to the CRUSH algorithm;
- A physical file is divided into multiple objects, each object is mapped to a PG, and a PG contains multiple objects;
- A PG can be mapped to a set of OSDs, where the first OSD is the master and the other OSDs are slaves, and the Object is evenly distributed to a set of OSDs for storage;
- OSDs hosting the same PG monitor each other's liveliness, and multiple PGs can be mapped to an OSD at the same time.

In the storage cluster mechanism, master and slave OSDs hosting the same PG need to exchange information with each other to ensure each other's survival state. The first time the client accesses, it first obtains the mapped data from the monitor, and when the data is stored, it compares the version of the mapped data with the OSD. As can be seen from the diagram above, an OSD can host multiple PGs at the same time, and each PG is usually 3 OSDs under the three-copy mechanism. As shown in the preceding figure, the data addressing process is divided into three mapping phases:

- Map the files to be manipulated by the user to the objects that can be processed by the storage cluster, that is, the files are sharded according to the size of the objects;
- Map the Object of all file shards to PG through the CRUSH algorithm;
- Map the PG to the OSD where the data is actually stored, and finally the client directly contacts the primary OSD for object data storage operations.

The distributed storage client obtains the cluster map diagram from the monitor and writes the objects to the storage pool. The logic of storing data in a cluster depends mainly on the size of the storage pool, the number of replicas, the rules of the CRUSH algorithm, and the number of PGs.

### Hyperscale Scaling
In a traditional centralized architecture, the central cluster component acts as a single entry point for clients to access the cluster, which severely impacts the performance and scalability of the cluster while introducing a single point of failure. In the design of the storage cluster, the storage unit OSD and the storage client can directly sense other OSD and monitor information in the cluster, allowing the storage client to directly interact with the storage unit OSD for data reading and writing, and allowing each OSD to directly interact with the OSD on the monitor and other nodes for data reading and writing, this mechanism enables the OSD to make full use of the CPU/RAM of each node, distribute the centralized task to each node to complete, and support ultra-large-scale cluster expansion capabilities. Provides exabyte-class storage capacity.

- OSDs directly serve clients, and storage clients communicate directly with OSDs, eliminating central controllers and single points of failure, improving the performance and scalability of the overall cluster.
- OSDs monitor each other's health status and actively update the status to monitors, so that monitors can be lightly deployed and run.
- OSD uses the CRUSH algorithm to calculate the location of copies of data, including data rebalancing. In the multi-replica mechanism, after the client writes an object to the primary OSD, the primary OSD recognizes the replica OSD through its own CRUSH map map and copies the object to the replica OSD; With the ability to perform data replica replication, OSD processes offload storage clients while ensuring high data availability and data security.

In order to eliminate the central node, both the distributed storage client and the OSD use the CRUSH algorithm to calculate the location information of the object on demand, avoid the central dependence on the cluster map on the monitor, and allow most data management tasks to be distributed on the clients and OSDs in the cluster, improving the scalability of the platform.
  
At the storage cluster expansion level, horizontal expansion, incremental expansion, and automatic data balancing of storage nodes are supported, and the overall performance of the cluster increases positively with the growth of capacity, thereby ensuring the performance and high scalability of the storage system.

### High availability and high reliability
In order to build high-availability distributed storage services on all platforms and ensure the reliability of virtualized computing and application service data storage, the distributed storage system ensures the stable operation of storage services from various aspects.
- The infrastructure is highly available

Storage clusters do not forcibly bind hardware and brands, and can use general-purpose servers and network devices to support heterogeneous storage clusters. The physical network device supports the 10GE/25GE underlying storage stacking network architecture, and the server layer adopts dual links to ensure IO performance and availability of data reading and writing.
- Storage Monitor is highly available

The cluster monitor maintains the master map between objects, PGs, and OSDs in the storage cluster, including cluster members, status, changes, and the overall health of the storage cluster. OSD and client will obtain the latest cluster map through the monitor, in order to ensure the availability of platform services, support monitor high availability, when a monitor due to delay or error caused by the state is inconsistent, the storage system will algorithmically agree on the monitor status in the cluster.
- Storage access load balancing

Object Storage and File Storage Access Gateway supports load balancing services to ensure high availability of Object Storage and File Storage Gateway, and provides traffic load distribution for Storage Gateway to improve the overall performance of storage. Under the access mechanism of load balancing, read-write I/O is balanced to all gateway services in the cluster, and when an exception occurs on one of the gateway servers, the abnormal gateway node is automatically eliminated, shielding the underlying hardware failure, and improving service availability.

### Multiple redundancy strategy
- Multi-copy mechanism

The multi-copy mechanism refers to the data redundancy technology that saves multiple copies of the written data, and the storage system ensures the consistency of the multi-copy data. The SCloudStack distributed block storage system adopts a multi-copy data backup mechanism by default, which first writes data to the primary replica when writing data, and the primary replica is responsible for synchronizing data to other replicas, and stores each copy of data on different disks across nodes, cabinets, and data centers to ensure data security in multiple dimensions. Storage clients read data from the primary replica first, and only if the primary replica data fails, the other replicas provide read operations.

The SCloudStack distributed storage system maximizes data security and availability through measures such as multiple copies, write confirmation mechanisms, and replica distribution policies. When disk damage and software failure lead to copy data loss, the system automatically detects and automatically performs copy data backup and synchronization, which will not affect the storage and reading and writing of business data, and ensure data security and availability. This section uses three replicas as an example to describe the working mechanism of multiple replicas:

- (1) Three copies

Users who write data to distributed storage through the client write three copies according to the number of replicas set by the pool3, and store them on disks of different physical hosts according to the replica distribution policy. Distributed storage keeps data secure at least 2 copies so that the storage cluster can run in a degraded state to keep data secure.

![1](/docs/assets/images/platform-technology-architecture-6.png)

- (2) Write confirmation mechanism
As shown in the figure above, during the writing process of the three replicas, only when all three write processes are confirmed will the write be returned to complete to ensure strong consistency in data writing.

The client writes the object to the master OSD of the target PG, and then the master OSD locates the second and third OSDs used to store the object replica through the GRUSH mapping diagram, and restores the object data to the two slave OSDs corresponding to the PG, and when the data of the three object replicas is written, the final response to the client confirms that the object write is successful.

- (3) Replica distribution strategy
Distributed storage supports the replica data disk distribution strategy (multi-level fault domains), which uses the CRUSH algorithm to allocate data objects according to the weight values of storage devices to ensure the uniform distribution of object data. By defining bucket types, the platform supports node-level, cabinet-level, and datacenter-level fault domains, and distributes replica data among different hosts, cabinets, and data centers to avoid data loss or unusability caused by single-host, single-cabinet, and single-data center overall failures, and ensure data availability and security.

To ensure the access latency of stored data, it is recommended to save up to three copies of data to different cabinets, and if three copies of data are saved to different data rooms, the IO performance of EVS disks may be affected due to network latency.

![1](/docs/assets/images/platform-technology-architecture-7.jpg)

As shown in the figure above, the client writes ABC three object data through the distributed storage system, and according to the fault domain defined by the CRUSH rule, the copies of the three objects need to be stored in different cabinets. Taking object A as an example, the storage system sets a replica distribution policy in advance to ensure that the object replicas are distributed in the server OSDs of different cabinets, that is, define the enclosure and host buckets. When the distributed storage system calculates the PG of the written object and the corresponding OSD location, it will first write A to the server OSD of enclosure 1, and at the same time replicate A' to the server OSD of enclosure 2 through the primary OSD replica A' to the server OSD of enclosure 2, and replicate A'' to the server OSD of enclosure 3.

![1](/docs/assets/images/platform-technology-architecture-8.jpg)

Object replica data is always maintained as 3 copies when there are no anomalies such as network outages or disk failures on the storage node. Only when the node is abnormal and the number of replicas is less than 3, the storage system will automatically rebuild the data copies to ensure that the data copies are permanently three copies and escort the data security of virtualized storage. As shown in the figure above, the third node fails, resulting in data D1-D5 being lost and failing, the storage system will automatically map the PG of object data to a new OSD, and automatically synchronize and rebuild D1'-D5' through the other two copies to ensure that the data is always three copies to ensure data security.