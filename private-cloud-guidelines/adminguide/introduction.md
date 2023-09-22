---
layout: default
title: Platform Overview
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/admin-guides/introduction/
nav_order: 1
---
# 1. Platform Overview

## 1.1 Platform Introduction

The cloud platform provides two sets of consoles for enterprise users: a tenant console and a management console. The tenant console is used for managing virtual resources of the platform tenants, while the management console is used for overall operation and maintenance of the cloud platform by platform administrators.

The tenant console is a web-based cloud resource management platform provided for tenants (main/sub-accounts) to manage virtual resources of all product services throughout their lifecycle. It supports tenant main/sub-account management and permission control, as well as monitoring, alerting, metering, billing, and log auditing of virtual resources.

The management console is a web-based operations and maintenance management platform for platform administrators and operations personnel. It provides unified administration of the entire cloud platform, with platform-wide account authentication, multi-tenant management, resource management, process approval, measurement and billing, monitoring and alerting, audit logging, platform management, and operations migration capabilities. It also provides large-screen monitoring services that support flexible screen layout and projection, enhancing the data visualization effect of platform resources.

## 1.2 Basic Concepts

As a software platform deployed on physical server nodes, a cloud platform ultimately provides virtualization products in the form of cloud services. The management console supports the management of computing, storage, and network resources at the data center level, and can logically divide the data center into multiple clusters to manage the cloud resources provided by the platform through a unified account authentication system. The following are some basic concepts related to the use and operation of the platform:

* **Region**

  A region is a logical concept in the cloud platform that refers to the classification of the physical location where resources are deployed, corresponding to cabinets, rooms, or data centers.

  Usually, a data center corresponds to a set of private cloud platforms. Resources and networks between data centers are completely physically isolated, and a single management platform can be used to manage private cloud platforms spread across different locations.

* **Cluster**

  A cluster is a logical division of the physical resources of the platform, used to distinguish service nodes with different configuration specifications and storage types, such as X86 computing clusters, ARM computing clusters, SSD storage clusters, or commercial storage clusters.

  A data center can support the deployment of multiple computing and storage clusters. A cluster generally consists of a group of physically identical nodes with the same configuration and purpose. Users can deploy virtual resources on different computing clusters and mount distributed block storage devices across clusters.

* **Administrator Account**

  The system administrator `admin` account is the management console account, which can create system administrators and regional administrators.

  The system administrator has full administrative rights over the entire platform, used for global management and operation of the entire cloud platform. The administrator account can manage the regions, clusters, tenants, resources, billing, approval, security, and platform-wide configuration of the cloud platform, as well as managing its own security.

  The regional administrator has management permissions specific to a particular region of the platform, used for operating the entire cloud platform within that region. The administrator account can manage the clusters, resources, billing, approval, and platform-wide configurations specific to that region of the cloud platform.

* **Tenant**：

  The platform supports a multi-tenant mode for enterprises with a multi-level organizational structure. Tenants can be operated as separate companies, enabling effective permission management to mitigate the risks of mixing resources between parent and subsidiary companies or different departments. This also allows for resource auditing.

  A tenant is a collection of resources in the platform that provides capabilities such as resource isolation, sub-account management, permission control, quota, and price configuration. Resources between different tenants are strongly isolated through VPC networks and permissions. All resources, costs, quotas, and approvals of the main accounts and sub-accounts within a tenant belong to that tenant.

- **Main Account**

  Each tenant must have a main account, which has all permissions for managing all resources under the tenant by default. The main account can create and manage sub-accounts, and manage the permissions of the sub-accounts.

- **Sub-Account**

  A sub-account is a user created by the main account. The permissions of the sub-account within the tenant are controlled by the main account. A tenant can have multiple sub-accounts, and permission control can be applied to these sub-accounts for resource management.

- **External Storage**

  The platform provides distributed storage by default. External storage refers to commercial storage resource pools (such as IPSAN) connected to the platform via the ISCSI protocol. It supports one-click scanning of LUN storage volume information in the commercial storage pool, and the storage volume can be used as the system disk and data disk of virtual machines.

* **Public Network Segment**

  The public network segment is the network used by the platform for external communication. It is generally allocated and configured to the cloud platform by administrators or operations personnel via a physical network. The public network segment is the IP resource pool that the platform assigns to tenants for allocating elastic EIPs. It supports both IPv4 and IPv6 IP types, and can configure network segment routing and automatically distribute routing to platform virtual machines.

