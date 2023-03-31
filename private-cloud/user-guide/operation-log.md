---  
layout: default
title: Operation Log
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/operation-log/
nav_order: 17
---
# Operation log
The operation log refers to the audit information of the operation behavior of the user on the console or API on the resource and the login and logout of the platform. The operation log will record all resource operations of the user on the platform, provide operation record query and filtering, and implement security analysis, resource change tracking and compliance audit through the operation log.

Through the operation log console, tenants can view the user-owned resource operations of the entire platform and audit logs of platform login and logout, etc., and can also query the operation logs and audit information of all resources in the tenant through the API. It supports viewing log information of 1 hour and custom time, and can query operation log information of up to 6 months. The specific information includes the operation (API) name, module it belongs to, associated resources, operator, operation result, notes and operation time, as shown in the figure below:

![1](/assets/images/user-guide/user-guide-150.png)

- Operation (API) name: refers to the operation name of the operation log, including the name of the interface calling the API and the interface display name of the operation, such as adjusting bandwidth.
- Module: Refers to the resource type of the operation log operation, including bare metal, virtual machine, virtual machine template, image, VPC, cloud disk, elastic network card, external network IP, security group, load balancing, NAT gateway, VPN gateway, automatic scaling , Monitoring alarm templates, timers, accounts, etc.
- Associated resource: The resource identifier corresponding to the operation log, and you can view all associated resource identifiers in an operation, such as the virtual machine ID corresponding to the bound elastic IP and the ID of the external network IP.
- Operator: The operator corresponding to the operation log, which can be traced back to the specific main account and sub-account.
- Operation result: the result of the operation log, such as operation success, operation failure, abnormal parameters, insufficient physical resources of the storage cluster, etc.
- Remark: Remark information of the operation log.
- Operation Time: The operation time of the operation log.

In order to facilitate users to view operation audit logs conveniently, the console supports log filtering and search retrieval, and supports exporting user operation audit logs as a local Excel table, which is convenient for account management and operation.

The operation log query and filtering function can support latitudes such as module, operation status, and query time range. The module to which it belongs supports the screening of all product modules, and supports viewing the logs and audit information of all resources at the same time, that is, does not filter the module to which it belongs; the operation status supports log screening of success and failure, and also supports viewing logs and audit information of all states ; The query time range supports log filtering of 1 hour and custom time, and can query operation logs of up to half a year.

*Operation logs do not record the operations and audits performed by users inside the virtual machine.*