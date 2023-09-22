---
layout: default
title: Backup Service
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/backup-as-a-service/
nav_order: 34
---
# 34. Backup Service

## 34.1 Overview

The backup service is a data protection service provided by the platform for users. It supports scheduled automatic backups and manual backups of Mysql, Redis, object storage, and file storage. Mysql supports logical backup, physical backup, and snapshot backup; Redis supports logical backup; object storage and file storage support snapshot backup.

## 34.2 Bind Storage Pool

The platform supports users to perform storage pool binding operations, and the storage type supports object storage. You can enter the "**Database Backup**" module through the navigation bar, switch to the "Storage Pool" page for operation, as shown in the following figure:

![createdbs](/assets/images/userguide/bindstoragepool.png)

## 34.3 View Storage Pool List

The platform supports users to view the storage pool list, including name, resource ID, status, storage type, object storage, associated backup plan, creation time, update time, and operation, as shown in the following figure:

![createdbs](/assets/images/userguide/storagepoollist.png)

## 34.4 Update Storage Pool

The platform supports users to update the storage pool, including storage pool name and remarks. You can perform this operation through the "**Update**" button in the operation items in the storage pool list, as shown in the following figure:

![createdbs](/assets/images/userguide/updatestoragepool.png)

## 34.5 Unbind Storage Pool

The platform supports users to unbind the bound storage pool. You can perform this operation through the "**Unbind**" button in the operation items in the storage pool list, as shown in the following figure:

![createdbs](/assets/images/userguide/unbindstoragepool.png)

## 34.6 Create Backup Plan

The platform supports users to create backup plans, which include backup strategies, backup source types, backup resources, storage pools, retention periods, and other information. You can enter the "Database Backup" module for operation, as shown in the following figure:

![createdbs](/assets/images/userguide/createdbs1.png)

![createdbs](/assets/images/userguide/createdbs2.png)

Mysql supports creating backup plans for part of the databases and tables

![createdbs](/assets/images/userguide/createdbs3.png)


## 34.7 View Backup Plan List

The platform supports users to view the backup plan list, including name, resource ID, status, storage pool, source data type, source data region, source data, backup strategy, timer, backup type, backup retention period (days), creation time, update time, and operation, as shown in the following figure:

![createdbs](/assets/images/userguide/dbslist.png)

## 34.8 View Backup Plan Details

The platform supports users to view the details of the backup plan, including basic information and backup data list. You can click on the backup plan name to enter the details page, as shown in the following figure:

![dbsdetails](/assets/images/userguide/dbsdetails.png)

### 34.8.1 Create Instance from Backup Data

The platform supports users to create instances from available backup data in the backup plan, as shown in the following figure:

![backupexample](/assets/images/userguide/backupexample.png)

### 34.8.2 Delete Backup Data

The platform supports users to delete backup data. Deletion is not allowed when the backup data is being restored. As shown in the following figure:

![deletebackup](/assets/images/userguide/deletebackup.png)

## 34.9 Update Backup Plan

The platform supports users to update backup plans, including backup plan name/remarks, backup strategy, backup source type, backup resource, repetition cycle, execution date, execution time, storage pool, retention period, etc. You can perform this operation through the "**Update**" button in the operation items of the backup plan list, as shown in the following figure:

![createdbs](/assets/images/userguide/updatedbs.png)

## 34.10 Execute Backup Plan

The platform supports users to execute backup plans, including backup plans with backup strategies for timers and manual backups. You can perform this operation through the "**Execute**" button in the operation items of the backup plan list, as shown in the following figure:

![createdbs](/assets/images/userguide/executedbs.png)

## 34.11 Delete Backup Plan

The platform supports users to delete backup plans. You can perform this operation through the "**Delete**" button in the operation items of the backup plan list, as shown in the following figure:

![createdbs](/assets/images/userguide/deletedbs.png)

After the backup plan is deleted, the backup task in the timer will also be deleted.