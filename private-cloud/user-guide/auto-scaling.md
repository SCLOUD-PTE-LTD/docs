---
layout: default
title: Auto Scaling
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/auto-scaling/
nav_order: 13
---
# Auto Scaling
## Product Introduction
### Overview
Auto Scaling refers to automatically increasing computing resources (virtual machines) to ensure computing power when business needs grow, and automatically reducing computing resources to save costs when business needs decline. At the same time, it can be combined with the load balancing and health check mechanism to meet scenarios where the request volume fluctuates and the service volume is stable.

You can use Auto Scaling to customize auto scaling groups and scaling policies, and automatically increase or decrease the number of virtual machines based on the customized VM template after the resources in the scaling group reach the threshold defined by the policy, improving the efficiency of service deployment and O&M.

### Logical Architecture
Auto scaling can be logically divided into three parts: scaling groups, scalers, and virtual machine templates.

![1](/assets/images/product-functional-architecture-18.jpg)

Scaling group: It is responsible for maintaining the number of instances in the group at the desired level, and the actions of adding/shrinking virtual machines are operated by the scaling group, which supports the "automatic scaling" and "fixed quantity" modes to maintain the number of instances in the scaling group.
- Scaling device: A scaling policy that defines the rules for scaling virtual machines in a scaling group, triggers scaling actions based on CPU usage thresholds for the scaling group, supports defining the minimum and maximum number of instances of the scaling group, and allows scaling in.
- VM Template: You can customize the VM template according to your needs, which is used to automatically create a VM template during Auto Scaling, and you can manually create a VM through a VM template.

After the scaling mode is defined, the "expectation" value of the instance of the scaling group is taken over by the scaling policy and dynamically modified, and finally the scaling group is responsible for the dynamic expansion and scaling of the virtual machine, and when a new virtual machine instance is added, a new virtual machine instance is created based on the virtual machine template.

### Scaling Group Workflow
The warm-up time of a virtual machine instance in a scaling group can be defined, which means that it takes a certain amount of time to pull up applications to undertake business traffic after the virtual machine is created. Therefore, after the scaling group initiates a request to create a virtual machine, when the virtual machine is successfully created and in the running state, the status of the virtual machine in the scaling group is "starting", which means that the virtual machine is in warm-up, and after the warm-up time is exceeded, it will automatically change to "running", which means that the virtual machine is healthy.

The scaling group obtains the status of all virtual machines controlled by it every 15 seconds to determine whether instances need to be added or removed. If Server Load Balancer is turned off in a scaling group, Server Load Balancer determines whether the instances in the scaling group are healthy.

- A healthy instance equals the expected value <br/>
The scaling group automatically moves unhealthy (based on the judgment of the three-cycle health detection) instances out of the scaling group and performs the delete VM operation.
- The healthy instance is larger than expected <br/>
Choose Move the latest healthy VM instance out of the scaling group and perform the delete VM operation, while the unhealthy instance is moved out of the scaling group and perform the delete operation.
- The healthy instance is less than expected <br/>
The scaling group automatically initiates the creation of instances using the VM template, maintains the expected number of instances, and removes unhealthy instances from the scaling group and deletes them.

### Scaler Workflow
Based on the minimum and maximum instance values set in the scaling policy, the scaler collects CPU monitoring data of healthy instances in a scaling group every 15 seconds to determine whether you need to scale up or scale out instances in the scaling group.
- Expansion <br/>
If the average CPU usage of healthy instances in a scaling group is greater than the threshold defined by the scaling policy, the scaling group is triggered to scale out instances.
- Scale-down <br/>
Generally, if the average CPU usage of healthy instances in a scaling group is less than the threshold defined by the scaling policy, the scaling group is triggered to scale in instances. To avoid cluster service fluctuations in a scaling group caused by frequent scaling in, the average CPU monitoring data of all healthy instances in the scaling group in the past 10 minutes is obtained during scaling in to determine whether instances in the scaling group need to be scaled in.

### Features
Auto scaling uses scaling groups, scaling policies, and virtual machine templates to jointly maintain the number of virtual machine instances in the cluster, and can detect the service health of virtual machine instances in the scaling group in combination with load balancing and eliminate virtual machine instances in an unhealthy state in a timely manner to ensure the availability and reliability of the overall cluster business.

- You can define a virtual machine template, which is used to automatically create a virtual machine by a scaling group, and manually create a virtual machine through a virtual machine template.
- Supports the warm-up time of the scaling group, so that after the virtual machine is successfully created, there is time to pull up the application to undertake business traffic.
- Supports two scaling modes: autoscaling and fixed quantity, to adapt to a variety of autoscaling scenarios.
  - The auto scaling mode maintains the number of instances in the scaling group according to the scaling policy of the scaler.
  - The fixed number mode scales the instances in the group based on the number of instances specified by the user.
- You can use the average CPU usage of healthy instances in a scaling group as the basis for whether scaling is required in the auto scaling mode.
- You can set the maximum number of instances in the scaling policy to avoid unlimited expansion of the number of instances in the scaling group due to high CPU usage, such as cluster virtual machines being attacked.
- You can set the minimum number of instances in the scaling policy to avoid problems such as the number of instances in the scaling group being 0 due to low CPU utilization rate, resulting in service interruption or service stoppage.
- You can set a scaling policy that restricts instances in a scaling group to only allow scaling and not scale-in.
- You can view the scaling logs of the scaling group and the instance information added to the scaling group to view all the execution actions and reasons of the auto scaling group, so as to facilitate the maintenance of the cluster business of the scaling group.
- Users can enable or disable a scaling group, which is unavailable after the scaling group is disabled, and will not perform instance scaling and health checks when the scaling policy is triggered, and disabling the scaling group will not affect the normal operation of existing instances in the scaling group.
- Provides the average CPU usage monitoring data of all instances in the scaling group, configures the monitoring data through the alarm template, and sends an alert email to users when the usage is too high and the scaling is triggered.

Supports auto scaling and load balancing, adding instances in a scaling group to a load-balanced listener to provide load balancing services for virtual machine services in a scaling group, and determining the service health status of all instances in a scaling group through the listener's health check mechanism, automatically removing unhealthy instances and adding healthy instances to the service cluster.

## Virtual Machine Template Management
The virtual machine template is a user-defined virtual machine template according to the needs. It is used to automatically create a virtual machine template during elastic scaling. It also supports manual creation of virtual machines through virtual machine templates, and supports the creation, viewing, deletion, and creation of virtual machine templates. and other life cycle management.

### Create a virtual machine template
According to business needs, users can create a virtual machine template suitable for the business through the virtual machine console - virtual machine template resource list, which is used for the template when the scaler automatically increases the virtual machine instance. The virtual machine template is the same as creating a virtual machine, you only need to provide the template name and template remarks, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-121.png)

The template name and remarks refer to the name and description of the virtual machine template to be created currently. The basic configuration, network equipment, and management configuration are consistent with creating a virtual machine, and represent the parameters specified when using this template to create a virtual machine, such as virtual machine specifications and operations system, etc.; at the same time, the virtual machine template does not need to specify the virtual machine name, and the virtual machine name is automatically generated when the auto scaling group is created.

When creating a template, you need to specify the payment method for the virtual machines in the template, that is, the payment method for the virtual machines created through the template, such as monthly; at the same time, when creating the template, it will display the fee that needs to be paid when the template is created.

Fees are not actually deducted when the template is created, but are deducted based on the estimated cost only when the auto scaling group automatically creates instances.

### View VM template
Users can view the list information of virtual machine templates on the virtual machine template list page, including name, ID, VPC, subnet, model, configuration, associated resources, billing method, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-122.png)
 
- Name/ID: Refers to the name and globally unique identifier of the virtual machine template.
- VPC/Subnet: Refers to the VPC and subnet to which the VM created through the VM template belongs.
- Model/Configuration: refers to the model and configuration of the virtual machine created through the virtual machine template.
- Associated resource: refers to the auto scaling group associated with the virtual machine template, that is, the scaling group will create instances based on the configuration in the virtual machine template.
- Billing method: refers to the billing method for the virtual machine created through the virtual machine template.

The operation item on the list refers to manually creating a virtual machine through a virtual machine template. At the same time, the user can also click the name of the virtual machine template to enter the details of the virtual machine template and view the detailed information of the virtual machine template, including basic information, virtual machine configuration, network configuration and Store configuration information.

### Create a virtual machine from a template
Allows users to manually create virtual machines from templates, which is applicable to scenarios where virtual machines need to be created from templates. As shown below:

![1](/assets/images/user-guide/user-guide-123.png)

When creating a virtual machine from a template, you need to specify the name of the virtual machine, and you can also specify the login password of the virtual machine. If not specified, the login password specified when creating the template is used.

### Modify name and remarks
Modify the name and notes of the virtual machine template, which can be operated in any state. It can be modified by clicking the "Edit" button to the right of each image name on the virtual machine template list page.

## Scaling group management
The scaling group is responsible for maintaining the number of instances in the group at the "desired" water level. The actions of adding/shrinking virtual machines are all operated by the scaling group, which supports "automatic scaling" and "fixed number" to maintain instances in the scaling group. Quantity, suitable for various automatic scaling scenarios. The scaling group maintains the expected level of instances in the group through the scaling rules and the number of scaling instances in the scaling policy, and creates new instances through virtual machine templates.

- The automatic scaling mode maintains the number of instances in the scaling group according to the scaling policy of the scaler;
- The fixed number mode scales the instances in the group according to the number of instances specified by the user, that is, the fixed number mode does not need to specify a scaling policy.

Support the warm-up time of the scaling group, so that after the virtual machine is successfully created, the application program is pulled up within the warm-up time to undertake business traffic; at the same time, it supports the associated load balancing of the scaling group, providing load balancing services for the virtual machine business in the scaling group, and at the same time Through the health check mechanism of the listener, determine the business health status of all instances in the scaling group, automatically remove unhealthy business instances and add healthy instances to the business cluster.

As the core module of the automatic scaling service, it supports the full lifecycle management of scaling groups, including creating scaling groups, viewing scaling groups, modifying scaling groups, enabling/disabling scaling groups, binding/unbinding load balancing, viewing scaling logs and deleting Stretching groups, etc.

### Create a scaling group
Through the scaling group console, users can specify the virtual machine template, warm-up time, and scaling mode to create an elastic scaling group, which is used to dynamically scale the virtual machine instances in the group to adapt to business scenarios that require elastic scaling. The following two figures show an example of creating a scaling group in two scaling modes: automatic scaling and fixed-number scaling:
 
![1](/assets/images/user-guide/user-guide-127.png)
 
Different scaling modes require different numbers of instances to be specified. In the automatic scaling mode, a scaling policy needs to be specified. In the fixed scaling mode, only the number of instances in the desired group needs to be specified.
- Name/Remark: The name and remarks of the scaling group are used to identify the scaling group.
- Virtual machine template: The scaling group is the virtual machine template used when adding virtual machine instances in the group, that is, new virtual machine instances will be added to the group according to the selected virtual machine template, which must be specified when creating, and only supports the selection of permission virtual machine template.
- Warm-up time: The warm-up time after the virtual machine instance in the scaling group is successfully created. During the warm-up time, the virtual machine can pull up the application to undertake business traffic. Middle] status. Must be specified when creating, defaults to 300s .
- Scaling mode: the basis for the scaling group to maintain the number of virtual machine instances in the group. It supports two modes: automatic scaling and fixed number, which are suitable for different application scenarios. The default is the automatic scaling mode.
  - When the automatic scaling mode is selected, the user needs to specify a scaling policy, that is, maintain the number of instances in the scaling group according to the configuration in the scaling policy.
  - When the fixed number mode is selected, the user needs to specify the number of instances, that is, the number of instances in the scaling group is maintained by the specified number.
- Scaling policy: the scaling policy associated with the scaling group, which can be selected only when the scaling mode is automatic scaling, and only one scaling policy can be selected at a time.
- Number of instances: the expected number of virtual machines in the scaling group, which can only be selected when the scaling mode is fixed, and the minimum is 1.

Template configuration and policy configuration are used to display the instance configuration and scaling policy configuration information of the currently selected virtual machine template. Template configuration includes model, image, configuration, data disk, external network IP, etc.; scaling configuration includes scaling rules, maximum instance number, minimum number of instances, and scaling policy.

After clicking Create, the platform will create a scaling group according to the specified configuration, automatically maintain the number of instances in the scaling group, and deduct the cost of adding new instances (the scaling group does not charge additional fees, but users need to create virtual machine instances), users can view the instances and related information of the scaling group through the scaling group list and details.

### View scaling group
#### Scaling group list
Users can view the list and related information of all scaling groups in the account through the elastic scaling console, and enter the scaling group details through the name of the list to view the monitoring information, instance information, and scaling logs of the scaling group. Load balancing performs load balancing binding and unbinding operations on the scaling group. The specific list information is shown in the following figure:

![1](/assets/images/user-guide/user-guide-125.png)
 
- Name/ID: the name and globally unique identifier of the scaling group.
- Launch template: the virtual machine template associated with the scaling group, that is, the virtual machine template used when adding an instance in the group.
- Scaling mode: The scaling group maintains the mode of the number of virtual machine instances in the group, including automatic scaling and fixed number.
- Scaling policy: The associated scaling policy name and ID will be displayed only when the scaling mode is auto scaling.
- Expected instances: the expected number of virtual machine instances in the scaling group.
- Current instance: the current number of virtual machine instances in the scaling group, which is normally equal to the expected number of instances.
- Status: refers to the running status of the scaling group, including available and unavailable.

The list supports operations such as updating, starting, disabling, and deleting scaling groups, as well as enabling, disabling, and deleting scaling groups in batches.

#### Details of the scaling group
Users can enter the details of the scaling group to view detailed information in the scaling group, including the basic information of the scaling group, scaling policy, monitoring information, instance information, scaling logs and associated load balancing information, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-128.png)
 
(1) Basic information: including resource ID, resource name, virtual machine template, warm-up time, and the number of current instances.

(2) Scaling strategy: including scaling mode and expected number of cases.

(3) Monitoring information: the monitoring information of scaling indicators. Users can view the average CPU usage rate of all virtual machine instances in the group through monitoring information, and configure alarms for monitoring data through alarm templates. When scaling down, an alarm email is sent to the user, which can be viewed for one hour by default, and can be filtered by time to view monitoring information at a custom time.

(4) Instance information: refers to the list information of the current instances in the scaling group, including the instance name, ID, startup template, IP address, creation time, and status. For details, see Viewing scaling group instances.

(5) Scaling log: Refers to the instance scaling log in the scaling group, such as the log information of adding or removing an instance, see Viewing the scaling log for details.

(6) Load balancing: refers to the load balancing instance associated with the scaling group, and can be bound or unbound through load balancing management. For details, see Load balancing management.

#### View scaling group instances
Users can view the instance information in the current scaling group through the Instance tab in the scaling group details, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-129.png)
 
- The name of the instance is automatically named by the scaling group, usually the name of the scaling group + a random string.
- The startup template is the virtual machine template used to create the current instance. You can view the specific configuration information through the virtual machine template.
- The IP address is the VPC intranet IP address of the VM instance.
- Status is the status information of the current instance, including starting, running and other related status information of the virtual machine.
  - Start means that the virtual machine in the scaling group is starting, or during the warm-up time of the scaling group, the state of the instance will keep starting. After the warm-up time exceeds, it will automatically switch to running, which means that the virtual machine is in a healthy state .
  - The scaling group obtains the status of all virtual machines controlled by it every 15 seconds to determine whether instances need to be added or deleted. If the scaling group has load balancing turned off, the load balancing will determine whether the instances in the scaling group are healthy.
- Creation time refers to the creation time of the current instance.

#### View scaling logs
You can check the instance change log in the current scaling group through Scaling Log in the scaling group details, such as adding an instance or removing an instance, and display the detailed reason and status of each change, including the operation time, execution action, reason and Status, which is convenient for users to maintain the scaling group cluster business, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-130.png)
 
- Operation time: refers to the operation time of the instance change log in the current scaling group.
- Execution action: the change operation of the current scaling group log, such as adding an instance, deleting an instance.
- Reason: The reason for the change of the current scaling log, such as adding an instance because the number of instances is less than the expected number of instances, or deleting an instance because the instance is not in a healthy state.
- Status: The status of the current scaling instance change, including operation success or operation failure.

### Modify the scaling group
It supports user-defined modification of the virtual machine template, warm-up time, scaling mode, and number of instances of the scaling group, which can be operated by updating the operation item in the scaling group list, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-131.png)
 
- Virtual machine templates<br/>
If the user modifies the virtual machine template of the scaling group, the configuration and parameters of the existing virtual machine instances in the group will not be affected, only the newly added instances will be affected, that is, the new virtual machine template will be used to create a virtual machine when a new instance is added after the update.
- Preheat time<br/>
After the user modifies the warm-up time, the newly added instance will be running according to the new warm-up time and accept business processing.
- Scaling mode/number of instances/scaling strategy<br/>
After the user modifies the scaling mode, the scaling group will immediately maintain the virtual machine instances in the group according to the number of instances expected by the new scaling mode.

### Enable/disable scaling group
Users can enable or disable a scaling group. After the scaling group is disabled, it will be unavailable and will not trigger the scaling policy to perform instance scaling and health check. Disabling the scaling group will not affect the normal operation of existing instances in the scaling group.

It is only supported to enable the scaling group in the disabled state. After enabling from the disabled state, the state of the scaling group changes to available, and the number of instances and the health status of the scaling group will be maintained based on the expected number of instances in the scaling mode.

### Load Balance Management
The platform supports scaling group-associated load balancing to provide load balancing services for the virtual machine business in the scaling group. At the same time, through the health check mechanism of the listener, it can judge the business health status of all instances in the scaling group, automatically remove unhealthy business instances and add new ones. Healthy instances to business clusters.

A scaling group can support the binding of multiple load balancing listeners (VServers), enabling the scaling group to provide various business services externally. Each load balancing listener will perform a load balancing business health check on the virtual machine instances in the group, and will automatically eliminate unhealthy instances, and the scaling group will automatically launch healthy instances into the group.

The scaling group obtains the status of all virtual machines controlled by it every 15 seconds to determine whether to add or delete instances. When the scaling group disables load balancing, the load balancing will determine whether the instances in the scaling group are healthy. If not, the specific process is as follows:

- Healthy instance is equal to expected value<br/>
The scaling group will automatically remove unhealthy instances (judged based on three-cycle health checks) out of the scaling group, and delete virtual machines.
- Healthy instances are larger than expected<br/>
Select to remove the latest created healthy virtual machine instance from the scaling group and perform the delete virtual machine operation, and remove the unhealthy instance from the scaling group and perform the delete operation.
- Healthy instances are smaller than expected<br/>
The scaling group will automatically initiate the instance creation operation based on the virtual machine template, maintain the number of instances at the expected value, and remove unhealthy instances from the scaling group and perform the deletion operation.

The platform supports the management of binding, unbinding, and viewing the bound load balancer of the scaling group, so that users can provide load distribution services for the virtual machine instances in the scaling group based on the automatic scaling load balancing, improving business efficiency. availability.

#### Binding load balancing
Users can bind a scaling group to multiple load-balancing VServer listeners, and the load-balancing listener will monitor the health status of the virtual machine instances in the scaling group. After binding, all instances in the scaling group will automatically be added as service nodes For the bound load balancing VServer, the user can enter the wizard page through scaling group details - binding in load balancing, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-132.png)
 
- Load Balancer/VServer: For the load balancer and VServer listener that needs to be bound, an existing and authorized load balancer instance must be specified. It must be specified when binding, and only one can be specified at a time.
- Port: The business service port provided by the virtual machine instance added to the load balancing service node in the scaling group, that is, the backend service port through which the load balancing forwards traffic to the virtual machine.

After binding, all instances in the scaling group will automatically join the bound load balancing VServer as service nodes, and the load balancing is responsible for checking the health status of the virtual machine instances in the group.

#### View the bound load balancer
After the scaling group is bound to load balancing, you can view the bound load balancing information through the load balancing details in the scaling group, including load balancing, VServerID, port, weight, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-133.png)
 
In the list, load balancer and VServer are the bound load balancer name and VServer instance ID; port and weight represent the backend service port and weight of the virtual machine instance in the scaling group added to the load balancer service node.

At the same time, in the operation item on the list, you can unbind each bound load balancer, which supports batch unbinding and facilitates resource maintenance.

#### Unbinding load balancing
Users can unbind the association between the scaling group and the load balancer when the business requires it. After unbinding, the virtual machine instances in the scaling group will be removed from the load balancing service nodes without affecting the virtual machines and services in the group. The operation and health status of itself are checked by the scaling group. Users can operate through scaling group details - load balancing unbinding or batch unbinding, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-134.png)

### Delete scaling group
The platform supports users to delete scaling groups that are not suitable for business scenarios, and can be deleted in any state. As shown below:


![1](/assets/images/user-guide/user-guide-126.png)
 
After deletion, the scaling group will be automatically unbound from the virtual machine template, scaling policy, and load balancer. At the same time, the virtual machines created by the scaling group will be automatically deleted. The virtual machine instances deleted by the scaling group will be directly destroyed and will not enter the recycle bin.



