---
layout: default
title: Billing Management
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/billing/
nav_order: 23
---

# 23. Billing Management

Billing management provides metering and billing services for user resource allocation and usage. Resources that need to be billed support three billing methods: hourly, yearly, and monthly. It also supports order management operations such as billing, deduction, renewal, and expiration recovery. At the same time, based on the account, it provides recharge, deduction, and other transaction management. Sub-accounts share the account balance of the main account. Resources created by sub-accounts can directly deduct fees through shared balance, and the main account or sub-account can view the transaction flow and order details of the account.

The platform resource billing is all prepaid, which means no matter if paying by hour, year, or month, the account balance must be able to cover the cost of one billing cycle when the resource is created, and the payment will be deducted before the next billing cycle begins.

- Pay by the hour: one hour is one billing cycle, and the resources are pre-charged at the hourly rate.
- Charge by month: one month (non-natural month) as one billing cycle, resources are pre-charged according to the unit price of each month.
- Charge by year: one year (rollover year) as one billing cycle, resources are pre-charged according to the single year.

> Resources purchased on an annual, monthly, or hourly basis can be upgraded or downgraded at any time and the difference in price will be automatically made up after the upgrade configuration.

If the account balance is insufficient for the next billing cycle, the resources will automatically enter the recycle bin and need to be recharged before they can be used again; for file storage, object storage, external network card, NAT gateway, VPN gateway, and load balancing resources, if the account balance is insufficient for the next billing cycle, the resources will be automatically deleted.

When the cloud platform administrator globally enables **Automatic Renewal of Resources** and **the account balance is sufficient**, the resources will be automatically renewed in the next billing cycle. If the cloud platform administrator globally disables **Automatic Renewal of Resources** and **the account balance is sufficient**, the resources will be automatically put into the recycle bin in the next billing cycle, and the resources need to be renewed and recovered in the recycle bin.

When a resource is created, the billing of all chargeable resources will be displayed through the resource calculator according to the billing method for confirming the cost of the order. Resources within each billing cycle can be released and deleted. When the account balance is insufficient, the cloud platform administrator can recharge.

## 23.1 Resource Calculator

The resource pricing calculator provides users with options for resource payment methods and displays the cost information of all resources under the payment mode, as well as the "**Purchase**" confirmation button for the resources, as shown in the example below:

![buy](/assets/images/userguide/buy.png)

- The payment method in the meter supports users to select hour, month, and year, which respectively represent hourly billing, monthly billing, and annual billing. When selecting month and year, the number of months and years to purchase can be selected.
  - The months can be selected from 1 to 11, representing 1 month or 11 months respectively.
  - The years can be selected from 1 to 5, representing 1 year or 5 years respectively.
- The total cost refers to the total cost of all billing resources in the current order for one billing cycle, such as a virtual machine order, including the specified CPU memory, cloud disk (if any), EIP (if any) and other resources according to the payment method.

After clicking "Buy Now", the total fee amount will be deducted from the account balance, and a new purchase order and a transaction flow of deduction will be generated. If the account balance is insufficient for one billing cycle, clicking "Buy Now" will prompt "Tenant's account balance is insufficient", and the account needs to be recharged before purchasing and creating resources.

## 23.2 Order Management

Order management is a service provided by the platform for users to query and statistic orders. Through order management, users can view all order records of the platform account and sub-accounts. It supports viewing the historical order records of a certain region, 1 day, 3 days, 7 days, 14 days, 30 days and custom time. When creating, renewing, changing configuration or deleting resources, new orders, renewal orders, upgrade orders, downgrade orders and refund orders will be generated respectively, as shown in the following picture:

![orderlist](/assets/images/userguide/orderlist.png)

- Order number: a globally unique identifier for the current order.
- Order type: The type of the current order, including five types: new purchase, renewal, upgrade, downgrade and return.
  - New purchases refer to the newly created billing resources of the user, including virtual machines, cloud disks, elastic IPs, public network cards, file storage, object storage, NAT gateways, NAT gateways and load balancers.
  - Renewal refers to the order generated when a prepaid resource is renewed for each billing cycle, including manual renewal and system automatic renewal.
  - Upgrade refers to the renewal orders generated when the resources charged by time, month, and year are changed in configuration, such as upgrading bandwidth, upgrading virtual machine configuration, etc.
  - Downgrading refers to the renewal orders generated when the resources charged by time, month, and year are changed in configuration, such as downgrading bandwidth and downgrading virtual machine configuration, etc.
- Resource ID: A resource identifier that generated the current order.
- Region: The region where the current order resources are located.
- Order amount: The current order amount, i.e. the fees paid for new purchase, renewal, upgrade and the fees refunded for cancellation, downgrade (refunds displayed as negative values).
- Platform account: The amount paid by the current order platform account.
- External account: The amount paid by the external account for the current order.
- Creation Time: The generation time of the current order record, as shown in the figure, a resource charged by time, generates a renewal order every hour.

> The orders and data of the main account and all sub-accounts are the same, and all order records can be viewed through one account.

## 23.3 Transaction Management

Transaction management is a service provided by the platform for users to view account balance related income and expenditure details, including deductions, recharges, refunds and statistical services. Through transaction management, users can view all transaction records of the platform account and sub-accounts, and support viewing historical transaction records of a certain region, 1 day, 3 days, 7 days, 14 days, 30 days and custom time, as shown in the following figure:

![translist](/assets/images/userguide/translist.png)

- Transaction ID: The current transaction record has a globally unique ID identifier, starting with "trade".
- Transaction Type: The type of the current transaction record, according to the different operations of the platform on the resources, including recharge, deduction and refund.
  - Recharge refers to the recharge operation performed by the platform administrator through the background for tenants.
  - Charging refers to the billing operation of the system for each resource life cycle, such as when creating a resource, charging is performed.
  - Refund refers to the billing operation for the life cycle of each resource, such as when deleting a resource, a refund operation is performed.
- Expense: The amount of fees deducted from the current transaction record, only valid when the transaction type is charged, recharge display as `0.00`;
- Income: Financial incoming of current transaction record, valid when the transaction type is recharge and refund, deduction fee displays as `0.00`;
- External recharge balance: The external recharge balance of the current account after the current transaction record occurs.
- Platform recharge balance: the internal recharge balance of the current account after the current transaction record.
- Transaction Time: The time when the current transaction record occurred.

> The transaction records of the main account and all sub-accounts are the same, and the overall income and expenditure records of the tenant can be viewed through one account.

## 23.4 Bill Management

### 23.4.1 Overview

Bill management includes Bill Overview, Resource Bill, and Bill Details. Among them, Bill Overview can view cost trends and monthly bill summaries, and Resource Bill and Bill Details support filtering and export functions.

### 23.4.2 Bill Overview

Tenants can access the Billing Management Console through the navigation bar to view the Billing Overview. The Billing Overview includes two modules: cost trends and a summary of this month's bills.

### 23.4.3 Cost Trend

Tenants can view cost trends on the Account Overview page, and can view the transaction information generated by the cloud platform in the past six months by customizing the cost type, as shown in the following figure:

![usercosttrend](/assets/images/userguide/usercosttrend.png)

### 23.4.4 Summary of this month's bill

This month's bill summary is displayed in six pie charts, including product summary, tenant summary, region summary, project group summary, order type summary and billing mode summary. The list includes aggregate type, name, total cost, platform cost and external cost.

* Group by product

![userproductsum](/assets/images/userguide/userproductsum.png)

* Summarize by tenant

![userusersum](/assets/images/userguide/userusersum.png)

* Summarize by region

![userregionsum](/assets/images/userguide/userregionsum.png)

* Group by project group

![userprojectsum](/assets/images/userguide/userprojectsum.png)

* By order type

![userordersum](/assets/images/userguide/userordersum.png)

* Summarize by billing model

![userbilltypesum](/assets/images/userguide/userbilltypesum.png)

### 23.4.5 Resource Bill

Tenants can view the resource billing information of the cloud platform from five dimensions: billing cycle/belonging product/billing mode/belonging region/belonging project. The list includes resource ID, region, tenant ID, master account name, master account email, belonging product, belonging project, billing mode, total fee, platform account, external account and transaction time, as shown in the following figure:

![userresourcebill](/assets/images/userguide/userresourcebill.png)

- Resource ID: Global Unique Identifier for the Bill
- Region: Information on the region where the resource is located
- Tenant ID: Tenant information that generates the order
- Main Account Name: The main account name under the tenant that is being charged.
- Main account email: The main account email for recharge
- Belonging products: Products of the cloud platform, including virtual machines, cloud disks, public IPs, VPN gateways, load balancers, NAT gateways, and network cards.
- Belonging project: The project bound to this transaction resource
- Billing Model: Hourly, Monthly, and Yearly billing models.
- Total cost: The total cost of this transaction
- Platform Account: The amount of the platform account consumed in this transaction
- External Account: The amount of the external account consumed in this transaction.
- Transaction Time: The time of this transaction.

### 23.4.6 Export resource bill

The platform supports tenants to filter resource bills from five dimensions: billing cycle, belonging product, billing mode, belonging region and belonging project, and export to a local Excel file, for the convenience of platform operation management and report statistics, as shown in the following figure:

![userexportingresbill](/assets/images/userguide/userexportingresbill.png)

### 23.4.7 Bill Details

Tenants can view the billing details of the cloud platform from six dimensions: billing cycle/product/order type/billing mode/region/project. The list includes resource ID, transaction number, transaction type, order number, order type, region, tenant ID, master account name, master account email, product, project, billing mode, total fee, platform account, external account and transaction time, as shown in the following figure:

![userbilldetails](/assets/images/userguide/userbilldetails.png)

- Resource ID: Global Unique Identifier for the Bill
- Transaction ID: Unique identifier of the transaction record on the cloud platform.
- Transaction type: Both account recharge and deduction will generate a transaction record, so the transaction types include account balance recharge, free account recharge and deduction.
- Order Number: Unique identifier of the order on the cloud platform
- Order type: including upgrade and new purchase
- Region: Information on the region where the resources are located.
- Tenant ID: Tenant information for generating orders
- Main Account Name: The main account name under the tenant that is being recharged
- Main account email: The main account email for recharge
- Belonging products: Products of the cloud platform, including virtual machines, cloud disks, public IPs, VPN gateways, load balancers, NAT gateways, and network cards.
- Belonging Project: The project bound to this transaction resource.
- Billing Model: Hourly, Monthly, and Yearly billing models
- Total cost: The total cost of this transaction.
- Platform Account: The amount of the platform account consumed in this transaction
- External Account: The amount of the external account consumed in this transaction.
- Transaction Time: The time of this transaction

### 23.4.8 Export Bill Details

The platform supports tenants to filter the bill details from six dimensions: billing cycle, product, order type, billing mode, region, and project, and export to a local Excel file for platform operation management and report statistics, as shown in the following figure:

![userexportingbilldetails](/assets/images/userguide/userexportingbilldetails.png)
