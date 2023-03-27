---
layout: default
title: PostgreSQL
parent: Database
grand_parent: Public Cloud
permalink: /public-cloud/database/postgresql/
nav_order: 3
---
# UDB PostgreSQL
## Introduction
### What is PostgreSQL
After more than 30 years of active development, PostgreSQL open source database has grown into a powerful database software with mature architecture, reliable and robust, complete functions, and complete implementation of SQL specifications. At the same time, strong extensibility and rich plug-in system make PostgreSQL have more advanced features, some of which are not available in most other databases.

SCloud Cloud cloud database UDB PostgreSQL is a stable and secure fully managed online PostgreSQL database service based on mature cloud computing technology, with high availability, high performance and elastic scalability. Cloud database for PostgreSQL provides a complete set of solutions such as disaster recovery, backup, and monitoring, completely solving the troubles of database operation and maintenance.

## Key concepts
### Instance type
PostgreSQL instances currently support Standard and High Availability instances.
version
PostgreSQL instances currently support PostgreSQL 9.4, PostgreSQL 9.6, and PostgreSQL 10.4, and users can choose the corresponding cloud database version according to their needs.
### Database model
PostgreSQL instances are currently available in standard models and SSD models to meet the needs of users.
### Memory
The memory size of the cloud database. Users can choose based on their hardware requirements for the cloud database.
### Hard disk
The size of the hard disk of the  cloud database for the ECB. Users can choose based on their hardware requirements for the cloud database.
### Payment method
Payment methods are divided into three methods: annual, monthly, and timely, and the payment methods are prepaid, that is, the fees for the corresponding service cycle are paid in advance.
For detailed billing instructions, please refer to the "Purchase and Billing" document.
### Quantity
By default, the number of CVDBS that you need to apply for is `1`, and you can select multiple databases to create them in batches.
### Node
PostgreSQL currently supports Master nodes and Standby nodes.
### Configuration file
Configuration files include various configuration parameters for the operation of  cloud database for ECSD, which can be customized and modified as needed, and default profiles are provided for different  cloud database versions.
Profiles include default profiles and custom profiles, which are created and imported by users.
### Administrator
By default, the super administrator (`root`) privilege is provided, allowing users to customize the administrator password.
### Instance name
You can customize the name of the  cloud database instance.
### Resource ID
After you create an  cloud database instance, the system automatically generates a resource ID, which is *globally unique*.
### IP and port
The IP address is the private network address of the user accessing the CVD, which is automatically generated after the CVD is created, and the external IP address is not currently provided.
The default PostgreSQL port is `5432`.
### Backup
The backup stores all the data of the cloud database at a certain point in time.  cloud database provides automatic backup and manual backup to prevent data loss and avoid risks caused by mis-operation.
### Log
A log is a log file used to record the operation events of a cloud database. Include database logs.
## Product advantages
### Data is safe and secure
UDB PostgreSQL multiple lines of defense ensure data security and reliability, and high-reliability hardware ensures the storage of data. The highly available instances of PostgreSQL guarantee redundant storage with multiple pieces of data, while users can even use the "Create Slave Library" feature to create backups of more database data, further increasing data security. UDB PostgreSQL supports the "second-level rollback" function, which allows users to use the second-level rollback function to restore data to any second in the past 7 days when users have human mis-operation. "Second-level rollback" can become a "reassuring pill" for users to use UDB PostgreSQL products.
### The protocol is fully compatible
UDB PostgreSQL is 100% fully compatible with the PostgreSQL protocol, and some typical PostgreSQL extensions (such as PostGIS `geodatabases`) are installed by default to achieve an "out-of-the-box" PostgreSQL service experience.
### Performance
Provides fast and efficient database query and transaction processing capabilities to easily cope with high-concurrency and large-scale data processing requirements. Every UDB PostgreSQL instance is backed by powerful hardware. When users have high requirements for hardware and performance, they can choose high-performance SSD disks as storage media; SCloud's OS kernel team has performed kernel-level tuning for each server running UDB PostgreSQL instances to ensure high-performance operation of the system. The parameters of UDB PostgreSQL instances have also been tuned by professional DBAs to ensure that they can cope with most usage scenarios.
### High availability
UDB PostgreSQL supports highly available deployments, that is, UDB PostgreSQL highly available instances. UDB PostgreSQL high-availability instances adopt a master-slave replication architecture, where the master database provides services while another set of database services continuously synchronizes data and is ready to standby, the general architecture is shown in the figure:

![1](/assets/images/pg-v4.png)

The powerful automatic disaster recovery module in the UDB background can automatically detect problems when UDB PostgreSQL high-availability instance services are problematic and automatically perform disaster recovery to ensure the stability and reliability of user PostgreSQL database services. When a UDB PostgreSQL instance is switched, the disaster recovery module promotes the standby PostgreSQL service to the master database and falls back to the slave database after the original master service is started. The entire process does not require any manual intervention and configuration modification. The schematic diagram of the entire disaster recovery is shown in the following figure:

![1](/assets/images/pg-v4-01.png)

### Tackle complex scenarios with ease
It can easily cope with business scenarios with larger data volumes and more complex data types, such as data mining and map spatial data. PostGIS extension supports standards in the field of geography and is suitable for business scenarios such as maps and LBS.
### Rapid deployment
Instances can be quickly deployed online, saving self-built databases such as procurement, deployment, and configuration, shortening the project cycle, and helping services go online quickly.
### Flexible and elastic scaling
UDB PostgreSQL can dynamically expand database resources on demand according to business needs. With a few clicks on the console, users can dynamically adjust the memory and disk resources of UDB PostgreSQL instances to meet the database performance and storage space requirements of different business stages. Especially for UDB PostgreSQL high-availability instances, during the process of resource expansion, the user's database service can be basically non-stop, with only a second-level flash disconnection. This can greatly reduce the impact time of database expansion on database services, and achieve a real "hot upgrade".
### Flexible and easy to use
Daily operation and maintenance automation, UDB PostgreSQL greatly simplifies the daily operation and maintenance work of DBAs: through the click of the interface, DBAs can create database services running the standard PostgreSQL protocol with one click in minutes; The UDB PostgreSQL service automatically backs up every day and uploads it to a highly reliable backup storage cluster, eliminating the need for users to perform daily backups. UDB PostgreSQL collects database monitoring data in real time, and users can receive various database exception alerts in time by configuring monitoring thresholds.
### Lower cost
It can immediately provision the required resources according to business needs, eliminating the need to purchase high-cost hardware in the early stage of the business, effectively reducing the initial asset investment and avoiding idle waste of resources.
## Product price
### Billing formula

```
Fee = (Memory Specification× Memory Unit Price + Hard Disk Specification ×Hard Disk Unit Price) × Purchase Duration
```