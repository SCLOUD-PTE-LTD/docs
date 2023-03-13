---
layout: default
title: UDSS
parent: Security Compliance
grand_parent: Public Cloud
permalink: /public-cloud/security-compliance/udss/
nav_order: 2
---
# UDSS - Data Security Solutions
## Product overview
The cloud database audit system is the first two-way audit mechanism, covering applications, middleware, and databases, achieving the three-dimensional defense effect of "pre-event prevention + in-event prevention and post-event evidence collection". The audit system adopts database deep message protocol parsing technology DPI and flowing coal body analysis technology DFI, etc., to parse and restore various access operations of the database to database-level operation statements, and intelligently analyze and monitor various operations of visitors through preset security rule matching, conduct real-time threat warning, and statistically analyze and record events, locate multiple identities, and effectively support electronic forensics.

The auditing system reviews audit and transaction logs to track various behaviors on database operations. A general audit records operations on the database, changes to the database, who performs the operation of the project, and other attributes. These databases are documented in an independent platform of the audit system with high accuracy and completeness. When conducting forensic inspections on database activities or status, audits can accurately feedback various changes in the database and provide evidence for various normal, abnormal, and illegal operations of the database.

The system adopts the mode of separation of powers, including three platforms, system management platform, security management platform, and audit management platform, each platform has different functions and responsibilities, different permissions, and mutual supervision. If the system administrator is responsible for the operation settings of the device; The security administrator is responsible for viewing relevant audit records and rule violations; The audit logist is responsible for viewing the operation log of the overall device.

## Product value
### Accurate visibility, safe and controllable
The user-friendly interface visually displays the current audit results according to the total score structure, helps users quickly understand the risk situation of their system, and quickly confirms and reviews according to the current risk situation.
### Traceability analysis, accurate responsibility
Through the business association function and audit function, statistical analysis is carried out from various dimensions, combined with the original user operation audit record, and the ability to quickly retrieve the original data is provided, so that users can quickly confirm the source of the problem, accurately locate the security incident, hold it to the person, and trace the source and collect evidence.
### Asset data security
By recording, analyzing, and reporting users' access to databases, it helps users generate compliance reports and trace the root cause of accidents after the fact, and strengthens internal and external database network behavior records to improve the security of data assets.
### Access control
When those privileged accounts have mis-operated or illegally performed, especially O&M personnel, some operations of the user can be blocked and controlled and timely alarmed, so as to reduce the impact and loss. It is also possible to block access to the database by controlling access to only certain IP ranges. Improve data asset security.
### Meet compliance requirements
To help users meet compliance requirements such as equal protection and reinsurance, governments and industries pay more and more attention to information security, and many relevant standards have been proposed to ensure the network security of various units. Government administrative institutions or state-owned enterprises have compliance requirements for protection and reinsurance. SCloud provides an independent audit program that helps improve the organization's internal control and audit system to meet various compliance requirements and enable the organization to pass the audit.

## Scope of support
### Database support type
(1) Relational databases: `Sqlserver, Oracle, Sybase, DB2, Informix, PostgreSQL, MariaDB, XUGU (virtual valley)`

(2) Domestic databases: `Dameng, Renmin University Golden Warehouse, Shentong Database, Nanda General`

(3) Post-relational database: `Caché DB`

(4) In-memory database: `HANA, Redis`

(5) Big data database: `RECORD_HIVE, SPARK_JAVA_API, Hive, HIVE_HSQL, Record_Hive, ES, GaussDB (Huawei Gauss), LibrA, HBASE_SHELL, Solr, MongoDB`

(6) Industrial control real-time database: `IP21_API, IP21_APIIP21_WEBSERVICE`

(7) Special cloud database: Alibaba Cloud `_PostgreSQL` - The current version does not support RDS

### Illustrate
Elastic database auditing only supports the audit function, not the command blocking function. You also need to deploy agents on App Service clients that access the database.

Agent: Forwards database protocol traffic to the audit end to implement audit operations, and supports simultaneous configuration and drainage on multiple audit objects
### Browser compatibility
This system adopts B/S architecture and supports IE10 and above browsers, Chrome browser, Firefox Firefox browser. In order to ensure a good browsing effect, it is recommended to use the Chrome browser.

## Features
### Platform high availability design
Audit platform abnormal operation records and alarms: When someone illegally operates or destroys the equipment, the system will send SMS or email in real time and make detailed records. 24-hour automatic monitoring.

System operation monitoring: Monitor the database audit system operation status, when the equipment has problems or insufficient disk space, the relevant person in charge will be notified to deal with it.

### Database auditing capabilities
Intelligent rule base: supports custom rules, and uses different rule configurations to make the system more in line with the different management requirements of each enterprise.

Multi-dimensional identity fingerprint: The system supports work number, source IP, MAC, user name, process, operating system type, operating system user name, identity identification and auditing, and multiple positioning.

Database abnormal operation recording and backtracking: Record all detailed operations on the database, when, who, and how to operate, and support event backtracking.

Audit logs: Supports query, analysis, statistics, and printing of audit logs.

Separation of powers: Provide audit administrators, rule administrators, and system administrators to achieve separation of powers, mutually restrain each other, and meet the requirements of equal protection.

Statistical reports: including the source IP address access ranking and database logon failure ranking.

Advanced auditing: Supports database command auditing of complex combinations of bound variables and nested statements. Supports three-layer fuzzy association and HTTP protocol auditing.

MS SQL for audit encryption protocols: Supports MS SQL Server encrypted identity deciphering to solve the problem that MS SQL Server 2005 and later databases cannot see the identity.

### Management scope
Supports self-built databases

Supports SCloud's UDB database

Provides an Openstack interface solution to support security audits of big data platforms

## Description of the data disk
(1) Disk calculation method

According to the number of SQL records in the database, 10,000 SQL messages per second, the amount of data in one month, according to the peak calculation of about 150 GB, customer cases are estimated to be about 100 GB.

(2) Disk storage instructions

The storage period of audit logs for database audit depends on the size of the data disk space, and the default rule is that no logs are deleted.

When disk space is low, the earliest audit log will be overwritten by itself, and customers can manually select the log that needs to be deleted according to the date.