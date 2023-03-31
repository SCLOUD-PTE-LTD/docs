---  
layout: default
title: Billing Management
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/billing-management/
nav_order: 19
---
## Billing Management

Billing management provides metering and billing services for user resource allocation and use. Resources that need to be billed support three billing methods: on-time, yearly, and monthly, and support billing, deduction, renewal, and expired recycling of resources. Management operations, while providing transaction management such as recharge and deduction based on accounts. The sub-account shares the account balance of the main account, and the resources created through the sub-account can be directly deducted from the shared balance, and the transaction flow and order details of the account can be viewed through the main account or sub-account.

Platform resource billing is pre-paid mode, that is, whether paid on time, yearly, or monthly, it is necessary to ensure that the account balance can meet the deduction of one billing cycle when the resource is created, and the deduction will be made before the start of the next billing cycle fee.

- Billing by hour: One hour is a billing cycle, and resources are withheld according to the hourly unit price;
- Billing by month: one month (non-natural month) is a billing cycle, and resources are withheld according to the unit price of each month;
- Annual billing: One year (extended year) is a billing cycle, and resources are withheld according to the annual bill;

Resources purchased on a yearly or monthly basis support upgrading and upgrading configurations at any time, and the price difference will be automatically filled after the configuration is upgraded.

When the account balance is insufficient for the next billing cycle, the resource will automatically enter the recycle bin, and the resource account and resources need to be renewed before they can be used again; for NAT gateway and load balancing resources, the account balance is insufficient for the next billing cycle , the resources are automatically deleted.

When the cloud platform administrator has enabled "Automatic Resource Renewal" globally and the account balance is sufficient, the resources will be automatically renewed in the next billing cycle; if the cloud platform administrator has globally disabled "Resource Automatic Renewal" and the account balance is sufficient, Then the resource will automatically enter the recycle bin in the next billing cycle, and the resource needs to be renewed in the recycle bin to restore the resource.

When resources are created, the billing pricing of all billable resources will be displayed according to the billing method through the resource meter, which is used to confirm the cost of the order. The resources in each billing cycle support release and deletion. When the account balance is insufficient, the cloud platform administrator can recharge.

## Resource Meter
The resource meter provides users with the choice of resource payment methods, and displays the cost information of all resources in the payment mode and the resource "buy" confirmation button, as shown in the example below:

![1](/assets/images/user-guide/user-guide-155.png)

- The payment method in the meter supports users to choose hours, months, and years, which represent hourly billing, monthly billing, and yearly billing, respectively. When selecting month and year, you can choose the number of months and years to purchase.
  - The month can be selected from 1 ~ 11, representing 1 month or 11 months respectively;
  - The year can be selected from 1~5, representing 1 year or 5 years respectively;
- The total cost refers to the total cost of all billable resources in the current order for one billing cycle. For example, in a virtual machine order, it includes the cost of resources such as the specified CPU memory, cloud disk (if any), and EIP (if any) according to the payment method total.

After clicking Buy Now, the total fee amount will be deducted from the account balance, and a new purchase order and a deduction transaction flow will be generated; if the account balance is less than one billing cycle, you cannot click Buy Now, and you need to recharge your account first , you can purchase and create resources.

## Order Management
Order management is an order query and statistical service provided by the platform for users. Through order management, you can view all order records of the platform account and sub-accounts, and support viewing 1-day, 3-day, 7-day, 14-day, 1-month and custom time. Historical order records. When creating, renewing, or changing the configuration of resources, orders for new purchases, renewals, and upgrades will be generated, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-156.png)

- Order number: refers to the globally unique identifier of the current order;
- Resource ID: Generate the resource identifier of the current order;
- Region: the region where the current order resource is located;
- Order type: the type of the current order, including three types: new purchase, renewal and upgrade;
  - New purchases refer to billing resources newly created by users, including virtual machines, cloud disks, elastic IPs, NAT gateways, and load balancing;
  - Renewal refers to the order generated when prepaid resources are renewed in each billing cycle, including manual renewal and system automatic renewal;
  - Upgrade refers to the renewal order generated when the resource configuration is changed on a monthly or annual basis, such as upgrading bandwidth, upgrading virtual machine configuration, etc.;
- Order amount: the current order amount, that is, the price difference paid for the new order or the price difference when the upgrade is made;
- Creation time: The generation time of the current order record. As shown in the figure, a resource billed by the hour generates a renewal order every hour.

The master account has the same order management and data as all sub-accounts, and all order records can be viewed through one account.

## Transaction Management
Transaction management is the details of income and expenditure related to the account amount provided by the platform for users, including deduction, recharge and statistical services. Through transaction management, you can view all transaction flow records of platform accounts and sub-accounts, and support viewing historical transaction records of 1 day, 3 days, 7 days, 14 days, 1 month and custom time, as shown in the figure below:

![1](/assets/images/user-guide/user-guide-157.png)

- Transaction number: The current transaction is recorded in the globally unique ID identifier. Generally, the deduction starts with trans, and the recharge starts with trade;
- Transaction type: the type of the current transaction record, including recharge and deduction according to the different operations of the platform on resources:
  - Recharge refers to the recharge operation performed by the platform administrator for tenants through the background;
  - Deduction refers to the system’s billing method for each resource, which is automatically deducted from the account balance in each billing cycle. For example, for a virtual machine billed by the hour, the bill is deducted once per hour according to the unit price;
- Expenditure: the amount deducted from the current transaction record, it is only valid when the transaction type is deduction, and the recharge type is displayed as 0.00;
- Income: The finance that the current transaction record is credited to, it is only valid when the transaction type is recharge, and the deduction type is displayed as `0.00`;
- Free balance: the current balance of the current account after the current transaction record occurs;
- Transaction time: the time when the current transaction record occurs.

The transaction records of the main account and all sub-accounts are the same, and the overall income and expenditure records of the tenant can be viewed through one account.