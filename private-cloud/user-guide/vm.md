---
layout: default
title: Virtual Machine
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/virtual-machine/
nav_order: 2
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

 ![1](/assets/images/user-guide/user-guide-4.png)

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

 ![1](/assets/images/user-guide/user-guide-5.png)

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

 ![1](/assets/images/user-guide/user-guide-6.png)

- Virtual machine name: The default configuration name of the platform is `host`, and the user can customize the virtual machine name, which can be searched and filtered by name;
- Login mode: Set the login credentials for the virtual machine, that is, the password to log in to the virtual machine;
  - The administrator of `CentOS` is `root`, the administrator of `Ubuntu` is `ubuntu`, and the administrator name of `Windows` system is `administrator`;

Select the purchase quantity and payment method, confirm the order as shown in the following figure, and click "Buy Now" to create the virtual machine.

 ![1](/assets/images/user-guide/user-guide-7.png)

- Purchase quantity: Create multiple virtual machines in batches according to the selected configuration and parameters, up to 10 virtual machines in batches, and manually set the IP address of virtual machines during batch creation.
- Payment method: Select the billing method of the virtual machine, support three modes: hourly, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: Users select resources such as virtual machine CPU, memory, data disk, and external IP to display the fees according to the payment method.
- Buy Now: After clicking Buy Now, you will be returned to the virtual machine resource list page, where you can view the creation process of the host, usually display the status of "Starting" first, and the creation can be completed within 1~2 minutes.

## View virtual machines
Enter the VM console through the navigation bar to view the list of VM resources, and use the VM name on the list to enter VM Details and view the details of VMs and related resources.
### List of virtual machines
On the VM List page, you can view the list of virtual machine resources that exist under the current account, including name/comment, resource ID, VPC, subnet, model, configuration, IP, billing method, creation time, expiration time, status, and operation.

 ![1](/assets/images/user-guide/user-guide-8.png)

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

 ![1](/assets/images/user-guide/user-guide-9.png)

#### Virtual Machine Overview
The Overview page displays basic information, configuration information, and monitoring charts, and can perform operations and manage virtual machines on the Overview page.

 ![1](/assets/images/user-guide/user-guide-10.png)

- Basic information includes resource ID, name, private IP address, VPC, subnet, status, creation time, and alarm template.
  - You can click the button to the right of the name to modify the name and remarks of the virtual machine;
  - You can click the button on the right side of the alarm template to modify the alarm template associated with the virtual machine, and the virtual machine will bind the default alarm template by default.
- The configuration information includes the model, image, CPU, memory, total capacity of the system disk, and total capacity of the system disk, where the data disk capacity is the sum of the capacities of all EVS disks associated with the current virtual machine.
- Monitoring charts: Includes CPU usage, memory usage, space usage, bandwidth of NICs, packets of NICs, read/write throughput of disks, read/write throughput, average load, number of TCP connections, and number of blocking processes.

Users can enable Auto Refresh in the upper-right corner of the monitoring chart to automatically refresh the page every 3x0 seconds to obtain the latest monitoring chart data.

#### Virtual Machine Hard Disk
The Disks page displays the system disks and mounted data disks of the current virtual machine, including the name, ID, cluster architecture, cluster, disk type, disk capacity, mount point, billing method, status, creation time, expiration time, and snapshot operation information of each disk. As shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-11.png)

- Cluster architecture refers to the cluster architecture of the physical machine to which the virtual machine system disk or data disk belongs, including `HDD, SSD, NVME`, etc., which represents the disk media type of the cluster.
- A cluster refers to the cluster where the physical machine to which the system disk or data disk of a virtual machine belongs is located, which can be customized by the administrator, such as capacity or performance type.
- The disk types include system disks and data disks, and the additional EVS disks bound to them are data disks except for system disks.
- Hard disk capacity is the current capacity size of each hard disk;
The mount point is the actual drive letter of the hard disk in the virtual machine, such as vdb;
- Only data disks support billing methods and expiration time information, and the life cycle of system disks and virtual machines is the same.

In the operation items in the virtual machine hard disk management list, you can renew and snapshot the system disk and data disk of the virtual machine, and only the data disk is supported for renewal, and the system disk is consistent with the life cycle of the virtual machine.

##### Hard disk snapshots
In order to ensure that the data of all applications is captured in the snapshot, it is recommended to shut down the virtual machine or unmount the data disk before taking a snapshot. The specific operation is shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-12.png)

##### Hard disk expansion
Support the system disk and data disk expansion operation separately, the expansion of the EVS disk is to increase the capacity of the block device, and does not expand the file system in the system, that is, the system disk and data disk need to enter the virtual machine to perform the file system expansion (resize) operation after expansion, the specific operation steps of the system disk are detailed in: System disk expansion.

 ![1](/assets/images/user-guide/user-guide-13.png)

Expansion will be charged according to the new capacity, and hard disks paid by the hour, and the next payment cycle for upgrading capacity will be charged according to the new configuration; For hard drives that are paid annually and monthly, the upgrade capacity takes effect immediately, and the price difference is automatically made up on a pro rata basis.
#### Data disk renewal
You can directly renew data disks attached to a virtual machine, and you will be charged for the renewal period when you renew, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-14.png)

If the billing method of the data disk is Hourly, you can select 1 to 24 hours. If the billing method of the data disk is Monthly, the renewal period can be from January to November. If the billing method of the data disk is Annual, the renewal period of the data disk is 1 to 5 years.

#### Virtual Machine Networking

The Network page displays the basic network information of the current virtual machine, and can manage the external IP address and ENI resources of the virtual machine. The basic network information includes the VPC, subnet, private IP address, public security group, and private network security group of the current virtual machine, and you can update the public network security group and private network security group of the virtual machine by clicking the buttons on the right side of the security group. As shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-15.png)

For more information about the public IP addresses and ENI resources of virtual machines, see Internet IP Management and ENI Management.

#### Internet IP Management
The platform supports IPv4/IPv6 dual-stack networks, and each virtual machine can bind up to 50 IPv4 and 10 IPv6 external IP addresses, and the first IP address with default route (including the IP address of the external ENI) is used as the default network egress of the virtual machine. At the same time, the bound external IP address and network route are viewed in the virtual machine, and the traffic of the virtual machine accessing the external network is directly transmitted to the physical network card through the virtual network card to communicate with the external network, improving the performance of network transmission.

The public IP address information includes the virtual machine and the bound external ENI IP, and only the public IP address with default route can be set as the default network egress of the virtual machine. You can view the details of the bound public IP addresses and related management operations through the list information, including the IP address, IP version, egress, bandwidth, route type, binding time, and status of the bound Internet IP address as shown in the figure.

- IP refers to the IP address and CIDR block name of the currently bound external IP address (the CIDR block is a pool of external IP addresses customized by the platform administrator);
- IP version refers to the IP version that is currently bound to the public IP address, including IPv4 and IPv6;
- Egress refers to whether the current IP is the default egress of the virtual machine, and a virtual machine supports up to two default egresses (one each for IPv4 and IPv6).
- Bandwidth refers to the bandwidth limit of the current IP address, which is specified when applying for an external IP address.
- The route type refers to the type of route delivered by the network segment to which the current IP address belongs (the network segment routing policy is customized by the platform administrator), including default routes and non-default routes, and only supports setting the external IP address with default routes as the default network egress of virtual machines.
  - The default route type means that when a VM binds the IP address, a route with a destination address of `0.0.0.0/0` is automatically delivered to the VM.
  - Non-default route means that when a VM binds the IP address, the specified destination address route configured by the administrator for the CIDR block is issued, such as a route with a destination address of `10.0.0.0/24` for the VM.
  - If multiple public IP addresses bound to a virtual machine are the default route type, the first IP address with a default route is used as the default egress of the virtual machine.

Users can use the operation items in the Internet IP Management console to bind, unbind, and set the default egress operation of the Internet IP address, and support batch unbinding.

The IP address of the external ENI bound to the virtual machine is also displayed to the public IP list, which can be set as an egress operation but cannot be unbound.

##### Bind an external IP address
You can bind up to 50 IPv4 and 10 IPv6 public IP addresses, and the first IP address with a default route is used as the default network egress of the virtual machine.

 ![1](/assets/images/user-guide/user-guide-16.png)

After the binding is successful, you can view the bound IP address configured on the second NIC of the virtual machine, this section uses the Centos operating system as an example, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-17.jpg)

##### Unbind the external IP address
If you unbind the default egress IP address, the next public IP address with default route is automatically selected as the default egress of the virtual machine.

 ![1](/assets/images/user-guide/user-guide-18.png)

If you want to unbind the IP address of an ENI bound to a virtual machine, you can unbind the ENI directly.
#### Set as default export
You can manually set a bound public IP address as the default network egress of a virtual machine, and only the public IP address with a default route can be set as the default network egress of a virtual machine. You can use the Virtual Machine Internet IP Management console to set default network egress for IPv4 and IPv6 Internet IP addresses bound to a virtual machine. As shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-19.png)

This section takes the `Centos 7.4` system to set IPv4 default egress to `106.75.234.39` as an example, as shown in the following figure, the new IP address is set to egress in the bound Internet IP address list:

 ![1](/assets/images/user-guide/user-guide-20.png)

Log in to the `Centos` virtual machine, enter `curl ifconfig.io` to view that the exit of the virtual machine to access the public network has been replaced with `106.75.234.39`, as shown in the output of the following figure:

 ![1](/assets/images/user-guide/user-guide-21.jpg)

The public IP address pool resources are customized by the administrator, and you can use private IP address blocks to simulate public IP addresses, and perform NAT translation on upper-layer physical network devices to access the Internet or IDC data center networks.
#### ENI management

x86 virtual machines can bind up to six ENIs, and ARM virtual machines can bind up to three NICs, which are used in application scenarios such as refined network management or high-availability services. In the virtual machine, you can view information such as the bound ENI and the associated IP address, which is usually named with `eth2` in the Linux operating system.

 ![1](/assets/images/user-guide/user-guide-22.png)

As shown in the preceding figure, the ENI tab can view the list of ENIs bound to the current virtual machine and the unbind operation, and supports batch unbind operations. The bound ENI information includes the name, ID, NIC type, network, IP address, security group, and status information.
- NIC Type: refers to the ENI type that is currently bound to a virtual machine, including internal NICs and external NICs.
  - Intranet type EIPs can only be assigned IP addresses automatically or manually from the associated VPC subnet.
  - An EI of the Internet type can only automatically or manually assign an IP address from the associated external CIDR segment, and the assigned IP address is consistent with the lifecycle of the ENI, and can only be released when the ENI is destroyed.
- Network: The network to which the IP address of the ENI belongs and the network to which the ENN belongs is the VPC network and subnet where the ENI resides. The network to which the external network type belongs is the information of the external CIDR segment to which the ENI belongs.
- IP address: refers to the IP address of the current ENI, the IP address of the NIC of the private network type is the private IP address assigned to the VPC subnet, and the IP address of the NIC of the external network type is the external IP address assigned to the CIDR block to which it belongs.
- Security Group: refers to the security group that is bound to the current ENI, or is empty if no security group is bound. Platform security groups act on NICs, that is, each ENI can specify its own security group to control the traffic of the associated NICs.

You can unbind the bound ENI in the VM details, bind the ENI to other VMs, and unbind the NIC to other VMs through the Unbind operation in the Bound ENI list, as follows:

 ![1](/assets/images/user-guide/user-guide-23.png)

## VNC login

VNC (Virtual Network Console) is a login method provided by SCloudStack to connect to virtual machines through WEB browsers, which is suitable for scenarios where virtual machines cannot be connected through remote login clients (such as SecureCRT, remote desktop, etc.). Connect to a virtual machine through VNC login, view the complete startup process of the virtual machine, manage the virtual machine operating system and interface like SSH and remote desktop, and support sending operation management commands, such as `CTRL+ALT+DELETE`.

Users can log in to the current virtual machine using a VNC link through the "Login" button in the virtual machine list or the operation of the details overview page, providing a display-like function to log in to the virtual machine operating system and perform system-level operation and management of the virtual machine. As shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-24.jpg)

 The prerequisite for logging in to a virtual machine is to have an operating system account and password, and VNC login is suitable for scenarios where the virtual machine does not have an external IP address.
## Start/Shutdown/Power Off/Restart

Users can perform basic operations such as shutting down, starting, power-off, and restarting virtual machines, and all of them support batch operations with multiple APIs. As shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-25.png)

- Shutdown
  - Users can click [Shutdown] through the console to shut down, and the status of the virtual machine must be running at the time of shutdown.
  - When a virtual machine is shut down, the status changes from running to shutdown, and finally to shutdown, indicating that the shutdown is successful.
  - If the virtual machine is stuck in shutdown, you can power off the virtual machine.
  - After shutdown, the memory information of the virtual machine is lost, and the data of all disks is retained;
  - After shutdown, you can start, delete, create an image, reinstall the system, modify the configuration, bind the external IP address, and modify the security group.
- Initiate
  - Users can click the [Start] button through the console to start the virtual machine, which is available only when the virtual machine status is powered off;
  - When the virtual machine is powered on, the status changes from shutdown to starting, and finally to running, indicating successful startup.
  - If the virtual machine is stuck in startup, you can power off the virtual machine;
  - Running virtual machines can perform operations such as shutdown, login, deletion, power failure, restart, password reset, hot upgrade, binding an Internet IP address, modifying security groups, and modifying alarm templates.
- Power
  - Power failure is to force the shutdown of a virtual machine, which is the same as the direct power off operation of a physical machine, which may cause data loss or even damage to the operating system.
  - The power-off operation is suitable for scenarios where the virtual machine is frozen and extreme testing, and the virtual machine can be forced to shut down through the "Power Down" button in the virtual machine list operation.
  - When you forcibly shut down, the virtual machine directly enters the shutdown state and can be started again.
- Reboot
  - Restart is to restart the operating system of a virtual machine normally, which is consistent with the restart of the operating system of a physical machine.
  - When the virtual machine restarts, the status transitions from running to restarting, and finally to running;
  - If the virtual machine is stuck in restart, you can power off the virtual machine;
  - After the reboot, the memory information of the virtual machine is lost and the data of all disks is preserved.

## Making an image
Self-made images are exported by the cloud platform account through the virtual machine, which can be used to create virtual machines, only the account itself has permission to view and manage, and only supports making images in the shutdown state of the virtual machine, that is, the virtual machine can be exported as an image in the *shutdown state*.

You can click the Create Image button in the VM list to create an image, and enter the image name and description, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-26.png)

- Image name: the name and logo of the self-made image;
- Image description: self-made image description and remarks information, optional;

During the image creation process, do not stop, start, power down, reinstall the system, or modify the configuration of the virtual machine, so as not to affect the image creation process. After the image is created, it is displayed on the Image Management page in the VM console, where you can view the image creation process, and when the image status is ready to available, you can use the custom image to create a virtual machine.

When you create an image from a virtual machine, only the data and information of the system disk are exported, and the data disk is not supported.

## Reinstalling the System
Reinstalling the system is to reset the operating system of the virtual machine, that is, replace the virtual machine image, Linux virtual machine only supports replacing Centos and Ubuntu operating systems, Windows virtual machine only supports replacing other versions of Windows operating system, the premise of reinstalling the system is that the virtual machine must be shut down.

After the virtual machine is shut down, use the Reinstall System button in the virtual machine console operation to replace the image of the virtual machine, as shown in the following figure, you can perform a password reset operation during reinstallation:

 ![1](/assets/images/user-guide/user-guide-27.png)

 Reinstalling the system automatically erases the system disk snapshots created by the virtual machine, and you can back up the system disk data of the virtual machine by making an image. During the reinstallation process, the status of the virtual machine is automatically changed to "reloading", and after the reinstallation is successful, it is converted to "shutdown", and the virtual machine can be turned on by starting the operation, and when the virtual machine starts, the virtual machine will be run with a new image.

Note: After the system is reinstalled, the operating system and data content of the virtual machine are erased, and the mounted EVS disk and snapshots are not affected.

## Reset Password
Reset password refers to changing the login password of the virtual machine operating system online, which is suitable for scenarios where you forget the login password or want to quickly change the password through the console.

The Linux operating system is to change the password of the root or ubuntu account, and the Windows operating system is to change the password of the administrator account. The virtual machine must be running when the password is reset. The user resets the password by clicking the Reset Password button in the VM console operation, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-28.png)

- Virtual machine name: the name and ID of the virtual machine that needs to change the password;
- Administrator password/confirm password: the new password that needs to be changed;
- If the user actively modifies the administrator account of the virtual machine operating system, the password cannot be reset.

Do not reset the password during the image creation process, users can also log in to the operating system, use operating system commands or interfaces to change the password.

## Modify configuration (upgrading)
Modifying the configuration changes the CPU and memory specifications of a virtual machine, supports upgrades and downgrades, and adapts to scenarios where the configuration of the virtual machine needs to be adjusted as your business changes.

Before modifying the configuration, the virtual machine needs to be `shut down`, that is, the configuration modification operation must be performed in the shutdown state, and after the configuration change, it needs to be restarted to take effect. You can click Modify Configuration in the resource list operation in the virtual machine console to adjust the CPU memory of the virtual machine, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-29.png)

If the virtual machine is downgraded, the new configuration will be charged in the next billing cycle. For hourly virtual machines, the new configuration will be charged in the next billing cycle. For virtual machines that are billed annually and monthly, the upgrade configuration takes effect immediately, and the price difference is automatically repaid on a pro rata basis.
- VM ID and Name: The name of the virtual machine that currently needs to change the specification and the globally unique ID ID;
- Billing method: the payment method of the current virtual machine;
- Current specifications: CPU memory configuration before the current virtual machine is changed;
- Change specification: The new specification configuration of the current virtual machine needs to be changed, and the configuration can be upgraded or downgraded.
- Estimated charges: Fees that the system is expected to deduct after the configuration is changed;

After clicking OK, the virtual machine remains in a shutdown state, and the next time it starts, the virtual machine will run with the newly changed configuration. Users can log in to the operating system to view the changed configuration after the virtual machine is powered on.

If the virtual machine is equipped with GPU capabilities, you cannot upgrade or upgrade the number of GPUs.

## Hot Upgrade
Virtual machines provide hot upgrade capabilities and support CPU and memory upgrades when virtual machines are powered on. Before using hot upgrade, you need to be familiar with the following basic concepts:

- Modify the configuration: that is, upgrade or downgrade the CPU and memory specifications of the virtual machine when the virtual machine is shut down;
- Hot upgrade: In the running state of the virtual machine, you can upgrade the CPU and memory of the virtual machine;
- Base image: A base image that allows you to start a virtual machine through the base image and create a custom image based on the virtual machine.

Note: Currently, only hot upgrade of virtual machines with Base image of `Centos 7.4` is supported, and online downgrade is not supported.

Virtual machines whose platforms support hot upgrade are automatically displayed on the list, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-30.png)

(1) When the user sees the hot upgrade prompt, you can adjust the virtual machine online through "hot upgrade" in the list operation items, as shown in the following figure:

  ![1](/assets/images/user-guide/user-guide-31.png)

(2) In the hot upgrade wizard, you can perform a hot upgrade operation on the CPU memory specifications of the virtual machine, which takes effect immediately after the hot upgrade, and the virtual machine purchased by the hour will be charged according to the new configuration in the next billing cycle, and the virtual machine purchased by year and month will automatically make up the difference in proportion, as shown in the following figure:

  ![1](/assets/images/user-guide/user-guide-32.png)

If you customize the image and the Base image is based on Centos 7.4, the hot upgrade operation is allowed by default.

## Modify the alarm template
Modifying the alarm template is to configure the monitoring data of the virtual machine to alarm, and through the indicators and thresholds defined by the alarm template, you can trigger alarms when the relevant indicators of the virtual machine fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure the normal operation of the virtual machine and services.

You can click the button on the right side of the alarm template on the VM Details Overview page to modify the alarm template, select a new VM alarm template in the Modify Alarm Template wizard, and click OK to take effect immediately.

  ![1](/assets/images/user-guide/user-guide-33.png)

- Resource ID: The ID of the virtual machine that currently needs to add or modify the alarm template;
- Resource Type: The resource type that you need to add or modify alarm templates.
- Alarm template: For the alarm template that needs to be changed, only one alarm template can be associated with a virtual machine.

If the default alarm template provided by the system cannot meet your requirements, you can go to the Alarm Templates page to add and configure it.

## Bind a public IP address
Binding a public IP address refers to binding a tenant's external IP address to a virtual machine to provide an external network egress for the virtual machine. The platform supports IPv4/IPv6 dual-stack networking, each virtual machine supports binding up to 50 IPv4 and 10 IPv6 external IP addresses, and can also bind external ENIs to virtual machines to provide external network communication capabilities, and the first IP address with default route is used as the default network egress of the virtual machine.

After binding the external IP address, the platform configures the specified external IP address to the network card of the virtual machine, including the gateway, subnet mask and routing information of the network segment to which the external IP address belongs, and users can view the bound public IP address and network route in the virtual machine, and the traffic of the virtual machine accessing the external network is directly transmitted to the physical network card through the virtual network card to communicate with the external network, improving the performance of network transmission.

Taking the Linux operating system as an example, the internal network card is generally eth0, the external network card is generally `eth1`, and the default bound external IP will be configured in the `eth1` of the virtual machine, that is, 50 external network IPs will be bound to the external network card and share the external network security group of the virtual machine; When you bind an ENI to a virtual machine, an ENI is directly added to the virtual machine, and the IP address and security group policy of the external ENI are automatically configured.

  ![1](/assets/images/user-guide/user-guide-16.png)

The virtual machine must be running or shut down to perform public IP binding, you can use the virtual machine management console list or the "bind EIP" button of the operation item of virtual machine details network management to perform external IP binding operations, the specific operation steps can refer to virtual machine external IP management. The bind operation specifies the public IP address to be bound, and only 50 IPv4 and 10 IPv6 public IP addresses can be bound.
- If the virtual machine has 50 IPv4 public IP addresses bound, you cannot bind IPv4 public IP addresses again.
- If the virtual machine has 10 IPv6 public IP addresses bound, you cannot bind IPv6 public IP addresses again.
- If a virtual machine has 50 IPv4 and 10 IPv6 external IP addresses bound to both, you cannot bind a public IP address and can continue to add an ENI on the Internet.

After the public IP address is bound, you can view the bound public IP address through the IP information of the virtual machine list, or view the details of the bound public IP address through the Internet IP tab of the VM Details network management, and perform operations such as setting as the default egress and unbinding.

  ![1](/assets/images/user-guide/user-guide-34.png)

Only public IP addresses of the same data center can be bound, and the bound public IP addresses must be unbound. To unbind the external IP address of a virtual machine, refer to Virtual Machine External IP Management.

## Modify security groups
By default, a virtual machine created by a platform user comes with two virtual NICs that are consistent with the life cycle of the virtual machine, namely an internal NIC and an external NIC, and can also bind ENI resources to the virtual machine.

- Internal NIC: Configure the IP address and network information of the VPC/subnet specified during virtual machine creation.
- Internet NIC: Configure all external IP addresses bound to the virtual machine, including 50 IPv4 and 10 IPv6 addresses.
- ENI: Configure the IP address and security group assigned to the network to which the ENI belongs, VPC and subnet if the ENI is an internal network type, and the external CIDR block if the ENI is external.

The cloud platform security group (software-defined virtual firewall) is at the NIC level, that is, the bound security group restricts the NIC traffic in the virtual machine. The platform defines the security group bound to the internal network NIC of the virtual machine as an intranet security group. The security group bound to the external network NIC of the virtual machine is defined as a public network security group. The security group bound to the ENI is the security group to which the ENI belongs.

The private network security group is used for the security access control of the virtual machine in the east-west direction (between NICs); Internet security groups are used to control the north-south (external IP) traffic of virtual machines. When you create a virtual machine, you can specify a private network security group or a public network security group, and modify the private network security group and public network security group after the virtual machine is running.

### Modify a public security group
Modifying a public network security group means modifying the security group associated with the virtual machine's external NIC, that is, changing the security group bound to all public IP addresses on the external network card.

You can use the Modify Internet Security Group button on the VM list and VM Details page, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-35.png)

Select the Internet security group that you want to modify, and you can bind only one Internet security group to a virtual machine. After the modification is successful, you can view the modified public network security group information through the network information of the virtual machine details.

The access restriction of a public security group rule applies to all public IP addresses bound to the current virtual machine.

### Modify the intranet security group
Modifying an intranet security group means modifying the security group associated with the internal NIC of a virtual machine, that is, changing the security policy of the virtual machine's internal network for traffic control between virtual machines and virtual machines. You can use the Modify Private Network Security Group button on the VM list and VM Details page, as shown in the following figure (Network/Elastic NIC):

 ![1](/assets/images/user-guide/user-guide-35.png)

Select the private network security group to be modified, and modify it to "Do not bind yet" to unbind the private network security group, and only one private network security group can be bound to a virtual machine. After the modification is successful, you can view the modified private network security group information through the network information of the virtual machine details.

Intranet security groups and external security groups can be bound to the same security group, that is, intranet and external security groups use the same security group and policy.

## Modify Name and Remarks
Modify the name and notes of the virtual machine to operate in any state. Click the button to the right of the virtual machine name on the VM List page to modify it, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-36.png)

## Virtual Machine Renewal
You can manually renew virtual machines, and the renewal operation is only for the resource itself, and does not renew additional resources associated with the resource, such as public IP addresses, EVS disks, and external ENIs. After the additional associated resources expire, they are automatically unbound, and to ensure normal service use, you must renew the relevant resources in a timely manner, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-37.png)

When the virtual machine is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the billing method of the virtual machine is [Hour], the renewal period can be selected from 1 to 24 hours; If the billing method of the virtual machine is Monthly, you can select the renewal period from January to November. If the billing method for a virtual machine is Annual, the renewal period of the virtual machine is 1 to 5 years.
## Obtaining VNC login information
Users are supported to obtain VNC login information of virtual machines, which is suitable for scenarios where VNC clients are used to connect to virtual machines. For example, in desktop cloud scenarios, desktop terminals of desktop cloud service providers usually use VNC, Spice, and RDP protocols to connect to desktop VMs provided by the platform, and VNC and SPICE protocols are basically common protocols for desktop service providers.

You can view the VNC login information of a virtual machine, including the VM ID, VNCIP address, VNC port, and VNC client login password, through the API interface or [Get VNC Information] in the VM console operation item, as shown in the following figure (Get console information):

 ![1](/assets/images/user-guide/user-guide-38.png)

- VNCIP address: The address assigned in the current cloud platform public IP address pool, and the network that can access the external IP address can use the <br/>VNCIP: port and password to connect to the virtual machine through VNC clients, such as VNCView client software.
- VNC port: The port used when VNC login, to ensure that the security platform will replace an unused port each time it obtains VNC information.
- VNC password: The password used when VNC login, each view will randomly provide a new VNC login password according to the algorithm, in order to ensure the security of VNC login to the virtual machine (scenario example: after the user logs in to the virtual machine for the first time, and enters the desktop or command line through the login password of the virtual machine operating system, the next login VNC will automatically enter the desktop and command line, bringing inevitable security risks to the user's virtual machine). ）

In order to ensure the security of VNC connection, the VNC login information obtained through the interface is valid for 300 seconds each time the API is called or obtained, if the user does not use the IP and port to connect within 300 seconds, the information will be directly invalid and a new login information needs to be obtained; At the same time, after the user logs in to the virtual machine using the VNC client, no operation is automatically disconnected for 300 seconds.

When using the cloud platform, the user will provide at least one external IP block that can access the cloud platform, and the VNC IP address is the assigned IP address in the cloud network block to ensure network reachability.

Users can log in to the platform virtual machine using a client such as VNCView in the environment of the network-reachable platform, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-39.jpg)

 ![1](/assets/images/user-guide/user-guide-40.jpg)

## Delete a virtual machine
Platform users can delete virtual machine resources that are powered off or running in the account in the console and can delete them in batches. When a virtual machine is deleted, it automatically goes to the Recycle Bin, which can be restored or completely destroyed. This can be done through Delete in the VM List action item, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-41.png)

- When you delete a virtual machine, it automatically unbinds resources such as external IP addresses, ENIs, and EVS disks bound to the virtual machine.
- If a virtual machine is added to the NAT gateway whitelist or a Load Balancing service node, the virtual machine is automatically unbound when it is deleted.
- When you delete a virtual machine, you can choose to delete bound resources, that is, automatically unbind and delete bound external IP addresses, ENIs, and EVS disks.
- When you delete a virtual machine, the deleted public IP addresses and EVS disks are automatically entered into the Recycle Bin, and the deleted ENIs are completely destroyed.
- If the virtual machine expires and is not renewed within the allowed time, the virtual machine is automatically reclaimed and the associated resources are automatically unbound.

After the virtual machine is deleted, the two default NICs, system disks, and system disk data created with the virtual machine are entered into the Recycle Bin along with the virtual machine, and can be entered to destroy or restore the virtual machine.

When the external IP addresses and EVS disks that are entered into the Recycle Bin with the virtual machine are restored, the original binding relationship is not maintained, and resource binding operations must be performed again.

## Remote login
Telnet refers to remote login and management of virtual machines over the network through remote management client software, and provides different ways to log in remotely for Linux and Windows virtual machines. The prerequisite for remote login is that the virtual machine must be bound to an external IP address and can access the remote login port of the server normally through the Internet (22 for Linux SSH and 3389 for Windows Remote Desktop).

### Remote login to Linux
For verification purposes, this manual assumes that the local client operating system is Linux or Mac OS, that is, the default comes with an SSH client, and you can directly log in to the remote SSH server by using SSH commands from the command line. The specific operation steps are:

(1) Bind a public IP address to a virtual machine that requires remote login, and the public security group allows SSH port 22 access, as shown in the following figure:

 ![1](/assets/images/user-guide/user-guide-34.png)

(2) The user opens the terminal that comes with the system and enters the SSH command to log in: ssh root@ the external IP address of the virtual machine, as shown in the following example:

```
admin ~ % ssh root@113.197.48.5
```

(3) Enter the login password of the virtual machine to log in to the Linux server directly, as shown in the following figure.

```
admin ~ % ssh root@113.197.48.5
root@113.197.48.5's password: 
Last login: Thu Mar 30 12:44:16 2023 from 113.23.114.96
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[root@localhost ~]# 
```

### Remote login to Windows
For easy verification, this manual assumes that the local client operating system is Mac OS, using Microsoft Remote Desktop Connection MAC version program RDC to log in, the operation steps are the same as Windows Remote Desktop, only need to enter the public IP address of Windows in the tool to connect.

The prerequisite for remote desktop connection is that the virtual machine must be bound to an external IP address, and the bound external network security group allows port 3389.

## System disk expansion
The default system disk size of a virtual machine is 40 GB, and the platform supports users to expand the system disk capacity up to 500 GB. If the default system disk capacity of 40 GB cannot meet the business requirements, you can specify the required system disk capacity to create a virtual machine, as shown in the following figure, specify a system disk capacity of 200 GB to create a virtual machine, and the system disk device capacity of the created virtual machine is 200 GB.

 ![1](/assets/images/user-guide/user-guide-42.png)

The expansion of the capacity of the system disk is to expand the capacity of the system disk device, and does not expand the file system in the virtual machine operating system, that is, after the system disk is expanded, it needs to enter the virtual machine to resize the file system.

The expansion operation is different for different types of operating system partitions, such as Windows, which usually uses its own disk management tools for expansion operations. According to different OS system disk expansion scenarios, the following are generally categorized:

- Linux system disk partition expansion
- The partition of the Windows system disk is expanded

Before you perform partition expansion and file system expansion, make sure that the storage capacity of the system disk is adjusted in the console.

### Linux system disk partition expansion
Linux systems usually use `growpart` and `resize2fs` tools to complete system partition expansion and file system expansion operations. This example uses the Centos 7.4 operating system as an example, and performs the following operations:

(1) Install the `growpart` file system expansion tool.
- Centos
```
yum install -y epel-release
yum install -y cloud-utils
```
(2) View the system disk capacity through `fdisk -l` is 200GB, run `df -th` to view the system disk `partition/dev/vda1` The capacity is 40GB, and the file system type is `ext4`.

```
[root@localhost ~]# fdisk -l

Disk /dev/vda: 214.7 GB, 214748364800 bytes, 419430400 sectors
Units = sector of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (min/optimal): 512 bytes / 512 bytes
Disk label type: DOS
Disk identifier: 0x000ba442

Device Boot Start End Blocks Id System
/dev/vda1   *        2048    83883775    41940864   83  Linux
[root@localhost ~]# 
[root@localhost ~]# df -Th

File System Type Capacity Used Available % Used mount point
devtmpfs       devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs          tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs          tmpfs     1.9G  8.4M  1.9G    1% /run
tmpfs          tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/vda1      ext4       40G  1.4G   36G    4% /
tmpfs          tmpfs     382M     0  382M    0% /run/user/0
```

(3) Run the `growpart <DeviceName> <PartionNumber>` command to resize the partition and restart the virtual machine, this example `growpart /dev/vda 1` indicates the capacity of partition 1 of the system disk.

```
[root@localhost ~]# LANG=en_US. UTF-8
[root@localhost ~]# growpart /dev/vda 1
CHANGED: partition=1 start=2048 old: size=83881728 end=83883776 new: size=419428319 end=419430367
[root@localhost ~]# reboot
```

(4) After the virtual machine is restarted, expand the file system of the virtual machine system disk, and use different ways to expand it in different file system types.

The `ext` file system can be resized using the `resize2fs <PartitionName>` tool as follows:

```
[root@localhost ~]# resize2fs /dev/vda1
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vda1 is mounted on /; on-line resizing required
old_desc_blocks = 5, new_desc_blocks = 25
The filesystem on /dev/vda1 is now 52428539 blocks long.
```
If you have an `XFS` file system, you can use the `xfs_growfs <mountpoint>` tool to resize the file.

(5) Run `df -Th` to view the system disk `partition/dev/vda1` with a capacity of 200GB.
```
[root@localhost ~]# df -Th
File System Type Capacity Used Available % Used mount point
devtmpfs       devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs          tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs          tmpfs     1.9G  8.3M  1.9G    1% /run
tmpfs          tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/vda1      ext4      197G  1.5G  187G    1% /
tmpfs          tmpfs     382M     0  382M    0% /run/user/0
```

### Windows system disk partition expansion
Windows systems typically use the Disk Management tool in Administrative Tools - Computer Management for volume expansion operations. The specific operation is as follows:

(1) Select the operation > rescan disk in the disk management tool to identify the newly expanded unallocated empty space, as shown in the following figure 20GB of unallocated space;

(2) Right-click the `C` drive, select Expand Volume, and perform volume expansion operations on the system disk.

(3) In the Expand Volume wizard, use the default configuration to extend the volume.

(4) After the expansion volume operation is completed, the new system disk capacity is automatically merged to the `C` disk, which means that the system disk file system is successfully extended.