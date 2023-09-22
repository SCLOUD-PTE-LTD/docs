---
layout: default
title: File Storage
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/file-storage/
nav_order: 25
---
# 25. File Storage

## 25.1 Overview of File Storage

File Storage is a NFS file server provided by the cloud platform, which can be used in conjunction with virtual machine instances and local servers. File Storage provides the standard NFS file access protocol with a protocol version of NFSv4 and supports the POSIX file interface.

After users create a File Storage instance on the console, they only need to install the File Storage client in the virtual machine instance and use the standard mount command to mount the created file system, making it easy to share files among multiple instances.

## 25.2 Creating File Storage

Cloud platform users can create File Storage by specifying basic information such as the computing cluster, storage cluster, capacity, VPC, subnet, public IP, public security group, project group, and File Storage name.

Navigate to the "File Storage" resource console through the navigation bar and enter the wizard page by clicking on "**Create**", as shown below:

![createFS](/assets/images/userguide/createFS.png)

1. Select and configure the basic configuration, network settings, and management configuration information for the File Storage:

* Name/Remarks: Specify the name and remarks of the requested File Storage; a name must be specified when requesting.
* Capacity: The supported capacity range is 100-1024 GB.
* When creating File Storage, you must select the VPC network and the subnet to which it belongs, that is, the network and IP address range to join.
- Public IP provides external mount services for File Storage. You can apply for and bind a public IP as an external mount address when creating File Storage. The platform supports IPv4/IPv6 dual-stack networks, and you can also bind multiple public IP addresses to File Storage after it is successfully created, supporting up to 50 IPv4 and 10 IPv6 public IP addresses.

2. Choose the purchase quantity and payment method, confirm the order amount, and click "**Buy Now**" to create the File Storage:

- Purchase Quantity: By default, only one File Storage can be created.
- Payment Method: Choose the billing method for File Storage, which supports monthly, yearly, and hourly payment methods. Choose the appropriate payment method according to your needs.
- Total Cost: Displays the cost of the File Storage resource based on the chosen payment method.
- Buy Now: After clicking "Buy Now", you will be taken back to the File Storage resource list page where you can view the creation process of the File Storage. It usually first shows the status of "Initializing", which switches to "Available" status within a few seconds, indicating that the creation was successful.

### 25.2.1 Mounting File Storage through Intranet

Users can mount the File Storage service through the intranet mount address in the File Storage list:

```
mkdir /datanfs
yum install -y nfs-utils
mount -t nfs4 10.0.0.28:/ /datanfs
```

### 25.2.2 Mounting File Storage through Internet

Users can mount the File Storage service through the internet mount address in the File Storage list.

```
mkdir /datanfs
yum install -y nfs-utils
mount -t nfs4 192.168.179.179:/ /datanfs
```

## 25.3 File Storage List

By accessing the file storage console through the navigation bar, you can view the file storage resource list.

The file storage list displays a list of all file storage resources under the current account, including name, resource ID, status, storage capacity, mount address, VPC, subnet, billing method, project group, creation time, expiration time, and operation options, as shown in the following figure:

![FSlist](/assets/images/userguide/FSlist.png)

- Name: The name of the file storage resource.
- Resource ID: The resource ID of the file storage, which serves as a globally unique identifier.
- Status: The status of the file storage resource, including initialization, available, and deleting.
- Storage Capacity: The memory capacity of the file storage, with a range of 100-1024 GB.
- Mount Address: The file storage service can be mounted via the internal network/external network mount address.
- VPC/Subnet: The VPC network and subnet specified when creating the file storage, that is, the VPC network and subnet information where the file storage's internal IP belongs.
- Billing Method: The payment method for the file storage, including hourly, annual, and monthly payments.
- Project Group: The project group bound when creating the file storage.
- Creation Time/Expiration Time: The creation time and fee expiration time of the file storage resource.
- Operation: The options on the list are operations on individual file storages, including expansion, binding, unbinding, renewal, and deletion.

## 25.4 View File Storage Details

Users can click on the name in the list to enter the file storage details page to view basic information and monitoring information. Basic information includes resource ID, file storage name, status, storage capacity, mount address, VPC, subnet, security group, billing type, creation time, and expiration time. Monitoring information includes CPU usage rate, memory usage rate, storage capacity, current storage volume, storage capacity utilization rate, total write throughput, and total read throughput, as shown in the following figure:

![FSdetails](/assets/images/userguide/FSdetails.png)

## 25.5 File Storage Expansion

The platform supports users to expand the capacity of file storage, which is suitable for scenarios where business changes require expanding file storage capacity. The platform only supports expanding the capacity of file storage, not reducing the capacity.

The file storage capacity expansion range is the specification of the current disk type, with a default range of 100GB-1024 GB.

Expanding the capacity of file storage will affect the cost. For hourly payment disks, the new configuration will be charged according to the next payment cycle after the capacity is expanded. For annual or monthly payment disks, the expanded capacity will take effect immediately, and the price difference will be automatically made up proportionally. Users can click on "**Expansion**" in the file storage console operation to perform capacity expansion operations, as shown in the following figure:

![FSstorageup](/assets/images/userguide/FSstorageup.png)

As shown in the figure, "**Change Capacity**" refers to the capacity that the file storage needs to be expanded to. The platform has already displayed the current size of the file storage. Since shrinking is not supported, the changed capacity must be greater than the current capacity when expanding. Users can view the new capacity through the file storage list.

## 25.6 Bind External Network IP

Binding an external network IP means binding the EIP address to the file storage, and users can use the file storage service via the external network mount address.

Users can enter the external network IP binding wizard page by clicking "**Bind**" in the file storage resource list operation options to perform resource binding operations, as shown in the following figure:

![FSlinkeip](/assets/images/userguide/FSlinkeip.png)

When binding, you need to select the EIP to be bound. After the binding is successful, the mount address in the file storage list will add an external network mount address.

## 25.7 Unbind External Network IP

Unbinding external network IP refers to separating the EIP address from a file storage resource and being able to rebind it to other virtual resources. Only unbinding external network IP resources that have been bound to file storage is supported. Users can enter the external network IP unbinding wizard page by clicking on the "**Unbind**" option in the file storage resource list, and perform the resource unbinding operation as shown in the figure below:

![FSunlinkeip](/assets/images/userguide/FSunlinkeip.png)

## 25.8 File Storage Renewal

Users can manually renew their file storage.

When renewing file storage, changing the renewal method is supported, but only from a short period to a long period. For example, the monthly renewal method can be changed to monthly or yearly renewal.

When renewing file storage, fees will be charged based on the renewal length, which matches the billing method of the resource. When the billing method for file storage is "hourly", the renewal period can be specified as 1 hour. When the billing method for file storage is "monthly", the renewal period can be selected from 1 to 11 months. When the billing method for file storage is "yearly", the renewal period is from 1 to 5 years. The operation can be performed through the "**Renew**" option in the file storage list, as shown in the figure below:

![FSrenew](/assets/images/userguide/FSrenew.png)

## 25.9 Modify File Storage Alarm Template

Users can modify the alarm template for file storage in the console. The operation can be performed by clicking on the "**Modify Alarm Template**" button in the file storage list, as shown in the figure below:

![FSmodifyTemplate](/assets/images/userguide/FSmodifyTemplate.png)

## 25.10 Search File Storage

Users can search and filter the file storage list through the search box, which supports fuzzy search from name, remarks, resource ID, and mount address as shown in the figure below:

![searchFS](/assets/images/userguide/searchFS.png)

## 25.11 Modify File Storage Name and Remarks

Modify the name and remarks of the file storage. The operation can be performed by clicking on the "**Edit**" button on the right side of the file storage list name, as shown in the figure below:

![modifyFSname](/assets/images/userguide/modifyFSname.png)

## 25.12 Modify IP

Supports users to modify the internal network IP address of file storage.

![updatefsip](/assets/images/userguide/updatefsip.png)

## 25.13 Create from Backup

Supports users to create a new instance from backup.

![createfsfrombak](/assets/images/userguide/createfsfrombak.png)


## 25.14 Delete File Storage

Users can delete file storage in their account in the console, and supports batch deletion of file storage. The operation can be performed through the "**Delete**" option in the file storage list, as shown in the figure below:

![FSrm](/assets/images/userguide/FSrm.png)

