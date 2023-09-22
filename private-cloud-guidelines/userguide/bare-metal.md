---
layout: default
title: Bare Metals
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/bare-metals/
nav_order: 17
---
# 17. Bare Metals

## 17.1 Product Introduction

### 17.1.1 Overview

Bare Metal provides users with a unified management of existing bare metal capabilities, allowing users to manage power, access remote consoles, and view hardware monitoring from the console.

### 17.1.2 Usage Process

Before using bare metal services, the bare metal equipment must be prepared in advance, and the IPMI network and business network of the bare metal server must be connected with the platform network according to the requirements. After entering the device information through the platform, the device will be added to the tenant management. The use process of bare metal service is divided into two parts: [Platform Administrator Process] and [Tenant Process], as follows:

1. **Hardware Environment Equipment**

   Prepare the hardware environment, configure the physical network switch and server IPMI network, so that the platform physical network and IPMI network can communicate with each other.

2. **Add bare metal for tenants**

   The platform administrator can add bare metal information for tenants, including server name, IPMI IP, IPMI User, IPMI Password, rack location, tags, and tenant email, which supports batch import of bare metal information.

3. **Bare Metal Management**

   The platform tenant will manage the lifecycle of the bare metal applied for, supporting operations such as start, stop, restart, prepare console, console login, release console, installation, reinstallation, forced shutdown, and shutdown and restart.

Platform tenants are able to use bare metal services provided that the bare metal is ready and added to the tenant, where the tenant can access the power management, remote console, and hardware monitoring of the bare metal from the console.

## 17.2 Add Bare Metal

When bare metal services are provided on the platform, the main account and sub-accounts of the tenant can be added or imported in batches directly on the platform for management. Users can log in to the console, enter the wizard page through the "Bare Metal" in the console navigation bar, and add bare metal through the "Add Bare Metal" operation, as shown in the following figure:

![applybms](/assets/images/userguide/applybms.png)

* Tenant email: Bare metal can only be added by tenant administrators, specifying the required users.
* Name: The name identifier of the bare metal on the platform, which must be specified when adding.
* Rack Position: The rack position where the bare metal is located, must be specified when adding. Driver Type: Default IPMI.
* IPMI IP: refers to the bare metal IPMI IP address, which must be specified when adding, and the IP address must be reachable from the platform.
* IPMI User: The bare metal IPMI username must be specified when adding.
* IPMI Password: The bare metal IPMI password must be specified when adding.
* Label: Label for Bare Metal.
* Monitoring address: node_exporter address of physical machine supporting add-on management to obtain monitoring information.

After the tenant adds and submits, a bare metal information with the status of "Resource Preparing" will be generated in the list. After the addition is successful, it will be set to "Running" status, and the tenant can manage the bare metal at this time.

## 17.3 View Bare Metal

The tenant's administrator can view the added bare metal information and current status under the account through the bare metal list and details, as shown in the following figure:

![bmslist](/assets/images/userguide/bmslist.png)

* Name/Resource ID: Name of Bare Metal and Global Resource Unique Identifier.
* SN: Bare Metal Serial Number.
* IPMI IP: The bare metal IP address.
* Model: Bare metal model.
* Rack Position: The rack position where the bare metal is located.
* Power: Bare metal power state, such as on, in shutdown, off, etc.
* KVM Status: Status of the bare metal console, such as not ready, preparing, ready, etc.
* Creation time: time of addition of bare metal.
* Status: The current status of the bare metal resource, such as running, resource preparation, etc., when the bare metal is added for the first time, the status is managed.

On the list of operations, you can turn on, turn off, restart, VNC login, prepare console, console login, release console, force shutdown, shutdown and restart, update bare metal and delete, etc. It supports batch deletion of bare metal, and after deletion, tenants can add again.

Tenants can also access the bare metal's detail page by clicking on the bare metal's name on the list, to view the details of the bare metal, including basic information and configuration information.

* The basic information includes: bare metal name, ID, IPMI IP, SN serial number and power status.
* Configuration information includes: bare metal CPU information, memory information, and PCIE information, etc.

## 17.4 Allocate Bare Metal

Tenant administrators can allocate the bare metal already on the shelf, as shown in the following figure:

![bmslist](/assets/images/userguide/pm1.png)

## 17.5 Bare Metal Installation

Tenants, the administrator can install the bare metal that has been put on the shelf, as shown in the following figure:

![bmslist](/assets/images/userguide/pm2.png)


> The image format used for installation is qcow2, which needs to support the cloud-init feature.
## 17.5 Bare Metal Boot/Shutdown

The platform supports tenants to perform shutdown, start, restart, force shutdown, and shutdown and restart operations on the added bare metal, which is used to maintain and manage the life cycle of the bare metal. When the power status is on, shutdown, restart, force shutdown, and shutdown and restart operations can be performed; when the power status is off, only start operation can be performed.


During the bare metal booting process, the power status of the bare metal is in the booting process, and it will switch to "on" after the booting is successful; when the user triggers the shutdown, the state of the bare metal is in the shutdown process, and it will switch to "off" after the shutdown is successful.


## 17.6 Bare Metal Console Operation

Tenants can perform basic operations such as power management, remote console access, and hardware monitoring on the managed bare metal in the console. When the bare metal KVM status is not ready, the preparation console operation needs to be performed, and after the operation is completed, the KVM status will be ready; when the bare metal KVM status is ready, the console login and release console are supported.

