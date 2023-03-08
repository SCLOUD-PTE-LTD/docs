---
layout: default
title: MySQL
parent: Database
grand_parent: Public Cloud
permalink: /public-cloud/database/mysql/
nav_order: 1
---
# MySQL UDB
## What is a cloud database
ApsaraDB for UDB MYSQL is a highly available and high-performance database service based on mature cloud computing technology, allowing you to deploy, set, operate, and scale in tens of seconds. Provides a complete set of solutions such as dual-primary hot standby architecture, backup, data rollback, read/write splitting, monitoring, and database auditing, which greatly simplifies database O&M and helps you focus on application R&D and business development.

UDB MySQL is fully compatible with the MySQL 5.5, MySQL 5.6, MySQL 5.7, Percona 5.5, Percona 5.6 and Percona 5.7 protocols.
## Key concepts
SCloud cloud database MySQL products mainly use the following terms, in order to facilitate and quickly use, do the following analysis:
### Instance type
MySQL instances include Standard Edition and High Availability Edition instance types.

Basic Edition instances provide basic database single instances, Standard Edition NVMe models provide MySQL instances provide a single database node, and data storage is an elastic block storage device based on distributed storage architecture, separating computing and storage.

The HA instance adopts a dual-primary, hot-standby architecture, which completely solves the problem of database unavailability caused by downtime or hardware failure. NVMe models and SSD models are supported, and high availability is deployed across zones to achieve data center-level disaster recovery.

### Version
MySQL instances currently support MySQL 5.5, MySQL 5.6, MySQL 5.7, Percona 5.5, Percona 5.6, and Percona 5.7, and you can select the corresponding cloud database version according to your needs.

### Database model
MySQL instances are currently available with NVMe models and SSD models.

The SSD model is suitable for business scenarios that require high database performance, and the master-slave architecture supports free read/write splitting.

NVMe models are suitable for service scenarios with large-capacity and high-performance requirements, and use ultra-high-performance RDMA NVMe SSDs to support large-capacity (`32T`) storage and high cost performance.

### Memory
The memory size of the cloud database. Users can choose based on their hardware requirements for the cloud database.
### Hard disk
The size of the hard disk of the ApsaraDB for the ECB. Users can choose based on their hardware requirements for the cloud database.
### Payment method
Payment methods are divided into three methods: annual, monthly, and on-demand, and the payment methods are prepaid, that is, the corresponding service cycle is paid in advance.
For detailed billing instructions, please refer to the "Purchase and Billing" document.
### Quantity
By default, the number of CVDBS that you need to apply for is 1, and you can select multiple databases to create them in batches.
### Master vs. slave libraries
The main library supports read and write operations.
On the one hand, the slave database can be used as a disaster recovery node of the main database, and at the same time, it can also support read operations, reduce the pressure of the main database, and realize read/write separation.
### Configuration file
Configuration files include various configuration parameters for the operation of ApsaraDB for ECSD, which can be customized and modified as needed, and default profiles are provided for different ApsaraDB versions.
Profiles include default profiles and custom profiles, which are created and imported by users.
### Administrator
By default, the super administrator (`root`) privilege is provided, allowing users to customize the administrator password.
### Instance name
You can customize the name of the ApsaraDB instance.
### Resource ID
After you create an ApsaraDB instance, the system automatically generates a resource ID, which is globally unique.
### IP and port
The IP address is the private network address of the user accessing the CVD, which is automatically generated after the CVD is created, and the external IP address is not currently provided.
The default port for MySQL and Percona is 3306.
### Backup
The backup stores all the data of the cloud database at a certain point in time. ApsaraDB provides automatic backup and manual backup to prevent data loss and avoid risks caused by mis-operation. You can select either the master or slave database as the backup source, but you cannot back up from a slave database across zones.
### Log
A log is a log file used to record the operation events of a cloud database. Includes binary logs, slow query logs, error logs, operation logs

### ApsaraDB for UDB Service Level Agreement (SLA)
The SCloud UDB Service Level Agreement specifies the general service level indicators and service considerations that SCloud provides to customers with SCloud UDB cloud database services.
## Product advantages
### Highly available architecture
High-availability instances use a dual-primary hot standby architecture to completely solve database unavailability caused by downtime or hardware failure, and the stable and reliable performance far exceeds the industry average.
### High-performance storage media
The SSD model is provided to provide fast and efficient database query and transaction processing capabilities in business scenarios with 100 million data processing volumes, and easily cope with high concurrency and large-scale data processing requirements.

NVMe models are provided as elastic block storage devices based on distributed storage architecture, and data is stored on NVMe SSD disks (RSSD cloud disks), which separates computing and storage. 

RSSD cloud disks are based on a new generation of distributed block storage architecture, NVMe SSD is used as the storage medium at the bottom layer, and RDMA is used for network transmission, providing users with up to 1.2 million random read and write capabilities and lower single latency capabilities per disk.
### The database is secure and reliable
It provides master-slave architecture to effectively ensure data redundancy, combines automatic backup strategy with manual backup, and provides database rollback function to make valuable data foolproof.
### Rapid deployment
Instances can be quickly deployed online, saving self-built databases such as procurement, deployment, and configuration, shortening the project cycle, and helping services go online quickly.
### Elastic scaling
Database resources can be flexibly expanded according to business pressure to meet the requirements for database performance and storage space in different business stages.
### Flexible and easy to use
You can monitor, alarm, and audit the database through the console. Open API interfaces are provided to facilitate intelligent and systematic application architecture, making system O&M management more efficient and convenient.
### Lower cost
It can immediately provision the required resources according to business needs, eliminating the need to purchase high-cost hardware in the early stage of business, effectively reducing initial asset investment and avoiding idle waste of resources.
## Model version
### MySQL product models
ApsaraDB for UDB MySQL is available in SSD models and NVMe models, and the reliability, durability, and read and write performance of UDB-MySQL will meet the product SLA commitments.

For SSD instances, SSD disks stored on the same node as the database engine can reduce I/O latency and are suitable for business scenarios with high IO performance.

The database instance of the NVMe model adopts the industry's mainstream computing and storage separation architecture: the computing layer uses high-performance SCloud cloud hosts, and the storage layer adopts ultra-high-performance RDMA NVMe SSD cloud disks. RSSD cloud disks are based on a new generation of distributed block storage architecture, NVMe SSD is used as the storage medium at the bottom layer, and RDMA is used for network transmission, providing users with up to `1.2` million random read and write capabilities and lower single latency capabilities per disk. IOPS and capacity correspondence: IOPS optimization is a fixed value of `50,000` when the disk size is less than 1T, and IOPS is calculated according to the fixed allocation algorithm above 1T, and the calculation formula is: `1800 + 50 * size (GB)`, and the maximum IOPS is 1.2 million. The disk step size is 10GB, supports large-capacity storage, and the maximum capacity can reach 32T.

ApsaraDB for MySQL provides a high-availability dual-primary hot standby architecture, supports cross-zone architecture for high availability, and supports cross-zone slave database construction to achieve zone-level disaster recovery and ensure high service availability.

### MySQL capacity specifications

| Memory specification (G). | 2 | 4 | 8 | 12 | 16 | 24 | 32 | 48 and above |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hard disk specification (G) is recommended | 50 | 200 | 300 | 400 | 500 | 800 | 1000 | 2000~32000 |

### MySQL product version
The highly available SSD model MySQL supports MySQL 5.5/5.6/5.7 and Percona 5.5/5.6/5.7 protocols.

The high-availability SSD model MySQL+Slave supports MySQL 5.6/5.7 and Percona 5.6/5.7 protocols.

The high-availability/standard NVMe model MySQL supports MySQL 5.6/5.7 and Percona 5.6/5.7 protocols.

The high-availability/standard NVMe model MySQL+Slave slave architecture supports MySQL 5.6/5.7 and Percona 5.6/5.7 protocols.

## Product architecture
SCloud Cloud Relational MySQL High Availability Edition architecture:
Principle of disaster recovery:
The high-availability UDB >adopts a dual-master architecture and realizes data synchronization through Semi-Sync.

The UDB availability management module monitors the availability of the underlying node in real time, and once the master DB is detected, it triggers automatic disaster recovery and drift VIP to Standby DB to ensure the stability and reliability of user UDB database services.

Benefits for users: single VIP access, seamless switching of application layer, users do not need any manual intervention and configuration modification in the whole process.

## Contrast with self-built databases

| Contrast items | UDB-MySQL | UHost is self-built | Self-built database |
| --- | --- | --- | --- |
| Price | Elastic sources | Elastic sources | The sunk cost of one input is large |
| Core tuning for improved performance | The open source version has no performance optimization | The open source version has no performance optimization |
| Free of charge | You need to purchase resources as a backup space | Independent provision of resources is required, which is extremely costly |
| Usability | The high-availability version provides a dual-primary hot standby architecture to recover from failures in about 20 seconds | You need to purchase a high-availability system separately | You need to purchase a high-availability system separately |
| Enable read-write splitting free of charge to achieve load balancing, making read-write splitting easy to use | You need to implement or purchase Server Load Balancer alone | You need to implement or purchase a load balancing device alone |
| Reliability | High data reliability, supporting physical and logical backup, backup recovery, and second-level rollback | High reliability can only be achieved under a good architecture | Data reliability is average and depends on the probability of damage to a single disk |
| Ease of use | Automatic backup recovery system, support for point-in-time recovery, etc | There is no automatic backup system, and the streaming backup capability needs to be implemented separately, and the recovery function at a point in time is costly | There is no automatic backup system, and the streaming backup capability needs to be implemented separately, and the recovery function at a point in time is costly |
| Automatic monitoring and alarm system, support second-level monitoring and control, cover all performance indicators of actual cases and data databases, support SMS, Mail boxes and other ways | You need to purchase a monitoring and control system and configure an alarm system in cloud monitoring and control | It is necessary to purchase or configure the monitoring and control system alone, which has fewer channels and higher costs |
| Supports cross-zone disaster recovery | The technical implementation is extremely difficult | Cross-zone data centers are extremely expensive and difficult to implement technology, making it difficult to achieve cross-zone disaster recovery |
| performance | MySQL local SSD disks have excellent performance, and MySQL NVMe models provide extreme performance and high cost performance | Local disks mean that data reliability is reduced, and the use of cloud disks requires a planning architecture, which is costly | Slower than cloud computing hardware updates, performance is generally lower than cloud databases |
| Add strong performance and load balancing after reading only real examples | The implementation of master+slave architecture is more difficult, the consulting cost is high, and the maintenance cost is extremely high | The implementation of master+slave architecture is more difficult, the consulting cost is high, and the maintenance cost is extremely high |
| CloudDBA provides second-level monitoring and control capabilities, which covers most monitoring and performance optimization data database scenes | Relying on senior DBAs, large expenses and subject to people | Relying on senior DBAs, large expenses and subject to people |
| Safe | Intranet isolation controls access and VPC network isolation | VPC network isolation | The consultation cost of network isolation is high |
| Supports database auditing | Auditing is difficult, and you need to save SQL logs alone | Auditing is difficult, and you need to save SQL logs alone |