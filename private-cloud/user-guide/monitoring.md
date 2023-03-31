---  
layout: default
title: Monitoring & Alarm
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/monitoring-alarm/
nav_order: 14
---

# Monitor and alarm
Monitoring and alarming is the operation and maintenance monitoring and alarming service for all products on the SCloudStack platform. It provides real-time monitoring data and chart information for all resources, and can set alarm policies for resources in batches according to the monitoring data. At the same time, the monitoring and alarm service also provides users with historical resource alarm records, allowing users to control the health status of business and cloud products in real time and accurately, and comprehensively guarantee the reliability and security of business.

The monitoring alarm service provides monitoring charts, alarm templates, notification groups, and alarm records with four major architectural functions. The overall architectural functions are based on monitoring data:

- The cloud platform uses an intelligent data collection system to fully mine the monitoring indicator data specified by resources such as virtual machines, cloud hard disks, elastic network cards, EIP, load balancing, NAT gateways, elastic scaling, MySQL, Redis, and VPN gateways;
- Store the collected monitoring data in the database, retrieve and count the data according to the specified rules, and display the monitoring chart graphically through the specified time dimension and data granularity;
- Based on existing monitoring data, users can configure alarm templates to specify alarm thresholds and alarm settings for specified monitoring indicators, and can determine and distinguish different levels of alarms and notifications by setting alarm repetition frequency;
- Configure the notification group for the alarm template, and specify the notifier and notification method when an alarm occurs;
- During the alarm period or after the fault is over, the historical alarm record information can be queried through the alarm record to determine the time and frequency of the fault.

## Monitoring Chart
The monitoring chart refers to the intelligently collected resource operation data collected by the platform, which is retrieved and counted according to the specified screening rules such as resources and indicators, and the monitoring chart is displayed graphically through the specified data granularity and time dimension. Through monitoring charts, users can intuitively view and understand the performance, capacity, and network status of virtual resources running on the platform, and keep abreast of the health status of resources and faulty nodes.

The platform provides real-time and historical monitoring charts of various monitoring indicators for virtual machines, elastic EIP, load balancing, NAT gateways, elastic scaling, MySQL, Redis, and VPN gateways built by users, and can configure related alarm templates according to monitoring indicator items. It is used to give alarm and notification when the threshold exceeds the standard.

- Virtual machine monitoring chart: You can view the monitoring information of a single virtual machine through the monitoring information column of the virtual machine details page, including network card out/in bandwidth, network card out/in packet volume, disk read/write throughput, and disk read/write times , average load, space usage, memory usage, CPU usage;
- Elastic EIP monitoring chart: Through the monitoring information on the EIP details page, you can view the monitoring information of a single EIP resource, including the network card outbound bandwidth usage, inbound bandwidth, outbound bandwidth, inbound packet volume, and outbound packet volume;
- Load balance monitoring chart: Through the monitoring information on the load balancing details page, you can view the monitoring information of the load balancing instance and the VServer listener respectively. The monitoring chart includes the number of LB connections per second, the outbound/incoming traffic of the LB network card per second, and the LB network card per second Number of outgoing packets, number of VServer connections, HTTP 2XX, HTTP 3XX, HTTP 4XX, HTTP 5XX;
- NAT gateway monitoring chart: Through the monitoring information on the NAT gateway details page, you can view the monitoring information of a single NAT gateway, including network card incoming bandwidth, network card outgoing bandwidth, number of connections, network card incoming packets, and network card outgoing packets.
- MySQL service monitoring chart: Through the monitoring information on the MySQL service details page, you can view the monitoring information of a single MySQL service, including CPU usage, memory usage, disk usage, TPS, network card in/out bandwidth, table lock, QPS, DeletePS , InsertPS, full table scan, SelectPS, UpdatePS, number of connections, slow query, number of active threads, number of connection threads, etc.
- Redis service monitoring chart: Through the monitoring information on the Redis service details page, you can view the monitoring information of a single Redis service, including memory usage, memory usage, connection number, QPS, RedisKeys, RedisExpiredKeys, RedisEvictedKeys, number of hits, number of misses, Hit rate, NIC out/in bandwidth, etc.
- VPN gateway monitoring chart: Through the monitoring information on the VPN gateway service details page, you can view the monitoring information of a single gateway, including gateway outbound/incoming bandwidth, gateway outbound bandwidth usage, and gateway outbound/incoming packet volume.
- VPN tunnel monitoring chart: Through the monitoring information on the VPN tunnel service details page, you can view the monitoring information of a single tunnel, including tunnel outgoing/incoming bandwidth, tunnel outgoing/incoming packet volume, and tunnel health status.

The monitoring chart can display real-time monitoring data according to the time dimension, and supports viewing monitoring data and chart information of 1 hour, 6 hours, 12 hours, 1 day and custom time.

## Alarm template
The alarm template is a function provided by the SCloudStack platform monitoring alarm service for users to set resource alarms in batches. By pre-defining the alarm rules and notification rules in the template, the rules defined in the template are applied to virtual resources; if the monitoring indicators of virtual resources When the data reaches or exceeds the threshold and condition set in the alarm rule, an alarm notification will be sent to the designated notification contact according to the notification method defined in the notification rule.

According to different resource types, alarm rules for different monitoring indicators and thresholds can be customized, and you can choose to apply monitoring indicators to a single network card or disk device associated with resources to meet the monitoring and alarm requirements in various application scenarios.
- An alarm template is composed of multiple alarm rules and associated resources;
- An alarm template can only be bound to one type of resource, including virtual machines, external network elastic IPs, NAT gateways, load balancing, Redis, MySQL, and VPN gateways;
- Each alarm template can contain multiple alarm rules, and each alarm rule includes monitoring objects, device names, comparison methods, alarm thresholds, detection periods, trigger periods, convergence strategies, and notification groups;
- Each alarm template can only be bound to one notification group, each notification group can contain multiple notifiers, and supports SMS and email notification methods.

### Create an alarm template
Users can specify the resource type, template name, and remarks to quickly create an alarm template, create and configure alarm rules suitable for business needs in the alarm rule management, and finally associate the alarm template with the virtual resource to complete the configuration of monitoring alarms. Users can enter the monitoring and alarm configuration console through "Monitoring" in the console navigation bar, and enter the alarm template creation wizard page through the "Create" button on the "Alarm Template" page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-137.png)

- Resource type: the type of resource to be bound to the alarm template, including virtual machine, external network elastic IP, NAT gateway, load balancer, Redis, MySQL, and VPN gateway, etc. An alarm template only supports one resource type;
- Template name/remark: the name identification and globally unique identifier of the alarm template;
After clicking OK, the wizard page will return to the alarm template list, through which you can view the created resource list and information.

### View Alarm Template
Users can enter the alarm template resource console through the navigation bar to view the resource list of the alarm template. At the same time, they can click the name of the alarm template on the list to enter the template details page, which is used to view the detailed information of the alarm template, alarm rule management and binding resources. manage.
##### List of alarm templates
On the alarm template list page, you can view the list of templates owned by the current account, including name, ID, resource type, number of bound resources, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-138.png)
 
- Name/ID: the name and globally unique identifier of the current alarm template;
- Resource type: the resource type specified when the current alarm template is created;
- Number of bound resources: the number of resources associated with the current alarm template;
- Operation: operation items for a single alarm template, including details, viewing resources and deleting;
- The resource list can be searched and filtered through the search box, and approximate search  is supported.

#### Alarm Template Details
Enter the template details page through the "name" of the alarm template list, and you can view the details of the current alarm template, as shown in the figure below, the details page is divided into basic information, alarm rule management and resource binding management:

![1](/assets/images/user-guide/user-guide-139.png)
 
- Basic information: Basic information of the current alarm template, including name, ID, resource type, bound resource quantity and other information. If the bound resource quantity is empty, it will be displayed as "0";
- Alarm rule management: alarm rule management of the current alarm template, including creation, viewing, update, deletion of alarm rules, etc. For specific management operations, see: [Alarm rule management] (#13.2.5 Alarm rule management);
- Resource binding management: management of the resources associated with the current alarm template, that is, the resources for which the rules in the alarm template can take effect, including binding resources, viewing bound resources, and unbinding resources. For specific management operations, see: [Binding Resources management] (#13.2.6 Binding resource management).

### Viewing resources
Viewing resources refers to viewing the information of the resources bound to the current alarm template. After clicking, you can enter [Binding Resource Management] (#13.2.6 Binding Resource Management).

### Delete Alarm Template
The deletion operation can only be performed when no resource is bound to the alarm template, and the successfully deleted alarm template will be destroyed directly. Users can delete the template through "Delete" in the resource console of the monitoring alarm template, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-140.png)
 
After the operation of deleting the alarm template is confirmed, the system automatically returns to the alarm template list page, where the deletion process can be viewed, and the resource is successfully deleted when it is emptied.

### Alarm rule management
Alarm rules are the core of alarm templates, and each alarm template consists of one or more alarm rules. The resource monitoring indicator data bound to the alarm template will trigger the relevant alarm policy according to the threshold defined in the alarm rule, and notify the alarm information through the notification method in the alarm rule, so as to quickly enter and process the alarm or fault.
#### Creating Rules
Users can create an alarm rule through the "Create" function on the alarm template details page. When creating an alarm rule, parameters such as monitoring object, device name, comparison method, alarm threshold, detection cycle, trigger cycle, convergence strategy, and notification group need to be specified. as the picture shows:

![1](/assets/images/user-guide/user-guide-141.png)
 
- Monitoring object: monitoring indicators, only the monitoring indicators contained in the alarm template resource type can be selected, and an alarm rule supports only one monitoring indicator:
- Comparison method: refers to the comparison method between the actual data of monitoring indicators and the alarm threshold, representing the alarm logic of the current alarm rule, including >= or <=:
  - When >= is selected, it means that an alarm cycle is triggered when the monitoring data is greater than or equal to the threshold;
  - When <= is selected, it means that an alarm cycle is triggered when the monitoring data is less than or equal to the threshold;
- Alarm threshold: Refers to the critical value of the monitoring index data, which is compared with the monitoring index data. If the comparison method is met, an alarm cycle is triggered. For example, the alarm threshold of the CPU usage is 80, and the comparison method is greater than or equal, that is, the CPU usage is greater than or equal to 80% triggers an alarm cycle;
- Detection period: The minimum period for data detection of monitoring indicators, that is, how often to collect monitoring data, the default value is 30s;
- Trigger cycle: The continuous cycle when the monitoring data meets the threshold condition, that is, the monitoring index data meets the threshold condition N times in a row, and an alarm cycle is triggered; if the trigger cycle is 1 time, it means that an alarm is triggered when an alarm cycle is triggered;
- Convergence strategy: the frequency of sending notifications to the notification group after an alarm occurs, and you can choose continuous alarm, exponential increase, and single alarm:
  - Continuous alarm: that is, an alarm notification is sent every time the trigger period is reached;
  - Exponential increase: that is, the alarm delay is increased by 2^N time;
  - Single alarm: After triggering the alarm period, only alarm and notification once, and reactivate after the alarm recovers;
- Notification group: when the alarm period is triggered and a notification needs to be sent, the method and contact person to send the alarm notification.

After the selection and configuration are complete, click OK to return to the alarm template details page, and you can view the successfully created alarm rules through the alarm rule list.

#### View Rules
You can view the list of rules contained in the current template through the alarm template details page. The list information includes monitoring objects, device names, comparison methods, alarm thresholds, detection intervals, trigger intervals, convergence policies, notification objects, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-142.png)
 
The operation item refers to the operation on a single notification rule, including update and deletion, respectively referring to the modification and deletion of a single alarm rule.

#### Update rules
Updating rules refers to the modification of a single alarm rule. The selection and configuration of the modification items are the same as creating an alarm rule. For details, please refer to Creating Rules.

#### Delete Rules
Deleting an alarm rule refers to deleting a single alarm rule. After the rule is deleted, it will be destroyed directly, and the alarm rule of the monitoring indicator can be added again.

### View Bound Resources
Enter the alarm template resource binding management page through the resource tab of the alarm template details, and you can view the list and information of the bound resources, including the ID, type, address, and availability zone of the bound resources, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-143.png)
 
## Notification Group Management
The notification group refers to the method and contact information of monitoring alarms to send alarm notifications. Through the collection of user email and phone information, different resource alarms are notified to the notifiers by email or SMS, so as to divide full responsibility and finely handle alarm notifications.

- A notification group is a combination of a group of notifiers, which can contain one or more contacts;
- The same contact can join multiple notification groups;
- Notification methods include email notification, SMS and email notification.

When using the monitoring alarm template, you need to create a notification group first, add relevant contact information, and set the notification method of the notification group so that the alarm template can be associated. For details on the management of the notification group, see below.

### Create a notification group
Users can enter the monitoring and alarm configuration console through the console navigation bar "Monitoring", switch to the "Notification Group" page, and click the "Create" button on the notification group management page to enter the notification group creation wizard page, specify the notification group name as shown in the figure below and notification methods to create operations:

![1](/assets/images/user-guide/user-guide-144.png)
 
- Notification group name: the name and ID of the notification group to be created currently;
- Notification method: the notification method of the notification group that needs to be created currently, that is, the channel through which the user is notified when the alarm notification is triggered, including email notification and SMS notification;

After clicking OK, you will enter the notification group list page, where you can view the created notification group information, and perform related operations and management on the notification group.

### View notification group
Users can enter the notification group page through the monitoring and alarm console to view the notification group list information. At the same time, they can click the ID of the notification group on the list to enter the details page, which is used to view the detailed information of the notification group and the management of the notifier.

#### Notification Group List
On the notification group list page, you can view the list of notification groups owned by the current account. The list information includes ID, name, notification method and operation items for a single notification group, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-145.png)
 
- Name/ID: the name identification and globally unique identifier of the notification group;
- Notification method: the notification method of the current notification group;
- Operation: Operation items for a single notification group, including detail, update, delete, etc.

#### Notification Group Details
Enter the notification group details page through the ID of the notification group list, you can view the basic information of the current notification group, and manage the notification contacts through the notification person management, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-146.png)
 
- Basic information: the basic information of the current notification group, including notification group ID, name and notification method;
- Notifier management: management of notification contacts of the current notification group, including creation, viewing, update and deletion of notifiers, see [Notifier Management] (#13.3.5 Notifier Management) for details.

### Update notification group
Updating a notification group refers to the modification of a single notification group. The selection and configuration of modification items are the same as creating a notification group. Please refer to [Create Notifier](#13.3.1 Create Notification Group).

### Delete notification group
Before deleting a notification group, you need to confirm that the notification group is not bound to any alert rule. If it has been added to an alert rule, it cannot be deleted. The notification group that is successfully deleted will be destroyed, and the user's confirmation is required for successful deletion. The user can delete the notification group through "Delete" in the list operation item of the notification group console, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-147.png)

### Notifier Management
The notifier refers to the specific contact person who sends the notification of the alarm rule, including the contact person's name, mobile phone, email address and other information. Each notification group can add one or more notifiers. Depending on the notification method of the notification group, an email or text message will be sent to all notifiers when an alarm occurs on a resource.
#### Add Notifier
Users can add notifiers through the "Create" function on the notification group details page. When creating a notifier, you need to specify the notifier's name, email address and other parameters, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-148.png)
 
- Notifier name: refers to the name or nickname of the contact person that needs to be created;
- Notifier email address: refers to the email address of the contact person that needs to be created currently;

After clicking OK, a notification contact can be successfully created, and the contact information can be viewed through the notification list of the notification group details.

#### Updating Notifier Information
Updating the notifier information refers to modifying the information of a single notifier. The configuration of the modification item is the same as the rule for creating a notifier. For details, please refer to Create a notifier.

#### Delete Notifier
Deleting a notifier refers to deleting a single notifier. After the notifier is deleted, it will be destroyed directly, and the contact information can be added again.

## Alarm record
Alarm records refer to all alarm records and information of the current account. Through the alarm records, you can view the historical alarm information of 1 hour, 6 hours, 12 hours, 1 day, 7 days, 15 days and custom time periods:
 
As shown in the figure above, the alarm record list information includes alarm ID, indicator name, resource ID, resource type, region, availability zone, alarm time, recovery time, threshold and current value:

![1](/assets/images/user-guide/user-guide-149.png)

- Alarm ID: refers to the globally unique identifier of the current alarm record;
- Indicator name: the resource monitoring indicator item that triggers the current alarm record, that is, the data source;
- Resource type/resource: the resource type and resource that trigger the current alarm record;
- Region/Availability Zone: The region and availability zone of the resource that triggers the current alarm record;
- Alarm time: the alarm trigger time of the current alarm record:
  - When the current value reaches the threshold condition, the specific alarm time will be displayed;
  - The current value does not meet the threshold condition, that is, after the alarm is restored, the alarm time displays "alarm cleared";
- Recovery time: the time when the current alarm record resource monitoring indicator data returns to normal:
  - When the current value reaches the threshold condition, that is, when the alarm is in progress, it will be displayed as "alarm";
  - When the alarm is restored, it will display the time when the resource monitoring indicator data returns to normal;
- Threshold: the threshold set in the resource alarm rule that triggers the current alarm record;
- Current value: the data value of the current alarm record monitoring indicator when the alarm is triggered or the alarm is restored.


