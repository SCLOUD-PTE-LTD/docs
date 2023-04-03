---
layout: default
title: Tenant Management
parent: Admin Guide
grand_parent: Private Cloud
permalink: /private-cloud/admin-guide/tenant/
nav_order: 1
---
# Tenant Management
## Overview
Tenants mainly provide organizational structure management for enterprise users, and implement functions such as resource access control, approval process, and expense management based on tenants.

![1](/assets/images/admin-guide/admin-guide-1.png)

Concept explanation
- Tenant<br/>
The platform supports multi-tenant mode, which is used for enterprises with multi-level organizational structure. Tenants can be operated as a separate company/subsidiary/department, effectively realizing authority management, and eliminating the possibility of mixing resources of the head office, subsidiaries and different departments. posed risks and enables resource auditing.<br/>
A tenant is a collection of resources on the platform, providing capabilities such as resource isolation, sub-accounts, permission control, quota and price configuration. The resources between different tenants are strongly isolated through the VPC network and permissions; the resources, fees, quotas and approvals of all primary accounts and sub-accounts in the tenant belong to the tenant
- Account
  - Primary account: A tenant has a primary account by default. The primary account is the initial administrator under the tenant. By default, it has the management authority of all resources under the tenant and the organization management authority. The main account can create sub-accounts and manage the permissions of sub-accounts.
  - Sub-account: A sub-account is a user created by the main account, and the permissions of the sub-account under the tenant are controlled by the main account. A tenant can have multiple sub-accounts, which supports resource management permission control for sub-accounts.
- Personnel: Personnel in the enterprise, personnel need to log in to the cloud platform with an account to use resources
- Role: A collection of permissions, granting permissions to users and member groups can obtain the ability to call related APIs for resource operations.
- Project team: Resource planning is carried out with the project team as the dimension, and an independent resource pool can be established for a specific project or business to achieve more fine-grained management of resources. At the same time, authorization for sub-accounts is also based on the dimension of the project group.
- Process approval: In order to meet the management and control requirements of enterprises for the use of core cloud resources, such as virtual machines, cloud hard disks, and external network IPs, the cloud resource work order approval process introduced.
- Approval management: Approval management is currently only available to the platform administrator admin.

## Create a tenant
The administrator can add a tenant of the platform by creating an account, and creating a tenant will also add a master account. As shown in the figure below, when creating an account, you need to enter the account name, account email address, account password, confirmation password, resource approval, automatic approval, etc.:

![1](/assets/images/admin-guide/admin-guide-2.png)

- Account Name: Name identifiers for tenant and master accounts.
- Account Email: The email address of the main account.
- Account password: the login password of the main account
- Confirm password; confirm the login password of the main account again
- Resource approval: Do you need to enable resource approval for tenants? Start the resource approval process for tenants with accounts. Users under this tenant create cloud hosts, cloud hard disks, private networks, and external network IPs, modify cloud host configurations, expand system disks, and expand data disks. <br/>Adjusting the bandwidth needs to go through the application process. The default approval process of the platform is for the platform administrator to approve resource applications.
- Automatic Approval: Whether to enable automatic approval of resources for tenants. After automatic approval is enabled, the tenant's master/sub-account provides resource applications and will be automatically approved, and resources can be approved and created without manual intervention.

After creation, send the email address and password of the account to the end user, and the user can use this account to log in to the cloud platform to perform related resource operations.

After the account is created, the default balance is 0. You need to enter the recharge management page of the account. After recharging the account, the user can create and use resources normally.

## View tenant list
The administrator can view the list of all tenants on the platform, and an account represents a tenant. When creating an account, the platform will create a tenant and a master account by default, and the master account is the manager of the tenant. The list information of the account is shown in the figure below:

![1](/assets/images/admin-guide/admin-guide-3.png)

- Account ID: The ID of the tenant.
- Account Name: The name of the account.
- Account Email: The email address of the primary account under the tenant.
- Cash Balance: The tenant's cash balance.
- Gift Balance: The tenant's gift balance.
- Credit Balance: The tenant's credit balance.
- Status: The status information of the tenant, including inactive, in use, frozen, etc.
  - Not Activated: When the user registers an account through the platform, the account is not activated through the email activation link, and the administrator can perform the activation operation on the tenant list.
  - In use: indicates that the status of the tenant is in use.
  - Frozen: means that the tenant has been frozen, and the main account and sub-account under the tenant cannot be logged in and used.
- Approval process: Whether the tenant has set up an approval process, including opening and closing.
- Automatic Approval: Whether the tenant has enabled automatic approval, including on and off.
- Creation Time: The creation time of the tenant.

The administrator can manage and operate each tenant through the tenant list, including freezing, unfreezing, login restrictions, and editing the approval process. At the same time, the administrator can enter the details of the tenant through the details button, and perform global management on the account and configuration of the tenant. At the same time, it supports modification The account name of the tenant, as shown in the following figure:

![1](/assets/images/admin-guide/admin-guide-4.png)

The account overview page mainly displays the basic information of the account and all members under the account. The basic information includes account ID, account name, account mailbox, cash balance, gift balance, credit balance, and creation time. For details about tenant member management, see Tenant Member Management.

## Freezing tenants
Freezing an account refers to locking a tenant. If the tenant is successfully frozen, the main/sub account will not be able to log in to the cloud platform, and will not affect the normal operation of the resources created in the tenant and the business. Only tenants whose status is [in use] can be frozen. The administrator can enter the wizard page of freezing tenants through freezing the tenant list, as shown in the following figure:

![1](/assets/images/admin-guide/admin-guide-5.png)

After the tenant is frozen, the main and sub-accounts in the tenant will be frozen, that is, all accounts cannot be used, and the tenant needs to be unfrozen before all accounts in the tenant can be used.

## Unfreezing tenants
When a tenant is frozen, the status of the tenant is frozen, and the administrator can unfreeze a tenant through tenant management, as shown in the following figure:

![1](/assets/images/admin-guide/admin-guide-6.png)

After the tenant is unfrozen, all accounts in the default tenant will be unfrozen and can log in to the console normally. If a sub-account is frozen before the tenant is frozen, after the tenant is unfrozen, the previously frozen sub-account remains frozen, and the frozen sub-account needs to be unfrozen separately in the tenant. scene.

## Login Restrictions
The administrator can set the login restriction policy for the tenant. The login access policy determines the IP address of the client that can log in to the console and access the API. After configuration, the account can only log in or initiate API access from the specified IP. By default, no IP is specified, which means no Restrict IP addresses for logging into the console and accessing the API.

An example of a scenario where the administrator sets login restrictions for a tenant: the login IP address set by the main account is wrong, and all accounts under the tenant cannot log in. You can contact the platform administrator to modify the login access restrictions of the tenant through the management console.

The administrator can configure the tenant's login policy through the "Login Access Restriction" function in the operation item of the tenant list, as shown in the figure below. The default value is empty, which means that there is no restriction on the entire network.

![1](/assets/images/admin-guide/admin-guide-7.png)

## Edit Approval Process
Administrators can manage the approval process settings under a tenant by editing the approval process to enable or disable the approval process for all accounts under a tenant to change resources, including enabling/disabling resource approval and enabling/disabling automatic approval.

![1](/assets/images/admin-guide/admin-guide-8.png)