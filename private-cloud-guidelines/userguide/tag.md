---
layout: default
title: Label Management
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/tag/
nav_order: 24
---
# 24. Label Management

## 24.1 Introduction to Label Functionality

### 24.1.1 Product Overview
Tags are used to label cloud resources, classify, search and aggregate cloud resources with the same characteristics from different dimensions, making resource management more convenient.
Tags are composed of a key-value pair (key: value), and users can customize the key-value pair content according to their needs, binding different resources.

### 24.1.2 Label Functionality
The tag management module has the following functions:

- Support batch creation of tags, single creation, and deletion of tags functions.
- Support viewing resources, display all resources bound to this tag.
- Support binding resources, select different resource types in different regions for binding.
- Support unbinding of resources, can be done in batches.

At the same time, tags support the selection of the required tags when creating resources, and support adding and deleting tags in the resource interface. The resource interface will display the key-value pairs of tags currently bound to the resource.
Support a unified search entry, which can bind resources to queries according to key/value, resource ID, and resource type. Flexibly operate resources, and search in the cloud resource interface and tag management interface, which is convenient for querying and managing a large number of tags, as well as quickly matching resources.

### 24.1.3 Usage Restrictions
**Label Naming Restrictions**
- The tag key and value support up to 127 characters, cannot be empty, and is case-sensitive.
- The tag key and value content support uppercase and lowercase numbers, Chinese characters, numbers, spaces, and special characters in UTF-8 format.

**Quantity Specification**
- One resource can be bound to a maximum of 50 tags.
- One tag contains one tag key and one tag value (tagKey:tagValue).
- One tag key on one resource can only correspond to one tag value.
- The maximum number of tags created in a single batch is not more than 5.

**Resource Status Restriction**
- Except for deleting, deleting, and failing, virtual machines cannot update tags for resources in other states, and can modify the tag content bound to the resource.

### 24.1.4 Supported Resources
Currently supported resource types on the tenant side include: virtual machines, virtual machine templates, images, VPCs, subnets, cloud disks, elastic network cards, snapshots, elastic IPs, load balancers, SSL certificates, security groups, IP groups, port groups, NAT gateways, Redis, MySQL, object storage, file storage, VPN gateways, peer gateways, tunnels, scaling groups, VIPs, multicasts, and isolation groups.

Currently, the resources supported by the management side include: public network, dedicated line access.

## 24.2 Label Operation Guide

### 24.2.1 View the tag management interface
Enter the Operation and Management module, click Tag Management to enter the management interface, which supports viewing the tag key, tag value, number of bound resources, and operations (view resources, bind resources, delete) as shown in the following figure:

![tagdiscirbe](/assets/images/userguide/tagdiscirbe.png)

### 24.2.2 Create tags
Click the Create button in the upper left corner of the Tags Management page to pop up the Tags Creation window. Fill in the tag key and tag value according to the format requirements [Use Restrictions](#_3513-使用限制). Click the plus sign after the Add Row to add a new key-value pair. Click the plus sign after the tag value to add a new value under the tag. As shown in the following figure:

![tagcreate](/assets/images/userguide/tagcreate.png)

### 24.2.3 Label adding resources

Click the Bind Resource button in the Tag Operation, and a Bind Resource pop-up window will appear, which supports viewing the list of bindable resources of different types in different regions. Click the box in front of the list to add resources in batches. The value of the tag that is displayed in the Bound Tag Value is the value of one of the resources currently bound to the key. If it is not displayed, the current resource under this key has not yet bound a value. The add interface is shown in the following figure:

![tagaddvm](/assets/images/userguide/tagaddvm.png)

**If the resource has already been tagged with a value under this key, binding a new tag value will unbind the old tag value**, and each resource can only correspond to one tag value for the same tag key.

You can click on the Edit Tags button on the relevant resource page to modify the tags associated with the resource. It supports deleting and adding tags under the resource.

### 24.2.4 Label Unbinding Resources

Click on "View Resources" in the Tag Operation, a list of resources bound to this tag will be popped up, which supports viewing the resource ID, resource type, region information, and unbind resources. Select the checkbox in front of the resource to select in batches, click "Unbind Resources" in the upper left corner to unbind in batches, or click the "Unbind Resources" button at the end of the resource row to unbind the resource.

![taguntievm](/assets/images/userguide/taguntievm.png)

### 24.2.5 Delete tag

Click the delete button on the tag line, a pop-up window as shown in the figure below will pop up, click confirm to delete the tag, and batch deletion is supported. When deleting, you need to make sure there are no resources bound to the tag, otherwise it cannot be deleted.

![tagdeletekey](/assets/images/userguide/tagdeletekey.png)







