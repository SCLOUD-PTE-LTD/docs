---
layout: default
title: Main Account
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/main-account/
nav_order: 2
---
# 2. Main Account

## 2.1 Main Account Registration and Login

This platform supports multi-tenant mode, where tenants are the main accounts. The platform administrator can create main accounts and recharge them through the administrator console, and the platform also provides a self-registration process. Users can register and use the cloud platform through the registration link. They can enter the account page through the platform registration link and register for a simple account.

![register](/assets/images/userguide/register.png)

1. As shown in the registration interface above, the information required for registration is as follows:
   * Account name: The name of the registered account, only supports Chinese and English characters, with a length between 5-50 characters.
   * Login email: The email account used to log in to the cloud platform, the email account needs to support receiving verification emails.
   * Login Password: Password complexity modification is allowed, the password must contain two or more of the following: uppercase letters, lowercase letters, numbers, and special characters (except for spaces), and the password length must be 6-30 characters.
2. After submitting the registration, the platform will send an activation email to the email account, you can log in to the mailbox to complete the activation operation.
3. After clicking the link for the **Private Cloud Platform Activation Account** email in your mailbox, complete the registration as shown in the following picture.

![register1](/assets/images/userguide/register1.png)

4. Login to the private cloud platform with the registered email and password, this article uses **[Private Cloud Platform Online POC Environment]** as an example.

![login.png](/assets/images/userguide/login.png)

> Cloud platform resources support metering and billing, and before use, you need to contact the platform administrator to recharge the account before you can create and use the resources normally.

5. After logging in successfully, the overview page of the private cloud platform will be displayed to the user, as shown in the following figure:

![generalview](/assets/images/userguide/generalview.png)

When users log in to the cloud platform console, they will be directed to the overview page by default. The overview page displays the comprehensive statistics of the current login account, including the alarm records of the last 12 hours, resource status information, system information, top 5 CPU usage of virtual machines, top 5 memory usage of virtual machines, top 3 total read throughput of virtual machines, top 3 total write throughput of virtual machines, and resource quota usage. The resource status information includes the total number, unstable status number and expired resource list of virtual machines, disks and external IPs, and users can log out of the platform.

- The recent 12-hour alarm record information displays the alarm records of the current account's resources in the last 12 hours, which is convenient for quickly viewing the resource health status.
- Resource status information: the total number of virtual machines, disks, and external IPs under the current account, the number of unstable states, and the list of expired resources.
- System Information: System information of the cloud platform, including system NTP service, billing function, resource automatic renewal function, and whether resources can be deleted.
- Top 5 Virtual Machine CPU Usage: The top 5 virtual machines with the highest CPU usage under the current account.
- Top 5 Virtual Machine Memory Usage: The top 5 virtual machines with the highest memory usage under the current account.
- Top 3 Virtual Machine Disk Total Read Throughput: The top 3 virtual machines with the highest total read throughput under the current account.
- Top 3 Virtual Machine Disk Total Write Throughput: The top 3 virtual machines with the highest total write throughput under the current account.
- Resource Quota Usage: Current account quota and usage information for various resources, resource quota values can be configured through the administrator console, and current tenant resource quota can be viewed through account information.

By clicking the alarm record on the overview page, you can enter the alarm history record page to view more alarm records. Click the resource ID to enter the resource detail page to view the resource detail information. Click the "View More" button of quota usage to enter the quota information page and view more resource quota information.

## 2.2 OAuth Login Authentication

The platform supports third-party OAuth 2.0 login authentication, users can log in and use the cloud platform resources by connecting the enterprise's unified OAuth authentication system with the cloud platform.

As shown in the login example in Chapter 2.1, the platform has been docked with the OAuth 2.0 system and provides a third-party login entry on the login page. Enterprise users can log in to the cloud platform through their own OAuth unified authentication platform, and can also log in to the cloud platform through the third-party login entry of the cloud platform with a unified user password, as shown in the following figure:

![OAuth](/assets/images/userguide/OAuth.png)

After the user logs in to the third-party login platform using a username and password, they can be redirected to the cloud platform overview page, using a unified user authentication method to manage cloud platform resources.

## 2.3 Retrieve Password

The platform supports the main account to retrieve the password independently through the console when forgetting the password. Verification needs to be done through the email when retrieving the password. Please make sure the account added by the administrator is a real and available email. Through the "Retrieve Password" function on the login page, you can use the email address to verify and reset the new password for the main account, as shown in the following figure:

![resetpass](/assets/images/userguide/resetpass.png)

After resetting the password successfully, you can log in to the cloud platform with the newly set password to use and manage the cloud platform resources.







