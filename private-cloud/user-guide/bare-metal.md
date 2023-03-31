---
layout: default
title: Bare Metal Service
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/bare-metal/
nav_order: 12
---
# Bare Metal Services
## Product Introduction
### Overview
Bare Metal Server (Bare Metal Server) is a service that the platform provides dedicated physical servers for user applications, which can ensure the high performance and stability of core databases, key application systems, high-performance computing services, and services that cannot be cloudified temporarily.

Combined with the elastic management of resources on the cloud platform, it can realize on-demand allocation of physical machines, flexible application, and automatic installation of operating systems. At the same time, combined with IPMI and PXE, it can support batch deployment of bare metal devices, and realize remote batch startup through automated system configuration. and deploy. Supports custom installation of the operating system of bare metal devices and self-service network configuration, and provides physical server lifecycle management, including server startup, shutdown, and release.

The platform provides PXE remote self-service installation service through the deployment server, guides multiple physical servers in the same network to be started by PXE network, downloads and installs the operating system software package through the deployment server to install the operating system for the physical machine, and configures the network template information according to the operating system , configure the server network information, connect the server with the external network of the platform, and combine the unified management service of the platform, so that users can apply for and manage the physical server by themselves through the platform. Examples of specific values are as follows:
- Through the bare metal service, platform tenants can deploy virtual machines and physical machines in a mixed manner to build unified management and flexible networking of heterogeneous infrastructure. The network of physical machines can communicate with the external network of platform virtual machines to meet the needs of virtual machine services Requirements for scenarios such as mutual access with physical machines.
- Tenants can self-apply for the physical machines provided by the platform manager according to their needs, and exclusively enjoy the computing, network and local disk resources of the physical servers, which can fully meet the requirements for high performance, stability, and data security; Shared network can meet the low-latency and high-throughput business scenarios for the network.

By applying for the bare metal physical machine of the platform, tenants can apply it to application scenarios such as core databases, big data services, and key applications. For example, database cluster OracleRAC can be deployed on bare metal services, and business applications can be deployed on platform virtual machines. The external network IP of the machine and the physical machine are directly interconnected through the physical device, which not only improves business performance and stability, but also improves the convenience of resource management.

### Usage process
Before using the bare metal service, the bare metal equipment must be prepared in advance, and the IPMI network and business network of the bare metal server should be connected with the platform network according to the requirements. After entering the equipment information through the platform, the equipment will be allocated to the tenants, and the tenants will help themselves After application, the operating system is automatically installed and the server network is configured. The use process of the bare metal service is divided into two parts: [platform administrator process] and [tenant process], and the first 6 steps are operated by the platform administrator, as follows:

(1) Hardware environment equipment

Prepare the hardware environment, configure the physical network switch and server IPMI network, so that the platform physical network and IPMI network can communicate with each other.

(2) Manage physical machine resource pools

The [Platform Administrator] creates and manages the physical machine resource pool, which is used to define the network segment and Vlan information used by a batch of physical machine server operating systems, and is also used to define the network card configuration template.

(3) Manage NIC configuration templates

The network card configuration template is created and managed by the [platform administrator], which is used to define specific network card configuration information for a batch of physical server operating systems, such as dual network card aggregation and Vlan and CIDR network segments used by the network card. The template automatically configures the network card and configures information such as IP.

(4) Add physical machines to the resource pool

[Platform Administrator] adds physical machine information to the physical machine resource pool, including the server's SN serial number, IPMI user name, password, and network card template of the server operating system, and supports batch import of physical machine information.

(5) PXE boot initialization

After the physical machine information is entered, if the IPMI network and login information can be accessed normally, the platform will automatically boot the entered physical machine through IPMI and PXE to restart and deploy the platform to prepare the system and initialize the server. During the initialization process, the status of the physical machine is [Preparing]. After the server is initialized successfully, the status of the server is set to [Normal]. At this time, you can check the detailed hardware configuration of the physical server in the details of the physical machine, such as CPU, memory, Disk information, network card information, etc.

(6) Physical Server Allocation

[Platform Administrator] allocates the initialized bare metal devices to tenants. One physical machine can only be allocated to one tenant at a time. Tenants with permission can go to the console to apply for a physical machine and install the operating system by themselves.

(7) Tenants apply for physical machines

[Platform tenants] can self-apply for authorized physical machines according to their needs through the physical machine function of the console, specify the specific physical machine that needs to be applied for, and specify the system disk, operating system, IP address and administrator login password of the physical machine .

(8) Automatic installation
According to the system disk and operating system specified by the user, the platform automatically installs the operating system remotely through PXE, and after the system is installed successfully, it automatically configures the network and password according to the configuration information specified at the time of application, so that the user can manage the installed physical machine.

(9) Bare metal physical machine management

[Platform tenants] manage the entire life cycle of the applied physical machines, support reinstalling the system, shutting down, and starting up, and can re-release the physical machines to the platform when the physical machines are not in use, and the platform will redistribute them to other tenants.

The prerequisite for platform tenants to use bare metal services is that the physical machine is ready and assigned to the tenant. The tenant only needs to apply for a simple application to conveniently use the bare metal equipment provided by the platform, and can flexibly deploy the business system to the bare metal to form a hybrid Heterogeneous infrastructure environment.

## Add a physical machine
When the platform has provided bare metal services and assigned physical machines to tenants, the tenant's primary account and sub-accounts can directly apply for authorized physical machines on the platform for installation and use. Users can log in to the console, and enter the wizard page through the operation of applying for a physical machine in the console navigation bar [Physical Machine], as shown in the following figure:

![1](/assets/images/user-guide/user-guide-135.png)
 
- Resource Pool: Select the physical machine resource pool to which the physical machine to apply for belongs, that is, the network segment and Vlan representing the network of the operating system of the physical machine.
- Physical machine: Select the physical machine that needs to be applied for. You can locate the desired physical machine through the physical machine name and serial number. You can also view the physical machine's CPU, memory, disk quantity, and disk capacity configuration through the physical machine configuration information.
- System Disk: Select the system disk where the operating system of the physical machine is installed, such as sda(hdd).
- Operating system: Select the operating system that needs to be installed, such as Centos 7.4. Different types of physical machines need to adapt to the operating system slightly differently.
- Network: Enter the IP address to be configured for the physical machine. The IP address must be in the resource pool network to which the physical machine belongs, and cannot conflict with other IP addresses in the physical network. Check the availability of the IP address before using it.
- Password: Enter the login password for the operating system of the physical machine.

The application can only be made when the platform administrator allocates physical machines for tenants. After the application is submitted, the list will generate a physical machine information of [Applying]. After the application is successful, it will be set to the status of [Completed]. Use Install OS to trigger the platform to automatically install the OS.

## View physical machine
The tenant's primary account and sub-accounts with permissions can view the list of physical machines applied for under the account and related detailed information through the physical machine list and details, and can view the detailed configuration and current status of the physical machines, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-136.png)
 
- Name/Resource ID: The name and globally resource unique identifier of the physical machine.
- SN: The serial number of the physical machine.
- IP: IP address of the physical machine.
- Configuration: hardware configuration information of the physical machine, including CPU, memory and disk capacity.
- Application time: the application time of the physical machine.
- Status: The resource status of the current physical machine, such as applying for, installing, running, booting, shutting down, shutting down, etc.
- Power State: The current power state of the physical machine.

In the operation items on the list, you can start, shut down, install and delete a single physical machine, and support batch deletion of physical machines. Deleting means releasing the physical machine to the cloud platform, which can be re-applied or assigned to other tenants.

Tenants can also enter the details page of the physical machine through the name of the physical machine on the list to view the detailed information of the physical machine, including basic information and configuration information:
- Basic information includes: physical machine name, ID, IP address, SN serial number, application time, status and power status.
- Configuration information includes: the CPU architecture of the physical machine, the number of CPU cores, memory specifications, system disk specifications, data disk specifications, and operating system version.

## Physical Machine Startup/Shutdown
The platform supports tenants to shut down and start the applied physical machines to maintain and manage the life cycle of the physical machines. The shutdown operation can only be performed when the power status is turned on; the startup operation can only be performed when the power status is turned off.

During the startup process of the physical machine, the status of the physical machine is booting, and it will be turned into running after the boot is successful; when the user triggers shutdown, the status of the physical machine is shutting down, and the status of the physical machine will be shut down after the shutdown is successful.

## Physical machine installation
Tenants can reinstall the physical machine, and the installation operation is only supported when the physical machine is [Running]. After the user triggers the installation operation, the user's target operating system is required, and the operating system of the physical machine will be reinstalled through PXE according to the new target operating system.

The network and password used when reinstalling the physical machine are the same as when applying for the physical machine. You can log in to the physical machine to change the login password after the reinstallation is complete.

## Delete physical machine
The platform supports tenants to delete the physical machines that have been applied for, that is, the tenants release the physical machines to the cloud platform, and can re-apply or be reassigned to other tenants by the platform administrator. After the physical machines are deleted, they will be released directly and will not enter recycling stand.

Deleting a physical machine does not actually delete the physical machine in the physical resource pool from the platform, but only releases the physical machine from the tenant console without affecting the running operating system and business services in the physical machine.