---
layout: default
title: Tenant Management
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/tenant/
nav_order: 7
---
# 7. Tenant Management
## 7.1 Overview

Tenants mainly provide organization structure management, resource access control, approval processes, cost management and other functions for enterprise users based on tenants.

**Organization Structure**

![organazation](/assets/images/adminguide/organazation.png)

**Concept Explanation**

* **Tenant**

  The platform supports a multi-tenant model for enterprises with multi-level organizational structures. Tenants can be operated as separate companies/subsidiaries/departments to effectively manage permissions, eliminate risks of sharing resources among different departments, and achieve resource auditing.

  A tenant is a collection of resources in the platform that provides capabilities such as resource isolation, sub-accounts, permission control, quota and price configurations. Resources between different tenants are strongly isolated through VPC networks and permissions. All resources, costs, quotas and approvals for main accounts and sub-accounts within a tenant belong to the tenant.

- **Account**
  
  - **Main account**： Each tenant has one default main account who is the initial manager of the tenant. The main account has default management privileges of all resources within the tenant and organizational management privileges. The main account can create sub-accounts and manage their permissions.
  - **Sub-account**Sub-accounts are users created by the main account, and their permissions within the tenant are controlled by the main account. A tenant can have multiple sub-accounts and support permission control for sub-account resource management.
  
- **Personnel**：Personnel in an enterprise need to log in to the cloud platform using an account and can use cloud platform resources.

- **Role**：A collection of permissions that grants users and member groups the ability to call relevant APIs for resource operations.

- **Project Group**：Resource planning is based on project groups, which can establish independent resource pools for a specific project or business to achieve finer-grained resource management. Authorization for sub-accounts is also based on the dimension of the project group, and all resources of a tenant must belong to a project.

- **Approval Process**：An approval process for cloud resource work orders is introduced to meet enterprise management requirements for core cloud resources such as virtual machines, cloud disks, public IP addresses, etc.

- **Approval Management**Approval management can only be operated by platform administrators.

## 7.2 Creating a Tenant

Administrators can create tenants, and when creating a tenant, they need to add a main account as the initial manager of the tenant. Login security policies for accounts can be set synchronously, with forced password modification on first login. Management settings for tenants can also be configured, including whether to enable resource approvals, automatic approvals, and designated regions for tenant usage.

![addacc](/assets/images/adminguide/addacc.png)

- Account Name: The name identifier for the tenant and main account.
- Email Address: The email address for the main account.
- Password: The login password for the main account.
- Confirm Password: Confirmation of the main account's login password.
- Resource Approval: After enabling the approval process for the tenant, users under this tenant need to submit an application for creating virtual machines, cloud disks, private networks, public IP addresses, modifying virtual machine configuration, hot upgrading, expanding system disks, expanding data disks or adjusting bandwidth. The platform administrator will approve the resource application by default.
- Automatic Approval: Indication of whether to enable automatic approval of resources for the tenant. When automatic approval is enabled, resource applications submitted by main and sub accounts under the tenant will automatically be approved without human intervention.

> After creating an account, the balance is defaulted to zero. To allow creation and use of resources by the accounts under the tenant, administrators need to go to the recharge management page to recharge the tenant.

## 7.3 Viewing Tenant List

Administrators can view the list of all tenants on the platform and perform relevant management operations on tenants.

![acclist](/assets/images/adminguide/acclist.png)

- enant ID: The ID of the tenant.
- Status: The status information of the tenant, including inactive, in use, frozen, etc.
  - Inactive: Indicates that the user has not activated the account through the email activation link when registering the account on the platform. The administrator can activate the account by performing the activation operation on the tenant list.
  - In use: Indicates that the tenant is in use.
  - Frozen: Indicates that the tenant has been frozen, and main and sub-accounts under the tenant cannot log in or use resources.
- Main Account Name: The name of the main account under the tenant.
- Main Account Email: The email address of the main account under the tenant.
- External Recharge Balance: The cash balance of the tenant.
- Platform Recharge Balance: The gift balance of the tenant.
- Approval Process: Whether the tenant has set up an approval process, including enabled and disabled.
- Automatic Approval: Whether the tenant has enabled automatic approval, including enabled and disabled.
- Creation Time: The creation time of the tenant.

Administrators can manage and operate each tenant through the tenant list, including freezing, unfreezing, login restrictions, editing approval processes, region authorization management, deletion, etc. They can also enter the details of a tenant through the detail button to perform global management of the tenant's accounts and configurations. Additionally, they can modify the name of the tenant's main account, as shown below:

![upaccname](/assets/images/adminguide/upaccname.png)

The tenant overview page mainly displays the basic information of the tenant, including tenant ID, main account name, main account email, external recharge balance, platform recharge balance, and creation time.

## 7.4 Freezing a Tenant

Freezing an account means locking a tenant. A successfully frozen tenant will render main and sub-accounts unable to log in to the cloud platform but it does not affect the normal operation of resources and business already created within the tenant. Only tenants with a status of "in use" can be frozen. Administrators can enter the freeze wizard page of a tenant through the freeze button on the tenant list, as shown below:

![lockacc](/assets/images/adminguide/lockacc.png)

After a tenant is frozen, all main and sub-accounts within the tenant will also be frozen and unable to use the platform. The tenant needs to be unfrozen first before accounts within the tenant can be used.

## 7.5 Unfreezing a Tenant

When a tenant is frozen, its status will be "frozen". Administrators can unfreeze a tenant through tenant management, as shown below:

![unlockacc](/assets/images/adminguide/unlockacc.png)

After a tenant is unfrozen, all accounts within the tenant will be automatically unfrozen and able to log in to the console. If any sub-accounts were in the frozen state prior to the freezing of the tenant, they will remain frozen after the tenant has been unfrozen and will need to be individually unfrozen in the tenant, for the scenario where sub-accounts have been frozen.

## 7.6 Login Restriction

Administrators can set login restriction policies for tenants. The login access policy determines the client IP addresses that can log in to the console and access the API. After configuration, accounts can only log in or initiate API access from specified IP addresses. By default, no IP is specified, indicating no restriction on the IP addresses that can log in to the console or access the API.

An example of an administrator setting a login restriction for a tenant is when a main account has set an incorrect login IP address, causing all accounts under the tenant unable to log in. In this scenario, the platform administrator can modify the tenant's login access restriction through the management console.

Administrators can configure a tenant's login strategy through the "Login Access Restrictions" function in the tenant list operation items, as shown below. By default, it is empty, which means there are no restrictions.

![LoginWhitelist](/assets/images/adminguide/LoginWhitelist.png)

## 7.7 Editing Approval Process

Administrators can manage the approval process settings under a tenant by editing the approval process. This includes enabling or disabling the approval process for all account changes related to resources under a tenant, including enabling/disabling resource approvals and enabling/disabling automatic approvals.

![editapprove](/assets/images/adminguide/editapprove.png)

## 7.8 Region Authorization Management

Region authorization management allows administrators to manage a tenant's authorization status in a region. Only when authorized in a specific region can accounts under the tenant use services in that region normally. Enterprises can manage the opening of tenants in corresponding regions based on the actual operation of the cloud platform.

![enaregion](/assets/images/adminguide/enaregion.png)

## 7.9 Tenant Member Management

Tenant member management supports viewing and managing existing main and sub-accounts under a tenant, including viewing account information, modifying passwords, freezing accounts, and unfreezing accounts.

### 7.9.1 Viewing Account Information

The member list mainly displays information about the main and sub-accounts under the tenant, including account ID, status, type, account name, email, and operation items. It also supports searching for main/sub-accounts. Administrators can enter the overview page of a tenant's details by using the tenant's ID in the tenant list to view the member list information, as shown below:

![acclist1](/assets/images/adminguide/acclist1.png)

- Account ID: The unique identifier of the main/sub-account on the platform.
- Status: The status of the account, including frozen or in use.
- Type: The role of the account, divided into main and sub-accounts.
- Account Name: The name of the account.
- Email: The login email of the account.
- Status: The status of the account, including frozen or in use.

### 7.9.2 Modifying Passwords

Administrators can modify the passwords of all main and sub-accounts within a tenant to accommodate scenarios where main and sub-accounts forget their passwords.

### 7.9.3 Freezing Accounts

Administrators can independently freeze a main or sub-account within a tenant. A successfully frozen main account will not be allowed to log in to the console, but it will not affect other sub-accounts. A successfully frozen sub-account will not be allowed to log in to the console, but it will not affect other sub-accounts and the main account.

![lockmasteracc](/assets/images/adminguide/lockmasteracc.png)

* If an account is frozen when the administrator freezes the tenant to which the account belongs, the account will be in a frozen state.
* When the administrator unfreezes a tenant, accounts that were previously frozen will not be affected. In other words, when a tenant is unfrozen, the account will still be in a frozen state.
* You need to unfreeze the account separately before you can log in and use the console normally.

### 7.9.4 Unfreezing Accounts

Administrators can independently unfreeze a main or sub-account within a tenant. Successfully unfrozen accounts will be allowed to log in to the console. For example, if an account is frozen due to multiple incorrect password attempts, it can be individually unfrozen on the tenant member management page.

![unlockmasteracc](/assets/images/adminguide/unlockmasteracc.png)

## 7.10 Tenant Order Management

Through tenant order management, administrators can view order records under a tenant. They can also view orders generated by the tenant within a certain time period by customizing the region and query time. Administrators can also download order management information as a local Excel file, as shown below:

![accorder](/assets/images/adminguide/accorder.png)

## 7.11 Tenant Transaction Management

Through tenant management, administrators can view transaction information of the corresponding tenant. They can also view transaction records generated by the tenant within a certain time period by customizing the query time. Administrators can also download transaction management information as a local Excel file, as shown below:

![acctran](/assets/images/adminguide/acctran.png)

## 7.12 Tenant Fund Management

Through tenant management, administrators can manage account recharge and withdrawal for tenants, view their recharge and withdrawal records, and view their recharge and withdrawal records generated within a certain time period by customizing the query time.

### 7.12.1 Recharge Management

Administrators can enter the tenant fund management tab page through the tenant details page to perform recharging for the tenant. The recharge methods are divided into internal recharge and external channel recharge. Administrators can recharge the tenant according to their needs:

* External channel recharge sources include bank transfer, Alipay, WeChat Pay, and Sina Pay.
* Internal recharge is the balance gifted to the tenant by the platform.

![recharge](/assets/images/adminguide/recharge.png)

The minimum amount for a single recharge is 100 and the maximum is 500,000.

### 7.12.2 Querying Recharge Information

Through recharge management, administrators can view recharge records within a certain time period, including recharge order number, recharge channel, recharge amount, and creation time, and download recharge record information as a local Excel file.

![rechargelist](/assets/images/adminguide/rechargelist.png)

### 7.12.3 Withdrawal Management

Administrators can use the tenant withdrawal management tab page on the tenant details page to handle withdrawals for tenants. Withdrawal accounts are divided into platform accounts and external accounts, and administrators can perform withdrawals for tenants according to their needs.

![recharge](/assets/images/adminguide/withdraw.png)

### 7.12.4 Querying Withdrawal Information

Through withdrawal management, administrators can customize table headers to view withdrawal records within a certain time period, including withdrawal order number, source account type, withdrawal amount, and creation time, and download withdrawal record information as a local Excel file.

![recharge](/assets/images/adminguide/withdraw1.png)

## 7.13 Managing Tenant Quotas

### 7.13.1 Viewing Tenant Quotas

Through tenant management, by selecting the desired region to view, administrators can query the current platform resource quota settings for the tenant. Administrators can also modify the quota information for each product under the tenant, including product type, resource type, quota factor, quota, and operation.

![accquota](/assets/images/adminguide/accquota.png)

Through the operation item, administrators can modify the quota value for each product in this region.

### 7.13.2 Modifying Tenant Quotas

The platform supports configuring tenant quotas for products such as virtual machines, images, VPCs, VIPs, disks, network cards, public IPs, load balancers, security groups, NAT gateways, and VPN gateways. The quotas can be configured for each region and all regions for each product.

![upaccquota](/assets/images/adminguide/upaccquota.png)

Tenant quotas inherit the platform's global quota settings by default. Administrators can customize the product quotas for each tenant.

## 7.14 Tenant Pricing Management

### 7.14.1 Viewing Tenant Pricing

Through tenant management, administrators can query the pricing information for billable resources of the tenant on the platform, including billing factors, regions, properties, billing types, billing rules, prices, discounts, discounted prices, update times, and operation items.

![setaccprice](/assets/images/adminguide/setaccprice.png)

Through the operation item, administrators can modify the prices for each product in different regions and clusters. The billing factors include CPU, memory, disk, public IP, GPU, and the cost of creating virtual resources for the tenant is deducted according to the total cost of the billing factors and charged according to the payment method.

* For calculation billing factors such as CPU, memory, and GPU, different prices and discounts can be defined and displayed in different compute clusters.
* For storage billing factors such as disks, different prices and discounts can be defined and displayed in different storage clusters.
* A global price and discount can be defined and displayed for public IPs, and different prices and discounts can be defined and displayed for different public IP segments.

The prices listed on the table are the global prices set in the platform's [price configuration], and administrators can view the benchmark prices for each resource in different clusters through the tenant pricing list. They can also view the discount rate and final price after applying the discount.

### 7.14.2 Setting Tenant Discounts

The pricing for tenants' resources on the platform inherits the platform's global price configuration by default. Administrators can customize the product prices and discounts for each tenant, setting prices for individual resources in different clusters. The following example shows how to modify the monthly payment discount for CPUs in cluster `ComputeSetPre01`.

![UpdateDiscount](/assets/images/adminguide/UpdateDiscount.png)

The discount is expressed as a percentage, such as 90 for a 10% discount off the benchmark price of the CPU rented on a monthly basis. The modification will take effect immediately, and existing resources created by the tenant will not be affected. New CPU resources will be charged according to the new pricing.

Modifying a tenant's product discounts only affects that tenant and does not affect the prices of other tenants, which can meet the scenario of providing different cloud resource prices for different customer types and improve operational efficiency.

## 7.15 Account Resource Overview

Through tenant management, administrators can view the resource overview information for the corresponding tenant, including resource utilization and resource quota usage, as shown below:

![accoverview](/assets/images/adminguide/accoverview.png)

* Resource Utilization: Refers to the average utilization of all resources owned by the tenant in the platform, including average CPU utilization, average memory utilization, average disk utilization, and average bandwidth utilization.
* Resource Quota Usage: Refers to the quota usage status of all resources owned by the tenant in the platform, including the total quota and used quota of virtual machines, images, disks, elastic network cards, VPCs, elastic IPs, security groups, load balancers, NAT gateways, VPN gateways, and tunnels.

## 7.16 Tenant Deletion

Tenant deletion is supported, and after a tenant is deleted, all accounts under the tenant will be synchronized and cleared, and these accounts cannot be used to log in to the platform again.

> Note that the prerequisite for deleting a tenant is that all resources under the tenant have been cleared. It is recommended to communicate with relevant management personnel of the tenant before deleting the tenant and confirm that the cloud platform is no longer needed, then clear the resources in a timely manner. If the tenant does not clear the resources on their own, administrators can search for the tenant's resources in various resource lists in the management console and delete them. After virtual machines, cloud disks, and public IPs are deleted, they will enter the recycle bin and need to be completely destroyed in order to release resources.

