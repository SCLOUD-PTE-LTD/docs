---
layout: default
title: Disaster Recovery Services
parent: Whitepaper
grand_parent: Private Cloud
permalink: /private-cloud/whitepaper/disaster-recovery-services/
nav_order: 5
---
# Disaster Recovery Services

SCloudStack cloud platform ensures the security of local data through distributed storage systems, and provides users with remote data backup and disaster recovery services through remote data backup services, which can archive and back up local cloud data to remote cloud platforms to ensure that services can be quickly restored through remote data centers in the event of major local disasters. There are two core indicators to consider in disaster recovery plans:
- RTO (RecoveryTime Object): Recovery Time Objective, which refers to the time required for the application system to recover from downtime to business recovery after a disaster in the data center, that is, the timeliness of business recovery, representing the maximum business recovery time that can be tolerated. The smaller the RTO value, the faster the business needs to be restored, and the relative cost is higher.
- RPO (Recovery Point Object): Recovery Point Objective, which refers to the time point in time corresponding to the data recovered by the disaster recovery system after a disaster occurs in the data center, that is, the maximum amount of data loss that can be tolerated when the application fails. The smaller the RPO value, the more important the data, and the frequency of data backup needs to be increased, and the relative cost is higher.

The standard of RTO and RPO has a linear relationship with the cost of disaster recovery scheme, and the requirements for RTO and RPO need to consider the characteristics and cost of the business system itself, as detailed in Information Security Technology Information System Disaster Recovery Specification.

SCloudStack disaster recovery service supports multiple service modes such as local disaster recovery, remote disaster recovery, public cloud disaster recovery, and three centers in two regions, and can flexibly select disaster recovery methods according to business characteristics and requirements to ensure RTO and RPO of services.

## Local disaster recovery

SCloudStack platform automatically shields software and hardware failures, disk damage, and software failures through distributed storage systems, RAID5 and multi-replica mechanisms, and the system automatically detects and automatically performs replica data backup and migration to ensure local data security. See [Distributed Storage] (#2.3.5 Distributed Storage). At the same time, the platform supports scheduled incremental backup of data such as local virtual machines, images, EVS disks, and databases to Object Storage Service. The local disaster recovery architecture is shown in the following figure:

![1](/assets/images/services-disaster-recovery-1.jpg)

- The platform supports flexible backup and recovery strategies, and can back up data in different time dimensions, full or incremental.
- When local data is damaged or accidentally deleted, you can restore the local backup data to the platform to restore business data and business operation.
- In the event of a disaster in an on-premises data center, the data center can be rebuilt and services restored through remote disaster recovery or public cloud disaster recovery.

## Remote disaster recovery services

 The SCloudStack cloud platform provides remote disaster recovery services while ensuring the security of business data in on-premises data centers, and incrementally replicates cloud service images and data to remote object storage services through leased lines, SD-WAN, VPN, or Internet connections to ensure business data RPO metrics. When a disaster occurs in an on-premises data center, you can quickly restore business through off-site data.

 ![1](/assets/images/services-disaster-recovery-2.jpg)

 The remote disaster recovery service supports multiple service deployment methods, provides different RTO indicators for cloud platform services, and controls the disaster recovery cost of cloud platform services.

### (1) High RTO indicators, long business recovery time, and low cost

- Service deployment: Only the object storage service is deployed in the remote disaster recovery center, and the cloud service image, business data, and database of the on-premises data center are fully or incrementally copied to the object storage service.
- Service recovery: Restore the backup data of the object storage service of the remote disaster recovery center to the local computer, restore cloud services and data locally, and rebuild the local data center.

### (2) Low RTO indicators, long business recovery time, and high cost

- Service deployment: All business applications, databases, and load balancers are deployed in local data centers and remote disaster recovery centers.
  - The on-premises data center is in Active mode, and the remote disaster recovery center is in Cold Standby mode.
  - Server Load Balancer: Each server for each service is deployed in the disaster recovery center.
  - Virtual machines: According to different requirements of the business for RTO, the disaster recovery center can deploy virtual machines with the same configuration or downgrade configuration.
    - For applications with high RTO requirements, the disaster recovery center needs to deploy virtual machines with the same configuration as the production center to quickly restore services and ensure the performance of the service operation environment when the service is switched.
    - For applications with low RTO requirements, degraded virtual machines can be deployed in the disaster recovery center to save resources and costs.
  - Database: Deploy the same set of database services in the disaster recovery center for each service, and the database of the disaster recovery center is read-only, and the databases of the two places use asynchronous data replication.
  - Storage: The disaster recovery center deploys the object storage service, and the disaster recovery center database is directly connected to the object storage for data reading and writing.
  - Intelligent DNS: Through Intelligent DNS Service, configure the business domain name A record as the IP address of the LB in the on-premises data center;
- Data replication: According to the different requirements of services for RPO, the two data centers adopt multiple network interconnections.
  - The database service asynchronously copies the data to the disaster recovery center object storage service.
  - Virtual machine images, business data, and file data are copied from the local data center to the disaster recovery center object storage in a full/incremental manner.
- Service recovery: When a disaster occurs in the on-premises data center or a service switch is required, modify the service domain name A record and database status.
  - Use intelligent DNS to manually modify the service domain name A record to the IP address of the service LB of the disaster recovery center to achieve failover and service recovery.
  - Modify the business application database service of the disaster recovery center to read-write state, and the business application database directly reads and writes data in object storage.

The interconnection between the remote disaster recovery center and the local data center network affects the frequency and integrity of business data backup. Due to the latency of the remote network, it is not recommended that both data centers be active.

## Public Cloud Disaster Recovery Service
SCloudStack provides disaster recovery services from on-premises data centers to public cloud platforms, and incrementally copies cloud service images and data to third-party public cloud platform object storage services through leased lines, SD-WAN, VPN, or Internet connections to ensure business data RPO metrics. When a disaster occurs in an on-premises data center, you can quickly restore services on the public cloud, and restore business data backups on the public cloud to the local data center and re-establish the local data center.

 ![1](/assets/images/services-disaster-recovery-3.jpg)

The public cloud disaster recovery service supports multiple service deployment methods, provides different RTO indicators for cloud platform services, and controls the disaster recovery cost of cloud platform services.
### (1) High RTO indicators, long business recovery time, and low cost
- Service deployment: Apply only for the object storage service on the public cloud platform, and copy the cloud service image, business data, and database of the on-premises data center to the object storage service in a full/incremental manner.
- Business recovery
  - Restore the backup data of the public cloud object storage service to the local computer, restore the business locally, and re-establish the local data center.
  - Use Object Storage to back up data, and directly deploy services such as service cloud hosts, load balancers, and databases on public cloud platforms to restore services.
### (2) Low RTO indicators, long business recovery time, and high cost
- Service deployment: All business applications, databases, and load balancers are deployed in local data centers and public cloud platforms.
  - The on-premises data center is the Active mode, and the public cloud platform is the Cold Standby mode;
  - Server balancing: Each service that requires load balancing applies for a Server Load Balancer instance in the public cloud and adds the service ECS to the backend.
  - Cloud host: According to the different requirements of the business for RTO, the public cloud can deploy cloud hosts with the same or downgraded configuration as the on-premises data center.
    - For applications with high RTO requirements, the public cloud needs to deploy cloud hosts with the same configuration as the production center to quickly restore services and ensure the performance of the service operation environment when the service is switched.
    - For applications with low RTO requirements, the public cloud can deploy cloud hosts with downgraded configurations to save resources and costs;
  - Database service: Deploy the same set of database services on the public cloud platform for each business, the cloud platform database services are read-only, and the databases in the two places use asynchronous mode for data replication.
  - Object storage service: The public cloud platform deploys the object storage service, and the public cloud platform business application database directly connects to the object storage for data reading and writing.
  - Intelligent DNS: Through Intelligent DNS Service, configure the business domain name A record as the IP address of the LB in the on-premises data center;
- Data replication: According to the different requirements of business RPO, multiple network interconnections are adopted between local data centers and public cloud platforms.
  - The database service asynchronously copies the data to the public cloud platform object storage service;
  - Virtual machine images, business data, and file data are copied from the on-premises data center to the public cloud platform object storage service in a full/incremental manner.
- Service recovery: When a disaster occurs in the on-premises data center or a service switch is required, modify the service domain name A record and database status.
  - Use intelligent DNS to manually modify the service domain name A record to the IP address of the service LB of the public cloud platform to achieve failover and service recovery.
  - Modify the business application database service of the public cloud platform to read-write state, and the business application database directly reads and writes data in object storage.

The network interconnection between the remote disaster recovery center and the public cloud platform will affect the frequency and integrity of business data backup. Due to the impact of network latency, it is not recommended that the public cloud platform business be active.

## Disaster preparedness services for three centers in two places
Three centers in two regions refers to a disaster recovery solution with dual centers in the same city and remote disaster recovery, which has both high availability and disaster backup capabilities. Two places refer to the same city and different places; The three centers refer to local data centers, intra-city disaster recovery centers, and remote disaster recovery centers. The dual centers in the same city have basically equivalent service processing capabilities and synchronize data in real time through high-speed links, usually sharing service operation and service access at the same time, and can switch operations. The remote disaster recovery center is used to back up data in the same city and dual centers.

The SCloudStack cloud platform provides disaster recovery services for three centers in two regions, supporting the addition of an off-site disaster recovery center on the basis of the active-active data center in the same city, and realizes synchronization with active-active in the same city. When both centers in the same city fail due to natural disasters, the remote disaster recovery center can restore data and further improve service RTO and RPO indicators.

 ![1](/assets/images/services-disaster-recovery-4.jpg)

The SCloudStack cloud platform uses two Availability Zones in a region in one region, namely the production zone and the disaster recovery zone. The distance between the two zones is about 30 kilometers, and the DWDM channel is used to directly interconnect the two zones network, which has the conditions of Layer 2 network connectivity and network load balancing, and meets the latency of the active-active network in the same city of less than 2ms.

### Service Deployment
In the disaster recovery mode of two regions and three centers, in which two centers in the same city are in one network, service deployment is different from that of remote disaster recovery services in order to achieve the goal of active-active high availability. SCloudStack provides multi-zone load balancing and database cross-zone high availability for dual-hub deployment. Remote disaster recovery center deployment deploys cloud services based on business RPO requirements.

Dual Centers in the Same City (ACtive-Active)

- Both the production availability zone and the intra-city disaster recovery zone are active, and both centers in the same city accept and handle business visits.
- Load balancing:
  - SCloudStack Server Load Balancer adopts a cluster architecture, based on cross-zone distributed deployment, and uses BGP+ECMP to implement cluster automatic disaster recovery to ensure RTO indicators for business availability zone level disasters.
  - Server Load Balancer instances deployed in intra-city regions are distributed in production zones and disaster recovery zones to ensure service reliability. Support all cloud hosts in the same city dual center as backend servers to achieve the goal of active in the same city;
  - For each service that requires load balancing, the same city dual center only needs to apply for one load balancing service instance in the production center, and the SCloudStack platform automatically deploys a load balancing service instance in the same city disaster recovery center for intra-city load balancing disaster recovery.
  - If all virtual machines of the same service in the same city and dual centers are added to the backend of the load balancer instance in the production center, the requests are loaded on the service virtual machines of the same city and dual centers, realizing the active-active request entry in the same city.
  - Typically, service requests are forwarded through the Server Load Balancer service instance in the production center, and the disaster recovery center in the same city automatically switches over the production center only in the event of a disaster.
- Virtual machines: For each service, deploy a certain number of virtual machines and data disks in the same city and dual centers, such as 4 production centers and 2 disaster recovery centers in the same city, and add 6 virtual machines to the backend of a Server Load Balancer service instance.
 - Database:
   - SCloudStack database service provides a disaster recovery solution, supports cross-zone high-availability instances, and can deploy disaster recovery instances based on multi-AZ.
  - If you deploy one set of database services in the production center for each service, the system will automatically deploy the same database instance in the disaster recovery center in the same city.
  - The dual-hub database in the same city synchronizes data in a semi-synchronous manner, and applications access the same database instance, and one copy of the data is stored in the production zone and one copy in the disaster recovery zone in the same city.
  - At the service level, you can also separate the database of the same city and dual centers from reading and writing, with the production center database in read-write mode and the intra-city disaster recovery database in read-only mode.
- Intelligent DNS: Through Intelligent DNS Service, configure the business domain name A record as the IP address of the LB in the on-premises data center;

Cold Standby

The remote disaster recovery service supports multiple service deployment methods, provides different RTO indicators for cloud platform services, and controls the disaster recovery cost of cloud platform services.
- High RTO indicators, long business recovery time, and low cost<br/>
  Only the object storage service is deployed in the remote disaster recovery center, and the cloud service image, business data, and database of the on-premises data center are copied to the object storage service in a full/incremental manner.
- Low RTO metrics, long business recovery time, and high cost
  - Server balancing: Each service that requires load balancing deploys a load balancer instance in an off-site disaster recovery center and adds the service virtual machine to the backend.
  - Cloud host: According to the different requirements of the business for RTO, the public cloud can deploy cloud hosts with the same or downgraded configuration as the on-premises data center.
    - For applications with high RTO requirements, the public cloud needs to deploy cloud hosts with the same configuration as the production center to quickly restore services and ensure the performance of the service operation environment when the service is switched.
    - For applications with low RTO requirements, the public cloud can deploy cloud hosts with downgraded configurations to save resources and costs;
  - Database service: Deploy the same set of database services in the remote disaster recovery center for each service, and the databases in the two regions copy data asynchronously.
  - Object storage service: Object storage is deployed in the remote disaster recovery center, and the business application database of the disaster recovery center is directly connected to the object storage for data reading and writing.

### Data Replication
In response to the different RPO requirements of services, the dual-center and remote disaster recovery centers in the same city adopt multiple network interconnections.

- Dual centers in the same city
  - The dual-hub database in the same city synchronizes data in a semi-synchronous manner, and applications access the same database instance, and one copy of the data is stored in the production zone and one copy in the disaster recovery zone in the same city.
  - Each service deploys virtual machines and storage in dual centers in the same city, and the cloud platform automatically copies the VM images of the production center to the disaster recovery center in the same city.
- Off-site disaster recovery center
  - The database service asynchronously copies the data to the disaster recovery center object storage service.
  - Virtual machine images, business data, and file data are copied from the local data center to the disaster recovery center object storage in a full/incremental manner.
### Business Recovery
- Dual-center in the same city is Active-Active mode, when the production center business fails or disaster:
  - Cross-zone Server Load Balancer automatically switches service requests to intra-city disaster recovery centers through internal DNS, and can restore services within minutes.
  - The database disaster recovery instance is automatically switched to the database instance of the disaster recovery center, which is transparent to users.
- The remote disaster recovery center is in Cold-Standby mode, when a disaster or service failure occurs in both centers in the same city:
  - Use intelligent DNS to manually modify the service domain name A record to the IP address of the service LB of the remote disaster recovery center to achieve failover.
  - Modify the business application database service of the remote disaster recovery center to read-write status, and the business application database directly reads and writes to object storage.
  - If you use a low-cost remote disaster recovery solution:
    - You can directly rebuild services from VM images, database backups, and related backup data in remote locations.
    - Restore data from the remote disaster recovery center to the local area and restore the local data center to restore services.

## Disaster recovery network architecture
The SCloudStack disaster recovery service network is a dual-center network in the same city and a disaster recovery network in different places, and different disaster recovery service construction methods are interconnected through different network links. The production center and the remote disaster recovery center can perform network communication and data replication through SD-WAN, leased line, VPN, and Internet, and can select different network connection solutions according to the RPO requirements of the business.

 ![1](/assets/images/services-disaster-recovery-5.jpg)

- Dual centers in the same city
  - The local data center and the intra-city disaster recovery center physically interconnect the intranet cores of the dual-center intranet through DWDM links, and connect the dual-center and layer-2 networks through Layer 3 to ensure network load balancing conditions and network latency of less than 2ms.
  - The dual centers in the same city are connected to the Internet through WAN links to carry the external network access of the dual centers in the same city.
  - Cross-zone deployment of load balancing, VPC, VMs, databases, and storage in the same city and dual hubs ensures high availability across zones.
- Off-site disaster recovery center
  - The remote disaster recovery center and the dual centers in the same city can be connected through multiple ways for data replication and database replication.
  - Remote disaster recovery network interconnection methods include SD-WAN, leased line, VPN, Internet, etc., and the network interconnection solution can be selected according to the RPO requirements of the service and the cost consideration.
  - SD-WAN/leased line
    - The external network core of the same city and dual centers is interconnected with the external network of remote disaster recovery through SD-WAN and dedicated line.
    - The line quality is good, the data replication and synchronization speed is fast, and the RPO of remote disaster recovery services can be guaranteed.
    - Low RPO indicators and high cost;
  - Internet / VPN
    - Directly interconnect the external network of the same city and dual centers and the external network of remote disaster recovery through the Internet or VPN;
    - Network quality cannot be guaranteed, data replication and synchronization speed is slow, and remote disaster recovery service RPO cannot be guaranteed.
    - High RPO indicators and low cost;

## Disaster Recovery Switch

SCloudStack disaster recovery services are divided into planned and unplanned switch according to business scenarios. According to the disaster recovery service mode, it is divided into intra-city and remote switching.
- The plan refers to business disaster recovery drills and cloud platform O&M, and the production center has not experienced disasters or failures, which is mostly used to verify disaster recovery service capabilities.
- Unplanned refers to the occurrence of large-scale disasters in the production center, such as earthquakes, electronic failures, virus attacks, etc., and the production center has been completely damaged;
- Intra-city switching refers to the service switch in which a disaster occurs in a data center in the same city dual center, and reasonable deployment of services can realize automatic disaster recovery in the same city and dual centers, without user intervention and switching, and automatic service recovery.
- Remote switching refers to the occurrence of disasters in both centers in the same city, which cannot provide services, and requires manual service recovery and switch.
- Under normal circumstances, dual centers in the same city automatically perform service disaster recovery switch without manual intervention, and the following describes only the remote disaster recovery switch.

### Planned Switch
- Data comparison: manual intervention compares the consistency and completeness of business data and resource data of the same city dual center and the remote disaster recovery center;
- Stop the production center business, network, load balancing, or turn off the power supply of hardware facilities;
- Check the service running status of the service virtual machine in the remote disaster recovery center, and check whether the virtual machine is on the backend of the service load balancer instance.
- Change the status of the service database of the disaster recovery center to read-write status, and test the status of accessing the service service address through the load balancing service address.
- Use intelligent DNS to manually modify the service domain name A record to the IP address of the service LB of the remote disaster recovery center to test the service service status.

If the remote disaster recovery center only deploys the object storage service, that is, only the backup of business data in the production center, you need to prepare the basic IaaS and PaaS environment for running the business in the remote disaster recovery center or production center, and restore the business VMs, load balancers, databases, and storage one by one through the backup data.

### Unplanned Switch
- Check and confirm that both centers in the same city are faulty or unavailable;
- Check the service running status of the service virtual machine in the remote disaster recovery center, and check whether the virtual machine is on the backend of the service load balancer instance.
- Change the status of the service database of the disaster recovery center to read-write status, and test the status of accessing the service service address through the load balancing service address.
- Use intelligent DNS to manually modify the service domain name A record to the IP address of the service LB of the remote disaster recovery center to test the service service status.
- Repair production centers or re-establish disaster recovery centers in different locations, and deploy related services to synchronize and replicate data and services.

If the remote disaster recovery center only deploys the object storage service, that is, only the backup of business data in the production center, you need to prepare the basic IaaS and PaaS environment for running the business in the remote disaster recovery center, and restore the service virtual machines, load balancers, databases, and storage one by one through the backup data.

### Disaster recovery failover
After the failure of the production center or the same city dual center is recovered or restored, the new production center in the remote area is switched back to the original production center, that is, the local data center. Disaster recovery fail-back is planned, and the switch process is consistent with planned switch.