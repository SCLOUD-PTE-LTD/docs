---
layout: default
title: MySQL Database
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/mysql/
nav_order: 32
---
# 32. MySQL

## 32.1 Overview

MySQL is a database service provided by the platform, supporting two models: standalone and master-slave, and providing functions such as backup, upgrade of models, monitoring, etc.

## 32.2 Creating MySQL

Platform users can create MySQL by specifying basic information such as cluster, memory capacity, data disk, version, parameter template, VPC, subnet, public IP, public security group, MySQL name, password, etc. Users can enter the [MySQL] resource console through the navigation bar and click the "**Create**" button to enter the wizard page, as shown below:

![createmysql](/assets/images/userguide/createmysql1.png)

![createmysql](/assets/images/userguide/createmysql2.png)

1. Select and configure MySQL's basic configuration, network settings, and management configuration information:

* Cluster: The clustering information of creating MySQL, supporting X86 clusters.
* Memory capacity: Choose the memory capacity of creating MySQL, supporting 1G, 2G, 4G, 8G, 16G, and 32G.
* Data disk: Create MySQL's data information, with optional capacity of 10-32000GB.
* VPC and subnet: It is necessary to select the VPC network and subnet to which MySQL belongs when creating MySQL, that is, to select the network and IP segment to join.
* Public IP: The public IP provides external network access service for MySQL, and supports applying and binding one public IP as the external network access address when creating MySQL. MySQL supports binding up to one public IP.

2. Choose the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to create Redis:

- Purchase quantity: By default, only one MySQL can be created.
- Payment method: Select the billing method of MySQL, supporting monthly, yearly, and hourly billing methods. You can choose the appropriate payment method according to your needs.
- Total cost: The cost of MySQL resources according to the billing method selected by the user is displayed.
- Buy now: After clicking buy now, the MySQL resource list page will return, where you can view the process of creating MySQL.

## 32.3 View MySQL List

The platform supports users to view MySQL list information, including name, resource ID, status, model, cluster, storage type, version, IP, memory capacity, data disk capacity, VPC, subnet, security group, billing method, project group, creation time, expiration time, operation. Users can enter the [MySQL] resource console through the navigation bar to view, as shown below:

![createmysql](/assets/images/userguide/mysqllist.png)

* Name: The name of MySQL, which can be clicked to enter the MySQL details page.
* Resource ID: The resource ID of MySQL, which serves as the global unique identifier.
* Status: The status of MySQL, including starting, changing configuration, deleting, etc.
* Model: The model information of MySQL, supporting standalone and master-slave versions.
* Version: The version information of MySQL, currently supporting MySQL 5.7.
* IP: The network information of MySQL, including private IP and public IP.
* Memory capacity: The memory capacity information selected when creating MySQL, which can be upgraded through configuration upgrade function.
* Data disk capacity: The data disk capacity selected when creating MySQL, which can be upgraded through the configuration upgrade function.
* VPC/subnet: The internal network information of MySQL, including the name and resource ID of VPC/subnet.
* Security group: If MySQL is bound with a public IP, it supports associating with a security group.
* Billing method: The billing method of MySQL resources, supporting hourly, monthly, and yearly billing.
* Project group: The security group to which MySQL resources belong, which can be modified by the transfer in/out function of security groups.
* Creation time/expiration time: The creation and expiration time of MySQL.
* Operation: The contents of the operable items of MySQL resources, including creating a slave database, deleting, renewing, resetting the password, applying parameter templates, configuring upgrades, upgrading to the master-slave model, modifying alarm templates, binding public IPs, unbinding public IPs, modifying security groups, modifying IPs, etc.

## 32.4 View MySQL Overview Information

The platform supports users to view MySQL resource overview information, including basic information, configuration information, and monitoring information. You can enter the overview page by clicking on the name in the MySQL list, as shown below:

![createmysql](/assets/images/userguide/mysqloverview.png)

## 32.5 Create MySQL Slave

The platform supports users to create a slave for a MySQL master, which includes selecting memory capacity, data disk capacity, public IP, and security group. You can use the "**Create Slave**" button in the operation column of the MySQL list, as shown below:

![createmysql](/assets/images/userguide/createmysql3.png)

![createmysql](/assets/images/userguide/frommysql1.png)

Each MySQL master can create up to 5 slaves.

## 32.6 Renew MySQL

The platform supports users to renew their MySQL instances. You can use the "**Renew**" button in the operation column of the MySQL list, as shown below:

![createmysql](/assets/images/userguide/mysqlrenew.png)

You can change the renewal method and duration. Changing the renewal method only supports changing from a shorter cycle to a longer one, such as changing from "**hours**" to "**months**".

## 32.7 Reset MySQL Password
 
The platform supports users to reset their MySQL password. You can use the "**Reset Password**" button in the operation column of the MySQL list, as shown below:

![createmysql](/assets/images/userguide/mysqlreset.png)

## 32.8 Parameter Configuration

The platform supports users to configure MySQL parameters, including modifying instance parameters, applying parameter templates, importing parameter files, exporting to templates, and exporting parameter files. You can click on the MySQL name to enter the details page and switch to the "**Parameter Settings**" page to view the settings, as shown below:

![createmysql](/assets/images/userguide/mysqlconfig.png)

### 32.8.1 Modify Instance Parameters

The platform supports users to modify instance parameters for MySQL. You can click the "**Modify Instance Parameters**" button to perform this operation, as shown below:

![createmysql](/assets/images/userguide/editmysql.png)

### 32.8.2 Apply Parameter Templates

The platform supports users to apply parameter templates for MySQL. You can click the "**Apply Parameter Template**" button to perform this operation, or use the "**Apply Parameter Template**" button in the operation column of the MySQL list, as shown below:

![createmysql](/assets/images/userguide/mysqltemplate.png)

### 32.8.3 Import Parameter Files

The platform supports users to import parameter files for MySQL. You can click the "**Import Parameter File**" button to perform this operation, as shown below:

![createmysql](/assets/images/userguide/mysqlfile1.png)

### 32.8.4 Export to Templates

The platform supports users to export their MySQL parameter settings as a template. You can click the "**Export to Template**" button to perform this operation, as shown below:

![createmysql](/assets/images/userguide/mysqltemplate3.png)

After successfully exporting to a template, a new template record will be added to the parameter template list.

### 32.8.5 Export Parameter File

The platform supports users to export MySQL parameter configuration files. Click "**Export Parameter File**" to download the parameter configuration file.

## 32.9 Configuration Upgrade

The platform supports users to perform configuration upgrades on MySQL, including changes in memory capacity and data disk capacity. Click the "**Configuration Upgrade**" button in the operation item of the MySQL list to perform the operation, as shown in the figure below:
 
![createmysql](/assets/images/userguide/mysqlupgrade.png)

## 32.10 Upgrade Master-Slave Version

The platform supports users to upgrade the standalone version of MySQL to the master-slave version. The computational cluster, memory capacity, storage cluster, and data disk capacity cannot be modified. Click the "**Upgrade to Master-Slave Version**" button in the operation item of the MySQL list to perform the operation, as shown in the figure below:
 
![createmysql](/assets/images/userguide/upgrademysqlmodel.png)

## 32.11 Modify Alarm Template

The platform supports users to modify alarm templates for MySQL. Click the "**Modify Alarm Template**" button in the operation item of the MySQL list to perform the operation, as shown in the figure below:

![createmysql](/assets/images/userguide/mysqlalarm.png)

## 32.12 Network

The platform supports users to view the network information of MySQL, including basic information and IP list. Click the MySQL name to enter the details page, and switch to the "**Network**" page to view, as shown in the figure below:

![createmysql](/assets/images/userguide/mysqlnetwork.png)

### 32.12.1 View Network List

The platform supports users to view the network list information of MySQL, including IP, IP ID, IP version, status, network type, belonging network, whether VIP, bandwidth, binding resources, MAC address, and operation, as shown in the figure below:

![createmysql](/assets/images/userguide/mysqlnetworklist.png)

### 32.12.2 Bind External Network IP

The platform supports users to bind external network IP to MySQL, which can be operated through the "**Bind**" button or the "**Bind External Network IP**" button in the operation item of the MySQL list, as shown in the figure below:

![createmysql](/assets/images/userguide/mysqlbindeip.png)

After the external network IP is successfully bound, a new IP data will be added to the MySQL network list.

Each MySQL can bind up to one external network IP at most.

### 32.12.3 Unbind External Network IP

The platform supports users to unbind the external network IP of MySQL that has been bound. This can be done by clicking the "**Unbind**" button or the "**Unbind External Network IP**" button in the operation item of the MySQL list, as shown in the figure below:

![createmysql](/assets/images/userguide/mysqlunbindeip.png)

## 32.13 Modify Security Group

The platform supports users to modify the security group of MySQL with an external network IP bound. This can be done by clicking the "**Modify Security Group**" button in the operation item of the MySQL list, as shown in the figure below:

![createmysql](/assets/images/userguide/editmysqlsg.png)

## 32.14 Backup Management

### 32.14.1 View Backup Management List

The platform supports users to view the backup management list information, including resource ID, status, associated storage pool, source backup plan, retention period (days), save path, creation time, update time, expiration time, and operation. Click on the MySQL name to enter the details page and switch to the "**Backup Management**" page for viewing, as shown below:

![mysqlbakview](/assets/images/userguide/mysqlbakview1.png)

![mysqlbakview](/assets/images/userguide/mysqlbakview2.png)

![mysqlbakview](/assets/images/userguide/mysqlbakview3.png)

### 32.14.2 Delete Backup

The platform supports users to delete backup data. You can click the "**Delete**" button in the operation column of the backup list to perform the operation, or use the "**Backup Delete**" button in the backup list to perform the operation, as shown below:

![createmysql](/assets/images/userguide/deletemysqldbs.png)

### 32.14.3 Create from Backup

The platform supports users to create MySQL from backup.

![createmysqlfrombak](/assets/images/userguide/createmysqlfrombak.png)

## 32.15 View Operation Logs

The platform supports users to view the operation logs of MySQL, and filtering can be done based on the operation result and cycle. Click on the MySQL name to enter the details page and switch to the "**Operation Log**" page for viewing, as shown below:

![createmysql](/assets/images/userguide/mysqllog.png)

## 32.16 View Events

The platform supports users to view the events of MySQL, and filtering can be done based on event cycle. Click on the MySQL name to enter the details page and switch to the "**Events**" page for viewing, as shown below:

![createmysql](/assets/images/userguide/mysqlevent.png)

## 32.17 Delete MySQL

The platform supports users to delete MySQL. Before deleting the master database, the slave database must be deleted first. You can click the "**Delete**" button in the operation column of the MySQL list to perform the operation, as shown below:

![createmysql](/assets/images/userguide/mysqldelete.png)

## 32.18 Create Parameter Templates

The platform supports users to specify the creation method to create parameter templates, including copying existing templates and importing template files, as shown below:

![createmysql](/assets/images/userguide/createmysqltemplate1.png)

![createmysql](/assets/images/userguide/createmysqltemplate2.png)

## 32.19 Apply to Instance

The platform supports users to apply parameter templates to instances. You can select the MySQL resources to apply to, and click the "**Apply to Instance**" button in the operation column of the parameter template for operation, as shown below:

![createmysql](/assets/images/userguide/usemysqltemplate.png)

## 32.20 Download Parameter Templates

The platform supports users to download parameter templates. Click the "Download" button in the operation column of the parameter template.

## 32.21 Delete Parameter Templates

The platform supports users to delete custom parameter templates. You can click the "**Delete**" button in the operation column of the template for operation, as shown below:

![createmysql](/assets/images/userguide/deletemysqltemplate.png)

## 32.22 Modify IP

The platform supports users to modify the private network IP (VIP) address of MySQL.

![updatefsip](/assets/images/userguide/updatemysqlip.png)
