---
layout: default
title: ULightHost
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/ulhost/
nav_order: 1
---
# ULightHost - Cloud Host
## Introduction
ULightHost is a lightweight cloud server product that provides convenient and efficient cloud construction services for small and medium-sized enterprises and developers. 

It is simpler and easier to use than ordinary cloud servers. It sells cloud resources as a whole in the form of packages, and provides high-bandwidth traffic packages. 

Users can build applications with one click without complex configuration process, making cloud access easier.

![1](/assets/images/public-cloud-user-guides/guide-22.png)

## Product Description
### Product advantages
Using lightweight application cloud hosts has the following advantages:
- Streamlined development: Offer comprehensive packages that bundle computing, storage, and network resources together, streamlining the complexity of advanced cloud computing features and the development of application systems.
- Widely used software: Pre-configure software combinations needed for application systems like websites and blogs, allowing for immediate use.
- Economical benefits: Deliver cost-effective high-bandwidth traffic packages that include computing, storage, and network resources.

### Differences from cloud hosting products

| Compare projects | ULightHost | UHost |
| --- | --- | --- |
| The product is suitable for groups | Individual developers, cross-border e-commerce, small and medium-sized enterprises | Medium and large enterprises |
| Business scenarios | + Cross-border store management, overseas independent website<br/>  + Blogs, forums, information and other websites<br/>  + Cloud disk development and testing environment, learning environment<br/> | + Concurrent access to high websites <br/> + Big data <br/>  + Large games <br/> + Medium and heavy duty business <br/> |
| Usage experience |  + Independent simplified console<br/>  + Rich application images, out-of-the-box<br/>  + Network/disk and other automated configurations<br/> |  There are many high-level concepts of cloud services, including VPC, EIP, and security groups<br/>|
| Billing model | Combined compute/network/storage plan model | Configure compute/network/storage resources and overlay billing separately |

## Basic concept
### Host
The computing resources provided by the lightweight application cloud host are the same as those of the SCloud cloud host. The lightweight application cloud host is usually suitable for supporting small websites, blogs, forums, cloud development/testing/learning environments, etc. Application scenarios.

### Mirror image
An image is a template for starting and running a lightweight application cloud host. The base image includes the operating system, and the application image includes the basic operating system and pre-installed software.

Currently, the lightweight application cloud host provides two image types: basic image and application image:

| | Application image | Base image |
| --- | --- | --- |
| Image description | The application image contains the basic operating system, pre-installed some application software (such as LAMP, WordPress, etc.) <br/>And the running environment and related initialization configuration files that the application depends on | The base mirror only contains the initial operating system (such as CentOS, Ubuntu, Windows, etc.). |
| Applicable scenarios | Deploy apps quickly out of the box without manually installing apps | Users install responsive applications on demand |
| Include images |  + LAMP<br/> + WordPress <br/> + Pagoda Linux Edition | <br/> + CentOS<br/> + Ubuntu<br/> + Windows<br/> + RedHat <br/> + Rocky <br/> + Contains all the base images available on the cloud hosting side 

### Network
The network service provided by the lightweight application cloud host is based on the SCloud network VPC service. After each lightweight application cloud host is created, it is assigned an intranet IP by default, which can be used for communication between different lightweight application cloud hosts. 

Under the same account, multiple lightweight application cloud hosts in the same region are in the same VPC by default, and lightweight application cloud hosts in different regions are in different VPCs. 

At the same time, each lightweight application cloud host is assigned an independent public network IP by default after creation, and is configured with exclusive public network bandwidth, which can be used for Internet public network access.

### Storage
The lightweight application cloud host product package includes a certain capacity of system disk storage space, which uses SCloud RSSD cloud disk. RSSD cloud disk is a high-availability, high-reliability, low-cost, and customizable block storage device. The bottom layer is based on all NVMe SSD storage media and adopts a three-copy distributed mechanism to provide low latency, high random IOPS and high throughput I/O capacity.

The life cycle of the system disk completely follows that of the lightweight application cloud host. It is purchased together with the lightweight application cloud host and used as a system disk. Mounting and unmounting are not supported.

## Product package
The lightweight application cloud host provides different combinations of computing, storage and network resources, and users can choose matching product packages based on business needs.

Supported 15 regions: Manila, Los Angeles, Singapore, Seoul, Tokyo, Ho Chi Minh City, Jakarta, Mumbai, Washington, London, Dubai, Sao Paulo, Bangkok, Taipei, Hongkong.

Currently SCloud lightweight application cloud host provides the following packages to choose from:

| Package | CPU | Memory | System Disk - RSSD | Peak bandwidth | Traffic packets | Price |
| --- | --- | --- | --- | --- | --- | --- |
| Package 1 | 1 Core | 1 GB | 40 GB | 30 Mbps | 200 GB | 4,76$ per month|
| Package 2 | 1 Core | 1 GB | 40 GB | 30 Mbps | 1024 GB | 6,41$ per month|
| Package 3 | 1 Core | 2 GB | 40 GB | 30 Mbps | 400 GB | 6,44$ per month|
| Package 4 | 1 Core | 2 GB | 40 GB | 30 Mbps | 1024 GB | 8,08$ per month|
| Package 5 | 2 Core | 2 GB | 60 GB | 30 Mbps | 600 GB | 8,12$ per month|
| Package 6 | 2 Core | 4 GB | 80 GB | 30 Mbps | 800 GB | 9,66$ per month|
| Package 7 | 2 Core | 2 GB | 60 GB | 30 Mbps | 2048 GB | 10,08$ per month|
| Package 8 | 2 Core | 4 GB | 80 GB | 30 Mbps | 3072 GB | 14,84$ per month|
| Package 9 | 4 Core | 4 GB | 100 GB | 30 Mbps | 3072 GB | 30,64$ per month|

```
- The network traffic package corresponding to the package is the upper limit of the maximum traffic package. After the traffic package of the lightweight application cloud host reaches the upper limit, the traffic will be billed according to the regular network bandwidth traffic billing method.

- Creating a lightweight application cloud host does not support specifying the CPU model of the underlying physical server, and SCloud will randomly allocate a physical CPU model that meets the package specifications

- The package specifications sold in different regions are slightly different, please refer to the information on the actual purchase console

- Package changes are currently not supported
```

## Billing Overview
Cloud host billing for lightweight applications currently involves only two parts: the basic package and the excess traffic outside the package

### Basic package
The product package in the lightweight application cloud host includes cloud server resources such as CPU, memory, system disk, and network traffic. For details of the price of each package, please refer to the relevant documents of [Product Package].

### Excess traffic outside the package
When the actual traffic usage exceeds the monthly traffic package limit of the basic package, the lightweight application cloud host will be charged according to the traffic, that is, the total amount of data transmitted over the public network (in GB).

| Region | Price (USD/GB) |
| --- | --- |
| Oversea | 0,098 |

### Arrears and Service Suspension Instructions
When your lightweight application cloud host fails to renew successfully, the resource will enter the state of arrears and suspension of service. 

Early warning and recovery strategy:

| Time node | Form of warning |
| --- | --- |
| Resource renewal reminders | The resource expiration warning is sent 7 days before, 3 days before, and 1 day before the expiration date |
| The day the resource expires | Send resource expiration reminders on the day the resource expires |
| 1 day after resource expiration | Warning about service termination |
| 2 days after resource expiration | By sending a discontinuation notification, the customer will not be able to use the resource |
| 5 days after the resource is stopped | Send recycling alert notifications |
| 6 days after the resource is stopped | Send a recycling notification, and the SCloud backend deletes the relevant data |

### Delete Refund Instructions
When you delete the purchased lightweight application cloud host, the fee will be refunded according to the following rules:
1. Refund amount = payment amount - [used time × (payment amount/purchase cycle) × 1.5];
2. The accuracy of the used time is up to one day, and if it is less than one day, it will be counted as one day;

```
- Example 1: Assume that you purchased a one-month light application cloud host with a package amount of 34 Credit, and you will be refunded for deleting resources after 9.5 days of use
Refund amount = 34 - [10×(34/30)×1.5]= 14 Credit (1,96$)
- Example 2: Assume that you purchased a 2-month light application cloud host, the package amount is 34×2=68 Credit, and you will be refunded if you delete the resource after 9.5 days of use
Refund amount = 68 - [10×(68/60)×1.5]= 51 Credit (7,14$)
```
