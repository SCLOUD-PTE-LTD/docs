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

## View virtual machines
Enter the VM console through the navigation bar to view the list of VM resources, and use the VM name on the list to enter VM Details and view the details of VMs and related resources.
### List of virtual machines
On the VM List page, you can view the list of virtual machine resources that exist under the current account, including name/comment, resource ID, VPC, subnet, model, configuration, IP, billing method, creation time, expiration time, status, and operation.

 ![1](/assets/images/user-guide-8.png)

- Host Name: The name and remarks of the virtual machine, which can be modified through the Edit button on the list page;
- Resource ID: The globally unique ID of the virtual machine, which can be copied through the Copy button;
- VPC/Subnet: The VPC network and subnet specified when the virtual machine is created, that is, the VPC network and subnet information where the virtual machine's IP is located;
- IP address: The IP address of the virtual machine, including the private IP and external IP (if any), and the IP address can be copied through the Copy button;
- Model: The cluster type of the physical machine on which the virtual machine runs, representing different architectures and models of CPU or hardware characteristics;
- Configuration: Basic configuration information of the virtual machine, including CPU memory specifications, number of GPUs, system disk image, total system disk capacity, and total data disk capacity.
- Billing method: The payment method specified when the virtual machine is created, including hourly, monthly, and annual;
- Creation time/expiration time: The creation time of the virtual machine and the expiration time within the billing cycle;
- Status: The current running status of the virtual machine, including starting, running, shutting down, shutting down, starting, modifying configuration, reinstalling, deleting, migration, and downgrade migration.
- Operations: Perform more operations on a single virtual machine, including details, login, startup, shutdown, deletion, power failure, restart, renewal, modify alarm template, create an image, reset password, reinstall the system, hot upgrade, bind a public IP address, modify a public security group, modify an internal network security group, modify a configuration, and obtain VNC information.

The default list can display 10 pieces of virtual machine information per page, support pagination and set the number of virtual machines that can be displayed on each page, and display up to 100 virtual machine data per page.

In order to facilitate tenants to maintain and operate virtual machines, the platform supports downloading the list of all virtual machine resources owned by the current user as an Excel table; At the same time, batch operations on virtual machines are supported, including batch startup, shutdown, power off, and deletion operations, and you can select multiple virtual machines and click the batch operation button to perform batch operations, as shown in the following figure:

### Virtual machine details
On the virtual machine list, click the name or ID of the virtual machine to enter the overview page of the current virtual machine to view the virtual machine details and monitoring information, and switch to the hard disk, network, and operation log page to view the relevant disk, network, IP address, ENI and operation log information of the virtual machine, as shown on the overview page of the following figure:

 ![1](/assets/images/user-guide-9.png)

#### Virtual Machine Overview
The Overview page displays basic information, configuration information, and monitoring charts, and can perform operations and manage virtual machines on the Overview page.

 ![1](/assets/images/user-guide-10.png)

- Basic information includes resource ID, name, private IP address, VPC, subnet, status, creation time, and alarm template.
  - You can click the button to the right of the name to modify the name and remarks of the virtual machine;
  - You can click the button on the right side of the alarm template to modify the alarm template associated with the virtual machine, and the virtual machine will bind the default alarm template by default.
- The configuration information includes the model, image, CPU, memory, total capacity of the system disk, and total capacity of the system disk, where the data disk capacity is the sum of the capacities of all EVS disks associated with the current virtual machine.
- Monitoring charts: Includes CPU usage, memory usage, space usage, bandwidth of NICs, packets of NICs, read/write throughput of disks, read/write throughput, average load, number of TCP connections, and number of blocking processes.

Users can enable Auto Refresh in the upper-right corner of the monitoring chart to automatically refresh the page every 3x0 seconds to obtain the latest monitoring chart data.

#### Virtual Machine Hard Disk
The Disks page displays the system disks and mounted data disks of the current virtual machine, including the name, ID, cluster architecture, cluster, disk type, disk capacity, mount point, billing method, status, creation time, expiration time, and snapshot operation information of each disk. As shown in the following figure:

 ![1](/assets/images/user-guide-11.png)

- Cluster architecture refers to the cluster architecture of the physical machine to which the virtual machine system disk or data disk belongs, including `HDD, SSD, NVME`, etc., which represents the disk media type of the cluster.
- A cluster refers to the cluster where the physical machine to which the system disk or data disk of a virtual machine belongs is located, which can be customized by the administrator, such as capacity or performance type.
- The disk types include system disks and data disks, and the additional EVS disks bound to them are data disks except for system disks.
- Hard disk capacity is the current capacity size of each hard disk;
The mount point is the actual drive letter of the hard disk in the virtual machine, such as vdb;
- Only data disks support billing methods and expiration time information, and the life cycle of system disks and virtual machines is the same.

In the operation items in the virtual machine hard disk management list, you can renew and snapshot the system disk and data disk of the virtual machine, and only the data disk is supported for renewal, and the system disk is consistent with the life cycle of the virtual machine.

##### Hard disk snapshots
In order to ensure that the data of all applications is captured in the snapshot, it is recommended to shut down the virtual machine or unmount the data disk before taking a snapshot. The specific operation is shown in the following figure:

 ![1](/assets/images/user-guide-12.png)

##### Hard disk expansion
Support the system disk and data disk expansion operation separately, the expansion of the EVS disk is to increase the capacity of the block device, and does not expand the file system in the system, that is, the system disk and data disk need to enter the virtual machine to perform the file system expansion (resize) operation after expansion, the specific operation steps of the system disk are detailed in: System disk expansion.

 ![1](/assets/images/user-guide-13.png)

Expansion will be charged according to the new capacity, and hard disks paid by the hour, and the next payment cycle for upgrading capacity will be charged according to the new configuration; For hard drives that are paid annually and monthly, the upgrade capacity takes effect immediately, and the price difference is automatically made up on a pro rata basis.
#### Data disk renewal
You can directly renew data disks attached to a virtual machine, and you will be charged for the renewal period when you renew, as shown in the following figure:

 ![1](/assets/images/user-guide-14.png)

If the billing method of the data disk is Hourly, you can select 1 to 24 hours. If the billing method of the data disk is Monthly, the renewal period can be from January to November. If the billing method of the data disk is Annual, the renewal period of the data disk is 1 to 5 years.

#### Virtual Machine Networking

The Network page displays the basic network information of the current virtual machine, and can manage the external IP address and ENI resources of the virtual machine. The basic network information includes the VPC, subnet, private IP address, public security group, and private network security group of the current virtual machine, and you can update the public network security group and private network security group of the virtual machine by clicking the buttons on the right side of the security group. As shown in the following figure:

 ![1](/assets/images/user-guide-15.png)

For more information about the public IP addresses and ENI resources of virtual machines, see Internet IP Management and ENI Management.

#### Internet IP Management
The platform supports IPv4/IPv6 dual-stack networks, and each virtual machine can bind up to 50 IPv4 and 10 IPv6 external IP addresses, and the first IP address with default route (including the IP address of the external ENI) is used as the default network egress of the virtual machine. At the same time, the bound external IP address and network route are viewed in the virtual machine, and the traffic of the virtual machine accessing the external network is directly transmitted to the physical network card through the virtual network card to communicate with the external network, improving the performance of network transmission.

The public IP address information includes the virtual machine and the bound external ENI IP, and only the public IP address with default route can be set as the default network egress of the virtual machine. You can view the details of the bound public IP addresses and related management operations through the list information, including the IP address, IP version, egress, bandwidth, route type, binding time, and status of the bound Internet IP address as shown in the figure.

- IP refers to the IP address and CIDR block name of the currently bound external IP address (the CIDR block is a pool of external IP addresses customized by the platform administrator);
- IP version refers to the IP version that is currently bound to the public IP address, including IPv4 and IPv6;
- Egress refers to whether the current IP is the default egress of the virtual machine, and a virtual machine supports up to two default egresses (one each for IPv4 and IPv6).
- Bandwidth refers to the bandwidth limit of the current IP address, which is specified when applying for an external IP address.
- The route type refers to the type of route delivered by the network segment to which the current IP address belongs (the network segment routing policy is customized by the platform administrator), including default routes and non-default routes, and only supports setting the external IP address with default routes as the default network egress of virtual machines.
  - The default route type means that when a VM binds the IP address, a route with a destination address of 0.0.0.0/0 is automatically delivered to the VM.
  - Non-default route means that when a VM binds the IP address, the specified destination address route configured by the administrator for the CIDR block is issued, such as a route with a destination address of 10.0.0.0/24 for the VM.
  - If multiple public IP addresses bound to a virtual machine are the default route type, the first IP address with a default route is used as the default egress of the virtual machine.

Users can use the operation items in the Internet IP Management console to bind, unbind, and set the default egress operation of the Internet IP address, and support batch unbinding.

The IP address of the external ENI bound to the virtual machine is also displayed to the public IP list, which can be set as an egress operation but cannot be unbound.

##### Bind an external IP address
You can bind up to 50 IPv4 and 10 IPv6 public IP addresses, and the first IP address with a default route is used as the default network egress of the virtual machine.

 ![1](/assets/images/user-guide-16.png)

After the binding is successful, you can view the bound IP address configured on the second NIC of the virtual machine, this section uses the Centos operating system as an example, as shown in the following figure:

 ![1](/assets/images/user-guide-17.jpg)

##### Unbind the external IP address
If you unbind the default egress IP address, the next public IP address with default route is automatically selected as the default egress of the virtual machine.

 ![1](/assets/images/user-guide-18.png)

If you want to unbind the IP address of an ENI bound to a virtual machine, you can unbind the ENI directly.
#### Set as default export
You can manually set a bound public IP address as the default network egress of a virtual machine, and only the public IP address with a default route can be set as the default network egress of a virtual machine. You can use the Virtual Machine Internet IP Management console to set default network egress for IPv4 and IPv6 Internet IP addresses bound to a virtual machine. As shown in the following figure:

 ![1](/assets/images/user-guide-19.png)

This section takes the `Centos 7.4` system to set IPv4 default egress to `106.75.234.39` as an example, as shown in the following figure, the new IP address is set to egress in the bound Internet IP address list:

 ![1](/assets/images/user-guide-20.png)

Log in to the `Centos` virtual machine, enter `curl ifconfig.io` to view that the exit of the virtual machine to access the public network has been replaced with `106.75.234.39`, as shown in the output of the following figure:

 ![1](/assets/images/user-guide-21.jpg)

The public IP address pool resources are customized by the administrator, and you can use private IP address blocks to simulate public IP addresses, and perform NAT translation on upper-layer physical network devices to access the Internet or IDC data center networks.
#### ENI management

x86 virtual machines can bind up to six ENIs, and ARM virtual machines can bind up to three NICs, which are used in application scenarios such as refined network management or high-availability services. In the virtual machine, you can view information such as the bound ENI and the associated IP address, which is usually named with `eth2` in the Linux operating system.

 ![1](/assets/images/user-guide-22.png)

As shown in the preceding figure, the ENI tab can view the list of ENIs bound to the current virtual machine and the unbind operation, and supports batch unbind operations. The bound ENI information includes the name, ID, NIC type, network, IP address, security group, and status information.
- NIC Type: refers to the ENI type that is currently bound to a virtual machine, including internal NICs and external NICs.
  - Intranet type EIPs can only be assigned IP addresses automatically or manually from the associated VPC subnet.
  - An EI of the Internet type can only automatically or manually assign an IP address from the associated external CIDR segment, and the assigned IP address is consistent with the lifecycle of the ENI, and can only be released when the ENI is destroyed.
- Network: The network to which the IP address of the ENI belongs and the network to which the ENN belongs is the VPC network and subnet where the ENI resides. The network to which the external network type belongs is the information of the external CIDR segment to which the ENI belongs.
- IP address: refers to the IP address of the current ENI, the IP address of the NIC of the private network type is the private IP address assigned to the VPC subnet, and the IP address of the NIC of the external network type is the external IP address assigned to the CIDR block to which it belongs.
- Security Group: refers to the security group that is bound to the current ENI, or is empty if no security group is bound. Platform security groups act on NICs, that is, each ENI can specify its own security group to control the traffic of the associated NICs.

You can unbind the bound ENI in the VM details, bind the ENI to other VMs, and unbind the NIC to other VMs through the Unbind operation in the Bound ENI list, as follows:

 ![1](/assets/images/user-guide-23.png)