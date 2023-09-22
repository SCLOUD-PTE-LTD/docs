---
layout: default
title: Organization and Account Management
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/admin-guides/organization-account-management/
nav_order: 2
---
# 2. Organization and Account Management

Organization and account management mainly provide organization structure management for enterprise users, supporting multi-tenant mode where each tenant represents an organization. The resource access control and resource creation process management can be based on tenants. The platform organization and account management are divided into two dimensions: tenant side and administrator side. This chapter mainly introduces the administrator account, please refer to the user manual organization and account management section for tenant-side account and organization management.

## 2.1 Administrator Account

The system administrator `admin` account is the administrator console account that can create system administrators and regional administrators.

The system administrator has all the management permissions of the platform and is used for global management and operation of the entire cloud platform. It can manage the regions, clusters, tenants, administrators, resources, billing, approval, security, and platform-wide configuration of the cloud platform through the administrator account.

Regional administrators have management permissions specific to a particular region of the platform and are used for operating the entire cloud platform. Using the administrator account, they can manage clusters, resources, billing, approval, and platform-wide configurations specific to a particular region of the cloud platform.

As the platform administrator account, it has the highest authority of the platform and provides security protection functions such as modifying login passwords, retrieving passwords, two-factor authentication login, login access restrictions, and modifying login email for the security of the administrator account itself. It also supports obtaining API key information for system administrators and regional administrators, which is used to query platform-wide resource and operational information through APIs.

> The platform account system currently uses email addresses as the login accounts for the platform. Before using them, ensure that the provided email address is valid for receiving alert emails or resetting passwords.

The platform defaults to using the email address admin@xxx.cn as the administrator account of the platform. When using the administrator account to log in to the console, it will automatically log in to the administrator console. When using a tenant account to log in to the console, it will automatically log in to the tenant console. Administrators can view account basic information and security settings on the Account and Organization Management / My Account page.

### 2.1.1 Modify Login Password

When using the administrator account, if you need to change the administrator password, you can modify it through the "Account and Organization Management" - "My Account" - "Account Security" - "Login Password" entrance. When modifying the password, you need to provide the current login password. After confirming that the current login password is correct, you can complete the account password modification.

If you forget your current login password, you can reset the password of the administrator account through the "Forgot Password" function to avoid being unable to log in to the console.

### 2.1.2 Forgot Password

The platform supports the administrator account to independently retrieve the password through the console when forgetting the password. During password retrieval, email verification is required, and the administrator account added must be a real and valid email address. Through the [Forgot Password] function on the login page, you can use the email address to verify and set a new password for the account.

After successfully setting the password, you can log in to the cloud platform using the newly set password to operate and manage cloud platform resources.

### 2.1.3 Two-Factor Authentication Login

The platform provides free TOTP (Time-Based One-Time Password Algorithm) login two-factor authentication services for administrator accounts. After opening this service, administrators need to verify each time they log in to the console. It supports national encryption hardware version and ordinary software version. Users can configure it according to their needs through deployment. To reduce the risk of account password leakage, it is recommended to enable account login. The steps to enable it are as follows:

1. Log in to the console and enter the account console. You can perform operations through the "Account and Organization Management" - "My Account" - "Account Security" - "Login Protection" entrance, as shown in the figure below:

   ![loginprotect](/assets/images/adminguide/login.png)

   Click next, the operation of the national encryption hardware version is as follows, enter the hardware SN, which cannot be empty:

   ![loginprotect](/assets/images/adminguide/login01.png)

   The operation of the ordinary software version is as follows:

   ![loginprotect](/assets/images/adminguide/loginprotect.png)

2. Check whether FortiToken is installed on the mobile device:

- The page provides IOS and Android user tool download addresses. If you have not installed FortiToken, you can download it by scanning the code.
- Android phone users can also search and download FortiToken through the application store provided by their mobile phone brand.

3. Open the FortiToken tool, scan the code to obtain the authorization code, or manually enter the key to obtain the authorization code.

4. Enter the authorization code obtained in the page box to complete the binding.

After enabling two-factor authentication, the administrator account needs to pass secondary authentication every time it logs in to the console. When logging in, after entering the account password, you need to additionally enter the authorization code to successfully log in to the account, as shown in the figure below:

![fortitoken](/assets/images/adminguide/fortitokenlogin.png)

You can close the two-factor authentication function by logging in to the console through the "Account and Organization Management" - "My Account" - "Account Security" - "Login Protection" entrance, and entering a 6-digit authorization code.

### 2.1.4 Login Access Restrictions

To ensure the security of account login and meet the needs of specific security scenarios, the platform provides the ability to restrict account login access, allowing administrators to set the client IP addresses for logging into the console and accessing APIs. After configuration, the administrator account can only log in or initiate API access from the specified IP address, ensuring the security of administrator login and resource management.

* Support configuration of multiple IP addresses or IP address ranges, with English commas used to separate multiple IP addresses/ranges.
* The configured IP address or IP address range is in whitelist mode, that is, only clients with the configured IP address/range can log in to the console or access APIs normally.
* By default, no IP is specified, which means there is no restriction on the client IP address that logs in to the console or accesses APIs. This means that the console can be accessed from anywhere on the Internet.

> To ensure that the administrator account can log in to the console normally, at least one reachable address segment must be configured within the login access range, otherwise it may cause login management failure.

Administrators can modify the login policy configuration through the "Account and Organization Management" - "My Account" - "Account Security" - "Access Restrictions" entrance, as shown in the figure below. The default is empty, meaning there are no restrictions on the entire network.

![LoginWhitelist](/assets/images/adminguide/LoginWhitelist.png)

You can enter the IP address or IP address range that can log in to the platform within the login access range, and click confirm to take effect. After successful configuration, the administrator account cannot log in to the console normally in an unspecified IP network, as shown in the figure below:

![LoginWhitelist1](/assets/images/adminguide/LoginWhitelist1.png)

The platform can only restrict the IP address that accesses the console directly, that is, the client IP address that requests the console URL address. If a user uses an administrator account to access the platform from a client address within NAT routing, when configuring the login policy, the platform needs to allow the IP address after NAT, that is, the exit address after NAT needs to be configured in the whitelist of the login access policy to ensure that clients within the NAT router can access the console normally.

### 2.1.5  Modify Login Email

The platform supports administrators to modify their account email address by clicking on the "Modify Login Email" option under the administrator account avatar in the upper right corner of the administrator console. This is used to change the administrator account to a real and usable email address that actually receives alert emails. The specific modification operation is shown in the following figure:

![updateadminemail](/assets/images/adminguide/updateadminemail.png)

After modifying the administrator's login email address, the original password can be used to log in to the console. You can reset the administrator's email password by modifying the login password.

### 2.1.6  Get API Key

Developers can obtain the administrator account's API key through the "Account and Organization Management" - "My Account" - "Account Security" - "API Key" or the "View API Key" option under the administrator account avatar in the upper right corner of the administrator console. This is used to use the platform-managed API interface to obtain information about platform-wide resources. The specific operation is shown in the following figure:

![adminapisec](/assets/images/adminguide/adminapisec.png)

Clicking the copy button can copy the information of the public and private keys for convenient calling of API commands.

## 2.2 Tenant 

The platform supports a multi-tenant mode, which is used for enterprises with a multi-level organizational structure. Tenants can be used as an independent company to operate and effectively implement permission management, avoid risks caused by shared resources among the parent company, subsidiaries, and different departments, and achieve resource auditing.

A tenant is a collection of resources within the platform that provides capabilities such as resource isolation, sub-account and permission control, quota and price configuration, etc. Resources between different tenants are strongly isolated through VPC networks and permissions; all main accounts, sub-accounts, resource usage fees, resource quotas, and approval processes are based on the tenant unit.

Tenants can be created by platform system administrators, and when creating a tenant, a user account will be generated by default, which is the main account of the tenant and also the administrator under the tenant. After successful registration, the administrator needs to recharge before using it. Platform system administrators can manage the life cycle of all tenants on the platform, including creating tenants, activating accounts, modifying names, freezing, unfreezing, login access restrictions, turning approval processes on and off, region authorizations, modifying email addresses, and deleting tenants. They can also manage main/sub-accounts, quota management, order management, transaction management, recharge management, price management, and resource overview viewing for tenants. For details, please refer to [Tenant Management](/CloudDocs_v2.x/adminguide/tenant.md)。

