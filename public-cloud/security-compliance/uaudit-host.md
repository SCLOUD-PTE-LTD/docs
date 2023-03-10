---
layout: default
title: UAuditHost
parent: Security Compliance
grand_parent: Public Cloud
permalink: /public-cloud/security-compliance/uaudithost/
nav_order: 1
---
# UAuditHost
## Product overview
The O&M audit system is also an O&M audit bastion host, which is used to control and audit the operation authority of O&M personnel.
The role of bastion host is mainly reflected in the following aspects:
- Eliminate abuse of permissions
- Violations were found
- Reduce human safety risks
- Reduce work complexity
Bastion host integrates O&M management and security, cuts off direct access to network and server resources, and uses protocol proxies to take over access to networks and servers. Figuratively speaking, the access of personnel to the cloud host needs to be translated by the bastion host.

## Explanation of terms
### Number of assets (or resources)
The sum of the number of hosts, network devices, and application publishers managed in the bastion host.
### Number of users
The sum of the number of employees in the company using bastion host.
The number of concurrent connections to the resource
Refers to the number of resource sessions (including hosts, network devices, and application publishers) that log in at the same time.
### App publisher
By deploying client software and browsers such as `plsql` on a Windows Server system that supports Remote Desktop (RDP). Enable users to remotely access client applications and web applications, fill in application usernames and passwords, and record audit user operations through video. (For more information about deploying the application publisher, please refer to the application publisher manual).
### Automatic login
Automatic login: If you have entered the resource account name password, you do not need to enter the resource account name password to log in.
Log in manually
Manual login means that you also need to enter the account name and password of the resource to log in.
### Privilege Escalation Login
Privilege login means that there is a privileged account on the resource, and you can switch to the normal account to log in.

## Usage scenarios
### Shared accounts are difficult to control
The company has a large number of employees, many departments, and a large number of hosts, and business handover, personnel replacement departments, and personnel resignations between different departments of the company also occur from time to time, which all lead to scattered operation authority and difficult to manage.
Once there is a problem with the use of shared accounts, it is difficult to hold specific individuals accountable afterwards.
Account (account management) can provide employee account + host account management function, easy to divide departments, groups, permission assignment, change and cancellation is easily completed!
### Device passwords are difficult to manage
According to their own habits, company employees use various protocols such as `SSH, VNCC, Telnet`, etc., and it is difficult for administrators to set up unified authentication and locate problems. After security vulnerabilities occur in various O&M tools such as `SecureCRT` and `Xshell`, it is difficult to ensure that everyone completes the upgrade defense in a timely manner.
Authentication (Authentication Management) uses the flagship version of bastion host as a secure unified authentication portal for centralized management, so that employees can still maintain their own operating habits, apply various common protocols, and use original O&M tools.
### The operation behavior is difficult to restrict
The company's outsourced personnel need to open a series of permissions for the convenience of work, but the open permissions cannot achieve restrictions and supervision.
UHAS's Authorization assigns sufficient operation and maintenance permissions to outsourced personnel, and the company assigns administrators and auditors to supervise and audit the permissions of outsourced developers and conduct audits for easy management!
### The operation process is not transparent
The operation process of the company's important business systems and the outsourcing team hired are not transparent, and high-risk operations cannot be supervised for enterprises, and once the system is deleted or a backdoor is installed, it will deal a fatal blow to the enterprise; For O&M personnel, security incidents cannot prove their innocence and cannot locate the source.
Audit administrators can monitor and suspend the operations of O&M personnel in real time, and can audit all operations that have passed through the bastion host, and the audit records cannot be tampered with.

## Ultimate product features
### Deployment method
Physical bypass and logical serial mode do not affect normal service traffic

HA dual machine hot standby

It supports horizontally scalable cluster architecture design and deployment, and supports cross-region, cross-data center, and multi-level deployment

NAT address mapping deployment is supported to access the bastion host through the mapped IP address

### Resource management
Support SSH, RDP, VNC, Telnet, FTP, SFTP and other protocols

Support FTP protocol, and can use commonly used FTP tools such as FlashFXP and FileZilla

Support SFTP protocol, and can use WinSCP, FlashFXP, xftp and other common tools

Supports batch import, export, delete, and join resource groups on hosts (including hosts, applications, application servers, and resource accounts) and accounts

Supports batch addition of cloud host resources, including Alibaba Cloud, Baidu Cloud, HUAWEI CLOUD, Tencent Cloud, and SCloud Cloud Platform

Extended support for applications/clients such as `MySQL, SQL Server, Oracle, IE, Firefox, Chrome, VNC Client, SecBrowser, VSphere Client` types, etc. can be implemented through application publishing

Different resources can use the same IP address or domain name

Support for built-in common system types, including `Linux, Windows, H3C, Huawei, Cisco`

Each user can tag each resource and add and delete tags in batches

Support `TELNET` and `SSH` protocol resources to automatically switch to root (or enable) accounts using ordinary accounts

Support `SSH, RDP` protocol file management and control functions

Support `RDP` clipboard control function of `RDP` protocol

Support resource account settings for automatic login (including elevated login) and manual login, among which manual login mode is divided into full manual (manually enter account and password) and semi-automatic mode (manual password entry)

Without installing any client, you can log in to the bastion host and access management resources with Windows, Linux, MAC OS and other operating systems
Support IE, Edge, Chrome, FireFox, Safari and other mainstream browsers

Support `Xshell`, `putty`, `MAC terminal` and other clients and Remote Broswer (`HTML5`) to access target resources, support two-person authorization and multi-factor authentication, O&M resources can be displayed in pages, and can be searched according to name, IP, tags and other conditions

Supports simultaneous access to multiple devices through bastion host

Bastion host supports executing the same command on multiple VMs/servers at the same time

Provides file storage in the form of cloud disks, supports file upload and download of RDP, SSH, and VNC protocol hosts, and conducts audits

### Resource O&M
Supports batch login of SSH, RDP, TELNET, AND VNC protocol resources

SSH clients, FTP clients, and SFTP clients can access target resources

Support for accessing target support through web pages, including SSH, RDP, TELNET, VNC, and application publishing resources

SSH key login is supported

Multiple SSH and TELNET protocol resources can be used to execute operation instructions in batches

Supports exporting O&M resource lists to configurations in `xshell` and SecureCRT formats

Supports filtering resources by tags

During O&M, session collaboration is supported, and other users can be invited to participate and assist in operations

In the process of session collaboration, participants control the session and creators are supported to forcibly obtain control

Multiple participants can enter a session using the same session invitation link

Support character protocol preset command function, can add 15 frequently used commands in the system

### User management
Authentication types such as local, RADIUS, and AD domains are supported

Support multi-factor authentication such as SMS and dynamic tokens

You can restrict user access to bastion host by setting source IP address control and access period control

Support user IP address (blacklist or whitelist) and MAC address restriction (blacklist or whitelist) restrictions, illegal addresses cannot be logged in

Supports batch modification of users, including resetting passwords, moving departments, changing roles, modifying multi-factor configurations, modifying validity periods, modifying IP restrictions, and modifying MAC restrictions

When creating a new user, randomly generating strong passwords is supported

You can filter users by their status, role, and department

Custom roles are supported to meet the needs of customers in complex and diverse business scenarios

Supports effective and expiration time settings for master accounts

Support department decentralization of user accounts and target devices, and different users and devices can belong to different departments (subdepartments)

Access control policies are supported by department, and configuration administrators of different departments can only set access permissions for devices in their own departments and their direct subordinate departments

Support the decentralization of password change planning departments, so that password keepers in different departments can only modify/store account passwords on devices in their own departments

Support the decentralization of audit functions by department, so that audit administrators of different departments can only audit the operation logs on devices in their own departments and their direct sub-departments

Supports batch import, export, deletion, password reset, and department movement of the master account

### Department management
Administrators belonging to different business units can only manage users, resources, policies, and audit management within their permissions

Support for setting scopes of users and resources that administrators can manage

Support unlimited group management of departments

Support quickly create and modify departments

Support batch creation of new departments

Support to quickly locate users and hosts in the department, and display the number of users and hosts

### Access policies and dynamic authorization
Without installing any client, you can log on to remote resources published by protocols and applications such as RDP, VNC, Telnet, and SSH

You can set many-to-many resource access authorization with users, user groups, resources, resource groups, accounts, and account groups as core elements

Detailed command permission control policies can be set based on the core elements of users, user groups, departments, roles, resources, IP addresses, command sets, and effective times

Command permission control actions include deny execution, allow execution, alarm, dynamic authorization, and disconnect

Bastion host itself prefabricates basic commands for hosts and network devices, and 
users can customize commands according to specific scenarios

You can set access policies such as alarm, disconnection, denial of execution, and secondary authorization for the operation behavior of devices with character protocols

Fine-grained restriction of user access time

Users can actively apply for O&M permissions for resources from administrators

In the mode based on user groups and account groups, new members of user groups and account groups automatically inherit access control and command control relationships

Supports dragging to change the priority order of policies

Supports enabling and disabling policies in bulk

Access control policies are set based on users, user groups, resource accounts, account groups, validity periods, file management controls, file transfer controls (uploads and downloads), RDP clipboard controls, time limits, and IP restrictions

Access control policies support the configuration of two-person authorization candidates, which require on-site approval by administrators for core devices

Support for setting matching rules for action commands in command control policies

### User and group management features
Support department decentralization of user accounts and target devices, and different users and devices can belong to different departments (subdepartments)

Access control policies are supported by department, and configuration administrators of different departments can only set access permissions for devices in their own departments and their direct subordinate departments

Support the decentralization of password change planning departments, so that password keepers in different departments can only modify/store account passwords on devices in their own departments

Support the decentralization of audit functions by department, so that audit administrators of different departments can only audit the operation logs on devices in their own departments and their direct sub-departments

### Resource and resource group management functions
You can manage resources in batches by grouping

### Work order management
Users can actively apply for O&M permissions for resources from administrators

Supports file management permissions, RDP clipboard permissions, and upload and download permissions

### Record of operations
Accurate identification of operation commands with 100% accuracy

Support the audit of character protocol SSH, TELNET, file transfer protocol FTP, SFTP, and record the execution results of operation instructions and operation instructions in detail

Support web page anti-jump function of secure browser

Supports exporting historical sessions and system logs

Supports end-of-session state auditing

Supports the recording of clipboard copying file behavior and text information content, and supports locating audit playback by searching for text content keywords

Support two-person authorization audit and collaborative user audit

Support a variety of system reports and operation and maintenance report templates built-in in the system, and automatically generate reports on a daily, weekly, and monthly cycle

The report format supports Word, Excel, PDF and HTML formats

Support resource logon sessions associated with system logon sessions

Supports text download commands to the local PC

Text audit is carried out on four categories of information: keyboard and mouse operation, clipboard operation, title bar operation, and text fuzzy recognition of the graphical interface

The FTP protocol can be audited, and the execution of the file upload and download functions of the bastion host itself can be audited

### Session replay
Supports the playback process from a command to the user, and pauses and accelerates the playback process

The input and output of user command operations are displayed on the same interface

Support online playback process, support playback speed adjustment, drag, pause, stop, replay and other playback control operations

Supports web online video playback to reproduce all operations performed by O&M personnel on resources

Supports offline playback to reproduce all operations of O&M personnel on resources, and supports downloading playback files to local playback

Graphical search can be carried out according to the content of the text audit for keywords, and the search results can be directly located to the relevant graphic screen for playback

Supports arbitrary switching of audits of the same virtual machine

### Secret change plan
You can generate a detailed password change plan based on the account, time, change cycle, and password change method, and automatically execute it when it expires

The password change method can support randomly generating different passwords, randomly generating the same password, and manually specifying the same password

You can send automatic password change results to the administrator mailbox of the specified password change plan

You can set the password change policy based on the resource account, account group, password change method, and execution mode

You can view the password change log to understand the total number of password change accounts, the number of successful password changes, the number of failed password changes, and the number of unmodified accounts

You can download the password change log to view password changes before and after the password change

The password change policy supports whether to use privileged accounts to change passwords and whether to modify privileged account passwords

### Real-time monitoring
Supports real-time monitoring and real-time cut-off of any type of active session without delay

### System maintenance

Supports full or incremental system backup

You can restore the system by uploading and restoring files

You can upgrade by importing an upgrade package with one click

Supports desktop display based on different roles and permissions

Supports statistics on the number of users, hosts, applications, application servers, and alarms

Supports host and application type statistics

Supports statistics on current active sessions and new sessions added today

Supports weekly and monthly trend charts of system logons and resource O&M

Support the Top5 display of O&M users and O&M resources

Support the display of recently logged in hosts and applications, and provide the ability to log in resources from the desktop

Support system status and system information display

Support for modifying personal information

Users can automatically lock their accounts or IPs if they fail to log in multiple times, and can configure the unlock duration, automatic unlocking upon expiration, or manual unlocking

Support built-in OpenVPN client

Supports external authentication methods such as RADIUS and AD domain, and supports configuring multiple AD domains

Supports automatic or manual deletion of stored data

Supports factory reset in the web interface

Supports system configuration backup and restore

Supports hot standby of dual machines

Supports spatial self-management to automatically clean historical data and automatically overwrite data when space is low

Supports automatic backup of log data to remote syslog servers

Supports outgoing notifications, including email and custom SMS gateways

Support for customizing the system language (Chinese and English) and system icons

Supports asynchronous operation tasks, real-time view of task progress, and termination of tasks

You can set whether and how to alarm based on message level and message type

Support WeChat mini program mobile token

Supports binding SSH public keys to achieve password-free login

You can view the permissions of the user's own role and understand the scope of permissions

You can view your own system logon logs, system operation logs, and resource logon logs

Supports web certificate replacement

Supports web and SSH login timeout settings

SNMP is supported, and versions include v2c and v3

Supports network multi-interface, static routing, and DNS settings

Supports custom alert methods and levels for system events

Support custom ticket application scope

You can modify the default port of the system to provide external services

Supports automatic backup of configuration and data to remote FTP and SFTP server storage

Support network diagnostics such as `ping, traceroute, and telnet`

Supports the collection of operating status information such as system load, kernel information, memory information, network card information, disk usage information, routing table information, and ARP table information

Supports downloading backups to local storage