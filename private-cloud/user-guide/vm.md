---
layout: default
title: Virtual Machine
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/virtual-machine/
nav_order: 1
---
# Virtual Machine
##  Overview
Virtual machine is the core service of SCloudStack cloud platform, providing scalable computing power services at any time, including the most basic computing components such as CPU, memory, operating system, etc., and combining with network, disk, security and other services to provide a complete computing environment. Build IT architecture by combining with services such as load balancing, databases, caching, object storage, and more.

- The SCloudStack cloud platform virtualizes the computing resources of physical servers through `KVM` (Kernel-based Virtual Machine) to provide computing resources for virtual machines.
- The computing resources of a virtual machine can only be located on one physical server, and when the physical server load is high or fails, it will be automatically migrated to other healthy physical servers.
- Virtual machine computing power is expressed by virtual CPU (vCPU) and virtual memory, and storage capacity is reflected in cloud storage capacity and performance;
- By controlling the QoS of vCPUs, memory, and disks, the hypervisor supports virtual machine resource isolation to ensure that multiple virtual machines do not affect each other on the same physical server.

Virtual machines are the basic environment for cloud platform users to deploy and run application services, which is used in the same way as physical computers, providing complete life cycle functions such as creation, shutdown, power off, power on, password reset, system reinstallation, and upgrading. Support Linux, Windows and other different operating systems, and can be accessed and managed through VNC, SSH, etc., and have full control of virtual machines. The resources involved in virtual machine operation and their association relationships are as follows:

 ![1](/assets/images/product-functional-architecture-3.jpg)

 As shown in the figure, the instance type, image, and VPC network are the basic resources that must be specified to run the virtual machine, that is, the CPU memory, operating system, virtual NIC, and IP information of the virtual machine. On top of the virtual machine, you can bind EVS disks, EIPs, and security groups to provide data disks, public IP addresses, and network firewalls for virtual machines to ensure data storage and network security of virtual machine applications.

In terms of virtualization computing capabilities, the platform provides GPU device transparent transmission capabilities, allowing users to create and run GPU virtual machines on the platform, allowing virtual machines to have high-performance computing and graphics processing capabilities. Devices that support transparent transmission include `NVIDIA's K80, P40, V100, 2080, 2080Ti, T4, and Huawei Atlas300`.

## Create a virtual machine
You can specify the model, specification, image, EVS disk, VPC network, public IP, security group, and basic information about virtual machines to create multiple virtual machines with one click to deploy your own applications and services.

### Prerequisites
- Before creating a virtual machine, you already have an account that can log in to the cloud platform and have recharged the account;
- Before you create a virtual machine, you need to select the data center of the virtual machine to be created and run by using Region in the upper-left corner of the console.
- Confirm that the quota of the region and account specified by the user is sufficient; If the quota is insufficient, apply for a resource quota from the cloud platform management.
- If you do not use the default security group provided by the system, you must create a security group in the target region and add security rules that meet your business requirements.

### Create Operation
After selecting the region (data center) where the virtual resource needs to run, select a virtual machine in the left-side navigation pane, enter the VM console, and click Create VM to pop up the VM creation wizard.

 ![1](/assets/images/user-guide-4.png)

Select the model of the virtual machine and determine the operating system image that the virtual machine is running;
- Model is the cluster type of the host running the virtual machine, representing different architectures, different models of CPU or hardware characteristics, can be customized by the administrator, such as x86 models, GPU models, ARM models, etc., the instance created by the ARM model is an ARM version of the virtual machine instance, has been adapted to domestic chips, servers and operating systems, and can run localized operating systems, such as UOS or Galaxy Alliance.
- The image is the template of the running environment of the virtual machine instance, and you can choose the base image or the self-made image.
  - The base image is provided by the official platform by default, including multiple versions of native operating systems such as Centos, Ubuntu and Windows, and the default time zone of the base image is Shanghai.
  - Custom images are self-owned images that users export or custom-import through virtual machines, and can be used to create virtual machines, and only the account itself has permission to view and manage them.

Select the specification configuration of the virtual machine, that is, define the CPU memory and GPU configuration that provide computing power, and the specifications can be customized by the platform administrator;
- CPU models provide 1-core 2G, 2-core 4G, 4-core 8G, 8-core 16G, 16-core 32G, and 64-core 128G by default.
- The platform provides GPU device transmission capability, and if the model is a GPU model, a virtual machine with GPU capability can be created and run;
- For GPU models, the platform supports up to 4 GPU chips, in order to make GPU virtual machines perform optimally, the platform limits the minimum CPU memory specification to more than 4 times the number of GPUs, such as 1 GPU chip requires a minimum of 4 cores 8G specifications, 2 GPU chips require a minimum of 8 cores 16G specifications, and 4 GPU chips require a minimum of 16 cores 32G specifications.

Select and configure the system disk and data disk of the virtual machine, and configure the capacity of the system disk and EVS disk, respectively.
- System disk: The system disk on which the virtual machine image is running, you must select the system disk type and system disk capacity when creating the virtual machine.
  - Select the disk type of the system disk, such as SSD disk or HDD disk, the disk type can be customized by the administrator;
  - Configure the system disk capacity, the default system disk for Linux and Windows images is 40GB, and the capacity of the system disk can be expanded to 500GB, and the step size is 1GB, that is, the capacity should be a multiple of 1GB.

Expanding the system disk is to increase the capacity of the block device, and does not expand the file system in the system, that is, after the system disk is expanded, it needs to enter the virtual machine to perform the file system expansion (resize) operation, the specific operation steps are described in: System disk expansion.
- Data disk: An elastic block device that provides persistent storage space for virtual machines based on a distributed storage system, and can create a cloud disk at the same time, automatically bind it to the virtual machine, and automatically format and mount the hard disk.
  - The default data disk mount path is /data, and you can also choose to add a data cloud disk after the virtual machine is created.
  - Select and configure the data disk type and capacity, and the specifications of the capacity range can be customized by the administrator.
  - The default specifications of the platform support a minimum capacity of 10GB and a maximum of 8000GB, with a step size of 1GB, that is, the capacity should be a multiple of 1GB.

Configure network-related settings, including VPC networks, subnets, private IP addresses, private security groups, public IP addresses, and public security groups to which the virtual machine needs to join:

 ![1](/assets/images/user-guide-5.png)

- A VPC network is a logically isolated, Layer 2 network broadcast domain environment that belongs to users. Within a VPC network, users can build and manage multiple three-layer networks, namely subnets, VPC VPCs are containers of subnets, and the networks between different VPCs are absolutely isolated.
  - When you create a virtual machine, you must select the VPC network and subnet, that is, select the network and IP block to be joined by the virtual machine.
  - The console calculates the number of available IPs for the selected subnet for the user, and you need to specify a subnet with a sufficient number of available IPs when creating it;
  - By default, the platform automatically assigns an IP address to the virtual machine from the CIDR segment of the subnet to which it belongs, and you can manually specify the IP address of the virtual machine through the Private IP option.

- A security group is a virtual firewall provided by the platform, which provides access control rules for inbound and outbound traffic, and defines which networks or protocols can access resources.
  - The external security group is used to control the traffic of the virtual machine in the north-south direction (external IP address), and the internal security group is used for the security access control of the virtual machine in the east-west direction (between NICs).
  - By default, the public network security group and the internal network security group are not bound until the virtual machine is created.
  - The system provides a default security group, but if you cannot meet the requirements, you can create a security group and bind it to a virtual machine.
- The external IP address is an elastic external egress service provided by a virtual machine, which supports you to apply for and bind an external IP address to a virtual machine when you create a virtual machine. The platform supports IPv4/IPv6 dual-stack networking, which can bind multiple public IP addresses to a virtual machine after it is created, bind up to 50 IPv4 and 10 IPv6 external IP addresses, and manually set the default egress of the virtual machine.

The platform supports viewing the bound external IP address and network route in the virtual machine, and the traffic of the virtual machine accessing the external network is directly transmitted to the physical network card through the virtual network card to communicate with the external network, improving the performance of network transmission.

Select and configure the basic management configuration of the virtual machine, including the virtual machine name, logon method, and logon password.

 ![1](/assets/images/user-guide-6.png)

- Virtual machine name: The default configuration name of the platform is `host`, and the user can customize the virtual machine name, which can be searched and filtered by name;
- Login mode: Set the login credentials for the virtual machine, that is, the password to log in to the virtual machine;
  - The administrator of `CentOS` is `root`, the administrator of `Ubuntu` is `ubuntu`, and the administrator name of `Windows` system is `administrator`;

Select the purchase quantity and payment method, confirm the order as shown in the following figure, and click "Buy Now" to create the virtual machine.

 ![1](/assets/images/user-guide-7.png)

- Purchase quantity: Create multiple virtual machines in batches according to the selected configuration and parameters, up to 10 virtual machines in batches, and manually set the IP address of virtual machines during batch creation.
- Payment method: Select the billing method of the virtual machine, support three modes: hourly, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: Users select resources such as virtual machine CPU, memory, data disk, and external IP to display the fees according to the payment method.
- Buy Now: After clicking Buy Now, you will be returned to the virtual machine resource list page, where you can view the creation process of the host, usually display the status of "Starting" first, and the creation can be completed within 1~2 minutes.