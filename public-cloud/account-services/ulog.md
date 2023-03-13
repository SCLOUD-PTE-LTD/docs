---
layout: default
title: ULog
parent: Account Services
grand_parent: Public Cloud
permalink: /public-cloud/account-services/ulog/
nav_order: 5
---
# ULog - Operation Log

### Operational logs
The operation log records the operation behavior of users on resources in the console and API.

Note: The operation log does not record what the user does inside the server.
### View operation records
(1) Operation record summary page
On the Console Management - > Operation Logs page, you can view the operation behavior records of all resources and filter them by operation time, resource category, and data center.

(2) Specify resource operation records (currently only UCDN and UMem are supported)
After you select a specified resource on the Product Management page, the Log tab on the details page on the right displays all recent operations of the resource.

### Operational logging scope: 

| Products | Support Scopes | Remarks |
| --- | --- | --- |
| UHost | Host mirror |
| Dreams | EIP subnet router VPN | | 
| ULB | ULB | Only ULB creation, deletion and modification of names are supported |
| UDB | all | |
| UMem | all | |
| UDisk | all | |
| UCDN | all | For the purchase traffic operation, check the order history |
