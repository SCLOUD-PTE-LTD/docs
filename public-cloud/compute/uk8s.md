---
layout: default
title: UK8S
parent: Compute
grand_parent: Public Cloud
permalink: /public-cloud/compute/uk8s/
nav_order: 3
---
# UK8S Container Cloud

SCloud Container Service for Kubernetes (UK8S) is a Kubernetes-based container management service. You can deploy, manage, and expand your containerized applications on UK8S without worrying about the operation and maintenance of the Kubernetes cluster itself. UK8S is fully compatible with native Kubernetes API, based on SCloud private network, and integrates cloud products such as ULB, UDisk, EIP, VPC, etc.

## About Kubernetes

Kubernetes (k8s) is an operating system for automated deployment, expansion, and management of containerized applications. It has reached a production-level container orchestration and management platform. Using K8S, you will get the following benefits:

- Self-Repair

Automatically restart the failed container; when the node is unavailable, automatically schedule the container on the node to other nodes; terminate the container that fails the health check.

- Reduce Costs

Under the premise of not affecting availability, containers are automatically scheduled according to container resource requirements and constraints to maximize resource utilization and save costs.

- Dynamic Scaling

According to CPU or other indicators, automatically adjust the number of copies of the application.

- Automatic Deployment and Rollback

Supports multiple deployment scenarios, and can quickly roll back to the previous version when an error occurs.

## Basic Concept

The above picture is a summary Kubernetes architecture, including ApiServer, Master, Node, Hub (Image Warehouse) and other concepts, let us give a brief introduction in turn.

| Component | Description |
| --- | --- |
| ApiServer | ApiServer is the only entry point for operating the cluster, and provides mechanisms for authentication, authorization, access control, API registration, and discovery. ApiServer runs as a component on the Master. |
| Node | Node is the working node of Kubernetes, which contains the services required to run Pod. Node can be a virtual machine or a physical machine. In UK8S, currently only the virtual machine that is UHost is supported. |
| Master (ControlPlane) | Master is also a working node of Kubernetes. Unlike Node, Master usually does not run business Pods, but installs components such as ApiServer, Scheduler, Controller Manager, Cloud Controller Manager, ETCD, etc. for controlling and managing the cluster. |
| Hub Image Warehouse | Hub provides Docker image management, storage, and distribution capabilities. |

## Product Advantages

|   | UK8S | Self-built |
| --- | --- | --- |
| Cluster Management | One-click to create a cluster in 5 minutes; multi-zone support, high availability of Master through ULB4; | Users deploy and build by themselves; |
| Network Solution | High-performance network plug-in adapting to VPC, with no loss in performance; inter working with physical cloud, hybrid cloud, and public cloud internal network by default; Choose and deploy third-party network plug-ins by yourself; | Need to adapt to the existing network architecture; |
| Storage Plan | Currently supports SATA, SSD UDisk and UFS, and will also support cloud storage such as US3 in the future; | Build a storage cluster by yourself and adapt to Kubernetes storage types; |
| Load balancing | Integrated internal and public network ULB4/ULB7 high performance, high availability, automatic failover; | Deploy load balancing by yourself; |
| Using K8S | Provide web terminal; provide a graphical management interface consistent with cloud products; | Install kubectl by yourself; install dashboard by yourself; |
| Technical Support | The K8S expert team provides 7\*24 hours technical support all year round; provides guidance for business migration from VM to K8S, and uses specifications; | Learn and explore on your own; |

## Product Features

- Automated cluster deployment and operation and maintenance

Create a Kubernetes cluster with one click, support cloud hosts of various specifications, and dynamically increase or decrease working nodes.

- Deep integration of SCloud cloud products

Based on the Kubernetes extension interface, it integrates cloud products such as UDisk, ULB, and VPC.

- High availability ensures uninterrupted business

Cluster 3 Master is highly available. Nodes and applications support cross-availability zone deployment to ensure high business availability.

- Compatible with native Kubernetes

Fully compatible with the community's native API and CLI, multi-version support, keeping up with the latest version of the community.

- Fully controllable private cluster

Tenants are isolated between different user clusters, and cluster resources are visible to users and completely transparent.