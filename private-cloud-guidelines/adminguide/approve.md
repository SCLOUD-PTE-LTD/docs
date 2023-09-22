---
layout: default
title: Approval Management
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/admin-guides/approve-management/
nav_order: 8
---
# 8. Approval Management

## 8.1 Overview

### 8.1.1 Product Introduction

With the practice and application of information digitization and transformation in industries such as government, education, finance, and manufacturing, the demand for standardized and process-oriented management of resource management is increasing. For cloud resources, it also needs to have a standard approval process to meet the service process requirements for platform resource application and approval.

For enterprise cloud resource management, the cloud platform provides self-service resource approval services for enterprise managers, which are used to develop standard usage processes for information system cloud resources. When tenants or sub-accounts need to use or manage resources, they will be automatically delivered by the platform after being approved by the defined approver and approval level in the process.

The platform approval process is defined and published by the platform administrator, and the platform manager sets whether to set up an approval process for a tenant, supporting manual and automatic approvals.

* Manual approval: When the main account and sub-account under the tenant perform virtual resource operations, they need to go through the application and approval process. After the approval is passed, the platform will automatically create or operate the required resources for the user and generate an approval record for tracing.
* Automatic approval: The main account and sub-account under the tenant can perform virtual resource operations without human intervention. The system will automatically approve them and generate an approval record to retain relevant application and approval records.

The prerequisite for opening resource approval is to set up an approval process, which defines how many levels of approval are required when a tenant applies for resources, who approves each level, and all levels must pass before resource creation and modification operations can be performed. To meet the approval business requirements of enterprises in various scenarios, the platform has built-in default approval processes.

The default approval process provides simple approval logic and only supports one-level approval. After the platform manager opens the resource approval process for the tenant, the creation and modification applications of resources under the tenant and sub-accounts will be uniformly approved by the [platform administrator]. After the platform administrator approves it, the platform will automatically perform the resource modification operation.

The approval process supports various resource modification operations, including virtual machines, cloud disks, VPCs, external IP addresses and load balancing, and elastic network cards. The supported modifications are as follows:

* Virtual machine: Create a virtual machine, modify its configuration, expand its system disk, and expand its data disk.
* Cloud disk: Create a cloud disk and expand its disk.
* VPC network: Create a VPC.
* External IP: Create an external IP address and adjust its bandwidth.
* Load balancing: Create a load balancing.
* Elastic network card: Adjust IP bandwidth.

### 8.1.2 Approval Use Process

**（1）The administrator opens the approval process for the tenant**

The administrator opens the approval process for the tenant through the tenant list, and can also open automatic approval. After opening, the tenant needs to initiate an approval process when creating a virtual machine. After the administrator approves it, the platform will automatically initiate the creation operation of the virtual machine.

**（2）The tenant applies for resource modification**

The tenant initiates a resource modification operation on the tenant console (taking the application for a virtual machine as an example):

1. Enter the virtual machine console and click [Create Virtual Machine] to enter the virtual machine creation guide page;

2. Fill in the application name and application remarks, and input other required information according to the requirements for creating the virtual machine. Click [Buy Now] to submit the application.

3. After submitting the application, the page will automatically jump to the [Application Management] page, and a new pending approval application record will be automatically added to the application list.

**（3） Administrator Approval**

The platform administrator approves or rejects the tenant's application through the pending approval in [Approval Management] to complete the approval process.

* If the platform administrator approves the application, the tenant's application status changes to "In Progress", and the platform will automatically create the virtual machine operation. You can view the virtual machine resources being created through the virtual machine list. After the resource is created successfully, the application status changes to "Success".

* If the platform administrator rejects the application, the tenant's application status changes to "Rejected", and the resource modification of the application will not be executed. You can contact the platform administrator or check the rejection reason in the approval remarks.

After the approval is completed, the administrator can view all approval records and resource details on the done approval list in the approval management interface for subsequent auditing of the platform and business.

## 8.2 Opening Approval for Tenants

The platform supports administrators opening approval processes for tenants when creating new tenants, as well as opening or closing approval processes for existing tenants, making it easier to manage and operate the platform.

### 8.2.1 Opening Approval Process when Creating a Tenant

When a new tenant is created by an administrator, it is possible to enable/disable the approval process for the tenant and set up automatic approval. This can be seen in the following image:

![approval](/assets/images/adminguide/approval.png)

- Resource Approval: Whether or not to enable resource approval for a tenant. If enabled, users under the tenant must submit requests for creating or modifying cloud resources such as virtual machines, disks, VPCs, and public IPs. The approval process is handled by platform administrators by default.
- Automated Approval: Whether or not to enable automated approval for a tenant. When enabled, requests made by the main/sub accounts of the tenant will be automatically approved without requiring manual intervention.

### 8.2.2 Approval Process for Existing Tenants

Administrators can enable or disable approval processes for existing tenants, as well as enabling or disabling automated approval. Through the Tenant Management page, administrators can modify existing tenant's approval processes, and view the current status of their approval process toggles, as shown below:

![editapprove](/assets/images/adminguide/editapprove.png)

## 8.3 Approval Management

Approval management provides platform administrators with a record of all the approval processes on the cloud platform, including pending and completed tasks.

* Pending tasks refer to resource modification requests that have been submitted but require administrator approval.
* Completed tasks refer to approval records that have already been processed by administrators, including both approved and rejected requests.


### 8.3.1 Viewing Approval Records

Platform administrators can view all pending and completed approval records through the Approval Management list. The completed tasks list displays records that have been approved or rejected by platform administrators, while the pending tasks list shows records that still require approval.

Administrators can access the Approval Management dashboard through the navigation bar and view all approval records information in the pending and completed lists, which are shown below:

![approvewait](/assets/images/adminguide/approvewait.png)

![approvedone](/assets/images/adminguide/approvedone.png)

Through the pending and completed lists, administrators can view lists of pending and completed approval records, including the request name, resource type, operation, account email, account ID, creation time, approval results, and operation items.

* Request Name: The name of the submitted request.
* Resource Type: The type of resource associated with the request.
* Operation: The operation performed on the resource as part of the request.
* Account Email: The email address of the account holder who made the request.
* Account ID: The ID of the account holder who made the request.
* Creation Time: The time when the request was created.
* Approval Results: The result of the approval process, either approved or rejected.

The operations menu in the Pending list allows administrators to approve or reject requests. Additionally, administrators can download both the pending and completed approval lists as local Excel files for operational convenience.

Administrators can access details pages for individual approval records by clicking on the Details button. This page displays information about the request, including basic information, resource information, related resources, and processing records. See the following image:

![approveinfo](/assets/images/adminguide/approveinfo.png)

（1）Request Information: Detailed information about the request, such as the request name, resource type, change operation, request status and time.

（2）Related Resources: Information about resources related to the current request. **Approved requests will be assigned related resources, while rejected requests will not execute resource operations and will not generate related resource information.**

（3）Resource Information: Specific configuration information for the resources submitted as part of the current request, such as specifications for creating virtual machines, VPC networks, and images, as well as billing information at the time of resource request.

（4）Processing Records: A record of the request's processing history, including all nodes in the approval process.

* Node: All nodes used in the current approval process for this request, such as submission, default review nodes, and resource operations.
* Processor: For each node in the approval process, the processor(s) responsible for that node, such as claire.kan@xxx.cn for the submitter or admin@xxx.cn for the default reviewer.
* Comment: Comments made by the processor for each node, such as notes provided by platform administrators during the approval or rejection process.
* Processing Time: The time required to process each node.

### 8.3.2 Approving Requests

After a tenant submits a modification request, administrators can approve it through the pending tasks list. When a request is approved, the platform will automatically execute the requested resource operation.

![approvepass](/assets/images/adminguide/approvepass.png)

### 8.3.3 Rejecting an Application

After a tenant initiates a resource change request, an approval record will be generated in the administrator's to-do list for approval management. Administrators can reject the tenant's application, i.e., deny the tenant's resource change request. After the approval is rejected, the application process will be terminated and the resource operation will not be executed.

![approverefuse](/assets/images/adminguide/approverefuse.png)

