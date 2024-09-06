---
layout: default
title: Global Network (UGN)
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/ugn/
nav_order: 8
---

# Product Overview

## What is UGN Cloud Network?

SCloud Global Network (UGN) is a next-generation global network product launched by SCloud, based on Segment Routing technology. It helps users quickly and flexibly build an enterprise-grade global network, enabling seamless interconnection of cloud and non-cloud network resources across all global regions.

The typical use cases of UGN Cloud Network are as follows:

1. **High-speed and stable private interconnection between VPCs**:
   * Multi-region business deployments in industries like online education/live streaming, requiring real-time synchronization of audio and video systems.
   * Game acceleration industry requiring multi-region interconnection to achieve high-speed and stable communication.
   * Business deployments across multiple locations that require multi-region disaster recovery.

2. **Private interconnection between VPCs and on-premises private sites**:
   * Network interconnection between multiple stores.
   * Connectivity between multiple private sites/offices and cloud VPC resources.
   * Cross-border/cross-region synchronization of business data and office operations.

## Components of UGN Cloud Network

UGN is a project-level product, allowing multiple UGN instances to be created under a single project. UGN instances are global and have no regional attributes.

UGN includes:

* **UGN Instance**: The foundational resource for creating and managing an integrated intelligent cloud network. A UGN instance contains one or more associated network instance types, including: private network instance (UVPC), UWAN virtual router, dedicated line virtual router, and hosted virtual router.

* **Network Instances**: UGN supports various types of network instances, enabling interconnection between cloud resources, cross-region resources, and cloud-to-ground resources such as multi-branch sites and VPC networks, hosted/IDC networks.
    * **Private Network Instance (UVPC)**: A customizable private network product for public cloud customers, connected to UGN to enable inter-VPC and cross-region communication.
    * **UWAN Virtual Router**: SDWAN access service, deployed at SCloud backbone core nodes. It connects the global backbone network and interfaces with on-premises sites via the Internet or dedicated lines. It provides enterprises with secure, encrypted transmission and smart networking functionalities for branches.
    * **Hosted Virtual Router**: Deployed at SCloud's hosted business nodes, it connects customers' hosted businesses to UGN, enabling easy cloud integration.
    * **Dedicated Line Virtual Router**: Allows customers to connect via dedicated lines to SCloud’s edge routers, deployed at POP locations in different regions. This provides a secure and stable cloud access through dedicated lines.
* **Bandwidth Package**: After creating a UGN instance and adding network instances, a bandwidth package is required to enable communication between instances.
    * **Intra-city Bandwidth Package**: Intra-city bandwidth packages are free and unlimited. If network instances are in the same region, joining UGN will enable communication within the same region.
    * **Cross-region Bandwidth Package**: If network instances are in different regions, a cross-region bandwidth package is required to enable communication. Users need to open point-to-point bandwidth packages between regions to achieve cross-region data transmission. The system provides a default 5kbps bandwidth package for testing connectivity.

## Use Cases

### **Intercommunication of Same-region Cloud Resources**

Multiple services within the same region need logical interconnection for business data transmission, which is closely related to customer operations. This requires secure and reliable data transmission, as well as support for flexible business configuration, switching, and access control to ensure efficient operations.

**Solution**

- Deploy multiple VPC instances in the same region.
- Deploy a UGN instance, load all VPC instances in the region, and establish same-region VPC communication.

![1](/assets/images/ugn-01.png)

### **Cross-region Cloud Resource Communication**

Public cloud resources are distributed globally, and services require logical interconnection for business data transmission, which is closely related to customer operations. This requires secure and reliable data transmission, as well as support for flexible business configuration, switching, and access control to ensure efficient operations.

**Solution**

- Deploy VPC instances in multiple regions.
- Deploy a UGN instance, load global VPC instances, and establish cross-region communication.
- The intelligent routing service allows users to choose the lowest latency path to avoid network detours and ensure business continuity.

![1](/assets/images/ugn-02.png)