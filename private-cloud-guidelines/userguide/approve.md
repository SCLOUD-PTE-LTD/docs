---
layout: default
title: Approval Process
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/approval-process/
nav_order: 18
---
# 18. Approval Process

## 18.1 Overview 

### 18.1.1 Product Introduction

With the practice and application of information digitization transformation in government, education, finance, manufacturing and other industries, enterprises have an increasingly strong demand for standardized and process-oriented resource management. For cloud resources, standard approval processes must also be set up to meet the business use process requirements of platform resource application and approval.

For the management of cloud-based resources for enterprises, the cloud platform provides a self-service resource approval service for enterprise managers to formulate standard usage processes for cloud-based information systems. When tenants or sub-accounts need to use or manage resources, after the approval is completed according to the defined approver and approval level in the process, the platform automatically delivers the business resources needed by the user.

The platform approval process is defined and published by the platform administrator, and the platform manager sets whether to set up an approval process for a tenant, supporting manual approval and automatic approval.

* Manual approval: When the main account and sub-account under the tenant operate virtual resources, they need to go through the application and approval process. After the approval is passed, the platform will automatically create or operate the required resources for the user, and generate an approval record for traceability.
* Automatic approval: Virtual resource operations of the tenant's main account and sub-accounts do not require manual intervention, the system will automatically approve and automatically generate an approval record to retain the relevant application record and approval record.

Enabling resource approval requires setting up an approval process to define how many levels of approval are required when tenants apply for resources, who will approve each level, and resources can only be created or changed after all levels have been approved. To meet the approval needs of various business scenarios, the platform provides a default approval process.

The default approval process provides a simple approval logic, supporting only one level of approval. When the platform administrator enables the resource approval process for the tenant, the creation and change requests of resources under the tenant and sub-accounts shall be approved by the Platform Administrator. That is, after the Platform Administrator approves, the platform will automatically execute the resource change operation.

The approval process supports various resource changes, including virtual machines, cloud disks, VPC networks, public IPs, and load balancers. The supported changes are as follows:

* Virtual Machine: Create virtual machine, modify configuration, expand system disk, expand data disk.
* Cloud disk: Create cloud disk, expand disk.
* VPC Network: Create VPC.
* External IP: Create external IP, adjust bandwidth.
* Load balancing: Create load balancing.

### 18.1.2 Approval Usage Process

This article describes the use of manual approval process with an example of applying for a virtual machine:

**(1) Prerequisite: The administrator enables the approval process for the tenant.**

**(2) Application Entry: Create Virtual Machine.**

* Enter the virtual machine console and enter the virtual machine creation wizard page through [Create Virtual Machine], as shown in the following figure:

  ![vmapprove](/assets/images/userguide/vmapprove.png)

* Fill in the application name and application note, enter other required information according to the requirements of the virtual machine creation, and click "Buy Now" to submit the application.

* After submitting the application, the page will automatically jump to the "Application Management" page, and a pending application record will be automatically added to the application list.

  ![approvelist](/assets/images/userguide/approvelist.png)

**(3) Administrator Approval: Approved by the platform administrator admin account.**

* If the platform administrator passes the application, the application status will be changed to "Applied" and the virtual machine creation operation will be automatically executed. You can check the virtual machine resources being created in the virtual machine list. After the resources are successfully created, the application status will be changed to "Success".
* If the platform administrator rejects the application, the application status will be changed to "Rejected" and the requested resource changes will not be executed. You can contact the platform administrator or check the approval notes to understand the reason for the rejection.

## 18.2 Application Management

After the tenant makes changes to the resources and submits the application, they can view the application record and application status through the application management console.

### 18.2.1 View application list

Users can view all submitted application records, including both manual and automatic approvals, under their tenant through my application list, as shown in the following figure:

![applicationlist](/assets/images/userguide/applicationlist.png)

* Name of Application: Name of this application.
* Resource type: The resource type of this application includes five types of resources, such as virtual machines, disks, VPCs, public IPs, and load balancers.

* Operation: Operation on resources. For example, creating virtual machines, creating VPCs.

* Account Email: The applicant's account email.

* Account ID: Applicant's account ID.

* Status: The status of the application, including processing, success, and failure.

* Current Node: The current node of the application, to quickly understand the progress of the application.

* Current handler: The current handler of the application, the default process is all platform administrators.

* Creation Date: The application time for this resource change.

### 18.2.2 View application details

Users can enter the overview page of the application by clicking the "Details" button on the right side of the application record. On this page, you can view the application information, the information of the resources involved in the application, the processing record of the application, and the associated resources of the application.

![applicaitondetail](/assets/images/userguide/applicaitondetail.png)

（1）Application Information: Detailed information about this application, such as the application name, type of resources applied for, change operations applied for, status and time of the application.

（2）Describe the associated resource information for this application. **Approved applications by the administrator will have associated resources, while rejected applications will not perform resource operations and will not generate associated resource information.**

（3）Resource information: It represents the specific configuration information of the resource change submitted at the time of this application, such as the specifications when applying to create a virtual machine, VPC network, image, etc., as well as the billing information of the resource at the time of application.

（4）Processing Record: Processing record of the current application, and can view the process node, processor, remarks and processing time of all processing nodes.

* Process nodes: refers to all nodes of the approval process currently used for the application record, such as the platform's default approval process, including submitting an application, default review node, resource operation, etc.
* Processor: Refers to the processor of each process node, such as the email address of the applicant for submission is cinder.lv@xxx, and the default processor of the audit node is admin@xxx.
* Remark: Refers to the remarks of each process node, such as the explanation given by the platform administrator when approving or rejecting the application.
* Processing time: refers to the processing time of each process node person.

> Regarding the opening, change and approval management of the tenant approval process, please refer to the "Approval Process" section in the Platform Administrator Manual.





































