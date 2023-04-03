---
layout: default
title: Platform Overview
parent: Admin Guide
grand_parent: Private Cloud
permalink: /private-cloud/admin-guide/platform-overview/
nav_order: 1
---
# Overview
## Platform Introduction
The cloud platform unified management service provides enterprise users with two sets of consoles from a tenant perspective and a management perspective. The tenant console is used for platform tenant virtual resource management, and the management console is used for platform managers to operate and maintain the cloud platform as a whole.

The tenant console is a Web cloud resource management platform provided for tenants (master/sub-accounts), which supports tenant master/sub-account management and authority control. Resource management such as monitoring and alarming, metering and billing, and log auditing.

The management console is a web operation and maintenance management platform provided for platform managers and operation and maintenance personnel. It uses an administrator account to manage the entire cloud platform in a unified manner, and has all the management rights of the platform, including full platform account authentication, multi-tenant management, resource management, Management function modules such as process approval, metering and billing, monitoring and alarming, audit log, platform management and operation and maintenance migration, etc., also provide large-screen monitoring services, support free layout and flexible screen projection, and improve the data visualization effect of platform resources.

## Basic concepts
The cloud platform is deployed as a software platform to physical server nodes, and finally provides virtualized products in the form of cloud services. The management console supports the management of computing, storage and network resources at the data center level, and can logically divide the data center into multiple clusters, and manage the cloud resources provided by the platform through the unified account authentication system. The basic concepts involved in the use and operation of the platform are as follows:

### Area

Region (Region) is a logical concept in the cloud platform, which refers to the physical location classification of resource deployment, which can correspond to cabinets, computer rooms, or data centers.

Usually, a data center corresponds to a set of SCloudStack cloud platform, and resources and networks between data centers are completely physically isolated, and a set of management platforms can be used to manage private cloud platforms distributed across data centers.

### Cluster

Cluster (Set) is a logical division of platform physical resources, used to distinguish service nodes with different configuration specifications and different storage types, such as X86 computing clusters, ARM computing clusters, SSD storage clusters or commercial storage clusters.

A data center can support the deployment of multiple computing and storage clusters. A cluster usually consists of a group of physical nodes with the same configuration and purpose. Users can deploy virtual resources in different computing clusters and mount distributed block storage across clusters. equipment.

### Administrator account

The administrator admin account is an administrator console account, which has all management rights of the platform and is used for global management and operation of the entire cloud platform. The region, cluster, tenant, resource, billing, approval, security, and global configuration of the platform can be managed through the administrator account; at the same time, the administrator account itself can be safely managed.

### Tenant:

The platform supports multi-tenant mode, which is used for enterprises with multi-level organizational structure. It can operate tenants as a separate company, effectively realize authority management, and reduce the risks that may be caused by the mixed use of resources of the head office, subsidiaries and different departments. And resource auditing can be realized.

A tenant is a collection of resources on the platform, providing capabilities such as resource isolation, sub-accounts, permission control, quota and price configuration. Resources between different tenants are strongly isolated through the VPC network and permissions; the resources, fees, quotas, and approvals of all primary accounts and sub-accounts in a tenant belong to the tenant.

### Main account

A tenant must have a master account, and the master account has all the resources and management permissions under the tenant by default. You can create and manage sub-accounts through the main account, and manage the permissions of sub-accounts.

### Sub-account

A sub-account is a user created by the main account, and the permissions of the sub-account under the tenant are controlled by the main account. A tenant can have multiple sub-accounts, which supports resource management permission control for sub-accounts.

### External storage

The platform provides distributed storage by default. External storage refers to the commercial storage resource pool (such as `IPSAN`) connected to the platform through the `ISCSI` protocol. It supports one-click scanning of LUN storage volume information in commercial storage, and can use the storage volume as a virtual machine system. disk and data disk.

### External network segment

The external network segment is the network for external communication of the platform, which is generally allocated and configured to the cloud platform by the administrator or operation and maintenance personnel through the physical network. 

The external network segment is the IP resource pool where the platform allocates external network IP (elastic EIP) to tenants. It supports both IPv4 and IPv6 IP types, and supports configuration of network segment routes and automatically sends routes to platform virtual machines.