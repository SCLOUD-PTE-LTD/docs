---
layout: default
title: Resource Events
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/event/
nav_order: 31
---
# 31. Resource Events

## 31.1 Resource Event

Resource events are used to record and notify partial operations of core resources on the cloud platform, such as changes in resource life cycle status, operation and maintenance execution, etc. Resource events will record user's partial core operation events on resource types, provide detailed event record query and screening, and timely notify users and locate problems.

Tenants can view the resource event record information of the entire platform through the Resource Event Console, and support query the detailed information of resource events according to the region, resource type, resource selection, and event cycle, including resource ID, resource type, event type, event level, event content, event occurrence times, start event and update event, as shown in the following figure:

![event](/assets/images/userguide/event.png)

* Resource ID: refers to the resource ID of resource event monitoring.
* Resource type: The resource type specified by the current resource event record.
* Event type: Event types are divided into life cycle changes and operational and maintenance events, such as virtual machine scheduling, virtual machine startup and shutdown, mounting disks, etc.
* Event Level: Event levels are divided into the following categories: Normal, Warning, Error.
* Event Content: Detailed record of specific information triggering the event.
* Number of occurrences: Record the cumulative number of times the event has been triggered.
* Start time: The time of the first resource event discovery.
* Update time: Time of resource event triggered for the second time and after.

In order to facilitate users to view resource events, the console supports event filtering, and also supports users to export resource events to local Excel tables for easy viewing and positioning. Resource events support viewing all event information, that is, no filtering of the modules they belong to; filtering can be done according to event level; and the query time range supports log filtering for 1 hour and custom time.

## 31.2 Notification Rules

By creating notification rules to monitor resource events and notify the notifier by email, you can set notification rules by selecting the monitoring region, notification group, monitoring module, and monitoring level. When the resource event meets the notification rule requirements, a monitoring email will be sent to the members of the notification group.

### 31.2.1 Create notification rules

Users can create notification rules through the resource event notification rule page by clicking the "Create" button. When creating notification rules, they need to specify the monitoring region, notification group, monitoring module and monitoring level. As shown in the following figure:

![eventrule](/assets/images/userguide/eventrule.png)

* Monitoring Region: Region information of the notification rule.
* Notification group: Notification group information for email notifications, only one notification group can be selected.
* Monitoring Module: The resources monitored by the module, such as virtual machines.
* Monitoring Level: Divide the impact of normal instance operation, including Normal, Warning, Error, multiple selections are allowed.

### 31.2.2 View Notification Rules

Support users to view notification rule information under their account, including monitoring region, notification group, monitoring module and monitoring level, as shown in the following figure:

![eventrule1](/assets/images/userguide/eventrule1.png)

### 31.2.3 Update Notification Rules

Support users to update the notification rules under their account, including monitoring region, notification group, monitoring module and monitoring level. As shown in the following figure:

![eventrule2](/assets/images/userguide/eventrule2.png)

### 31.2.4 Delete Notification Rules

Support users to delete notification rules under their account, and the rules will be destroyed after deletion, as shown in the following picture:

![eventrule3](/assets/images/userguide/eventrule3.png)





