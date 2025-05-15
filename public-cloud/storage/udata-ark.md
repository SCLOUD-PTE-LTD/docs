---
layout: default
title: UDataArk (Backup)
parent: Storage
grand_parent: Public Cloud
permalink: /public-cloud/storage/udata-ark/
nav_order: 4
---
# UDataArk
## Overview
UDataArk provides continuous data protection (CDP) services for SCloud local disks/EVS disks, and is an automatic data backup system that is completely heterogeneous with local disks/EVS disk storage, which can prevent data deletion or loss caused by mis-operation and hacker attacks to the greatest extent.

There are two important metrics for evaluating the capability of a data backup product: a recovery point objective (RPO), which refers to the point in time to which the data can be recovered, and a recovery time objective that refers to how long it takes for the business to go from stop to recovery at the time of recovery. 

The data ark shows its superiority on both indicators:

### Recovery Point Objective (RPO)
The RPO of Data Ark reaches the second level, and by default, it supports restoring any second within 12 hours, restoring any whole point within 24 hours, and restoring any zero point within 3 days.
### Recovery Time Objective (RTO)
The RTO of the data ark can be restored in as little as 5 minutes, and the amount of 1TB data can be restored within 1 hour.

## Product Advantages

### Online Backup Without Interrupting Business

Snapshots can be taken without pausing services or stopping disk read/write operations. There is no impact on live business activities and no degradation in disk I/O performance.

### Second-Level Recovery and Real-Time Data Protection

Supports recovery to any second within the past 12 hours, and any full-hour point within the past 24 hours—minimizing the risk of data loss.

### System and Manual Backups to Meet Custom Needs

In addition to comprehensive system backup policies, users can manually back up data based on specific business needs, supporting a wide range of backup scenarios.

### Console-Based Self-Service Operations

All backup policies and actions can be managed directly through the console, reducing labor costs and lowering the technical barrier for data backup.

## Related Case Studies

### Case 1: File System Corruption Due to Misoperation

In December 2017, an AI startup faced a major crisis when one of their operations engineers made a critical mistake while expanding a cloud disk that contained important data. The improper operation caused a file system failure, rendering the data inaccessible.

The affected disk had multiple partitions, and the company lacked experience in repairing multi-partition file systems. Fearing further damage, they hesitated to perform any recovery attempts. Fortunately, the company had enabled the SCloud Data Ark service for critical disks. They immediately initiated a recovery and were able to restore all data in less than 30 minutes.

### Case 2: Rapid Data Recovery

In May 2017, the WannaCry ransomware outbreak affected a large number of Windows hosts globally. It encrypted important enterprise files and demanded a high ransom for decryption. A cloud server belonging to a printing company was also hit, resulting in encryption of customer print files.

Prior to this incident, the printing company had partnered closely with SCloud and enabled Data Ark for continuous real-time protection of customer data. Once the contamination timestamp was identified, they restored the data to a point before the infection directly from the console, ensuring maximum data integrity.

### Case 3: Accidental Deletion of Database Table

In April 2018, an engineer at an internet company accidentally deleted a production database table during routine operations. Because the backup cycle was long, restoring from backup would have resulted in significant data loss. Additionally, they had not practiced recovery from backups before, so instead, they used the Data Ark service to roll back to a point before the deletion. Within 10 minutes, all the data was successfully recovered.

Traditional database recovery methods rely on backups. If the backup is missing or corrupted, even specialized database recovery services may not fully restore the data. For example, in 2017, GitLab experienced extended service downtime and 6 hours of data loss due to accidental data deletion. In comparison, Data Ark significantly minimizes the impact of such incidents.

## About Order Reconciliation

### Why Reconcile Data Ark Charges?

The Data Ark service began billing officially on December 10, 2016. We sent a prior notification, but your account did not have sufficient balance at the time, which caused the Data Ark fee in your order to remain unpaid.

We kindly ask that you settle the outstanding Data Ark fees as soon as possible.

If, after evaluation, you decide that Data Ark is not needed, you can disable it via:
Host Details Panel → Disk & Recovery → Disable Data Ark, which will stop the backup service.

### How is the Reconciliation Fee Calculated?

> Total reconciliation cost per host = (Expiration Date – Current Date) × Disk Size × Data Ark Unit Price

Example: if your monthly plan expires on March 10, and you choose to reconcile on March 9, with a 20 GB system disk and 20 GB data disk, the total cost would be:

```
40 × (0.4 / 31) = RMB 0.52
```

## Product Pricing

| Region                      | Price          |
|-----------------------------|----------------|
| Hong Kong Zone B            | RMB 0.4 per GB/month |
| Ho Chi Minh City Zone A     | RMB 0.4 per GB/month |

> Note: For pricing in other regions, please contact your account manager.