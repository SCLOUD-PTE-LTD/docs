---
layout: default
title: Authorization Management
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/auth-management/
nav_order: 11
---
# 11. Authorization Management

## 11.1 Overview

Authorization management supports customers to separate authorization for basic modules and value-added service modules on demand. Users can distinguish authorization nodes according to x86/arm architecture and select the effective time and expiration time of each module's authorization according to business needs. The platform uses authorization certificates to activate and ensure the unique non-cloneability verification of keys.

The platform provides users with complete authorization management capabilities, including authorization management and node management modules.

- Authorization Management: Check the platform's basic license, extended license, and service license to understand product authorization status, effective time, expiration time, and quantity limit information, etc.
- Node Management: View the authorization status and node information of all nodes on the platform.

## 11.2 Authorization Management

The administrator can click [**Information Collection**] to collect the information of the platform, and click to provide the operation platform for use. After the operation platform is confirmed to be used, and then the user's needs are determined, the successful platform can be used by clicking the [**Update Authorization**] button. As shown below:

![order](/assets/images/adminguide/license.png)

### 11.2.1 View Basic Extended Licenses

Basic license: Supports licenses for cloud computing infrastructure management system suite-standard version, cloud computing infrastructure management system suite-xinchuang version, and distributed block storage suite.

Extended license: Supports licenses for cloud suite function extension, advanced network extension, bare metal services, USB pass-through services, GPU services, elastic scaling services, commercial storage services, file storage services, object storage services, MySQL 5.7 services, Redis 4.0 services, and heterogeneous platform migration software services, as shown in the figure below:

![order](/assets/images/adminguide/license.png)

- Authorization name: The name of the authorized product.
- Authorization ID: The unique identifier for authorization.
- Description: License description information.
- Status: Project authorization status.
- Start time: The time when the authorization certificate takes effect.
- End time: The time when the authorization certificate expires.
- Operation: Supports viewing the details of basic license authorization, as shown in the figure below:

### 11.2.2 View Service Licenses

Service license: Supports licenses for cloud computing infrastructure management system + value-added service 5 * 8 maintenance services, cloud computing infrastructure management system + value-added service 7 * 24 maintenance services, and gold VIP maintenance services, as shown in the figure below:

![order](/assets/images/adminguide/service.png)

### 11.2.3 View Authorization Details

Authorization management supports viewing product authorization details, as shown in the figure below:

![order](/assets/images/adminguide/licensedetail.png)

- Name: The name of the authorized product.
- Content: Specific authorization information.
- Unit: Authorized product unit.

## 11.3 Node Management

Users can view node IP addresses, serial numbers, roles, authorization status, CPU models, total CPU, memory, and architecture through the node management page, as shown in the figure below:

![order](/assets/images/adminguide/nodeset.png)

- Name: Named after the IP address corresponding to the platform node.
- Serial number: Node's serial number information.
- Status: View the role and authorization status of the platform node.
- CPU model: Node CPU model information.
- Total CPU: Node total CPU information.
- Total memory: Node total memory information.
- Architecture: Node architecture information.

### 11.3.1 View Node Information

Node management supports accessing node basic information, CPU information, and Memory information through node names, as shown in the figure below:

![order](/assets/images/adminguide/node.png)







