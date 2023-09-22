---
layout: default
title: Object Storage
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/object-storage/
nav_order: 26
---
# 26. Object Storage

## 26.1 Overview of Object Storage

Object Storage Service (OSS) is compatible with Amazon Cloud's S3 API protocol. By creating an object storage instance on a private cloud platform, any type of data can be stored and accessed at any application, any time, and any location. Object storage is designed for cloud-native systems and can efficiently utilize CPU and memory resources even under high loads, making it suitable for private cloud scenarios.

## 26.2 Creating Object Storage

Cloud platform users can create object storage by specifying basic information such as compute clusters, storage clusters, capacity, VPC, subnet, public IP, public security group, project group, and object storage name.

Enter the "Object Storage" resource console through the navigation bar, and enter the wizard page through "**Create**", as shown below:

![createoss](/assets/images/userguide/createoss.png)

1. Select and configure the basic configuration, network settings, and management configuration information of object storage:

* Name/Remarks: The name and remarks of the requested object storage must be specified during the application;
* Capacity: Supports a capacity range of 100-1024 GB;
* When creating object storage, you must select the VPC network and the associated subnet, which is to choose the network and IP address segment to join;
- The public IP provides external network access services for object storage, and supports applying for and binding a public IP as an external network access address when creating object storage. The platform supports IPv4/IPv6 dual-stack networks, and multiple public IP addresses can be bound to the object storage after it is created. It supports up to 50 IPv4 and 10 IPv6 public IP addresses.

2. Select the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to create the object storage:

- Purchase Quantity: By default, only one object storage can be created;
- Payment Method: Choose the billing method for the object storage, which supports monthly, yearly, and hourly billing methods. You can choose a suitable payment method according to your needs;
- Total Cost: The cost of the object storage resource is displayed according to the selected payment method;
- Buy Now: After clicking "Buy Now", the object storage resource list page will be returned. You can check the creation process of object storage on the list page. Generally, it will first display the "Initializing" status, and within a few seconds it will change to the "Available" status, indicating that the creation is successful.

### 26.2.1 Configuring Object Storage through Intranet

Users can configure object storage services through the intranet address of the object storage list:

```
wget -c https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
./mc alias set Name http://intranetip:9000 admin password
```

### 26.2.2 Configure Object Storage through Public Network

Users can configure object storage services via the public network address in the object storage list:

```
wget -c https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
./mc alias set Name http://Public_IP:9000 admin password
```

## 26.3 Object Storage List

By accessing the object storage console through the navigation bar, users can view the object storage resource list.

The object storage list displays a list of all object storage resources under the current account, including name, resource ID, status, storage capacity, domain name, VPC, subnet, billing method, project group, creation time, expiration time, and operation items, as shown in the figure below:

![osslist](/assets/images/userguide/osslist.png)

- Name: name of the object storage resource;
- Resource ID: the resource ID of the object storage as a globally unique identifier;
- Status: the status of the object storage resource, including initializing, available, deleting, and other states;
- Storage capacity: memory capacity of the object storage, with a capacity range of 100~1024 GB;
- Domain name: object storage service can be configured by accessing the address through internal or external networks;
- VPC/Subnet: the VPC network and subnet specified when creating the object storage, which contains information about the VPC network and subnet where the object storage's internal IP is located;
- Billing method: the payment method of the object storage, including hourly, yearly, and monthly;
- Project group: the project group bound when creating the object storage;
- Creation time / Expiration time: the creation time and expiration time of the object storage resource;
- Operation: the operation item on the list is the operation for a single object storage, including expansion, binding, unbinding, renewal, and deletion.

## 26.4 Expand Object Storage Capacity

The platform supports users in expanding object storage capacity to meet the needs of business growth that require an increase in object storage capacity. The platform only supports expanding object storage capacity and does not support reducing object storage capacity.

The expansion range of object storage capacity is the specification of the current hard disk type, which is by default 100GB~1024 GB.

Expanding object storage capacity will affect the cost. For hourly-billed hard disks, the expanded capacity will be charged based on the new configuration in the next billing cycle; for yearly or monthly billed hard disks, the expanded capacity will take effect immediately and the price difference will be automatically supplemented proportionally. Users can click "**Expand**" in the object storage console operation to expand the capacity, as shown in the figure below:

![ossStorageup](/assets/images/userguide/ossStorageup.png)

As shown in the figure, "**Change Capacity**" refers to the capacity that the object storage needs to expand to. The platform has displayed the current capacity of the object storage. Since it does not support reducing capacity, the new capacity must be greater than the current capacity when expanding. Users can view the new capacity through the object storage list.

## 26.5 Bind External IP

Binding an external IP refers to binding an EIP address to object storage, allowing users to access object storage services through the internet.

Users can enter the external IP binding wizard page by selecting "**Bind**" in the operation menu of the object storage resource list, as shown below:

![osslinkeip](/assets/images/userguide/osslinkeip.png)

When binding, users need to select the EIP to be bound. After successful binding, the access address of the object storage list will have a new external access address.

## 26.6 Unbind External IP

Unbinding an external IP refers to separating an EIP address from an object storage resource and being able to bind it to other virtual resources. Only unbinding external IP resources that are already bound to object storage is supported. Users can enter the external IP unbinding wizard page by selecting "**Unbind**" in the operation menu of the object storage resource list, as shown below:

![ossunlinkeip](/assets/images/userguide/ossunlinkeip.png)

## 26.7 Object Storage Renewal

Users can manually renew object storage.

When renewing object storage, users can change the renewal method, but only from a shorter period to a longer period. For example, the monthly renewal method can be changed to monthly or yearly.

When renewing object storage, fees will be charged according to the length of the renewal period, which must match the billing method of the resource. When the billing method of the object storage is "hourly", the renewal period is specified as one hour. When the billing method of the object storage is "monthly", the renewal period can be selected between 1 and 11 months. When the billing method of the object storage is "yearly", the renewal period is 1 to 5 years. You can perform this operation by selecting "**Renew**" in the operation menu of the object storage list, as shown below:

![ossrenew](/assets/images/userguide/ossrenew.png)

## 26.8 Reset Password

Users can reset the password of the object storage by selecting "**Reset Password**" in the operation menu of the object storage list, as shown below:

![ossreset](/assets/images/userguide/ossreset.png)

## 26.9 Delete Object Storage

Users can delete object storage in their account on the console and batch delete object storage is also supported. You can perform this operation by selecting "**Delete**" in the operation menu of the object storage list, as shown below:

![ossrm](/assets/images/userguide/ossrm.png)

Users can reset the password from the client-side using the command line tool.

```
./mc alias set NAME http://external-ip:9000 admin new_password
```

## 26.10 Search object storage

Users can search and filter the object storage list through the search box, supporting fuzzy search by name, remarks, resource ID, domain name, as shown in the figure below:

![searchoss](/assets/images/userguide/searchoss.png)

## 26.11 Modify object storage name and remarks

Modify the name and remarks of the object storage. You can modify it by clicking the "**Edit**" button on the right side of the object storage list name, as shown in the figure below:

![modifyossname](/assets/images/userguide/modifyossname.png)

## 26.12 Object storage monitoring page

Users can click on the "**Name**" in the operation item of the object storage list to enter the monitoring page of the object storage, and can also use the "**Modify Alarm Template**" in the operation item to set alarm for the monitoring data.

![ossmonitor01](/assets/images/userguide/ossmonitor01.png)
![ossmonitor02](/assets/images/userguide/ossmonitor02.png)

## 26.13 Bind and unbind security groups for public object storage

Users can set network security policies for public object storage by clicking on "**Modify Public Network Security Group**" in the operation item of the object storage list.

![osssg](/assets/images/userguide/osssg.png)

## 26.14 Modify IP

Support users to modify the internal IP address of the object storage.

![updateossip](/assets/images/userguide/updateossip.png)

## 26.15 Create from backup

Users can create object storage from backup.

![createossfrombak](/assets/images/userguide/createossfrombak.png)

## 26.16 Common commands of MinIO Client
```

mc version                          Display the version of mc

mc ls play                          List all buckets on https://play.min.io

mc mb play/mybucket                 Create a bucket named "mybucket" on https://play.min.io

mc cat play/mybucket/myobject.txt   Display the contents of the file myobject.txt

mc cp myobject.txt play/mybucket    Copy a text file to object storage

mc rm play/mybucket/myobject.txt    Delete an object

mc find s3/bucket --name "*.jpg" --watch --exec "mc cp {} play/bucket"    Continuously search for all JPEG images in the s3 bucket and copy them to the minio "play/bucket" bucket

```

