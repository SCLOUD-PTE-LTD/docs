---
layout: default
title: Isolation Group
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/isolation-group/
nav_order: 28
---
# 28. Isolation Group

## 28.1 Product Overview

A isolation group is a simple arrangement policy for virtual machine resources, which supports instances within the group or between groups to be dispersed to different physical machines in order to ensure the high availability of the business. Node isolation group, which supports setting virtual machine and node affinity, anti-affinity policy, is an isolation group policy that is exclusive to administrators.

## 28.2 Create isolation groups

On the platform console, users can create isolation groups by specifying names, clusters, policy objects, and whether to enforce them. The isolation group is enabled by default, as shown in the following figure:

![createisolation](/assets/images/userguide/createisolation.png)

- Name: Name of the isolation group.
- Remark: Remarks information for the isolation group.
- Cluster: the computing cluster to which the isolation group belongs.
- Strategy Object Type: Virtual Machine Group.
- The policy object: Currently, the isolation group supports specifying the policy object of the isolation policy as intra-group or inter-group. Intra-group isolation is the virtual machine in the same group according to the policy scheduling, and inter-group is the virtual machine between the two isolation groups according to the policy scheduling.
- Strategy: The current isolation group supports both affinity and anti-affinity strategies.
- Enforced Execution: Once enforced execution is enabled, if the node cannot satisfy the virtual machine scheduling policy, the virtual machine instance will remain in the scheduling state until the scheduling policy conditions are met.
- Enable or not: Whether to enable isolation group, enabled by default.


## 28.3 View isolation group

You can enter the isolation group page through the navigation bar to view the list of isolation group resources under the current account and related details, including isolation group name, resource ID, compute cluster ID, status, number of hosts, policy objects, policies, whether to enforce, whether to enable, creation time and operation items, as shown in the following figure:

![isolationlist](/assets/images/userguide/isolationlist.png)

- Isolation Group Name: Current Isolation Group Name.
- Resource ID: Unique ID identifier for isolation group.
- Calculate Cluster ID: The ID of the computing cluster to which the isolation group belongs (the scheduling isolation granularity of the isolation group is scattered under a cluster ID).
- Status: Status of the isolation group, including Completed (all virtual machines in the isolation group have been scheduled except for shutdown and power off), Scheduling (there are virtual machines in the isolation group that have not been scheduled), Deleting.
- Number of Hosts: Number of Virtual Machines under Isolation Group.
- The policy object: Currently, the isolation group supports specifying the isolation policy object as intra-group or inter-group. Intra-group isolation is the mutual scheduling of virtual machines within the same group, and inter-group is the mutual scheduling of virtual machines in two isolation groups.
- Strategy: Currently, the isolation group supports both affinity and anti-affinity strategies.
- Is enforcement enforced: When enforcement is enabled, if the node cannot meet the virtual machine scheduling policy, the virtual machine instance will remain in scheduling state until the scheduling policy conditions are met.
- Enable: Whether to enable the isolation group.
- Creation Time: Creation time of the isolation group.
- Operation items: Modify, disable, and delete the isolation group.

## 28.4 View details of isolation group 

By entering the isolation group name into the isolation group detail page, you can view the basic information and instance information of the current isolation group, as shown in the following figure:

![isolationdetail](/assets/images/userguide/isolationdetail.png)

**(1) Basic Information**

The basic information of the isolation group, including name, compute cluster, compute cluster ID, policy, policy object, number of instances, forced execution, whether enabled, and status.

**Example Information**

The isolation group details page displays a list of instances under the current isolation group, including name, resource ID, status, belonging isolation group, node and operation.

## 28.5 Add an example

Support adding virtual machines that are in a shutdown/power-off state and consistent with the computing cluster belonging to the isolation group, as shown in the following figure:

![addinstance](/assets/images/userguide/addinstance.png)

- Isolation group: Isolation group name and ID.
- Example: Instances that can be joined.

## 28.6 Remove instance

Support removing instances from the isolation group, as shown in the following figure:

![deleteinstance](/assets/images/userguide/deleteinstance.png)

- Name: Instance Name.
- Status: Instance power state.

## 28.7 Enable isolation group 

When the isolation group is in disabled state and has policy objects bound, you can enable the isolation group.

## 28.7 Disable isolation group

When the isolation group is in the enabled state, the disable isolation group operation can be performed. When the isolation group is disabled, the instances in the isolation group will be removed and no instances can be added.

## 28.8 Modify the isolation group

When the isolation group status is disabled, you can modify the contents of the isolation group. It supports modifying policy objects and whether to enforce them, as shown in the following figure:

![updateisolation](/assets/images/userguide/updateisolation.png)

## 28.9 Delete the isolation group

Support user operations to delete isolation groups. When the isolation group status is idle, it can be operated to delete. When the isolation group status is scheduling/scheduling completed, it needs to be operated to disable the isolation group first, and then operated to delete. As shown in the following figure:

![deleteisolation](/assets/images/userguide/deleteisolation.png)



## 28.10 Node Isolation Group

### 28.10.1 Create node isolation group

The administrator supports creating node isolation groups, with the policy object type being node groups, supporting affinity and anti-affinity policies. Enabling forced execution when creating node groups may cause virtual machines to be unschedulable.

![deleteisolation](/assets/images/userguide/createnodeisolation.png)

### 28.10.2 Add and Remove Instances

The administrator supports adding/removing virtual machine instances in node isolation groups, and the administrator supports selecting node isolation groups when creating virtual machines for tenants. Virtual machine instances that are in shutdown or power-off state can be added to the isolation group.


### 28.10.3 Add and Remove Destination Nodes

The administrator can support adding available nodes to the destination node group or removing the destination node from the node group.


