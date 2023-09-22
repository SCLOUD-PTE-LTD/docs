---
layout: default
title: Elastic Scaling
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/auto-scale/
nav_order: 14
---
# 14. Elastic Scaling

## 14.1 Product Introduction

### 14.1.1 Overview

Elastic scaling refers to automatically increasing computing resources to ensure computing capability when business demand increases, and automatically reducing computing resources to save costs when business demand decreases. At the same time, combined with load balancing and health check mechanisms, it can meet the scenarios of request fluctuation and business volume stability. Users can customize elastic scaling groups and scaling strategies through elastic scaling services. When the resource quantity in the scaling group reaches the threshold defined by the strategy, the number of resources or the resource specification can be automatically increased or decreased according to the customized resource template, which can improve the efficiency of business deployment and operation.

### 14.1.2 Logical Architecture

Horizontal scaling can be logically divided into three parts: scaling groups, scalers, and virtual machine templates.

![ASArch](/assets/images/userguide/ASArch.png)

* Scaling group: Responsible for maintaining the number of instances in the group at the "expected" level, adding/reducing virtual machines are all operated by the scaling group, supporting "automatic scaling" and "fixed quantity" two modes to maintain the number of instances in the scaling group.
* Scaler: Also known as scaling strategy, used to define the rules for scaling the virtual machines within the scaling group, supports defining the minimum and maximum number of instances in the scaling group, and can configure whether to allow shrinking.
  * Virtual machine type triggers scaling actions based on thresholds of CPU and memory usage.
  * Listener type triggers scaling action according to the threshold of seven-layer connection number, seven-layer QPS, seven-layer response time, four-layer connection number, and four-layer CPS in load balancing Vserver.
* Virtual Machine Template: Users can customize virtual machine templates according to their requirements, which can be used to automatically create virtual machines during elastic scaling. At the same time, virtual machines can also be manually created through virtual machine templates.

After the scaling group is defined, the "expected" value of the instances in the scaling group is taken over and dynamically modified by the scaling policy, and the scaling group is ultimately responsible for the dynamic expansion and contraction of the virtual machines. When a new virtual machine instance is added, a new virtual machine instance will be created according to the virtual machine template.

### 14.1.3 Scaling Group Workflow

The virtual machine instances in the scaling group can define a warm-up time, which means that after the virtual machine is successfully created, it takes a certain amount of time to start the application to take on business traffic. Therefore, after the scaling group initiates the request to create the virtual machine, when the virtual machine is successfully created and in the running state, the status of the virtual machine in the scaling group is "starting", which represents the virtual machine is warming up. After the warm-up time is exceeded, it will automatically switch to "running", which represents the virtual machine is in a healthy state.

The scaling group gets the status of all the virtual machines it controls every 15 seconds and determines if additional or fewer instances are needed. If the scaling group has the load balancer turned off, the load balancer will determine if the instances in the scaling group are healthy. If not, the following process will take place:

* Healthy examples equal expected values.

  The scaling group will automatically remove unhealthy instances (based on the health check of three cycles) from the scaling group and perform the delete virtual machine operation.

* Health outcomes are greater than expected.

  Remove the latest created healthy virtual machine instance from the scaling group and perform the delete virtual machine operation, while also removing the unhealthy instance from the scaling group and performing the delete operation.

* The actual health value is less than the expected value.

  The scaling group will automatically initiate the creation of instances with virtual machine templates, and maintain the number of instances at the expected value, while removing unhealthy instances from the scaling group and performing deletion operations.

### 14.1.4 Expander Workflow

The scaler will collect the scaling threshold data of healthy instances in the scaling group every 15 seconds based on the minimum and maximum instance values set in the scaling policy, in order to determine whether to scale out or scale in the instances in the scaling group.

* Expansion

  If the scaling threshold of healthy instances in the scaling group is greater than the threshold defined in the scaling policy, the scaling group will trigger the scaling-out operation of the instances.

* Downsize

  If the healthy instance threshold in the scaling group is less than the threshold defined in the scaling policy, the scaling group will trigger a scaling down operation of the instance. **In order to avoid frequent scaling down causing cluster service oscillation in the scaling group, the average of all healthy instance scaling metrics monitoring data in the past 10 minutes will be obtained when scaling down to determine whether to scale down the instance in the scaling group.**

### 14.1.5 Features

Horizontal scaling maintains the number of virtual machine instances in the cluster through scaling groups, scaling policies, and virtual machine templates. The virtual machine type can detect the business health of the virtual machine instances in the scaling group according to the load balancing and timely remove the virtual machine instances in an unhealthy state to ensure the overall availability and reliability of the cluster business. When creating a scaling group of listener type, it needs to be bound to the load balancer.

* Support defining virtual machine templates for auto-creating virtual machines in scaling groups, and also support manually creating virtual machines through virtual machine templates.
* Support scaling group pre-warming time to give virtual machines time to start up applications after successful creation to take on business traffic.
* Support both automatic scaling and fixed number scaling modes, suitable for various auto scaling scenarios.
  * The automatic scaling mode maintains the number of instances in the scaling group according to the scaling policy of the scaler.
  * Fixed-size pattern scales the instances in the scaling group according to the specified number of instances by the user.
* Support scaling in or out based on the virtual machine and listener metrics of healthy instances in the scaling group as the basis for whether to scale in or out in the auto scaling mode.
* Support setting the maximum number of instances for scaling policies to avoid unlimited scaling of the number of instances in the scaling group due to too high thresholds, such as cluster virtual machines being attacked.
* Support setting the minimum number of instances for scaling policies to avoid problems such as business interruption or service stoppage due to the threshold being too low and the number of instances in the scaling group being 0.
* Support setting a scaling policy for shrinkage policy, i.e. limiting the instances within a scaling group to only allow scaling out, not scaling in.
* Support users to view the events of the scaling group and the instance information added to the scaling group, to view all the actions and reasons of the automatic scaling group, and to facilitate the user to maintain the scaling group cluster business.
* Support users to enable or disable an auto scaling group. When the auto scaling group is disabled, it will be unavailable and will not trigger the scaling policy to execute instance scaling and health checks. Disabling the auto scaling group will not affect the normal operation of existing instances in the auto scaling group.
* Provide average monitoring metrics data for all instances in the scaling group, and configure alarms for monitoring data through alarm templates. When the scaling metrics trigger scale-out or scale-in, an alarm email will be sent to the user.

  The scaling policy of virtual machine type can be associated with elastic scaling and load balancing. By adding the instances in the scaling group to the listener of the load balancer, the load balancing service is provided for the business of the virtual machines in the scaling group. At the same time, through the health check mechanism of the listener, the business health status of all instances in the scaling group can be judged, and the unhealthy instances will be automatically removed and healthy instances will be added to the business cluster.


## 14.2 Virtual Machine Template Management

Virtual machine templates are user-customized virtual machine templates used for automatic creation of virtual machines when elastic scaling is required, and also support manual creation of virtual machines through virtual machine templates. It supports the creation, viewing, deletion and creation of virtual machines, as well as other life cycle management of virtual machine templates.

### 14.2.1 Create virtual machine template

Users can create virtual machine templates suitable for their business needs through the virtual machine console - virtual machine template resource list. The virtual machine template is the same as creating a virtual machine, just provide the template name and template notes, as shown in the following figure:

![createtemplate](/assets/images/userguide/createtemplate.png)

The template name and remark refer to the name and description of the virtual machine template to be created currently. The basic configuration, network device, and management configuration are the same as creating a virtual machine, representing the parameters specified when using the template to create a virtual machine, such as virtual machine specifications and operating system, etc. At the same time, the virtual machine template does not need to specify the virtual machine name, which is automatically generated by the elastic scaling group when created.

When creating a template, you need to specify the payment method of the virtual machine in the template, which represents the payment method of the virtual machine created through the template, such as monthly; at the same time, when creating the template, the cost to be paid when creating the virtual machine from the template will be displayed.

> Creating a template does not incur a charge, only when the elastic scaling group automatically creates an instance will it be charged according to the estimated cost.

### 14.2.2 View virtual machine templates

Users can view the list of virtual machine templates on the virtual machine template list page, including name, ID, VPC, subnet, model, configuration, associated resources, billing method, and operations, as shown in the following figure:

![templatelist](/assets/images/userguide/templatelist.png)

* Name/ID: Refers to the name of the virtual machine template and its globally unique identifier.
* VPC/Subnet: Refers to the VPC and subnet to which the virtual machines created through the virtual machine template belong.
* Model/Configuration: Refers to the model and configuration of the virtual machine created through the virtual machine template.
* Associated resources: refers to the elastic scaling group associated with the virtual machine template, that is, the scaling group will create instances through the configuration in the virtual machine template.
* Billing Method: Refers to the payment method for virtual machines created through virtual machine templates.

The operation items on the list refer to manually creating virtual machines through virtual machine templates, and users can also click the name of the virtual machine template to enter the virtual machine template details to view the detailed information of the virtual machine template, including basic information, virtual machine configuration, network configuration and storage configuration information.

### 14.2.3 Create Virtual Machines with Templates

Support users to manually create virtual machines through templates, suitable for scenarios that require virtual machines to be created through templates. As shown in the following figure:

![templateaddvm](/assets/images/userguide/templateaddvm.png)

When creating a virtual machine through a template, you need to specify the virtual machine name, and you can also specify the login password for the virtual machine. If not specified, the login password specified when creating the template will be used.

### 14.2.4 Modify Name and Remarks

You can modify the name and remarks of the virtual machine template at any state. You can edit by clicking the "Edit" button on the right side of each image name on the virtual machine template list page.

## 14.3 Horizontal Stretch

Horizontal scaling is responsible for maintaining the number of instances in the group at the "expected" level, and the actions of adding/reducing virtual machines are all operated by the scaling group. It supports two modes of "automatic scaling" and "fixed number" to maintain the number of instances in the scaling group and adapt to various automatic scaling scenarios. Horizontal scaling maintains the expected level of instances in the group through the scaling rules and scaling instances in the scaling policy, and creates new instances through the virtual machine template.

* Autoscaling mode maintains the number of instances in the scaling group according to the scaling policy of the scaler.
* The Fixed-Size Pattern scales the instances in the scaling group according to the specified instance quantity by the user, so no scaling policy needs to be specified in the Fixed-Size Pattern.

Support the scaling group preheating time, so that after the virtual machine is successfully created, the application can be pulled up within the preheating time to take on business traffic; At the same time, support the load balancing associated with the virtual machine type scaling group to provide load balancing service for the virtual machines in the scaling group, and through the health check mechanism of the listener, judge the business health status of all instances in the scaling group, automatically eliminate the unhealthy instance and add healthy instance to the business cluster.

As the core module of the auto-scaling service, it supports the full life cycle management of horizontal scaling, including creating scaling groups, viewing scaling groups, modifying scaling groups, enabling/disabling scaling groups, binding/unbinding load balancers, viewing events, and deleting scaling groups.

### 14.3.1 Create Horizontal Scaling

Users can create an elastic scaling group through the scaling group console, specifying the virtual machine template, preheating time, and scaling mode, in order to dynamically scale the virtual machine instances in the group to adapt to business scenarios that require elastic scaling. The following two pictures are examples of creating scaling groups for virtual machine types and listener types:

![createscale](/assets/images/userguide/createscale.png)

![createscale1](/assets/images/userguide/createscale1.png)

**Virtual Machine Scaling Type** currently supports setting CPU and memory utilization as scaling indicators, and can later bind load balancers to provide load balancing services for resources in the scaling group.
**The listener scaling type** currently supports three protocol types: TCP, HTTP, and HTTPS. When selecting the TCP protocol, the scaling indicators can be selected as four-layer connections and four-layer CPS. When selecting the HTTP and HTTPS protocols, seven-layer connections, seven-layer QPS, and seven-layer response time can be selected as scaling indicators. When creating a listener scaling group, a load balancer and the corresponding protocol Vserver listener must be created in advance.

* Name/Remark: The name and remark information of the scaling group, used to identify the scaling group.
* Virtual Machine Template: The virtual machine template used when a new virtual machine instance is added to the scaling group. That is, the virtual machine instance will be added to the group according to the selected virtual machine template. It must be specified when creating, and only virtual machine templates with permission are supported.
* Warm-up time: The warm-up time after the virtual machine instance in the scaling group is successfully created. During the warm-up time, the virtual machine can start the application to take on business traffic. The virtual machine instance in the warm-up is in the "Starting" state in the scaling group. It must be specified when creating, and the default is 300s.
* Member quantity setting: The minimum and maximum member quantities can be set, and the number of resources dispatched will not exceed the set upper and lower limits of the member quantity. If you want to achieve a fixed quantity effect, you can set the maximum and minimum member quantities to be the same.
* Is downsizing allowed: When downsizing is allowed, the scaling group will reduce the number of resources when the indicator is not met. When downsizing, a SIGTERM signal will be sent to the business process in the virtual machine. The virtual machine will be closed and deleted after waiting for a maximum of 90 seconds. Business processes can use this mechanism to shut down gracefully.
* Expandable Indicators
  * Virtual machine metrics CPU Memory (unit %)
  * Listener metrics: Seven-layer connection count (number), Seven-layer QPS (number/s), Seven-layer response time (ms), Four-layer connection count (number), Four-layer CPS (number/s).
* Indicator threshold: Average of all virtual machine monitoring data within 10 minutes

The template configuration interface displays the basic information of the template used by the current scaling group, including image, CPU, memory, disk configuration, and external network binding (if bound to EIP, the template will automatically apply for EIP when starting the virtual machine) as shown in the following figure.

![createscale2](/assets/images/userguide/createscale2.png)

After clicking Create, the platform will create the scaling group according to the specified configuration, automatically maintain the number of instances in the scaling group, and deduct the fees for the newly added instances (the scaling group does not charge extra fees, but users need to pay for the virtual machine instances created in the scaling group). Users can view the instances and related information of the scaling group through the scaling group list and details.

### 14.3.2 View Horizontal Scaling

#### 14.3.2.1 Scaling Group List

Users can view a list of all the scaling groups they have in their account and related information through the elastic scaling console, and can enter the scaling group details to view the monitoring information, instance information, and events of the scaling group by the name of the list. At the same time, the load balancing of the scaling group can be bound and unbundled through the load balancing of the scaling group, and the specific list information is shown in the following figure:

![scalelist](/assets/images/userguide/scalelist.png)

* Name/ID: Name of the scaling group and its globally unique identifier.
* Launch Template: The virtual machine template associated with the scaling group, which is the virtual machine template used when adding new instances to the group.
* Expandable type: Expandable type is divided into two types, virtual machines and listeners.
* Current Instance: The number of virtual machine instances in the scaling group, which is normally equal to the expected instance number.
* Status: Refers to the running state of the scaling group, including available and unavailable.

The list supports updating, starting, disabling, deleting, and other operations on the scaling group, and also supports batch starting, batch disabling, and batch deleting operations on the scaling group.

#### 14.3.2.2 Horizontal scaling details

Users can enter the scaling group details to view the detailed information of the scaling group, including the basic information of the scaling group, scaling policies, monitoring information, instance information, events and associated load balancer information, as shown in the following figure:

![scaledetails](/assets/images/userguide/scaledetails.png)

 (1) Basic information: including resource ID, resource name, virtual machine template, preheating time, and current instance quantity.

 (2) Scaling strategy: Includes scaling mode and expected instance count.

（3）Monitoring Information: Monitoring information of scaling indicators, users can view the average CPU usage of all virtual machine instances in the group through monitoring information, and can configure alarms for monitoring data through alarm templates. When the usage is too high and triggers scaling out and in, an alarm email will be sent to the user. The default can view one hour, and can filter by time to view the monitoring information of the custom time.

（4）Instance information: a list of the current instances in the scaling group, including the instance name, ID, launch template, IP address, creation time and status, see [View Scaling Group Instance](#_14323-View Scaling Group Instance).

（5）Load balancing: refers to the load balancing instance associated with the scaling group, and the load balancing can be bound or unbound through the load balancing management, see [Load Balancing Management](#_1445-Load Balancing Management).

（6）Event: Refers to the instance scaling information within the scaling group, such as the event information of adding or removing instances, see [View Scaling Event](#_14324-View scaling events) for details.

#### 14.3.2.3 View the instances of the scaling group

Users can view the instance information in the current scaling group through the [Instance] tab in the scaling group details, as shown in the following figure:

![scalevm](/assets/images/userguide/scalevm.png)

Scaling group instances support adding from existing virtual machine resources to the scaling group, and also removing virtual machines from the scaling group instance. Virtual machines removed will be automatically deleted.
* The instance name is auto-named by the scaling group, usually the scaling group name plus a random string.
* The launch template is the virtual machine template used to create the current instance, and you can view the specific configuration information through the virtual machine template.
* The IP address is the private IP address of the virtual machine instance within the VPC.
* The status is the status information of the current instance, including starting, running and other virtual machine related status information.
  * Among them, the virtual machines in the scaling group are starting up, or during the scaling group preheating time, the instance status will remain in the startup state, after exceeding the preheating time, it will automatically switch to running, representing the virtual machine is in a healthy state.
  * The scaling group gets the status of all the virtual machines it controls every 15 seconds to determine if instances need to be added or removed. If the scaling group turns off the load balancer, the load balancer will determine if the instances in the scaling group are healthy.
* Creation time refers to the time when the current instance was created.

#### 14.3.2.4 View scaling events

You can view the instance change events in the current scaling group in the Scaling Group Details [Events] to see the details of each change, such as adding or removing instances, including event type, event level, event content, event occurrence times, start time, and update time, to facilitate users to maintain the scaling group cluster business, as shown in the following figure:

![scalelog](/assets/images/userguide/scalelog.png)

### 14.3.3 Modifying the scaling group

Support users to customize the modification of the virtual machine template, minimum member quantity, maximum member quantity, shrinkage policy, preheating time, scaling index, and index threshold of the scaling group. It can be operated through the update in the operation item of the scaling group list. The scaling type cannot be modified after creation, as shown in the following figure:

![updatescale](/assets/images/userguide/updatescale.png)

* Virtual Machine Template

  If the user modifies the virtual machine template of the scaling group, it does not affect the configuration and parameters of the existing virtual machine instances in the group, only affects the newly added instances, that is, when the new instances are added after the update, the virtual machine is created with the new virtual machine template.

* Warm-up time

  After the user modifies the preheating time, the newly added instance will run according to the new preheating time and accept business processing.

* Number of instances/Scaling policy/Scaling metric/Scaling down policy/Metric threshold

  After the user modifies the scaling group, the scaling group will immediately maintain the virtual machine instances within the group according to the new scaling expectation instance quantity.

### 14.3.4 Enable/Disable Scaling Group

Support users to enable or disable an autoscaling group. When the autoscaling group is disabled, it will be unavailable and will not trigger the execution of instance scaling and health checks. Disabling the autoscaling group will not affect the normal operation of the existing instances in the autoscaling group.

Only enabling a scaling group from the disabled state is supported. After enabling from the disabled state, the scaling group status changes to available, and the number of instances and health status of the scaling group will be maintained according to the expected number of instances of the scaling mode.

### 14.3.5 Load Balancing Management

The platform supports scaling group association load balancing, **only virtual machine type scaling groups can be bound and unbundled**, providing load balancing services for virtual machine business in scaling groups. At the same time, through the health check mechanism of the listener, the business health status of all instances in the scaling group is judged, and unhealthy instances are automatically removed and healthy instances are added to the business cluster.

A scaling group can support multiple load balancer listeners (VServers) to provide multiple services to the outside. Each load balancer listener will perform a health check on the virtual machine instances in the group, and will automatically remove unhealthy instances, and the scaling group will automatically start healthy instances into the group.

The scaling group retrieves the status of all the virtual machines it controls every 15 seconds to determine if additional or fewer instances are needed. When the scaling group is turned off, the load balancer will determine the health of the instances in the scaling group. If they are not healthy, the following process will take place:

* Healthy examples equal expected values

  The scaling group will automatically remove unhealthy instances (based on the health check of three cycles) from the scaling group and perform the delete virtual machine operation.

* Health outcomes exceed expectations

  Remove the latest created healthy virtual machine instance from the scaling group, and perform the delete virtual machine operation, while also removing the unhealthy instance from the scaling group and performing the delete operation.

* The actual health value is less than the expected value.

  The scaling group will automatically initiate the creation of instances with virtual machine templates and maintain the number of instances at the expected value. At the same time, unhealthy instances will be removed from the scaling group and deleted.

The platform supports binding, unbinding, and viewing the load balancer that is bound to the scaling group, allowing users to provide load distribution services for the virtual machine instances in the scaling group based on the automatically scaled load balancer, thus improving the availability of the business.

#### 14.3.5.1 Bind load balancing

Users can bind a scaling group to multiple VServer listeners of the load balancer, **only virtual machine type scaling groups can be bound and unbounded**. The load balancer listener monitors the health status of the virtual machine instances in the scaling group. After binding, all instances in the scaling group will automatically join the bound load balancer VServer as service nodes. Users can enter the binding wizard page through the scaling group details - Load Balancer, as shown in the following figure:

![scalebindlb](/assets/images/userguide/scalebindlb.png)

* Load balancing/VServer: The load balancing and VServer listeners to be bound must specify an existing and authorized load balancing instance. When binding, it must be specified, and only one is allowed at a time.
* Port: The business service port provided by the virtual machine instance that joins the scaling group to the load balancing service node, that is, the load balancing forwarding traffic to the backend service port of the virtual machine.

After binding, all instances in the scaling group will automatically join the bound load balancer VServer as service nodes, and the load balancer will be responsible for checking the health status of the virtual machine instances in the group.

#### 14.3.5.2 View Bound Load Balancing

After the scaling group is bound to the load balancer, the bound load balancer information can be viewed through the load balancer details in the scaling group, including load balancer, VServerID, port, weight, and operation items, as shown in the following figure:

![scalelblist](/assets/images/userguide/scalelblist.png)

In the list, Load Balancer and VServer are the bound Load Balancer name and VServer instance ID; Port and Weight represent the backend service port and weight of the virtual machine instances in the scaling group joining the Load Balancer service node.

At the same time, in the list operation items, each bound load balancer can be unbound, supporting batch unbinding, which is convenient for resource maintenance.

#### 14.3.5.3 Unbind the load balancer

Users can unbind the association between the scaling group and the load balancer when the business needs it. After unbinding, the virtual machine instances in the scaling group will be removed from the service nodes of the load balancer, which will not affect the running of the virtual machines and the business itself, and the health status will be checked by the scaling group. Users can operate through the unbinding or batch unbinding in the load balancer in the scaling group details, as shown in the following figure:

![scaleunbindlb](/assets/images/userguide/scaleunbindlb.png)

### 14.3.6 Delete the scaling group

The platform supports users to delete scaling groups that are not suitable for business scenarios, which can be deleted in any state. As shown in the following figure:

![rmscale](/assets/images/userguide/rmscale.png)

After deleting the scaling group, it will automatically be unbound from the virtual machine template, scaling policy, and load balancer, and the virtual machines created by the scaling group will be automatically deleted. The virtual machine instances deleted by the scaling group will be destroyed directly without entering the recycle bin.


## 14.4 Vertical scaling
   
Vertical scaling supports scaling group settings for two types of resources, virtual machines and EIPs. Virtual machines do not support scaling down, but need to support hot upgrade. Virtual machine scaling indicators include CPU and memory; EIP scaling indicators are bandwidth utilization.
EIP supports shrink operations.

### 14.4.1 Create Vertical Scaling

Create vertical scaling, basic settings include name, remarks, project group, tags, scaling group settings include scaling type, scaling metrics, select resources, maximum specifications, and metric thresholds.

![createvtas](/assets/images/userguide/createvtas.png)

![createvtaseip](/assets/images/userguide/createvtaseip.png)


### 14.4.2 Update Vertical Scaling

Update the scaling group, virtual machine type supports updating scaling metrics, maximum specifications, metric thresholds, and the maximum specifications cannot be less than the current virtual machine specifications. EIP type supports updating the maximum specifications, whether to allow shrinkage, and metric thresholds.

![updatevtas](/assets/images/userguide/updatevtas.png)

![updatevtaseip](/assets/images/userguide/updatevtaseip.png)


### 14.4.3 View Vertical Scaling Details

Display basic information of the vertical scaling group, scaling group settings, monitoring information and events.

![vtasview](/assets/images/userguide/vtasview.png)





