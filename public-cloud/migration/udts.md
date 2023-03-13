---
layout: default
title: UDTS
parent: Multi-Cloud and Migration
grand_parent: Public Cloud
permalink: /public-cloud/multi-cloud-migration/udts/
nav_order: 1
---
# UDTS
## Introduction
### What is a data transfer service
The data transmission service UDTS (SCloud Data Transmission Service) supports full/incremental data transfer between multiple homogeneous and heterogeneous data sources. 

UDTS can easily help users adjust the data architecture, migrate data across data centers, and synchronize real-time data for subsequent data analysis. UDTS does not need to stop the use of the current database service, which greatly reduces the impact of data transmission on the business.

It mainly includes the following functional modules:
### Data transmission
Data transfer is mainly responsible for full data migration and incremental synchronization. It is suitable for scenarios such as business migration to the cloud, cross-cloud disaster recovery, and cross-zone migration.
### Data integration
Data integration focuses on multi-source data aggregation and establishes a many-to-one data synchronization channel for users. You can automatically handle data conflicts by specifying a policy, and you can modify the name of the database table. 

It is suitable for scenarios such as enterprise BI library integration and database schema adjustment.

## Instance type
From January 1, 2022, three practical types will be provided for users to choose from. 
### Ultimate version
Configured with 1T disk, it is suitable for large-scale transmission business.

| Data type | Maximum transmission rate (Mbps). | Incremental synchronization ops limit |
| --- | --- | --- |
| MySQL | 800 | 20000 |
| TiDB | 800 | ON |
| Repeat | 800 | 90000 |
| MongoDB | 800 | 50000 |
| PostGre | 800 | 40000 |

### Lightweight version
Equipped with a 200G disk, it is suitable for medium-sized transmission services. 

| Data type | Maximum transmission rate (Mbps). | Incremental synchronization ops limit |
| --- | --- | --- |
| MySQL | 240 | 4000 |
| TiDB | 240 | ON |
| Repeat | 400 | 60000 |
| MongoDB | 400 | 20000 |
| PostGre | 400 | 20000 |

### Basic version
Equipped with 100G disk, suitable for small transmission services. 

| Data type | Maximum transmission rate (Mbps). | Incremental synchronization ops limit |
| --- | --- | --- |
| MySQL | 80 | 2000 |
| TiDB | 80 | ON |
| Repeat | 80 | 30000 |
| MongoDB | 80 | 10000 |
| PostGre | 80 | 10000 |

## Usage Restrictions
### Functional limitations
Currently, the destination side of UDTS only supports the SCloud public cloud.

### Quota limits
The default quota per user is 15, if the user wants to use more than 15 tasks, contact technical support to adjust the quota. If you need a large quota, contact technical support at least one month in advance to ensure sufficient time for resource scheduling.

