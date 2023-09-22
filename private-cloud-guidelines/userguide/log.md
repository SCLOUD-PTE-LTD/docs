---
layout: default
title: Operation Logs
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/logs/
nav_order: 20
---
# 20. Operation Logs

## 20.1 Operation Logs

Operation logs refer to the audit information of user operations on resources in the console or API and log in and out of the platform. Operation logs record all resource operations of users on the platform, provide operation record query and screening, and can achieve security analysis, resource change tracking and compliance audit through operation logs.

Tenants can view all platform resources operations and platform login and logout audit logs belonging to the user through the operation log console. They can also query the operation logs and audit information of all resources in the tenant through the API. It supports viewing log information for 1 hour and custom time, and can query operation log information for up to 6 months. Specific information includes operation (API) name, module, region, associated resources, operator, operation result, remarks and operation time, as shown in the following figure:

![log](/assets/images/userguide/log.png)

* Operation (API) Name: Refers to the operation name of the operation log, including the interface name of the API called and the display name of the operation, such as adjusting bandwidth.
* Belonging Module: Refers to the resource type of the operation log operation, including bare metal, virtual machine, virtual machine template, image, VPC, cloud disk, elastic network card, EIP, security group, load balancer, NAT gateway, VPN gateway, auto scaling, monitoring alarm template, timer, account, etc.
* You can switch regions and view log information in different regions.
* Associated Resources: Resource identifiers corresponding to the operation log, and all associated resource identifiers in an operation can be viewed, such as the virtual machine ID and external IP ID corresponding to the bound elastic IP.
* Operator: The operator corresponding to the operation log, which can be traced back to specific main accounts and sub-accounts.
* Operation result: Result of operation log, such as operation success, operation failure, parameter exception, insufficient physical resources of storage cluster, etc.
* Remark: Remarks information of operation log.
* Operation time: Operation time of the operation log.

**To facilitate users to conveniently view the operation audit log, the console supports log filtering and search retrieval, and also supports exporting the user's operation audit log to a local Excel table, which is convenient for account management and operation.**

The operation log query and filtering function supports dimensions such as the belonging module, operation status and query time range. The belonging module supports the filtering of all product modules, and also supports the viewing of logs and audit information of all resources, that is, no filtering of the belonging module; the operation status supports the filtering of logs with status as successful and failed, and also supports the viewing of logs and audit information of all statuses; the query time range supports the filtering of logs in 1 hour and custom time, and the longest query can be half a year of operation logs.

> **The operation log does not record the operations and audits performed by the user inside the virtual machine.**

## 20.2 Notice Rule

By creating notification rules to monitor resource events and notify the notifier by email, you can set notification rules by selecting the monitoring region, notification group, monitoring module, and monitoring level. When the resource event meets the notification rule requirements, a monitoring email will be sent to the members of the notification group.

### 20.2.1 Create Notification Rules

Users can create notification rules by clicking the "Create" button on the notification rule page. When creating notification rules, they need to specify the monitoring region, notification group, monitoring module, and monitoring level. As shown in the following figure:

![logrule](/assets/images/userguide/logrule.png)

* Monitoring Region: Region information for notification rules.
* Notification group: Notification group information for email notifications, only one notification group can be selected.
* Monitoring Module: The content of the monitored resource module, such as virtual machines, cloud disks, supports selecting multiple modules.
* Monitoring level: operation results of operation log, including operation success, operation failure, supporting multiple selections.

### 20.2.2 View Notification Rules

Support users to view notification rule information under their account, including monitoring region, notification group, monitoring module and monitoring level, as shown in the following figure:

![logrule1](/assets/images/userguide/logrule1.png)

### 20.2.3 Update Notification Rules

Support users to update the notification rules under their account, including monitoring regions, notification groups, monitoring modules and monitoring levels. As shown in the following figure:

![logrule2](/assets/images/userguide/logrule2.png)

### 20.2.4 Delete Notification Rules

Allow users to delete notification rules under their account, and the rules will be destroyed immediately after deletion, as shown in the following figure:

![logrule3](/assets/images/userguide/logrule3.png)
