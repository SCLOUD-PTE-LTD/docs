---
layout: default
title: Redis
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/redis/
nav_order: 33
---
# 33. Redis

## 33.1 Overview

Redis is a database service provided by the platform that supports both standalone and master-slave configurations, and provides backup, monitoring, and other functions to meet high-performance read/write scenarios.

## 33.2 Creating Redis

Platform users can create Redis by specifying basic information such as cluster, memory capacity, parameter templates, VPC, subnet, public IP, public security group, Redis name/remark, password, project group, etc. They can enter the [Redis] resource control panel through the navigation bar and then access the wizard page by clicking the "**Create**" button, as shown below:

![createRedis](/assets/images/userguide/createRedis1.png)

![createRedis](/assets/images/userguide/createRedis2.png)

1. Select and configure the basic configuration, network settings, and management configuration information of Redis:

* Cluster: The cluster information for creating Redis;
* Memory Capacity: Choose the memory capacity for creating Redis, supporting 1G, 2G, 4G, 8G, 16G, 32G;
* Data disk: Create Redis data information, optional capacity of 10-33000GB;
* VPC and subnet: When creating Redis, you must select the VPC network and the associated subnet, that is, select the network and IP segment to join;
* Public IP: Public IP provides external network access services for Redis. You can apply for and bind one public IP as the external network access address when creating Redis. Redis supports binding up to one public IP;

2. Select the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to create Redis:

- Purchase quantity: Creation of one Redis is supported by default;
- Payment method: Choose the billing method of Redis, supporting monthly, yearly, and hourly payments. You can choose the appropriate payment method based on your needs;
- Total cost: The cost of Redis resources based on the selected payment method is displayed;
- Buy now: After clicking Buy Now, the Redis resource list page will be returned, where you can view the creation process of Redis.

## 33.3 Viewing Redis List

The platform supports users to view Redis list information, including name, resource ID, status, model, IP and port, instance capacity, VPC, subnet, security group, billing method, project group, creation time, expiration time, and operations. You can access it through the navigation bar in the [Redis] resource control panel, as shown below:

![createRedis](/assets/images/userguide/Redislist.png)

* Name: The name of Redis, which can be clicked to enter the Redis details page;
* Resource ID: The resource ID of Redis, which is used as a globally unique identifier;
* Status: The status of Redis, including starting, changing configuration, deleting, etc.;
* Model: The model information of Redis, supporting standalone and master-slave configurations;
* IP and port: The network information of Redis, including internal/external IP and port;
* Instance capacity: The memory capacity information selected when creating Redis, which can be upgraded using the memory upgrade function;
* VPC/subnet: The internal network information of Redis, including the name and resource ID of VPC/subnet;
* Security group: After binding the public IP, Redis supports associating a security group;
* Billing method: The billing method of Redis resources, supporting hourly, monthly, and yearly payments;
* Project group: The security group to which Redis resources belong. You can modify the association relationship through the transfer-in/transfer-out function of the security group;
* Creation time/expiration time: The creation and expiration time of Redis;
* Operations: The available operations for Redis resources, including creating a slave, deleting, renewing, resetting passwords, applying parameter templates, upgrading memory, clearing data, upgrading to a master-slave configuration, modifying alarm templates, associating a public IP, and dissociating a public IP.

## 33.4 Viewing Redis Overview Information

The platform supports users to view the overview information of Redis resources, including basic information, configuration information, and monitoring information. You can click the name in the Redis list to enter the overview page, as shown below:

![createRedis](/assets/images/userguide/Redisoverview.png)

## 33.5 Creating Redis Slave

The platform supports users to create a slave for Redis master, which can select memory capacity, public IP, and security group. The memory capacity cannot be lower than that of the master. You can perform this operation by clicking the "**Create Slave**" button in the operation item of the Redis list, as shown below:

![createRedis](/assets/images/userguide/createRedis3.png)

![createRedis](/assets/images/userguide/fromRedis1.png)

![createRedis](/assets/images/userguide/fromRedis2.png)

Each Redis master supports creating up to 5 slaves.

## 33.6 Renewal of Redis

The platform supports users to renew Redis. You can perform this operation by clicking on the "**Renew**" button in the Redis list, as shown in the following figure:

![createRedis](/assets/images/userguide/Redisrenew.png)

Renewal supports changing the renewal method and duration. Changing the renewal method only supports switching from a shorter cycle to a longer one, for example, from "**hours**" to "**months**".

## 33.7 Redis reset password

The platform supports users to set passwords for Redis, which can be done through the "**Set Password**" button in the operation options of the Redis list, as shown below:

![createRedis](/assets/images/userguide/Redisreset1.png)

![createRedis](/assets/images/userguide/Redisreset2.png)

## 33.8 Parameter Configuration

The platform supports users to perform parameter configuration operations on Redis, including modifying instance parameters, applying parameter templates, importing parameter files, exporting as templates, and exporting parameter files. You can click on the Redis name to enter the details page and switch to the "**Parameter Settings**" page to view it; you can also click the "**Modify Instance Parameters**" button in the Redis list operation item to jump, as shown below:

![createRedis](/assets/images/userguide/Redisconfig1.png)

![createRedis](/assets/images/userguide/Redisconfig2.png)

### 33.8.1 Modify Instance Parameters

The platform supports users to modify instance parameters for Redis. Click the "**Modify Instance Parameters**" button to do so, as shown below:

![createRedis](/assets/images/userguide/editRedis.png)

### 33.8.2 Apply Parameter Templates

The platform supports users to apply parameter templates for Redis. Click the "**Apply Parameter Template**" button to do so, as shown below:

![createRedis](/assets/images/userguide/Redistemplate.png)

### 33.8.3 Import Parameter Files

The platform supports users to import parameter files for Redis. Click the "**Import Parameter File**" button to do so, as shown below:

![createRedis](/assets/images/userguide/Redisfile1.png)

### 33.8.4 Export as Template

The platform supports users to export Redis parameter configurations as templates. Click the "**Export as Template**" button to do so, as shown below:

![createRedis](/assets/images/userguide/Redistemplate3.png)

After the export as template operation is successful, a new template data will be added to the parameter template list.

### 33.8.5 Export Parameter File

The platform supports users to export Redis parameter configurations as parameter files. Click the "**Export Parameter File**" button to download the parameter configuration file.

## 33.9 Upgrade Memory

The platform supports users to upgrade memory for Redis. You can click on the "**Upgrade Memory**" button in the operation options of the Redis list to do so, as shown below:
 
![createRedis](/assets/images/userguide/Redisupgrade.png)

## 33.10 Upgrade to Master/Slave Edition

The platform supports users to upgrade standalone Redis to Master/Slave edition. The computing cluster, memory capacity, storage cluster, and data disk capacity cannot be modified. You can click on the "**Upgrade to Master/Slave Edition**" button in the operation options of the Redis list to do so, as shown below:
 
![createRedis](/assets/images/userguide/upgradeRedismodel.png)

## 33.11 Modify Alarm Template

The platform supports users to modify alarm templates for Redis. You can click on the "**Modify Alarm Template**" button in the operation options of the Redis list to do so, as shown below:

![createRedis](/assets/images/userguide/Redisalarm.png)

## 33.12 Network

The platform supports users to view the network information of Redis, including basic information and IP list. You can click on the Redis name to enter the details page and switch to the "**Network**" page to view it, as shown below:

![createRedis](/assets/images/userguide/Redisnetwork.png)

### 33.12.1 View Network List

The platform supports users to view the network list information of Redis, including IP, IP ID, IP version, status, network type, associated network, whether it is a VIP, bandwidth, bound resource, MAC address, and operation, as shown below:

![createRedis](/assets/images/userguide/Redisnetworklist.png)

### 33.12.2 Bind External IP

The platform supports users to bind external IP for Redis. You can do so by clicking the "**Bind**" button or clicking the "**Bind External IP**" button in the operation options of the Redis list, as shown below:

![createRedis](/assets/images/userguide/Redisbindeip.png)

After the external IP is bound successfully, a new IP data will be added to the Redis network list.

Each Redis instance can bind up to one external IP.

### 33.12.3 Unbind External IP

The platform supports users to unbind external IP for Redis that has been bound. You can do so by clicking the "**Unbind**" button or clicking the "**Unbind External IP**" button in the operation options of the Redis list, as shown below:

![createRedis](/assets/images/userguide/Redisunbindeip.png)

## 33.13 Modify Security Group

The platform supports users to modify the security group of Redis that has been bound with an external network IP. Users can perform this operation through the "**Modify Security Group**" option in the operation items of the Redis list, as shown below:

![createRedis](/assets/images/userguide/editRedissg.png)

## 33.14 Clean up Data

The platform supports users to clean up data of Redis. Users can perform this operation through the "**Clean up Data**" option in the operation items of the Redis list, as shown below:

![createRedis](/assets/images/userguide/cleanRedis.png)

## 33.15 Backup Management

### 33.15.1 Backup Management List

The platform supports users in viewing the backup management list, which includes resource ID, status, storage pool ownership, source backup plan, retention time (days), save path, creation time, update time, expiration time, and operation. Users can click on the Redis name to enter the details page, and switch to the "**Backup Management**" page for viewing, as shown in the figure below:

![createRedis](/assets/images/userguide/Redisdbs.png)

### 33.15.2 Delete Backups

The platform supports users in deleting backup data. Users can click the "**Delete**" button in the operation item of the backup list, or use the "**Batch Delete**" button in the backup list, as shown in the figure below:

![createRedis](/assets/images/userguide/deleteRedisdbs.png)

## 33.16 View Operation Log

The platform supports users in viewing the operation log of Redis and filtering it by operation result and cycle. Users can click on the Redis name to enter the details page, and switch to the "**Operation Log**" page for viewing, as shown in the figure below:

![createRedis](/assets/images/userguide/Redislog.png)

## 33.17 View Events

The platform supports users in viewing Redis events and filtering them by event cycle. Users can click on the Redis name to enter the details page, and switch to the "**Events**" page for viewing, as shown in the figure below:

![createRedis](/assets/images/userguide/Redisevent.png)

## 33.18 Delete Redis

The platform supports users in deleting Redis. Before deleting the master database, the slave database must be deleted first. Users can click the "**Delete**" button in the operation item of the Redis list, as shown in the figure below:

![createRedis](/assets/images/userguide/Redisdelete.png)

## 33.19 Create Parameter Template

The platform supports users in specifying the creation method for creating parameter templates, including copying existing templates and importing template files, as shown in the figures below:

![createRedis](/assets/images/userguide/createRedistemplate1.png)

![createRedis](/assets/images/userguide/createRedistemplate2.png)

## 33.20 Delete Parameter Template

The platform supports users in deleting custom parameter templates. Users can click the "**Delete**" button in the operation item of the parameter template list, as shown in the figure below:

![createRedis](/assets/images/userguide/deleteRedistemplate.png)

## 33.21 Modify IP

The platform supports users in modifying the internal IP (VIP) address of Redis.

![updatefsip](/assets/images/userguide/updateredisip.png)

## 33.22 Create from Backup

The platform supports users in creating Redis from backups.

![createredisfrombak](/assets/images/userguide/createredisfrombak.png)

