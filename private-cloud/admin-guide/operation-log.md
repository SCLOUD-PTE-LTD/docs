---
layout: default
title: Operation Log
parent: Admin Guide
grand_parent: Private Cloud
permalink: /private-cloud/admin-guide/operation-log/
nav_order: 3
---
## Operation log
The operation log of the management console is a global operation log of the platform provided for enterprises and administrators. Through the global operation log of the platform, you can query the operation and audit information of resources on the current platform itself and all tenants; by default, you can view all the resources on the platform within one day. Operation audit log, you can choose to view 3 days, 7 days, and user-defined long operation logs, and support querying operation logs of up to 6 months; at the same time, administrators can check operation logs through operation names, associated resource IDs, and operator emails to search.

Currently, the platform’s global operation day display supports viewing logs of products including virtual machines, self-made images, elastic network cards, cloud disks, VPC/subnets, external network IPs, NAT gateways, VPN gateways, elastic scaling, load balancing, security groups, and accounts , alarm template and timer, etc., as shown in the following figure:

![1](/assets/images/admin-guide/admin-guide-9.png)

- Operation (API) name: refers to the operation name of the operation log, including the name of the interface calling the API and the interface display name of the operation, such as adjusting bandwidth.
- Module: Refers to the resource type of the operation log operation, including bare metal, virtual machine, virtual machine template, image, VPC, cloud disk, elastic network card, external network IP, security group, load balancing, NAT gateway, VPN gateway, automatic scaling , Monitoring alarm templates, timers, accounts, etc.
- Associated resource: The resource identifier corresponding to the operation log, and you can view all associated resource identifiers in an operation, such as the virtual machine ID corresponding to the bound elastic IP and the ID of the external network IP.
- Operator: The operator corresponding to the operation log can be traced back to the specific platform administrator, main account and sub-account.
- Operation result: the result of the operation log, such as operation success, operation failure, abnormal parameters, insufficient physical resources of the storage cluster, etc.
- Remark: Remark information of the operation log.
- Operation Time: The operation time of the operation log.
In order to facilitate administrators to view operation audit logs conveniently, the console supports log filtering and search retrieval, and supports exporting user operation audit logs as a local Excel table, which is convenient for account management and operation.

The operation log query and filtering function can support latitudes such as module, operation status, and query time range. The module to which it belongs supports the screening of all product modules, and supports viewing the logs and audit information of all resources at the same time, that is, does not filter the module to which it belongs; the operation status supports log screening of success and failure, and also supports viewing logs and audit information of all states ; The query time range supports log filtering of 1 hour and custom time, and can query operation logs of up to half a year.

Operation logs do not record the operations and audits performed by users inside the virtual machine.