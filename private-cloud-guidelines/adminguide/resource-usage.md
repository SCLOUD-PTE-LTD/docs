---
layout: default
title: Resource Usage
parent: Admin Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/admin-guides/resource-usage/
nav_order: 14
---
# 14. Resource Usage

## 14.1 Overview of Resource Usage

Resource Usage is a private cloud platform that aggregates resource monitoring across the entire platform, displays resource usage based on multidimensional queries and metric analysis, and supports administrators in creating monitoring reports and exporting Excel spreadsheets.

## 14.2 Creating Resource Usage Reports

Administrators can create Resource Usage reports by statistics on controlled console resources from four dimensions: resource usage cycle, tenant information, project group information, and resource type.

You can access the Resource Usage control panel through the navigation bar and then click "**Create**" to enter the wizard page, as shown in the figure below:

![createRU](/assets/images/adminguide/createRU.png)

* Name: The name of the Resource Usage report, which must be specified when created;
* Resource Usage Cycle: Ranges from 1 hour to 6 months and can be selected from 1 day/3 days/7 days/14 days/30 days/custom, with customizable start and end times accurate to the hour;
- Tenant Information: Tenant information can be selected from the platform tenants;
- Project Group Information: The project group list displays the selected project groups under the tenant. If no tenant is selected, all project groups on the platform will be displayed;
* Resource Type: Resource type can be selected from computing clusters, storage clusters, and virtual machines.

## 14.3 Resource Usage Report List

Administrators can view the Resource Usage list by accessing the Resource Usage control panel.

The Resource Usage list shows all created Resource Usage report information on the platform, including name, resource ID, status, tenant ID, project group information, resource type, creation time, start time, end time, and operation items, as shown in the figure below:

![RUlist](/assets/images/adminguide/RUlist.png)

- Name: The name of the Resource Usage report;
- Resource ID: The ID of the Resource Usage report serves as a global unique identifier;
- Status: The status of the Resource Usage report, including initialization, availability, and deletion;
- Tenant ID: The tenant ID bound to the Resource Usage report;
- Project Group Information: The project group information bound to the Resource Usage report;
- Resource Type: The resource type bound to the Resource Usage report;
- Creation Time: The creation time of the Resource Usage report;
- Start Time: The start time of the resource usage statistics cycle;
- End Time: The end time of the resource usage statistics cycle;
- Operation: The operation item on the list is for a single Resource Usage report, including view, download, and delete.

## 14.4 Viewing Resource Usage Details

Only reports that include virtual machines as a resource type can be viewed. The distribution chart of resource usage includes CPU usage distribution of virtual machines and memory usage distribution of virtual machines, with usage rate as the horizontal axis and the number of virtual machines as the vertical axis for statistics. Administrators can click "**View**" in the Resource Usage control panel operation to view details, as shown in the figure below:

![RUdetails](/assets/images/adminguide/RUdetails.png)

## 14.5 Downloading Resource Usage Reports

Administrators can download Resource Usage reports by clicking "**Download**" in the Resource Usage control panel operation. The report will be displayed based on the filtering options set when creating it. When the resource type is computing cluster, storage cluster, or virtual machine, the report is displayed as shown in the figures below:

* Computing Cluster

![RUcomputercluster](/assets/images/adminguide/RUcomputercluster.png)

* Storage Cluster

![RUstoragecluster](/assets/images/adminguide/RUstoragecluster.png)

* Virtual Machine

![RUvm](/assets/images/adminguide/RUvm.png)

* Distribution of virtual machine CPU usage

![RUcpu](/assets/images/adminguide/RUcpu.png)

* Distribution of virtual machine memory usage

![RUmem](/assets/images/adminguide/RUmem.png)

## 14.6 Delete Resource Usage Reports

Administrators are supported to delete resource usage reports. Click on "**Delete**" in the resource usage control console to delete resource usage reports, as shown in the figure below:

![RUrm](/assets/images/adminguide/RUrm.png)

