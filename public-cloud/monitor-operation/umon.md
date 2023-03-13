---
layout: default
title: UMon
parent: Monitor and Operation
grand_parent: Public Cloud
permalink: /public-cloud/monitor-operation/umon/
nav_order: 1
---
# UMon - Resource Monitoring
## Product Introduction
The cloud monitoring system can provide monitoring of products and resources in SCloud cloud platform, and enable you to grasp the status of resources and applications in real time through alarm notification management and monitoring template settings, ensuring the healthy operation of your services and applications.
Introduction to the feature
### Monitoring services
Basic monitoring: It includes monitoring of all products and resources such as cloud hosts, cloud databases, cloud memory storage, elastic IP, shared bandwidth, load balancing, hybrid cloud, VPC, big data hosting cluster, and CDN, and updates are synchronized when the product is updated, and monitoring items are added in real time.
Host in-depth monitoring: On the basis of the original CPU usage, network card throughput, network card I/O, disk throughput, and disk I/O, you can install monitoring agents to increase memory usage, disk usage, and process monitoring.
### Alerting service
Alarm template: After you create an alarm template, you can define the alarm threshold in the template, and after the setting is complete, you can use the template to bind alarm rules to resources in batches.
Notifier management: including the definition of notifier and notification group, through user mailbox, phone input, combined with the concept of grouping, you can assign alarms of different resources to different notifiers, so as to divide full responsibility and refine the handling of alarm notifications.
### Message subscriptions
News subscription productizes important messages in the life cycle and operation of each product instance and platform service, builds a complete news consumption channel and process, and supports customer monitoring and operation and maintenance on the cloud. The provided subscribable messages are obtained by internal product modules and underlying infrastructure services, and finally presented after aggregation, determination, and convergence.

According to the source and the scope of the message, the message types are divided into two categories: platform messages and product messages.
- Platform messages: Provide subscriptions to various types of messages from the global dimension of the platform. Such as financial news, product related, security news, operation and maintenance information, etc.
- Product messages: Provide subscriptions to corresponding messages from the dimensions of each product. Such as cloud database, WAF, SMS package, etc.

## FAQ
### Ubuntu 16.0.4 and other systems cannot collect data after UMA is installed
Ubuntu 16.0.4 and other systems do not support Python 2.x related versions by default.

Workaround:

(1) Uninstall UMA
```
dpkg -P uma
```
(2) Install Python 2.x related versions
```
sudo apt-get install python2.7
sudo apt-get install python-pip
```
(3) Reinstall UMA
