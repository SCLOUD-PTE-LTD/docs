---
layout: default
title: Approval Process
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/approval-process/
nav_order: 13
---
# Approval Process
## Overview
### Product Introduction
With the practice and application of informatization and digital transformation in government, enterprise, education, finance, manufacturing and other industries, enterprises have increasingly strong demand for standardized and process-based management of resources. For cloud resources, it is also necessary to set up a standard approval process to meet the requirements of the platform. Resource application and approval business use process requirements.

For the management of enterprise cloud resources, the cloud platform is a resource approval service in a self-service mode for enterprise managers. It is used to formulate a standard use process for information system cloud resources. After the approval by the approvers and approval levels defined in the process is completed, the platform will automatically deliver the business resources required by the user.

The platform approval process is defined and published by the platform administrator, and it is up to the platform manager to set whether to set up the approval process for a tenant, which supports manual approval and automatic approval.

- Manual approval: When operating virtual resources under the tenant's primary account and sub-account, it is necessary to go through the application and approval process. After the approval is passed, the platform will automatically create or operate the required resources for the user, and generate an approval record for traceability.
- Automatic approval: The virtual resource operation of the master account and sub-account under the tenant does not require manual intervention. The system will automatically approve and pass, and automatically generate an approval record for retaining relevant application records and approval records.

The premise of enabling resource approval is to set the approval process, which is used to define how many levels of approval are required when tenants apply for resources, who will approve each level, and resources can only be created and changed after all levels have passed. In order to meet the approval business in various scenarios of the enterprise, the platform has a built-in default approval process.

The default approval process provides simple approval logic and only supports level 1 approval. When the platform manager starts the resource approval process for tenants, the creation and change applications of resources under tenants and sub-accounts are uniformly approved by the [platform administrator]. After the administrator approves, the platform will automatically execute the resource change operation.

The approval process supports the change operation of various resources, including virtual machine, cloud disk, VPC network, external network IP and load balancing. 

The supported changes are as follows:

- Virtual machine: create virtual machine, modify configuration, expand system disk, expand data disk;
- Cloud disk: create cloud disk, expand disk;
- VPC network: create a VPC;
- Extranet IP: create extranet IP, adjust bandwidth;
- Load Balancer: Create a load balancer.

### Approval and use process
This document uses the application for a virtual machine as an example to describe the manual approval process:

(1) Precondition: The administrator starts the approval process for the tenant.

(2) Application entry: Create a virtual machine.

- Enter the virtual machine console, and enter the virtual machine creation boot page through [Create Virtual Machine], as shown in the following figure:

- Fill in the application name and application remarks, enter other required information according to the creation requirements of the virtual machine, and click [Buy Now] to submit the application.
- After submitting the application, the page will automatically jump to the [Application Management] page, and an application record to be approved will be automatically added in the application list.
 
(3) Approval by the administrator: Approval is performed by the platform administrator admin account.

- If the platform administrator passes the application, the application status will change to [applied], and the virtual machine creation operation will be automatically performed. You can view the virtual machine resources being created through the virtual machine list. After the resources are successfully created, the application status will change It is 【Success】.
- If the platform administrator rejects the application, the status of the application will change to [Rejected], and the requested resource change will not be implemented. You can contact the platform administrator or check the approval notes to understand the reason for the rejection.

## Application Management
After a tenant changes a resource and submits an application, he or she can view the application record and status through the application management console.
### View application list
Through the My Application List, users can view all submitted application records under the tenant, including all records of manual approval and automatic approval as shown below:
 
- Application Name: The name of this application.
- Resource type: The resource type of this application, including five types of resources, including virtual machine, hard disk, VPC, external network IP, and load balancing.
- Action: Action on the resource. For example, create a virtual machine and create a VPC.
- Account Email: The applicant's account email.
- Account ID: The applicant's account ID.
- Status: The status of the application, including applied, successful, and rejected.
- Current node: the current node of the application, to quickly understand the progress of the application.
- Current processor: The current processor of the application, the default process is the platform administrator.
- Creation Date: The application time for this resource change.

### View application details
The user can enter the overview page of the application through the "Details" button on the right side of the application record. On this page, you can view the application information, the resources involved in the application, the processing records of the application, and the resources associated with the application.
 
(1) Application information: Describe the detailed information of this application, such as application name, application resource type, application change operation, application status and time.

(2) Related resources: describe the related resource information of this application. The application records approved by the administrator will have associated resources, and the rejected applications will not perform resource operations and will not generate associated resource information.

(3) Resource information: represents the specific configuration information of the resource change submitted in this application, such as the specification, VPC network, mirroring, etc. when applying for creating a virtual machine, and also includes the billing information when applying for resources.

(4) Processing record: the processing record of the current application record, and you can view the process nodes, processors, notes and processing time of all processing nodes.
- Process node: refers to all nodes of the approval process used by the current application record. For example, the default approval process of the platform includes application submission, default review node, resource operation, etc.
- Processor: Refers to the processor of each process node. For example, the email address of the submitting applicant is `john.doe@scloud.sg`, and the default processor of the audit node is admin@scloud.sg.
- Remarks: Refers to the remarks of each process node, such as the instructions given by the platform administrator when the application is approved or rejected.
- Processing time: refers to the processing time of each process node.

For the opening and changing of the tenant approval process and the approval management of the platform manager, please refer to the [Approval Process] chapter in the Platform Administrator Manual.