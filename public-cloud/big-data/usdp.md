---
layout: default
title: USDP
parent: Big Data
grand_parent: Public Cloud
permalink: /public-cloud/big-data/usdp/
nav_order: 1
---
# USDP Intelligent Big Data Platform
## Product overview
SCloud Smart Data Platform (USDP) Professional Edition is an intelligent and lightweight big data basic service platform launched by SCloud, which can help you quickly build big data analysis and processing capabilities.

Based on years of experience in the development of big data platforms, SCloud released USDP Professional Edition, a one-stop intelligent big data platform for privatization deployment scenarios. It has the ability to build on IDC physical servers and cloud IaaS virtual servers, and supports the management of multiple sets of big data clusters through self-developed USDP Manager management tools, allowing you to create big data clusters with exclusive resources. Supports HDFS, Kudu, and ES ecosystems, and manages open source big data components such as Hive, HBase, Spark, Flink, and Presto in the cluster, such as cluster nodes, service configuration, monitoring alarms, and fault diagnosis, so as to help you easily build and manage the analysis and processing capabilities of big data services.

Self-developed management components, higher security and reliability
As a one-stop intelligent big data platform independently developed by SCloud big data team, USDP has the overall architecture as shown in the following figure:

Manager Server is a USDP management service that requires a MySQL instance to store metadata related to the cluster. The agent is a USDP slave node control service, which is used to manage and operate the node and the big data service on the node. Among them, "big data ecosystem services and component services" are various big data services (such as HDFS, YARN, etc.).

In USDP, InfluxDB, Prometheus, and Grafana are used as monitoring services to summarize and display the monitoring data of the entire cluster.
USDP supports a cluster size of at least 3 nodes and a maximum of thousands of nodes, and allows related services such as Manager Server and Agent to be deployed on the same node, so as to meet the needs of large businesses and help users meet the data analysis requirements of small businesses with a small cost as much as possible.
### The core advantages of USDP's one-stop intelligent big data platform

1. No need to worry about business binding
The big data services and components included in USDP all meet the Apache 2.0 open source license, and the SCloud big data team actively gives back to the community after doing a lot of compatibility tests, and fully releases the compiled compatibility package. <br/>Since it closely follows the pace of the open source community, users can perform independent replacement, independent construction, independent data migration, and cluster migration at any time, so there is no need to worry about the binding of big data services to closed-source services.<br/>
Based on the open management architecture, USDP integrates and adapts more than 30 open source big data ecosystem development components, covering all aspects of big data processing such as data integration, data storage, computing engine, task scheduling, and permission management. You can choose the corresponding components to build your own big data processing platform according to your business characteristics and needs.

2. Easy deployment
In order to allow users to experience a simplified big data deployment, O&M and management solution, USDP provides rich and detailed deployment, one-click environment repair tools and installation deployment, and users do not need to worry about preparing a lot of content during installation, and initializing the environment only needs a few simple steps to automatically complete the configuration.<br/>
The USDP management service provides complete cluster control and management functions, wizard-style operation procedures, and perfect scenario creation case recommendations and component distribution recommendations.

3. Comprehensive and rich monitoring indicators
The monitoring metrics provisioned by USDP mainly include three parts:
  - JMX full index acquisition
  - Http common indicator collection
  - Custom indicator collection

The above three parts of monitoring data will eventually be summarized in USDP's `Prometheus`, and the most commonly used monitoring indicators will be displayed in the overview page of each service, and in `Grafana`, users can view the most detailed monitoring indicators through the USDP official preset monitoring template (Dashboard). If the monitoring icons preset by USDP cannot meet your business needs, you can also customize and add the required monitoring charts.

4. Flexible and convenient alarm service
USDP provides preset alarm templates, and users only need to guide and configure simple to achieve the need to send cluster metric alarms to different destinations (WeChat, DingTalk, email, interface calls, etc.). Similar to the design of monitoring metrics, if you think that the preset alarm templates cannot meet your business needs, you can customize and modify the alarm templates or add new alarm rules.

5. Stable operation
The underlying resources of USDP are exclusive to you, and big data clusters can be combined with the underlying virtual networks of the business environment to achieve effective security isolation. Each component of USDP integration is compiled from the Apache Community stable version, has undergone rigorous compatibility testing and stress testing, and the key components all support high availability features to ensure stable and reliable cluster operation.<br/>
Based on years of experience in big data O&M, USDP presets perfect monitoring and alarm templates for each component, rich monitoring indicators and flexible alarm methods to help you grasp the operation status of each component in a timely manner and perform necessary maintenance and optimization. At the same time, intelligent fault diagnosis tools and professional technical support team escort the stable operation of your cluster.

6. Controllable cost
SCloud provides friendly and easy-to-deploy management services, users can operate with one click, no additional learning costs, greatly reduce the threshold of use, and assist users to easily use and maintain big data business systems. <br/>
For the privatization deployment scenario of big data applications, it supports the big data appliance solution to achieve one-stop delivery and out-of-the-box use. It supports a separate architecture of storage and computing, which is easy to achieve cost optimization.

7. Professional technical support
SCloud big data team has accumulated many years of experience in public cloud big data operation and maintenance and business optimization, providing users with big data expert technical support and customized solution capabilities, and continuously updating and integrating experience into products to provide users with automated services to solve users' worries about building big data services.

### USDP product version released

The various versions that have been released are suitable for the following requirements:
- Deploy CPU physical servers and virtual machines in common `X86_64` architecture;
- Deployed in domestic information innovation `X86_64`, `aarch64` architecture CPU physical servers, virtual machines;
- For specific private edition selections for normal server deployments, please see the list of USDP normal editions.
- For specific private version selections applicable to the server deployment, please refer to the USDP version list.
