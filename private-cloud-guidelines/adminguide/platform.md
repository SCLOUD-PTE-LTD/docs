---
layout: default
title: System Management
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/system-management/
nav_order: 10
---
# 10 System Management

Platform management provides administrators with capabilities for platform-wide configuration, quota allocation, and custom resource specification, enabling enterprises and administrators to provide customized configuration services for the platform, enhancing its customization capabilities.

## 10.1 Global Configuration

Global configuration provides administrators with information on the global configuration of the entire cloud platform, including platform settings, billing settings, console security, disk management, network settings, email settings, website settings (enterprise customization), login page settings (enterprise customization), recycling policies, resource management, and quota settings.

### 10.1.1 Billing Strategy

Billing strategy setting refers to the administrator's setting of whether resources are automatically renewed for the platform. The default value is yes, meaning that when a tenant's account balance is sufficient, their billing resources will be automatically renewed; if the administrator disables automatic renewal, the resources will expire and become outdated within seven days without renewal, after which they will be deleted or moved to the recycle bin.

### 10.1.2 Console Security

The console security configuration item is a global configuration made by the platform to ensure the security of the console and accounts, including whether to enable freezing of accounts after multiple login failures, password login validity period, whether to prohibit simultaneous login from multiple devices, and the duration of automatic logout for inactivity.

1. **Whether to enable freezing of accounts after multiple login failures**

   The platform supports the administrator's ability to enable or disable the global account login function that freezes the account after multiple login failures.

   The default value is no, but if enabled, an account will be frozen after six failed login attempts, and the platform administrator needs to go to the tenant management account list to unfreeze the account.

2. **Password login validity period**

   Administrators can set a default policy for all global accounts to change passwords every 90 days to ensure account security.

   The default setting is 90 days and can be changed. Every 90 days, when an account logs into the console, they will be required to change their password.

3. **Whether to prohibit simultaneous login from multiple devices**

   Administrators can enable multi-device login for global accounts to accommodate different login needs.

   If set to no, the platform account only supports single sign-on, meaning that an account can only log in on one client at a time, and logging in from another client will automatically disconnect the previous client's connection.

   The default is yes, meaning that a platform account can log in and manage platform resources on multiple clients simultaneously.

4. **Duration of automatic logout due to inactivity**

   Administrators can set an idle time for the console to ensure the security of console resources and data.

   The default value is 30 minutes, meaning that if there is no activity on the console for 30 minutes, it will automatically log out. The setting supports values between 15 to 1440 minutes.

5. **Custom Password Complexity**

   Administrators can configure the password length, which defaults to 6 characters and can be between 6-30 characters.

   The administrator can modify the password complexity policy, which requires passwords to contain two or more of uppercase letters, lowercase letters, numbers, and special characters (excluding spaces) and cannot include illegal characters beyond [A-Z], [a-z], [0-9], and [()`~!@#$%^&*-+=_|{}[]:;'<>,.?/].

   Administrators can require users with non-compliant passwords to change them, but this feature is turned off by default.

### 10.1.3 Disk Management (Global Disk QoS)

Disk management allows administrators to enable or disable QoS control for all cloud disks on the platform to ensure the performance and reliability of all tenant disk resources.

1. By default, the platform globally enables Disk QoS, meaning that QoS applies to all new and existing disks.
   * The default QoS value is assigned to newly created disks according to the platform's calculation formula.
   * QoS for existing disks will be determined by their default value or the value modified by the administrator.
   * If configured as enabled, only the custom QoS defined by the administrator for each disk will take effect.

2. When configured as disabled, global Disk QoS is disabled for both new and existing disks.
   * Newly created disk QoS will not be restricted.
   * Existing disk QoS values are not restricted.
   * When configured as disabled, custom QoS defined by the administrator for each disk will not take effect.

3. After expanding the capacity of the hard disk, the QoS value of the new capacity will be recalculated according to a calculation formula, and the QoS of the hard disk will be reset according to the calculated QoS value.
   - If the QoS value set before expanding the hard disk is less than the QoS value of the new capacity, the QoS value of the new capacity will be used.
   - If the QoS value set before expanding the hard disk is greater than the QoS value of the new capacity, the value set before expanding will be used.

### 10.1.4 Network Settings

The network settings allow administrators to configure the VPC network segment and internal bandwidth of the platform. The VPC network segment refers to the CIDR network that the administrator sets for the VPC available to the platform's tenants. The internal bandwidth refers to the QoS of the internal bandwidth in the platform's VPC network.

#### 10.1.4.1 VPC Network Segment

Administrators can set the CIDR network segment that the tenant can use for the VPC on the platform. The default is three private network addresses: `10.0.0.0/16, 172.16.0.0/16, 192.168.0.0/16`. The 10 network segment supports creating an address with a maximum of 8 bits of mask, and the 192 and 172 network segments support creating an address with a maximum of 16 bits of mask. The 172 network segment supports `172.16.0.0/16~172.29.0.0/16`, and administrators can modify it according to the actual usage of the platform.

After setting the VPC network segment, when a platform tenant creates a VPC network, only the private network segment configured within the VPC network segment can be selected to adapt to the scenario of internal network segment conflicts in private cloud environments. The platform recommends configuring the VPC network segment, external network segment, and physical network segment as three different address segments, such as configuring the VPC network segment as `172.16.0.0/16`, the external network segment as `192.168.0.0/16`, and the physical network environment as `10.0.0.0/16`, which is convenient for network configuration and routing distribution and improves operational efficiency.

Administrators can configure the network segment that VPC can use according to the actual network environment. This article is for example only. The actual private cloud network and deployment environment shall prevail.

#### 10.1.4.2 Internal Bandwidth QoS

The internal bandwidth QoS refers to the upper limit of the bandwidth set by the platform administrator for the VPC internal network globally, which specifies the bandwidth QoS restriction between virtual machine VPC internal network cards (eth0). The default value is 1000Mbps, and the supported bandwidth range is 0~8192.

When the internal network bandwidth is set to 0, it means that the QoS of the global internal network of the platform is not limited. The platform recommends setting a QoS for the internal network to ensure the bandwidth performance and reliability of all tenant resources in the VPC internal network on the platform.

### 10.1.5 System Services

#### 10.1.5.1 Email Settings

Email settings refer to the configuration of the platform's email service, mainly used for password recovery, receiving and sending monitoring alert emails. The platform supports administrators defining whether the email supports SSL, the sender's email address, the sender's email password, the email server IP, the email server Port, and the email subject prefix, as shown in the figure:

![email](/assets/images/adminguide/email.png)

- Email support SSL: Configure whether the email supports SSL.
- Sender's email address: Configure the sender's email address.
- Sender's email password: Configure the sender's email password.
- Email server address: Set the IP address of the sending mailbox.
- Email server port: Set the port of the sending mailbox, with a default value of 994 and a range of 0-65535.
- Email subject prefix: Configure the subject prefix of the reminder emails sent by the platform, such as Private Cloud, Government Cloud, etc.

> When deploying the platform, email settings must be provided by default to avoid being unable to receive password recovery and monitoring alert emails.

After the email configuration is completed, users can test whether the email is configured correctly. The following figure shows how:

![email](/assets/images/adminguide/email1.png)

After entering the recipient's email and clicking confirm, if the email configuration is correct, the recipient's email will receive the test email.

#### 10.1.5.2 Platform Data Backup

* Platform database backup: After platform deployment, an automatic backup strategy will be generated.
Backup file storage location: /data/backups/taishancms-xxx.gz
Default scheduling policy: backup once every hour.

* Platform configuration file backup: After platform deployment, an automatic backup strategy will be generated.
Backup file storage location: /data/backups/configs-xxx.tar.gz
Default scheduling policy: 01:20 every day.

Administrators can modify the scheduling policy, and specific automatic strategy settings can be found in the document timer design.

### 10.1.6 Website Settings (Customization)

Website settings are customization capabilities provided by the platform for enterprises and administrators, including website favicon images, website titles, cloud platform logo images, and browser logos - customizing the logo of the cloud platform and the browser icon.

- Website Favicon image: The Favicon image displayed on the browser tab must be in the ICO format and not exceed 100KB. The recommended size is 48px*48px.
- Website Title: The website description displayed on the browser tab, such as `Private Cloud Platform`, supports Chinese, English, and special characters.
- Cloud Platform Logo Image: The logo above the tenant and administrator console navigation bar can be customized by administrators. The image supports PNG, JPEG, and JPG formats, with a maximum size of 200KB, and a recommended size of 352px*72px.

### 10.1.7 Login Page Settings (Customization)

Login page settings are customization capabilities provided by the platform for enterprises and administrators, including SSO background images, SSO titles, SSO title colors, etc., as shown in the figure below:

![ssoimage](/assets/images/adminguide/ssoimage.png)

* SSO background image: Represents the background image behind the login box. The image supports PNG, JPEG, and JPG formats, with a maximum size of 500KB.
* SSO title: Represents the title description on the login box, such as Private Cloud Platform, which can be in Chinese, English, and special characters.
* SSO title color: Represents the color of the title on the login box. It supports white, black, red, yellow, green, cyan, blue, brown, purple, orange, gray, and gold to adapt to the readability of text on different background images.

### 10.1.8 Virtual Machine Password Custom Complexity

   Supports administrators to configure the minimum length of virtual machine passwords, which defaults to 6 digits and supports 6-30 digits.

   Supports modifying password complexity. The password must contain at least two of the following: uppercase letters, lowercase letters, numbers, and special characters (excluding spaces). It cannot contain illegal characters other than [A-Z], [a-z], [0-9], and [()`~!@#$%^&*-+=_|{}[]:;'<>,.?/].

### 10.1.9 Monitoring Dashboard

The monitoring dashboard is a customization capability provided by the platform for enterprises and administrators, which allows modification of the description of the monitoring dashboard title.

### 10.1.10 Recycle Policy

The recycle policy is a customizable configuration for the recycle bin policy provided by the platform for administrators, including whether recycled resources are automatically destroyed, the automatic destruction cycle of recycled resources, and whether expired resources are automatically deleted and entered into the recycle bin.

- **Whether recycled resources are automatically destroyed**

  After setting resources to enter the recycle bin, whether to destroy them after a fixed period, which is enabled by default. If in actual use, resources entering the recycle bin do not need to be automatically destroyed, this configuration can be turned off.

- **Automatic destruction cycle of recycled resources**

  The cycle for automatically deleting resources in the recycle bin after being deleted, which is only valid when the automatic destruction of recycled resources in the recycle bin is enabled. The default value is 360000 seconds, and the supported range is `1-360000` seconds.

- **Whether expired resources are automatically deleted and entered into the recycle bin**

  When enabled, virtual machines, cloud disks, external IP addresses, and other resources that have expired for 7 days will automatically enter the recycle bin. When they enter the recycle bin, the resources associated with the virtual machine will be unbound. If the expired resources on the platform do not need to be deleted, they will automatically change to an expired state and be deleted after 7 days.

### 10.1.11 Resource Management Configuration

The resource management configuration allows administrators to configure whether resources are allowed to be deleted.

* Whether resources are allowed to be deleted: Administrators can set whether the platform globally allows resources to be deleted, with the default value being "Yes," indicating that all tenants on the platform are allowed to delete resources. If this configuration is changed to "No," it means that all tenants on the platform cannot delete resources.

### 10.1.12 Quota Settings

Quota settings are customizable configurations for global resource quotas provided by the platform for administrators, including whether to enable quota management and the upper limit of snapshots created for a single disk.

- Whether to enable quota management: Set whether the platform needs to enable quota management, with the default state being enabled. When disabled, tenants can create any number of resources without being restricted by tenant quotas.
- Upper limit of snapshots created for a single disk: Set the upper limit of the number of snapshots that can be created for a single disk, with a default value of 10 and a supported range of `0-200`. 0 means that the number of snapshots created for a single disk is not limited, and administrators can adjust the upper limit according to the actual usage of the platform.

### 10.1.13 Shared Disk Binding Quantity Limit

Limit the number of virtual machines that can be bound to shared cloud disks and shared external storage disks simultaneously.


## 10.2 Specification Configuration

Specification configuration is the capability provided by the platform for enterprises and administrators to customize specifications. Management personnel can adjust the types of cloud product services on the cloud platform through custom specification configuration, including virtual machines, disks, external IP addresses, etc. Among them, virtual machines support defining CPU and memory specifications; disks support defining the capacity range of cloud disks that tenants can create; and external IP addresses support defining the bandwidth range that tenants can apply for.

The platform defaults to providing recommended specifications for virtual machines, disks, and external IP addresses. Administrators can modify the specifications based on enterprise needs, including viewing, deleting, and modifying.

### 10.2.1 Creating Specifications

Creating specifications supports only creating CPU/memory specifications for virtual machines, and the specifications for cloud disks and external IP addresses are generated by the platform by default and can only be modified. Administrators can create specifications through global configuration-specification configuration, as shown in the following figure:

![createspec](/assets/images/adminguide/createspec_1.png)

Creating virtual machine specifications supports creating different types of specifications for different clusters. Different types of specifications can be created for different machine models, and when tenants create virtual machines with different machine types, they can create virtual machines of different specifications to adapt to different application scenarios where cluster hardware configurations are inconsistent. Both CPU and memory can be defined separately:

* CPU specification support (C): Except for 1, multiples of 2 increase, such as 1C, 2C, 4C, 6C, with a maximum value of 240C.
* Memory specification support (G): Except for 1, multiples of 2 increase, such as 1G, 2G, 4G, 6G, with a maximum value of 1024G.

The created specifications can be viewed and used by all tenants, and different specifications can be created in different clusters according to business needs.

### 10.2.2 Viewing Specifications

Administrators can view all defined specifications for products such as virtual machines, hard disks, and external IP addresses. By default, the platform generates default specifications for each product according to the cluster type. For example, in cluster A for computation, the default virtual machine specification is generated, and for storage clusters, the default hard disk specification is generated. It is important to note that hard disk and external IP specifications are only supported for viewing and modification, not for creation or deletion.

* Virtual machine specifications define the CPU/memory combinations that tenants can choose when creating a virtual machine, such as 1U2G, 2U4G, etc.
* Hard disk specifications define the range of capacity that tenants can set when creating a hard disk (minimum and maximum capacity), and each storage cluster type corresponds to a default hard disk capacity range specification.
* External IP specifications define the range of bandwidth that tenants can apply for when requesting an external IP address (minimum and maximum bandwidth), and each external network segment corresponds to a default bandwidth range specification.

Administrators can use the specifications configuration list to view the information on existing specifications, including product, cluster type, specification type, specification value, status, update time, and operation items, as shown in the following figure:

![speclist](/assets/images/adminguide/speclist.png)

- Product: Represents the product corresponding to the specification, including virtual machines, hard disks, and external IP addresses.
- Cluster Type: Represents the cluster type or external network segment corresponding to the specification, which indicates the dimension in which the specification takes effect, such as the CPU/memory specification of a virtual machine in computation cluster A; the capacity range specification of a hard disk in storage cluster A; and the bandwidth range specification of an external IP address in the BGP network segment.
- Specification Type: Custom specifications correspond to different types of specifications for different products. The specification type for virtual machines is CPU and memory, for hard disks it is capacity range, and for external IP addresses it is bandwidth range.
- Specification Value: Represents the specification for custom specifications, such as 1U2G for CPU specifications; 10GB~32000GB for hard disk capacity range specifications; and 1Mb~10000Mb for external IP bandwidth specifications.
- Status: Refers to the status of the custom specification.
- Update Time: Refers to the update time of the custom specification.

Administrators can modify and delete each specification through the list operation items, and the list also supports search for specification configuration.

### 10.2.3 Modifying Specifications

Administrators can modify the created and default generated specification values.

（1）Virtual machine product specifications support modifying the CPU and memory values. The CPU can be specified as 1, 2, 4, 8, 16, 24, 32, or 64, and the memory can be set as 1, 2, 4, 8, 16, 24, 32, 64, or 128 per year fee. Tenants can customize the combination of CPU and memory specifications according to their business needs.

![upvmspec](/assets/images/adminguide/upvmspec.png)

（2）Hard disk product specifications support modifying the minimum and maximum capacity range, with a range of 10GB to 8000G. This means that tenants can create cloud disks with a minimum size of 10GB and a maximum size of up to 32000GB. The minimum and maximum values can be adjusted according to business needs.

![updiskspec](/assets/images/adminguide/updiskspec.png)

（3）External IP specifications support modifying the minimum and maximum bandwidth range, with a range of 1Mb to 10000Mb. This means that tenants can apply for external IP addresses with a minimum bandwidth of 1Mb and a maximum bandwidth of 10,000Mb. The minimum and maximum values can be adjusted according to business needs.

![upnetspec](/assets/images/adminguide/upnetspec.png)

### 10.2.4 Deleting Specifications

Administrators can delete specified custom virtual machine specifications, but cannot delete hard disk and external IP specifications. As shown in the figure below:

![rmspec](/assets/images/adminguide/rmspec.png)

After the specification is deleted, the platform tenant cannot create a virtual machine using the current specification in the corresponding model, but this does not affect the normal operation of resources created through the specification.

> It is important to note that a default specification for virtual machines is generated within each cluster, which cannot be deleted, in order to prevent all specifications on the platform from being deleted, resulting in the inability to create virtual machines.

## 10.3 Quota Management

### 10.3.1 Overview

Quota is a limit on the number or capacity of virtual resources that a tenant (including sub-accounts) can create in a region. By limiting the resource quotas of each tenant, cloud platform physical resources can be effectively shared and allocated reasonably, improving resource utilization while meeting the resource needs of every account on the cloud platform.

**The premise for quota management to take effect is that the [Whether to Enable Quota Management] configuration item in the global configuration is set to Yes, otherwise the quota management configuration values will not take effect.**The cloud platform globally provides default quotas for each resource in each data center, which is the resource quota template provided by default when each tenant is created. Platform administrators can customize the resource quotas of each tenant through quota management in tenant management. Only the tenant main account and its sub-accounts cannot modify the resource quota quantity, but they can view the quota.

Sub-accounts and main accounts share the tenant's resource quotas, that is, the resource quota for each kind of resource is the sum of the number or capacity that the main account and all its sub-accounts can create. For example, if the quota for cloud disks for a tenant is 10, then the upper limit for the number of cloud disks that the main account and all its sub-accounts can create is not more than 10.

For the resources of virtual machines, cloud disks, external IP addresses that are deleted or expire and enter the recycle bin, they do not occupy tenant resource configurations, and when recovering resources, the resource quotas are checked.

Administrators can adjust the default quotas based on the actual usage of the platform, and after the adjustment takes effect, the quotas for all existing tenants on the platform will be executed according to the new quota standard, and new tenants will also set the quota limit according to the new quota standard.

Currently, quota limits mainly include the number of virtual machines, virtual machine templates, images, VPCs, VPC subnets, cloud disks, network cards, external IPs, security groups, load balancers, load balancer SSL certificates, VPN gateways, and NAT gateway quantities. It supports administrators to view the global quota configuration of the platform, and also supports administrators to modify quota items.

> The platform supports adjusting quotas for individual tenants, please refer to the description of adjusting tenant quotas in the tenant management section.

### 10.3.2 Viewing Quotas

Administrators can view the existing quota configuration information in the current platform global quota management through global configuration - quota management, including product type, resource type, quota factor, quota and operations, as shown in the following figure:

![quatolist](/assets/images/adminguide/quatolist.png)

Where region represents the quota setting of the product in the current region, and quota value represents the quota value of the product in the current region. Administrators can modify the quota by clicking the Modify button.

### 10.3.3 Modifying Quotas

Administrators can modify the quota values of specified regions on the platform, as shown in the following figure:

![upquato](/assets/images/adminguide/upquato.png)

Modifying the global quota settings does not affect the customized quota settings for tenants. After the global quota is modified, if the number of products that a tenant already has exceeds the set value, it will not affect the normal operation of the resources that the tenant already has. If the tenant deletes the resources, the tenant cannot create resources that exceed the quota value.

## 10.4 Service Catalog

### 10.4.1 Overview

The main function of the service catalog is to provide a unified cloud service viewing entrance for cloud platform administrators. They can quickly view the opening and authorization status of the overall cloud services on the platform, including basic services and advanced services. Authorized services are the services that the platform has opened and authorized through license, and unauthorized services are the services that the platform has not yet authorized. The definitions of basic services and advanced services are as follows:

Basic services: virtual machines, virtual machine templates, elastic network cards, images, disks, external storage, security groups, VPCs, external IPs, NAT gateways, VPN gateways, load balancers, elastic scaling, VIP.

Advanced services: physical machines, external storage.
![servicecata](/assets/images/adminguide/servicecata.png)

### 10.4.1 Closing Services

Authorized services can be closed as a whole. After closing, all users on the platform will not be able to use this service. If there are resources under this service in a tenant, this service cannot be closed. Please make sure to clear all resources under this service for every tenant before closing the service.
> This feature is rarely used. It is usually used when the cloud platform needs to completely offline a cloud service.

### 10.4.2 Starting a Service

A service that has been shut down can be started as a whole. Once it is started, current platform users will be able to use this service normally.

### 10.4.3 Removing Tenants from a Service

For the sake of platform operation, it may be necessary to specify certain cloud services for use by only some tenants. In this case, you can click the "Manage" button on the target service module to enter the service management details page and remove authorized tenants. After the tenant is removed, all accounts under the tenant will not be able to use this service and will be prompted that the service is unauthorized upon login.
![removete](/assets/images/adminguide/removete.png)

### 10.4.4 Adding Tenants to a Service

Authorized tenants who have been removed from the service can be reauthorized by adding them back. After being authorized, all accounts under the tenant will be able to use this cloud service normally.

![addte](/assets/images/adminguide/addte.png)


