---
layout: default
title: Master Account
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/master-account/
nav_order: 1
---
# Master Account
## Master account registration and login
SCloudStack cloud platform supports multi-tenant mode, the tenant is the main account, the platform administrator can independently create the master account and recharge the main account through the administrator console, and the platform provides a self-service registration process, users can automatically register and use the cloud platform through the registration link. You can enter the account page through the platform registration link for simple account registration.

 ![1](/assets/images/user-guide-1.png)


As shown on the registration screen in the figure above, the information required for registration is as follows:
- Login email: the email account used to log in to the cloud platform, and the email account needs to support receiving verification emails;
- Login password: the password must contain two types of uppercase and lowercase letters, numbers and symbols, and the length of the password is 6-20 characters;

After submitting the registration, the platform will send an activation email to the email account, and you can log in to the email to complete the activation operation;

Click the link in the "SCloudStack Activate Account" email in the mailbox to complete the registration.

Log in to the SCloudStack cloud platform with a registered email and password, and this article uses [SCloudStack Online POC Environment] as an example.

 ![1](/assets/images/user-guide-2.png)

Cloud platform resources support metering and billing, and you need to contact the platform administrator to recharge the account before using it before you can create and use resources normally.

After logging in successfully, the user is presented with the overview page of SCloudStack Cloud Platform, as shown in the following figure:

 ![1](/assets/images/user-guide-3.png)

The overview is the comprehensive statistical information and resources of the current login account, including login account information, account balance, free balance, alarm record information in the last 12 hours, resource utilization statistics and account quota usage information, etc., and can log out of the platform:
- The login account information displays the account attributes and email address of the current login platform, and if you use a sub-account to log in to the platform, it is displayed as a sub-account and email address.
- The alarm record information in the last 12 hours displays the alarm records of the last 12 hours of the resource owned by the current account, which is convenient for you to quickly view the resource health status.
- Resource utilization statistics: The average usage of CPU, memory, disk, and bandwidth of the current account, as shown in the figure above, the average CPU usage is about 2%.
- Quota usage information: The quota and usage information of various resources of the current account, including resource quotas such as virtual machines, images, hard disks, NICs, VPCs, public IP addresses, security groups, NAT gateways, load balancers, VPN gateways, Redis, and MySQL.

Click the account name on the Overview page to enter the account details page and view the details of the current account, sub-accounts, and quota information. 

Click Alarm Records to go to the Alarm History page to view more alarm records.