---
layout: default
title: Load Balancing
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/load-balancing/
nav_order: 9
---
# Load balancing
## Introduction to Load Balancing
Load Balance is a collection of servers composed of multiple servers in a symmetrical manner, each server has an equivalent status, and can provide services independently without the assistance of other servers. Platform Load Balancing Service (ULB—SCloudStack Load Balance) is a control service that automatically distributes network access traffic among multiple virtual machines based on the TCP/UDP/HTTP/HTTPS protocol, similar to a hardware load balancer of a traditional physical network.

Through the virtual service address provided by the platform load balancing service, add virtual machines from the same data center and the same VPC network to the load balancing forwarding backend, build the joined virtual machines into a high-performance, highly available, and highly reliable application server pool, and distribute requests from clients to the optimal virtual machines in the server pool for processing according to the forwarding rules of load balancing.

It supports two types of access ingress, both internal and external, and provides load access distribution for VPC intranet and EIP extranet, respectively, to adapt to multiple network architectures and high-concurrency load application scenarios. It provides Layer 4 and Layer 7 forwarding capabilities and multiple load balancing algorithms, supports features such as session protection and health checks, automatically isolates abnormal virtual machines, and provides SSL offloading and SSL certificate management capabilities, effectively improving the availability and service capabilities of the overall business.

ULB supports collecting and displaying monitoring data of various network metrics of load traffic, and can monitor, alarm, and notify according to alarm templates to ensure the normal operation of services. At present, ULB provides NAT proxy-based request distribution for the incoming virtual machine service pool, and in NAT proxy mode, all service requests and return data must go through ULB, similar to LVS's NAT working mode.

### Application scenarios
The platform provides two types of load balancing services: external network and intranet, corresponding to two scenarios: external network service and intranet service, respectively. You can choose to create a Server Load Balancer instance that is public or private to the outside or internally, and the platform assigns the IP address of the external network or the IP address of the VPC VPC according to the type of Server Load Balancer, that is, the service access address of Server Load Balancer.

- Usage scenarios of Internet load balancer:
  - Business services deployed on the platform need to build a cluster of highly available virtual machines and provide a unified access to the Internet.
  - Business services deployed on the platform need to build clusters of highly available virtual machines and provide unified access to IDC data centers.
- Intranet Server Load Balancer usage scenarios:
  - Service services deployed on the platform need to build a cluster of highly available virtual machines and only need to provide unified access to the VPC intranet.
  - Virtual machine clusters deployed in VPC VPCs need to mask the real IP addresses of other users or services and provide transparent services to clients.

You can also bind the IP address assigned by Server Load Balancer with your own domain name to access backend App Service through the domain name.

### Architecture Principles
A load balancer that provides services is mainly composed of three parts: LB instance (LoadBalancer), virtual server (VServer), and backend server (Backend Real Server). As shown in the architecture diagram:

![1](/assets/images/product-functional-architecture-15.jpg)

- LoadBalancer (LB): The Server Load Balancer instance is an active and standby high-availability cluster architecture, which can automatically fail over load balancers and improve the availability of access to Server Load Balancer. At the same time, combined with the internal and external IP addresses, according to the listener configured by VServer, the virtual machine is added to the backend to become a real server to achieve traffic balancing and service fault tolerance.
- Virtual Server (VServer): Listener, each listener is a set of load-balanced listening port configurations, including protocol, port, load algorithm, session persistence, connection idle timeout, health check and other configuration items, used to distribute and process requests to access LB.
- Backend Server Pool: A group of virtual machine server pools at the back end, and the real server (RealServer) that actually processes the request, that is, the virtual machine instance where the real business is deployed.
- External IP (EIP): An Internet Elastic IP address that is bound to an LB instance of the Internet network type and provides Server Load Balancer access to the Internet or IDC data centers.
- Private IP: The private IP address, the access address provided by the private network type LB instance, which is usually automatically assigned by the VPC specified when creating the private load balancer.

The load balancer is used to host VServer and access ingress, and VServer is responsible for port listening and request distribution of access ingress addresses. When the load balancer receives a request from a client, it distributes access request routing to the backend virtual machine server pool for request processing through a series of load balancing algorithms, and VServer returns the processing result to the client.

- Service request traffic is forwarded through weighted round-robin, minimum number of connections, and load-balanced scheduling policies based on source addresses to meet the service load requirements of multiple scenarios.
- Through the session persistence mechanism, sessions from the same client are forwarded to the same virtual machine for processing during the lifetime of the request session, which is suitable for application scenarios such as TCP persistent connections.
- Monitor RealServer health and service availability through the health check mechanism to ensure that traffic is distributed only to business-healthy virtual machines. When the back-end VM business is inaccessible, the scheduler stops distributing load traffic to the VM. After the VM business returns to normal, it rejoins the VM to the VServer backend and distributes traffic to the VM.

The working mode of the load balancer is NAT request proxy, and the request and return are forwarded and processed by the load balancer, that is, after the back-end RealServer virtual machine processes the request, it returns the request to the load balancer, which returns the result to the client.

### Features
The platform load balancing service provides Layer 4 and Layer 7 forwarding capabilities, supports both intranet and external network ingress, and supports functions such as health check, session persistence, connection idle timeout, content forwarding, SSL Offloading, and SSL certificate management on the basis of multiple load scheduling algorithms to ensure the availability and reliability of backend application services.
- Supports both intranet and public load balancers to meet the requirements of VPC intranet, IDC data center, and Internet service load balancer application scenarios.
- Provides Layer 4 and Layer 7 service load distribution capabilities, and supports listening and request forwarding based on TCP, UDP, HTTP, and HTTPS protocols.
- Supports weighted round robin, minimum number of connections, and load scheduling algorithms based on source addresses to meet traffic load services in different scenarios.
  - Weighted round robin: Based on weight-based polling scheduling, after the load balancer receives a new access request, it distributes traffic to each backend virtual machine according to the weight probability specified by the user for business processing.
  - Minimum number of connections: Based on the minimum number of connections of backend servers, the load balancer receives a new access request, counts the number of connections of the backend server pool in real time, selects the virtual machine with the lowest number of connections to establish a new connection, and performs business processing.
  - Source address: Based on the client's source IP address, the hash algorithm is used to forward access requests from the same IP address to a back-end virtual machine for processing.
- Provides the session persistence function to ensure that requests from the same client are forwarded to the same backend service node during the session lifetime. Layer 4 and Layer 7 each use different methods for session persistence.
  - For the UDP protocol, session persistence is guaranteed based on IP address, access requests from the same IP address are forwarded to the same back-end virtual machine for processing, and session persistence of the UDP protocol is supported for closing the session.
  - For HTTP and HTTPS protocols, it provides cookie implantation for session persistence, and supports automatic generation of KEYS and custom KEYS. Auto-generated keys are automatically generated by the platform for implantation, and custom keys are implanted by user-defined keys.
- Supports connection idle timeout configuration for TCP, HTTP, and HTTPS protocols, and automatically interrupts connections that have no access requests within the timeout period.
  - The request sent by the client to the LB address will maintain two connections on the platform, one from the client to LB and one from LB to the backend virtual machine;
  - The connection idle timeout refers to the idle timeout period of the connection from the client to the LB, if no data is sent or received within the timeout period, the connection from the client to the LB will be automatically interrupted;
  - The default connection idle timeout period is 60 seconds, which means that there are no new data requests for 60 seconds after the connection is established, and the connection is automatically interrupted.
- Health check: Supports port check and HTTP check, performs business health check on backend business servers based on rules, automatically detects and isolates virtual machines that are unavailable, and rejoins virtual machines to VServer backend and distributes traffic to virtual machines after the virtual machine business returns to normal.
  - Port inspection: For Layer 4 and Layer 7 load balancing, you can detect the health status of backend service nodes by IP address + port, and remove unhealthy nodes in time.
  - HTTP inspection: For Layer 7 load balancing, you can perform health checks based on URL paths and domain names carried in the HOST header of the request to filter healthy nodes.
- Content forwarding: For load balancing of Layer 7 HTTP and HTTPS protocols, it supports traffic distribution and health check capabilities based on domain names and URL paths, and forwards requests to different backend service nodes according to domain names and paths, providing more accurate service load balancing.
- SSL certificate: Provides unified certificate management services and SSL offloading capabilities for the HTTPS protocol, and supports one-way and two-way authentication of HTTPS certificates. SSL certificates are deployed to Server Load Balancer, and decryption authentication is performed only on Server Load Balancer, eliminating the need to upload the certificate to backend business servers, reducing the performance overhead of backend servers.
- HTTP to get the real IP of the client: The HTTP listener supports additional HTTP header fields to obtain the real IP address of the client through X-Forwarded-For and X-Real-IP.
- TCP gets the real IP of the client: The TCP listener uses Nginx's official Proxy-Protocol solution.
  - After the LB TCP listener receives the client's request, when forwarding the request to the back-end service node, encapsulate the client's source IP address in the TCP request packet and send it to the back-end service node, so that the server can obtain the client's IP address after parsing the TCP packet.
  - Proxy Protocol is an Internet protocol used to transmit connection information from the source of the request connection to the destination of the request connection, and obtain the client source IP by adding a Proxy Protocol header to the TCP packet, so the backend service node needs to do the corresponding adaptation work and resolve the Proxy Protocol header to obtain the client source IP address. The official documentation for Proxy-Protocol is detailed in https://docs.nginx.com/nginx/admin-guide/load-balancer/using-proxy-protocol/.
- Get the listener protocol: The HTTP listener supports additional HTTP header fields to obtain the listener's protocol through X-Forwarded-Proto.
- Additional Http Host: Http Listeners Support Attaching Http Header Fields, And Appending Host Domain Names To Http Requests Through Host, Which Is Used To Adapt To Services That Need To Detect The Http Header Host Field.
- Monitoring data: The load balancing level provides monitoring and alarming on the number of connections per second, inbound and outbound traffic per second, and the number of inbound and outbound packets per second. The VServer level provides monitoring data and alarms such as the number of connections, HTTP 2XX, HTTP 3XX, HTTP 4XX, and HTTP 5XX.
- Security control: Security groups are used to control access to the external network load balancer, and only traffic within the security group rules is allowed to reach the real backend server through load balancing, ensuring the security of service loads.

Server Load Balancer provides users with a service-level high-availability solution, which can deploy business applications to multiple virtual machines at the same time, and configure traffic balancing and forwarding through the SLB and DNS domain name solutions to achieve multi-service traffic load balancing. When large concurrent traffic accesses the VM service through Server Load Balancer, the request can be forwarded to the most robust VM in the backend for processing through algorithms such as the minimum number of connections and weighted round robin, and the request result is returned to the client through Server Load Balancer to ensure service availability and reliability.

You can use Intelligent DNS Service to bind a load balancer instance from two data centers to a domain name at the same time, and use DNS to implement cross-data center disaster recovery solutions.

### Load balancing isolation
- Resource isolation
  - Server Load Balancer has data center attributes, and the load balancing resources are physically isolated between different data centers.
  - Server Load Balancer resources are isolated from each other, and tenants can view and manage all Server Load Balancer resources under accounts and sub-accounts.
  - Server Load Balancer resources in a tenant can only bind VPC subnet resources in the same data center as the tenant.
  - Server Load Balancer resources in a tenant can only bind external IP resources in the same data center as the tenant.
  - You can bind a Server Load Balancer resource in a tenant to a security group resource in the same data center as the tenant.
- Network isolation
  - The load balancing resource network between different data centers is physically isolated from each other.
  - The load balancing network in the same data center is isolated by VPC, and the load balancing resources of different VPCs cannot communicate with each other.
  - The isolation of the external IP address network bound by Server Load Balancer depends on the configuration of the user's physical network, such as different VLANs.

## Load Balancing Management
### Usage Process
Before using Server Load Balancer, you must plan the network type and listener type of Server Load Balancer based on your business requirements, and deploy and configure service VMs on the platform according to your business requirements.

- Create and deploy multiple service VMs on the platform according to business requirements and planning, and ensure the availability of the service VMs on a single VM.
- Based on your business requirements, select the network ingress type and VPC of Server Load Balancer, and deploy a highly available Server Load Balancer instance on the cloud platform.
- In the created Server Load Balancer instance, configure the listener VServer as required, including parameters such as the protocol, port, load balancing algorithm, certificate, session persistence, and health check of the service.
- Add a service node for the configured VServer to determine the target of load balancing ingress request routing, that is, add the business VM instance deployed in step 1 to the service node of VServer;
- Server Load Balancer immediately checks the service nodes added to VServer and removes unhealthy service nodes in a timely manner.
- Access business services through the unified ingress IP address provided by the load balancing service.

### Creating a Load Balancer
To create a load balancer on the platform, you need to specify the cluster type, network type, VPC network, subnet, public IP address, and security group, and you can enter the Server Load Balancer resource console through the navigation bar, and enter the wizard page through Create Server Load Balancer, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-72.png)

This topic describes how to create a server load balancer of the Internet type, which does not require you to specify the External IP address and security group information.

(1) Select and configure the basic configuration and network information of the load balancer:
- Model: The cluster type of the host where the Server Load Balancer instance is located, customized by the platform administrator, such as x86 model and ARM model, and the instance created by the ARM model is an ARM version of the Server Load Balancer instance, which is adapted to domestic chips, servers, and operating systems.
- Network Type: You can select the type of network entry of the Server Load Balancer instance, which can be selected as internal network or external network. The intranet type provides the network ingress address of the VPC, and the public network type uses the bound public IP address as the network ingress address of the load balancer.
- VPC network: The VPC network served by Server Load Balancer only supports joining virtual machines of the same VPC network to the Server Load Balancer service node, and the Server Load Balancer instance itself runs in the specified VPC network.
- Subnet: The system automatically assigns the private IP address as the ingress address for the internal load balancer based on the subnet where the instance is located.
- External IP: When the network type is Internet, you can configure the public IP address automatically bound to the Server Load Balancer instance, and only the External IP address bound to IPv4 and with default routes can be used as the ingress address of Server Load Balancer.
- Security group: If the network type is Internet, you can configure an Internet security group automatically bound by Server Load Balancer to control the security of Internet access Server Load Balancer.
- Instance Name/Comment: The name and remarks of the Server Load Balancer instance.

(2) Select the purchase quantity and payment method, confirm the order amount, and click Buy Now to create a Server Load Balancer instance.
Purchase quantity: Create multiple SLB instances in batches according to the selected configuration and parameters, and only one SLB instance can be created at a time.
- Payment method: Select the billing method of Server Load Balancer, which supports three modes: hourly, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: The user selects the Server Load Balancer resource to display the fee according to the payment method.

After confirming the order, click Buy Now, after clicking Buy Now, you will return to the Server Load Balancer resource list page, where you can view the resource creation process, usually displaying the status of "Creating" first, and switching to the "Active" status within minutes, which means that the creation is successful.

### Viewing Load Balancing
Enter the Server Load Balancer console through the navigation bar to view the list of Server Load Balancer resources, enter the details page by the name and ID on the list to view the overview and monitoring information of Server Load Balancer, and switch to the VServer tab to manage VServer for Server Load Balancer.
#### Server Load Balancing List
The Server Load Balancer list can view the information of all Server Load Balancer resources in the current account, including name, resource ID, IP, VPC, subnet, number of VServer, creation time, expiration time, billing method, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-73.png)

- Name/ID: The name and globally unique identifier of the load balancer.
- IP address: The access address of the external service provided by Server Load Balancer, the IP address automatically assigned to the subnet if the network type is intranet, and the bound external IP address when the network type is external.
- Number of VServers: The number of listeners that have been created on the Server Load Balancer instance.
- Creation time/expiration time: The creation time and fee expiration time of the load balancer.
- Billing method: The billing method specified when the load balancer is created.
- Status: The running status of the load balancer, including Creating, Active, and Deleted.

The operation items on the list refer to operations on a single Server Load Balancer instance, including deleting or modifying alarm templates, modifying security groups, etc., and you can search and filter the list of Server Load Balancer resources through the search box, supporting fuzzy search.

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all load balancing resources owned by the current user as an Excel table. You can also delete load balancers in batches.

#### Server Load Balancing Details
On the Server Load Balancer resource list, click Name to go to the Overview page to view the details of the current Server Load Balancer instance, and switch to the VServer page to manage the VServer listener of the current Server Load Balancer, as shown on the Overview page:

![1](/assets/images/user-guide/user-guide-74.png)

(1) Basic information

For the basic information of the load balancer, including resource ID, name, creation time, expiration time, billing method, status, and alarm template information, you can click the button on the right side of the alarm template to modify the alarm template associated with the load balancer.

(2) Network information

Information about the network entrance of Server Load Balancing, including VPC network, subnet, and private IP address, if the Server Load Balancer is the Internet type, displays the public IP address and the security group information bound to it.

(3) Monitoring information

Monitoring charts and information related to Server Load Balancer instances, including the number of new connections, ingress/outward traffic, and outbound/inbound packets, can view monitoring data for 1 hour, 6 hours, 12 hours, 1 day, and custom time.

(4) VServer management

The current load-balanced listener lifecycle management includes VServer adding, viewing, modifying, and deleting operations, and can also manage VServer's backend service nodes and seven-layer content forwarding rules, see VServer Management.

### Modify the alarm template
Modify the alarm template to configure the monitoring data of the load balancer to alarm, and use the indicators and thresholds defined by the alarm template to trigger alarms when the relevant indicators of the load balancer fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure the network communication of the load balancer and services.

You can modify the alarm template through the action items in the load balancing list or the details page, and select a new load balancing alarm template in the Modify Alarm Template wizard to modify it.

### Modifying Security Groups
You can modify security groups from the perspective of Server Load Balancer, and you can modify the security group of Server Load Balancer only if the network type of the Server Load Balancer instance is Internet. You can modify the security group by modifying the Security Group in the load balancing list action item, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-75.png)

After a successful modification, the Internet Server Load Balancer restricts inbound and outbound traffic with a new security group policy, and you can view the bound security group information through the network information of the Server Load Balancer details.

### Modifying the Name and Remarks
Modify the name and comment of the Server Load Balancer resource to perform operations in any state. You can modify it by clicking the Edit button to the right of each load balancer name on the Server Load Balancer resource list page.

### Deleting Load Balancing
You can delete unwanted Server Load Balancer instances through the console or APIs, and automatically unbind the associated External IP addresses, backend service nodes, and bound SSL certificates, and clear the VServer listener and content forwarding rule policies created by Server Load Balancer.

If a Server Load Balancer instance is deleted, it must be directly destroyed, and no traffic requests must be made before deletion, otherwise the normal access of the service may be affected.

### Server Load Balancer Renewal
You can manually renew the load balancer operation, which applies only to the resource itself, and does not renew the resources associated with the resource, such as the public IP resources bound to the load balancer. After the additional associated resources expire, they are automatically unbound from Server Load Balancer, and to ensure normal service usage, you must renew the relevant resources in a timely manner.

When Server Load Balancer is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the billing method of Server Load Balancer is [Hours], the renewal period can be selected from 1 to 24 hours. If the billing method of Server Load Balancer is Monthly, you can select the renewal period from 1 to 11 months. If the billing method of Server Load Balancer is Annual, the renewal period of Server Load Balancer is 1 to 5 years.