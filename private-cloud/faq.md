---
layout: default
title: Frequently Asked Questions
has_children: false
parent: Private Cloud
permalink: /private-cloud/faq
nav_order: 8
---
# Frequently asked questions

### Does SCloudStack support Oracle and RAC?

First, Oracle does not currently acknowledge that Oracle databases run in virtualized environments other than VMWare, i.e., Oracle databases installed in other virtualized environments may not provide official Oracle technical support, and customers need to assess the risks themselves.

### How many hardware devices are needed for the smallest size of SCloudStack?

A SCloudStack production environment requires a minimum of 3 compute node servers, 1 master server and 1 switch.

3 compute node servers to guarantee distributed storage 3 copies; 1 management node is a single point, and it is recommended to configure 2 management nodes for the production environment; 1 switch is a single point, 2 switches are recommended for production environments.

Management nodes can be deployed in compute node servers or virtual machines, and if physical server resources are tight, you can omit the management node server.

### How many hardware devices are needed for the SCloudStack test environment?
A test environment can deploy a complete SCloudStack platform with 1 server; Network devices require only one switch or one switch interface. If you need to test the overall function of the platform and related indicators, it is recommended to deploy at least 2~3 server nodes.

### Does SCloudStack restrict hardware device brands?
SCloudStack does not limit the make and model of servers and switches, supporting virtualized `x86` servers and general-purpose Layer 2 & Layer 3 switches.

### If you do not need external network services, how should I build a SCloudStack network?
If the cloud platform does not need to provide external network services, you can omit the switch in the external network area, and the server only needs two network ports to access the internal network access switch.

### How many network ports does a SCloudStack server node need at least?

The recommended standard solution is a high-availability architecture of all platforms, and two network ports are required for dual NIC binding (bond) for the internal and external networks, that is, four network ports are required.

If you do not need external network services, you can omit 2 network ports, and only need 2 network ports to access the internal network access switch. If you do not need NIC redundancy, you can eliminate two network ports, and only need two network ports to connect to the internal network and the external network.

The server network supports VLAN configuration, that is, only 2 network ports are required to do dual NICs Bond4 is connected to the switch interface (configured as trunk), and supports internal and external network services.

### Can the server nodes be fully configured as gigabit network interfaces?

Yes, the standard solution recommends 10 Gigabit interfaces and 10 Gigabit switches for the private network based on performance considerations, and if the services running on the virtual machine do not have high performance requirements, you can use all-gigabit link networking.

### How many server nodes does SCloudStack support at the maximum scale?

The standard network solution has a set of core switches for internal and external networks, supporting up to 900 nodes. If you add core access switches, you can scale more server nodes.

### How many management nodes are required in the actual environment?

Only 1~2 management nodes are required, or 1 is not considered HA high availability. 

The SCloudStack platform has the technical feature of separating services, that is, the management node only carries northbound interfaces such as account authentication, metering and billing, resource management, intelligent scheduling, API-Gateway, and service monitoring.

Southbound interfaces that carry VMs, EVS disks, VPC networks, EIPs, and load balancers are deployed on compute nodes, and communication and data forwarding between VMs do not pass through the management node, greatly reducing the load on the management node, so a single management node can manage hundreds or thousands of compute nodes.

Management nodes can be deployed in compute node servers or virtual machines, and if physical server resources are tight, you can omit the management node server.

### Is the basic monitoring node a required node?

No, the basic monitoring node is SCloudStack peripheral monitoring, which is used to monitor the monitoring and alarming of all network devices, servers, disk arrays and other hardware devices connected to the cloud platform, as well as common services such as MySQL, Redis, and MongoDB in the cluster.

At the same time, the base monitoring node can also monitor physical servers and hardware devices outside the SCloudStack platform. If you already have a basic monitoring platform, you do not need to build additional basic monitoring nodes.

### What is the difference between computational storage hyper-converged nodes and independent storage nodes?

The computing and storage hyper-converged node integrates both computing and storage capabilities, that is, the distributed storage of the cloud platform directly uses the disks of each computing node to host virtual machines and cloud disk services. Isolated storage nodes provide only distributed storage capabilities and deploy only distributed storage systems.

Computing and storage hyper-converged nodes need to host computing services with high CPU and memory configurations. Isolated storage nodes only provide distributed storage services, with low CPU and memory configurations, but generally larger disk capacities.

### What is the function of managed gateway nodes, and how many are generally needed?

The managed gateway node provides a hybrid cloud channel between SCloudStack and the physical IDC environment to open up the virtual network (overlay) and the physical network (Underlay), and through the managed gateway node, the physical regional database server can directly communicate with the cloud platform virtual machine, which is suitable for the old physical server hosting business scenario.

The managed gateway node is an optional node, which is generally deployed by two servers through HA to ensure the high availability of the managed gateway. Alternatively, deploy managed gateway nodes directly in compute nodes without occupying stand-alone server nodes.

### In the actual environment, can the managed gateway access switch be omitted?

The standard scenario recommends that you equip managed gateway nodes with two managed gateway access switches to carry managed gateway server business data transfers.

In the actual environment, the managed gateway access switch can be omitted according to the requirements, and the managed gateway server can be directly connected to the internal network access switch or the internal core switch.

If the managed gateway service is deployed on a compute node, both the managed gateway access switch and the managed gateway server can be eliminated and the resources of the compute node can be used directly to provide hybrid cloud gateway services.

### Can 4 core switches be omitted from the general network architecture?
Yes, if the service scale is small (within 48 nodes), you can eliminate 4 core switches and use 2 groups of 4 access switches to carry SCloudStack internal and external networks respectively.

If there is no requirement for strong physical isolation between the internal and external networks, only two VSwitches are required to carry the internal and external network services of the cloud platform.

### Do compute/storage nodes have to be configured with RAID5 before deploying distributed storage?

No, `RAID5` is only an operational scenario, and you can determine whether to configure `RAID5` according to your needs in the actual project. SCloudStack underlying storage adopts the `Ceph` scheme, when a disk is damaged, `Ceph` synchronizes the data copy of the damaged disk, which will bring performance fluctuations to the overall performance of `Ceph`.

In order to avoid performance fluctuations caused by damaging a disk, first group the disks into RAID5 for distributed storage, and a RAID5 is an OSD for `Ceph`; When a disk is damaged, due to RAID5, Ceph does not sense the disk damage, does not need to synchronize the data copy, and guarantees performance to a certain extent.

### What is the difference between a managed gateway and a NAT gateway?
A managed gateway is used to connect a virtual network and a physical network so that the physical network server communicates with virtual machines in the VPC network through the internal network.

NAT gateway is used in scenarios where multiple virtual machines share an EIP, saving public IP addresses and providing DNAT and other functions.

### Do VMs and EVS disks have backup functions?
Virtual machines can be backed up by exporting as images. EVS disks do not have a backup function, and EVS disk snapshots will be available in Q2 2019.

### How should the ratio of SATA and SSD/NVME nodes be planned?
SCloudStack only supports the deployment of one type of disk per compute/storage node, and the ratio of SATA (common disk) nodes and SSD/NVME (high-performance disk) nodes depends on the service characteristics, such as services with more database services, more SSD nodes need to be configured.

### Does SCloudStack support resource overscoring (overselling/overselling)?

SCloudStack supports CPU resource over-partitioning, but does not support memory and disk over-splitting. Generally, the CPU overscore ratio needs to be set according to the service characteristics of the platform, and it is usually recommended that it should not exceed 2 times.

### Can the recommended server configuration of SCloudStack be changed?

No, since the recommended servers are all centralized procurement by the company, arbitrarily changing the configuration (such as adding a disk, a network card, etc.) may affect the price, delivery time and maintenance issues. If you have to change the configuration, you can contact the relevant personnel to apply in advance.

### What is the difference between SCloudStack POC online test and on-premises server test?
The online POC can test the functionality and usability testing of end users such as compute, storage, network, monitoring, logs, and sub-accounts of the tenant interface.

The on-premises server test can test the overall operation and maintenance operation and permission management of the platform, including the resource management of physical machines, physical networks, physical storage, account management, platform configuration, billing and pricing financial management, overall resource utilization of the platform, overall monitoring, logs, etc.

At the same time, on-premises server testing can also test the stability, reliability, and security of the platform cluster, such as downtime testing, online migration, failover, and data recovery.