---
layout: default
title: Load Balancing
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/load-balancing/
nav_order: 10
---
# Load balancing
## Introduction to Load Balancing
Load Balance is a collection of servers composed of multiple servers in a symmetrical manner, each server has an equivalent status, and can provide services independently without the assistance of other servers. Platform Load Balancing Service (ULB—SCloudStack Load Balance) is a control service that automatically distributes network access traffic among multiple virtual machines based on the TCP/UDP/HTTP/HTTPS protocol, similar to a hardware load balancer of a traditional physical network.

Through the virtual service address provided by the platform load balancing service, add virtual machines from the same data center and the same VPC network to the load balancing forwarding backend, build the joined virtual machines into a high-performance, highly available, and highly reliable application server pool, and distribute requests from clients to the optimal virtual machines in the server pool for processing according to the forwarding rules of load balancing.

It supports two types of access ingress, both internal and external, and provides load access distribution for VPC intranet and EIP extranet, respectively, to adapt to multiple network architectures and high-concurrency load application scenarios. It provides Layer 4 and Layer 7 forwarding capabilities and multiple load balancing algorithms, supports features such as session protection and health checks, automatically isolates abnormal virtual machines, and provides SSL offloading and SSL certificate management capabilities, effectively improving the availability and service capabilities of the overall business.

ULB supports collecting and displaying monitoring data of various network metrics of load traffic, and can monitor, alarm, and notify according to alarm templates to ensure the normal operation of services. At present, ULB provides NAT proxy-based request distribution for the incoming virtual machine service pool, and in NAT proxy mode, all service requests and return data must go through ULB, similar to LVS's NAT working mode.

### Application scenarios
The platform provides two types of load balancing services: external network and intranet, corresponding to two scenarios: external network service and intranet service, respectively. You can choose to create a Load Balancing instance that is public or private to the outside or internally, and the platform assigns the IP address of the external network or the IP address of the VPC VPC according to the type of Load Balancing, that is, the service access address of Load Balancing.

- Usage scenarios of Internet load balancer:
  - Business services deployed on the platform need to build a cluster of highly available virtual machines and provide a unified access to the Internet.
  - Business services deployed on the platform need to build clusters of highly available virtual machines and provide unified access to IDC data centers.
- Intranet Load Balancing usage scenarios:
  - Service services deployed on the platform need to build a cluster of highly available virtual machines and only need to provide unified access to the VPC intranet.
  - Virtual machine clusters deployed in VPC VPCs need to mask the real IP addresses of other users or services and provide transparent services to clients.

You can also bind the IP address assigned by Load Balancing with your own domain name to access backend App Service through the domain name.

### Architecture Principles
A load balancer that provides services is mainly composed of three parts: LB instance (LoadBalancer), virtual server (VServer), and backend server (Backend Real Server). As shown in the architecture diagram:

![1](/assets/images/product-functional-architecture-15.jpg)

- LoadBalancer (LB): The Load Balancing instance is an active and standby high-availability cluster architecture, which can automatically fail over load balancers and improve the availability of access to Load Balancing. At the same time, combined with the internal and external IP addresses, according to the listener configured by VServer, the virtual machine is added to the backend to become a real server to achieve traffic balancing and service fault tolerance.
- Virtual Server (VServer): Listener, each listener is a set of load-balanced listening port configurations, including protocol, port, load algorithm, session persistence, connection idle timeout, health check and other configuration items, used to distribute and process requests to access LB.
- Backend Server Pool: A group of virtual machine server pools at the back end, and the real server (RealServer) that actually processes the request, that is, the virtual machine instance where the real business is deployed.
- External IP (EIP): An Internet Elastic IP address that is bound to an LB instance of the Internet network type and provides Load Balancing access to the Internet or IDC data centers.
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
- SSL certificate: Provides unified certificate management services and SSL offloading capabilities for the HTTPS protocol, and supports one-way and two-way authentication of HTTPS certificates. SSL certificates are deployed to Load Balancing, and decryption authentication is performed only on Load Balancing, eliminating the need to upload the certificate to backend business servers, reducing the performance overhead of backend servers.
- HTTP to get the real IP of the client: The HTTP listener supports additional HTTP header fields to obtain the real IP address of the client through X-Forwarded-For and X-Real-IP.
- TCP gets the real IP of the client: The TCP listener uses Nginx's official Proxy-Protocol solution.
  - After the LB TCP listener receives the client's request, when forwarding the request to the back-end service node, encapsulate the client's source IP address in the TCP request packet and send it to the back-end service node, so that the server can obtain the client's IP address after parsing the TCP packet.
  - Proxy Protocol is an Internet protocol used to transmit connection information from the source of the request connection to the destination of the request connection, and obtain the client source IP by adding a Proxy Protocol header to the TCP packet, so the backend service node needs to do the corresponding adaptation work and resolve the Proxy Protocol header to obtain the client source IP address. The official documentation for Proxy-Protocol is detailed in https://docs.nginx.com/nginx/admin-guide/load-balancer/using-proxy-protocol/.
- Get the listener protocol: The HTTP listener supports additional HTTP header fields to obtain the listener's protocol through X-Forwarded-Proto.
- Additional Http Host: Http Listeners Support Attaching Http Header Fields, And Appending Host Domain Names To Http Requests Through Host, Which Is Used To Adapt To Services That Need To Detect The Http Header Host Field.
- Monitoring data: The load balancing level provides monitoring and alarming on the number of connections per second, inbound and outbound traffic per second, and the number of inbound and outbound packets per second. The VServer level provides monitoring data and alarms such as the number of connections, HTTP 2XX, HTTP 3XX, HTTP 4XX, and HTTP 5XX.
- Security control: Security groups are used to control access to the external network load balancer, and only traffic within the security group rules is allowed to reach the real backend server through load balancing, ensuring the security of service loads.

Load Balancing provides users with a service-level high-availability solution, which can deploy business applications to multiple virtual machines at the same time, and configure traffic balancing and forwarding through the SLB and DNS domain name solutions to achieve multi-service traffic load balancing. When large concurrent traffic accesses the VM service through Load Balancing, the request can be forwarded to the most robust VM in the backend for processing through algorithms such as the minimum number of connections and weighted round robin, and the request result is returned to the client through Load Balancing to ensure service availability and reliability.

You can use Intelligent DNS Service to bind a load balancer instance from two data centers to a domain name at the same time, and use DNS to implement cross-data center disaster recovery solutions.

### Load balancing isolation
- Resource isolation
  - Load Balancing has data center attributes, and the load balancing resources are physically isolated between different data centers.
  - Load Balancing resources are isolated from each other, and tenants can view and manage all Load Balancing resources under accounts and sub-accounts.
  - Load Balancing resources in a tenant can only bind VPC subnet resources in the same data center as the tenant.
  - Load Balancing resources in a tenant can only bind external IP resources in the same data center as the tenant.
  - You can bind a Load Balancing resource in a tenant to a security group resource in the same data center as the tenant.
- Network isolation
  - The load balancing resource network between different data centers is physically isolated from each other.
  - The load balancing network in the same data center is isolated by VPC, and the load balancing resources of different VPCs cannot communicate with each other.
  - The isolation of the external IP address network bound by Load Balancing depends on the configuration of the user's physical network, such as different VLANs.

## Load Balancing Management
### Usage Process
Before using Load Balancing, you must plan the network type and listener type of Load Balancing based on your business requirements, and deploy and configure service VMs on the platform according to your business requirements.

- Create and deploy multiple service VMs on the platform according to business requirements and planning, and ensure the availability of the service VMs on a single VM.
- Based on your business requirements, select the network ingress type and VPC of Load Balancing, and deploy a highly available Load Balancing instance on the cloud platform.
- In the created Load Balancing instance, configure the listener VServer as required, including parameters such as the protocol, port, load balancing algorithm, certificate, session persistence, and health check of the service.
- Add a service node for the configured VServer to determine the target of load balancing ingress request routing, that is, add the business VM instance deployed in step 1 to the service node of VServer;
- Load Balancing immediately checks the service nodes added to VServer and removes unhealthy service nodes in a timely manner.
- Access business services through the unified ingress IP address provided by the load balancing service.

### Creating a Load Balancer
To create a load balancer on the platform, you need to specify the cluster type, network type, VPC network, subnet, public IP address, and security group, and you can enter the Load Balancing resource console through the navigation bar, and enter the wizard page through Create Load Balancing, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-72.png)

This topic describes how to create a Load Balancing of the Internet type, which does not require you to specify the External IP address and security group information.

(1) Select and configure the basic configuration and network information of the load balancer:
- Model: The cluster type of the host where the Load Balancing instance is located, customized by the platform administrator, such as x86 model and ARM model, and the instance created by the ARM model is an ARM version of the Load Balancing instance, which is adapted to domestic chips, servers, and operating systems.
- Network Type: You can select the type of network entry of the Load Balancing instance, which can be selected as internal network or external network. The intranet type provides the network ingress address of the VPC, and the public network type uses the bound public IP address as the network ingress address of the load balancer.
- VPC network: The VPC network served by Load Balancing only supports joining virtual machines of the same VPC network to the Load Balancing service node, and the Load Balancing instance itself runs in the specified VPC network.
- Subnet: The system automatically assigns the private IP address as the ingress address for the internal load balancer based on the subnet where the instance is located.
- External IP: When the network type is Internet, you can configure the public IP address automatically bound to the Load Balancing instance, and only the External IP address bound to IPv4 and with default routes can be used as the ingress address of Load Balancing.
- Security group: If the network type is Internet, you can configure an Internet security group automatically bound by Load Balancing to control the security of Internet access Load Balancing.
- Instance Name/Comment: The name and remarks of the Load Balancing instance.

(2) Select the purchase quantity and payment method, confirm the order amount, and click Buy Now to create a Load Balancing instance.
Purchase quantity: Create multiple SLB instances in batches according to the selected configuration and parameters, and only one SLB instance can be created at a time.
- Payment method: Select the billing method of Load Balancing, which supports three modes: hourly, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: The user selects the Load Balancing resource to display the fee according to the payment method.

After confirming the order, click Buy Now, after clicking Buy Now, you will return to the Load Balancing resource list page, where you can view the resource creation process, usually displaying the status of "Creating" first, and switching to the "Active" status within minutes, which means that the creation is successful.

### Viewing Load Balancing
Enter the Load Balancing console through the navigation bar to view the list of Load Balancing resources, enter the details page by the name and ID on the list to view the overview and monitoring information of Load Balancing, and switch to the VServer tab to manage VServer for Load Balancing.
#### Server Load Balancing List
The Load Balancing list can view the information of all Load Balancing resources in the current account, including name, resource ID, IP, VPC, subnet, number of VServer, creation time, expiration time, billing method, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-73.png)

- Name/ID: The name and globally unique identifier of the load balancer.
- IP address: The access address of the external service provided by Load Balancing, the IP address automatically assigned to the subnet if the network type is intranet, and the bound external IP address when the network type is external.
- Number of VServers: The number of listeners that have been created on the Load Balancing instance.
- Creation time/expiration time: The creation time and fee expiration time of the load balancer.
- Billing method: The billing method specified when the load balancer is created.
- Status: The running status of the load balancer, including Creating, Active, and Deleted.

The operation items on the list refer to operations on a single Load Balancing instance, including deleting or modifying alarm templates, modifying security groups, etc., and you can search and filter the list of Load Balancing resources through the search box, supporting approximate search .

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all load balancing resources owned by the current user as an Excel table. You can also delete load balancers in batches.

#### Server Load Balancing Details
On the Load Balancing resource list, click Name to go to the Overview page to view the details of the current Load Balancing instance, and switch to the VServer page to manage the VServer listener of the current Load Balancing, as shown on the Overview page:

![1](/assets/images/user-guide/user-guide-74.png)

(1) Basic information

For the basic information of the load balancer, including resource ID, name, creation time, expiration time, billing method, status, and alarm template information, you can click the button on the right side of the alarm template to modify the alarm template associated with the load balancer.

(2) Network information

Information about the network entrance of Server Load Balancing, including VPC network, subnet, and private IP address, if the Load Balancing is the Internet type, displays the public IP address and the security group information bound to it.

(3) Monitoring information

Monitoring charts and information related to Load Balancing instances, including the number of new connections, ingress/outward traffic, and outbound/inbound packets, can view monitoring data for 1 hour, 6 hours, 12 hours, 1 day, and custom time.

(4) VServer management

The current load-balanced listener lifecycle management includes VServer adding, viewing, modifying, and deleting operations, and can also manage VServer's backend service nodes and seven-layer content forwarding rules, see VServer Management.

### Modify the alarm template
Modify the alarm template to configure the monitoring data of the load balancer to alarm, and use the indicators and thresholds defined by the alarm template to trigger alarms when the relevant indicators of the load balancer fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure the network communication of the load balancer and services.

You can modify the alarm template through the action items in the load balancing list or the details page, and select a new load balancing alarm template in the Modify Alarm Template wizard to modify it.

### Modifying Security Groups
You can modify security groups from the perspective of Load Balancing, and you can modify the security group of Load Balancing only if the network type of the Load Balancing instance is Internet. You can modify the security group by modifying the Security Group in the load balancing list action item, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-75.png)

After a successful modification, the Internet Load Balancing restricts inbound and outbound traffic with a new security group policy, and you can view the bound security group information through the network information of the Load Balancing details.

### Modifying the Name and Remarks
Modify the name and comment of the Load Balancing resource to perform operations in any state. You can modify it by clicking the Edit button to the right of each load balancer name on the Load Balancing resource list page.

### Deleting Load Balancing
You can delete unwanted Load Balancing instances through the console or APIs, and automatically unbind the associated External IP addresses, backend service nodes, and bound SSL certificates, and clear the VServer listener and content forwarding rule policies created by Load Balancing.

If a Load Balancing instance is deleted, it must be directly destroyed, and no traffic requests must be made before deletion, otherwise the normal access of the service may be affected.

### Load Balancing Renewal
You can manually renew the load balancer operation, which applies only to the resource itself, and does not renew the resources associated with the resource, such as the public IP resources bound to the load balancer. After the additional associated resources expire, they are automatically unbound from Load Balancing, and to ensure normal service usage, you must renew the relevant resources in a timely manner.

When Load Balancing is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the billing method of Load Balancing is [Hours], the renewal period can be selected from 1 to 24 hours. If the billing method of Load Balancing is Monthly, you can select the renewal period from 1 to 11 months. If the billing method of Load Balancing is Annual, the renewal period of Load Balancing is 1 to 5 years.

## VServer Management
VServer is a load-balanced listener, which mainly carries Layer 4 and Layer 7 listeners of the load-balanced service network, and requests through the IP address of the Load Balancing can only access the protocol and port being listened to, and distribute the request traffic to the backend service nodes according to the forwarding policy defined by the scheduling algorithm.
Users can add, modify, delete, and view listeners, and manage VServer's backend service nodes and Layer 7 content forwarding rules. For each VServer listener, users can configure the listening protocol, port, load balancing algorithm, session persistence, and health check, and if the protocol is HTTP or HTTPS, Layer 7 content forwarding or SSL certificate can be configured and managed. A Load Balancing supports multiple VServer listeners, each of which corresponds to an application load balancing service.
### Adding VServer
Adding VServer refers to adding a listener to a load balancer to listen on the IP address of the load balancer, so that users can access the service load through the IP address of the load balancer. ENABLE USERS TO CREATE LISTENERS IN TCP, UDP, HTTP, AND HTTPS PROTOCOLS ACCORDING TO APPLICATION REQUIREMENTS, SUCH AS ADDING AN HTTP:80 VServer listener to a load balancer to provide highly available web services based on load balancing.
- TCP listener: TCP protocol-based listener, that is, only listen on TCP port, suitable for reliability and high data accuracy requirements, such as file transfer FTP, sending or receiving mail SMTP & POP3, remote login 22/3389, etc.
- UDP listeners: UDP protocol-based listeners that focus on real-time and relatively little reliability, such as DNS applications.
- HTTP listener: A listener based on HTTP protocol and content forwarding policy, suitable for web services and application services.
- HTTPS listener: A listener based on HTTPS and certificate encryption, which is suitable for application services that encrypt transmission.

#### Adding TCP listeners
This topic uses the TCP:23 (Telnet) service as an example to create a TCP:23 (Telnet) service. You can go to the VServer listener creation wizard page in the left navigation pane of VServer on the VServer details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-76.png)

Configure the VServer listener according to the wizard page, including protocol, port, load balancing algorithm, connection idle timeout, and health check:
Listening protocol: The network protocol of the Load Balancing service, which supports TCP, UDP, HTTP, HTTPS, and TCP is selected in this example.

- Port: The application port used to receive requests when the Load Balancing service provides external or inbound services, the port range is 1~65535, this example uses port 23 to provide highly available Telnet services.<br/>
Ports `323, 9102, 9103, 9104, 9105, 60909, 60910`, etc. are occupied and cannot be used under any protocol.
- Load balancing algorithm: The scheduling calculation strategy of load balancing to distribute requests to backend RealServer supports three algorithms: weighted round robin, minimum number of connections, and source address.
- Connection idle timeout: The connection idle timeout limit from the client to the load balancer ranges from 1~86400 seconds. The default value is 60 seconds, that is, within 60 seconds, the client has no access request to the load balancer, and the platform automatically disconnects the connection.
- Health check: Performs robustness checks on backend service nodes based on rules to automatically detect and isolate backend service nodes that are unavailable. - - The TCP protocol only supports port checking, that is, detecting the availability of services through IP:port.

During the creation process, the resource status of VServer is "Creating", and when the status is updated to "Running", the creation is successful, and users can view the added TCP listeners through the VServer list and details, and view the health status of all service nodes under VServer.

#### Adding UDP listeners
This topic uses the UDP:53 (DNS) service to create a UDP:53 (DNS) service as an example. You can go to the VServer listener creation wizard page in the left navigation pane of VServer on the VServer details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-77.png)

Configure the VServer listener according to the wizard page, including protocol, port, load balancing algorithm, session persistence, and health checks:
Listening Protocol: The network protocol of the Load Balancing service, which supports TCP, UDP, HTTP, HTTPS, and UDP is selected in this example.
- Port: The port range is 1~65535, and port 53 is used to provide highly available DNS services.<br/>
Ports 323, 9102, 9103, 9104, 9105, 60909, 60910, etc. are occupied and cannot be used under any protocol.
- Load balancing algorithm: The scheduling calculation strategy of load balancing to distribute requests to backend RealServer supports three algorithms: weighted round robin and source address.
- Session persistence: For the UDP protocol, session persistence is guaranteed based on the IP address, and access requests from the same IP address are forwarded to the same back-end virtual machine for processing, and the session persistence feature of the UDP protocol can be turned on or off.
- Health check: Performs robustness checks on backend service nodes based on rules to automatically detect and isolate backend service nodes that are unavailable. The UDP protocol only supports port checking, that is, detecting the availability of services through the IP:port method.

During the creation process, the resource status of VServer is "Creating", and when the status is updated to "Running", the creation is successful, and users can view the added UDP listeners through the VServer list and details, and view the health status of all service nodes under VServer.

#### Adding HTTP listeners
In this example, you create an HTTP:80 (WEB) service as an example to create an HTTP:80 (WEB) service for a Load Balancing instance. You can go to the VServer listener creation wizard page in the left navigation pane of VServer on the VServer details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-78.png)

Configure the VServer listener according to the wizard page, including protocol, port, load balancing algorithm, session persistence, connection idle timeout, and health check:

- Listening Protocol: The network protocol of the Load Balancing service, which supports TCP, UDP, HTTP, HTTPS, and HTTP is selected in this example.
- Port: The application port used to receive requests when the Load Balancing service provides external or inbound services, the port range is 1~65535, this example uses port 80 to provide highly available web services based on the HTTP protocol.<br/>
Ports 323, 9102, 9103, 9104, 9105, 60909, 60910, etc. are occupied and cannot be used under any protocol.
- Load balancing algorithm: A scheduling calculation strategy for load balancing to distribute requests to backend Realservers, which supports three algorithms: weighted round robin, minimum number of connections, and based on source address.
- Session persistence: For HTTP and HTTPS protocols, it provides cookie insertion for session persistence, and supports automatic generation of KEYS and custom KEYS. Select Auto generate KEY to automatically generate a key for implantation, and enter the KEY value (only numbers, letters, and _ characters) when selecting a custom key.
- Connection idle timeout: The connection idle timeout limit from the client to the load balancer ranges from 1~86400 seconds. The default value is 60 seconds, that is, within 60 seconds, the client has no access request to the load balancer, and the platform automatically disconnects the connection.
- Health check: Performs robustness checks on backend service nodes based on rules to automatically detect and isolate backend service nodes that are unavailable. There are two ways of HTTP protocol inspection and HTTP inspection, in which HTTP inspection supports health checks and filters healthy nodes by URL path and the domain name carried in the request HOST header.
  - HTTP health check path: default is `/` , you can enter a path in Linux format, only use letters, numbers, and `-/.%?#&` these characters, must start with `/`, such as `/data`;
  - HTTP Health Domain Name: The domain name in the HOST field of the verification request during checking, you can enter the standard domain name to verify the domain name carried in the host field of the request.

Domain name role in HTTP health check: Some application servers verify the host field in the request, that is, the host field must exist in the request header. If a domain name is configured in the health check, Load Balancing configures the domain name in the host field and carries the domain name to check the backend service node during the health check. If the application server needs to verify the host field of the request, you need to configure the relevant domain name to ensure that the health check works normally.

During the creation process, the resource status of VServer is "Creating", and when the status is updated to "Running", it means that the creation is successful, and users can view the added HTTP listeners through the VServer list and details, and view the health status of all service nodes under VServer.

#### Adding HTTPS listeners
This topic creates an `HTTP:443` (WEB) service based on SSL certificate encryption to create a listener based on the HTTP protocol and provides a Load Balancing service based on the HTTPS protocol. You can go to the VServer listener creation wizard page in the left navigation pane of VServer on the VServer details page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-79.png)

Configure the VServer listener according to the wizard pages, including protocol, port, SSL resolution mode, server certificate, client certificate, load balancing algorithm, session persistence, connection idle timeout, and health check:

- Listening protocol: The network protocol of the Load Balancing service, which supports TCP, UDP, HTTP, HTTPS, and HTTPS is selected in this example.
- Port: The port range is 1~65535, this example uses port 443 to provide high-security and high-availability web services based on the HTTPS protocol and encrypted authentication using SSL certificates.<br/>
Ports 323, 9102, 9103, 9104, 9105, 60909, 60910, etc. are occupied and cannot be used under any protocol.
- SSL parsing mode: The parsing mode of SSL certificate authentication of the HTTPS protocol supports one-way authentication and two-way authentication, usually selecting one-way authentication.
  - One-way authentication: SSL certificate and authentication are provided by the website server to ensure the data security of the HTTPS website, and any user who visits the website can access the website at any time without having a CA certificate;
  - Two-way authentication: Both the website server and the user need to provide SSL certificates, and only clients that provide CA certificates are allowed to access the website.
- Server certificate: The user proves the identity of the server, and HTTPS checks whether the certificate sent by the server is signed by a center that he trusts.
  - Deployed and configured on a Load Balancing to provide SSL server certificates and authentication for the websites of the Load Balancing backend service nodes.
  - When you create a server certificate, you need to upload the server certificate to the platform in advance, and you can upload it by creating a new server certificate, see SSL certificate management for details.
- Client certificate: The client CA public key certificate is used to verify the issuer of the client certificate, and the certificate provided by the client needs to be verified in HTTPS two-way authentication to successfully establish a connection.
  - The website server verifies the signature of the client certificate with the CA certificate and rejects the connection if the verification does not pass;
  - When you create a client certificate, you need to upload the client certificate to the platform in advance, and you can upload it by creating a new client certificate, see SSL certificate management.
- Load balancing algorithm: A scheduling calculation strategy for load balancing to distribute requests to backend Realservers, which supports three algorithms: weighted round robin, minimum number of connections, and based on source address.
- Session persistence: For HTTP and HTTPS protocols, it provides cookie insertion for session persistence, and supports automatic generation of KEYS and custom KEYS. Select Auto generate KEY to automatically generate a key for implantation, and enter the KEY value (only numbers, letters, and _ characters) when selecting a custom key.
- Connection idle timeout: The connection idle timeout limit from the client to the load balancer ranges from 1~86400 seconds. The default value is 60 seconds, that is, within 60 seconds, the client has no access request to the load balancer, and the platform automatically disconnects the connection.
- Health check: Performs robustness checks on backend service nodes based on rules to automatically detect and isolate backend service nodes that are unavailable. There are two ways of HTTP protocol inspection and HTTP inspection, in which HTTP inspection supports health checks and filters healthy nodes by URL path and the domain name carried in the request HOST header.
  - HTTP health check path: default is / , you can enter a path in Linux format, only use letters, numbers, and -/.%?#& these characters, must start with /, such as /data;
  - HTTP Health Domain Name: The domain name in the HOST field of the verification request during checking, you can enter the standard domain name to verify the domain name carried in the host field of the request.

The resource status of VServer is "Creating" during the creation process, and when the status is updated to "Running", the creation is successful, and users can view the added HTTPS listeners through the VServer list and details, and view the health status of all service nodes under VServer.

After the VServer listener is configured, you need to add a service VM to the listener's service node before it can provide services normally. Listeners of HTTP and HTTPS protocols can configure content forwarding rules according to requirements, and accurately distribute requests according to the domain name and URL of the request.

### Viewing VServer
You can enter the VServer resource console through the Load Balancing details page to view the VServer list information and the health status of the service nodes that already exist in the current Load Balancing instance, and switch VServer to view the basic information and monitoring information of VServer in the overview on the right, and switch to the Service Node and Content Forwarding tab to manage service nodes and content forwarding rules.
#### VServer list
On the VServer list page, you can view the list of VServer resources already owned by the current Load Balancing instance, including the protocol port and status, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-80.png)

- Protocol port: VServer listener protocol and port, which is the entry basis for load balancing to process requests;
- Status: The service status of the VServer listener, including green, yellow, and red;
  - Green: The health status of all service nodes added in VServer is healthy;
  - Yellow: Some service section exceptions added in VServer;
  - Red: The health status of all service nodes added in VServer is abnormal, which means that VServer has stopped working; If no service nodes are added, the default status of VServer is All Exceptions.

On the list page, you can add, modify and delete VServer, click VServer to view the details of the current VServer on the right, and click the status button to display the status description.

#### VServer details
Through the "Protocol Port" of the VServer resource list, you can view the VServer details page on the right side, and you can view the details of the current VServer resource, as shown in the following figure, the details page is divided into basic information, VServer monitoring information, service node management, and content forwarding information:

![1](/assets/images/user-guide/user-guide-81.png)

(1) Basic information

The basic information of VServer, including ID, protocol port, load balancing algorithm, session persistence, session persistence key, connection idle timeout, health check mode, running status, VS status, alarm template, and creation time. If the VServer listener protocol is HTTP/HTTPS, you can view information such as the HTTP health check path, HTTP check domain name, SSL resolution mode, server certificate, and client certificate.

- Session persistence: The switch and type of session persistence. The UDP protocol value is On or Off, and the HTTP/HTTPS protocol value is Off, Automatically generated KEY, or Custom KEY.
- Running Status: The service status of the VServer listener, including all exceptions, partial exceptions, and all normal.
- VS Status: The status of the VServer listener resource, including Available, Updating, Deleted.
- Alarm template: A monitoring alarm template bound to VServer, or displayed as None if not bound.
- Server Certificate/Client Certificate: The name of the HTTPS listener SSL certificate, which can be queried by viewing the contents of the certificate.
(2) Monitoring information

VServer instance monitoring charts and information, including the number of new connections, HTTP 2XX, HTTP 3XX, HTTP 4XX, HTTP 5XX, and can view monitoring data for 1 hour, 6 hours, 12 hours, 1 day, and custom times.

(3) Service nodes and content forwarding

- Service nodes: VServer's service node lifecycle management, including adding, viewing, modifying, enabling, disabling, and deleting service nodes, see Service node management.
- Content forwarding rules: Manage the lifecycle of content forwarding rules configured by VServer, including adding, viewing, modifying, and deleting forwarding rules.

### Modifying VServer
You can modify the VServer listener configuration through the console, such as modifying the listener's load balancing algorithm, session persistence, connection idle timeout, and health check configuration information, and if the protocol is HTTPS, the SSL resolution mode and SSL certificate of the replaceable listener. Modifications can be made through the Modify button on the VServer list, as shown by the Modify Wizard:

![1](/assets/images/user-guide/user-guide-82.png)

Modifying the parameter settings of the configuration is the same as when creating VServer, and modifying the protocol and port of VServer is not supported. During the modification process, the VS status changes from "running" to "Update", and after the update is successful, the flow changes to "Running", which means that the update is successful, and you can view the newly modified configuration through the details page. After the modification is successful, the platform immediately re-checks the service node according to the new configuration, and distributes requests according to the newly modified scheduling algorithm.

Modify VServer's scheduling algorithm, session persistence, and connection idle timeout, which take effect only for new connections and do not affect services that have already established connections.

### Modify the alarm template
Modify the alarm template to configure the VServer monitoring data to alarm, through the indicators and thresholds defined by the alarm template, you can trigger alarms when VServer-related indicators fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure load balancing and service network communication.

You can modify the alarm template through the action items on the VServer Details Overview page, and select a new alarm template on the Modify Wizard page to modify it. VServer and Load Balancer can share a single load balancing monitoring alarm template, that is, you can define the metric alarm policy of LB instances in the load balancing alarm template, and you can define VServer monitoring indicator alarms, and customize the rules of the alarm template according to your needs.

### Removing VServer
Users can delete VServer resources through the console or API, and the content forwarding rule policy created under VServer will be automatically cleared when deleted, and the associated SSL certificate will be automatically unbound, and the deletion operation can be performed only when there is no back-end RealServer resource in VServer.

![1](/assets/images/user-guide/user-guide-83.png)

VServer cannot be recovered after deletion, you need to check and confirm whether it is necessary to delete VServer resources when deleting, the VServer tab in the console can view the deletion process, and when the deleted VServer resources are emptied, the deletion is successful.

## Service Node Management
Service nodes refer to the back-end real servers in the load balancing architecture, that is, RealServer, which are used to provide real services and process service requests, which are generally composed of multiple virtual machine clusters.
- Adding a service node requires that the VServer listener be created before it can be added.
- After a service node is added, Load Balancing checks whether the service node is normal by health checking.
- If the service node cannot handle the request sent by VServer normally, the platform prompts the service node that the status is invalid and needs to detect the service status deployed in the service node.
- If the service node can process Check requests normally, that is, the status of the service node is valid, the load balancer can work normally.

### Adding Service Nodes
Before you add a service node, you must ensure that the services on the service node are running normally and can be accessed normally. You can enter the "Service Node" resource console through the VServer details page and click "Add Service Node" to add the backend RealServer. When you add a service node, you can select only virtual machines that are in the same data center and have the same VPC network as the load-balanced instance. As shown in the following figure:

![1](/assets/images/user-guide/user-guide-84.png)

- Virtual Machine: A virtual machine that needs to be added to the current VServer service node of Load Balancing, and you can specify the port and weight exposed by the service node.
- Port: The service port exposed by the backend service node, such as VServer listening on port 80 and the service node listening on port 8080, enter 8080 at the port, and Load Balancing will distribute requests arriving at port 80 of VServer to port 8080 of the service node.
- Weight: The weight of the backend service node, the range is 1~100. A higher number indicates a higher weight, and Load Balancing preferentially distributes requests to service nodes with higher weights, with a default value of 1.

You can add multiple ports of the same virtual machine to VServer's service nodes, that is, requests from VServer listener ports are forwarded to multiple ports of the same service node to meet the load distribution requirements of different application scenarios.

After you add a service node, you can view the process of adding a service node on the Service Node Resource List page, and if the status of the service node is Valid or Invalid, the service node is successfully added. If the status of the service node is invalid, you need to check the running status of the service in the service node, and the prerequisite for the service status is that the service can be accessed normally through the network address and health check of the virtual machine.

If you add a virtual machine to a Load Balancing backend that provides the Internet, you can directly access the service node through the Internet without configuring the loopback service address on the backend service node.

### Viewing Service Nodes
On the VServer details page, you can view the list of service node resources added by the VServer listener backend, including service node ID, resource ID, private IP address, port, weight, node mode, node status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-85.png)

- Service node: The global RS unique identifier of the current service node.
- Resource ID: The name and ID of the virtual machine to which the current service node is bound.
- IP/Port: The private IP address of the current service node and the configured service port.
- Weight: The forwarding weight configured by the current service node.
- Node Mode: The enabled and disabled mode of the current service node.
- Status: The service load status of the current service node, including active and invalid.
  - Valid: means that the business services in the current service node operate normally and can be accessed through the network, that is, the service node is healthy;
  - Invalid: It means that the business services in the current service node are not running normally or cannot be accessed through the network, which means that the service node is not unhealthy.

The operation items on the list refer to the operations on a single service node, including enable, disable, delete, and modify, and support batch enable, batch disable, and batch delete operations of service nodes.

### Enable/Disable
Users enable and disable service nodes added to VServer for load balancing, and support batch enablement and disablement.

- Disable: Disable the service node, after which Load Balancing stops distributing requests to the service node and stops its health checks.
- Enabled: Enable the service node, after which Load Balancing will check its health, and if the health check is passed, new requests will be distributed to the service node according to the scheduling algorithm.
- Disable operations can only be performed when the node mode is enabled;
- Enable operation is available only if the node mode is disabled.

### Modifying Service Nodes
You can modify the service port and weight of a VServer service node, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-86.png)

Modifying the port and weight does not affect established business connections and takes effect only for new distribution requests for Load Balancing. After clicking OK, you will return to the service node list page, the node status will change from "valid" or "invalid" to "update", and after the modification is successful, it will be transferred back to "valid" or "invalid", which means that the health check is successful and the service node can provide services normally.

### Deleting Service Nodes
If you need to change the service of a service node or disconnect a service node from the backend of Load Balancing, you can delete the service node function to perform offline operations, which will not affect the operation and use of the virtual machine itself. You can delete a service node by deleting it from the service node list action item, and then add it back to the Load Balancing instance.

![1](/assets/images/user-guide/user-guide-87.png)

If the listening protocol of VServer is HTTP/HTTPS and content forwarding rules are configured, the content forwarding rules are automatically unbound when the service node is deleted.

## SSL Certificate Management
Load Balancing supports HTTPS load forwarding and SSL certificate loading to ensure that user services are protected by encryption and authenticated by authoritative authorities. For server certificates and client certificates of HTTPS protocol, the platform provides unified certificate management services, including certificate uploading, binding, and deletion.

The certificate does not need to be uploaded to the service node, and the decryption process is performed on load balancing, which reduces the CPU overhead of the backend server, that is, the listener of the HTTPS protocol only implements HTTPS requests and SSL encryption and decryption from the client to the load balancer, and the load balancer to the backend service node still uses the HTTP protocol to forward requests.

Before uploading and creating a certificate, you need to confirm the type of certificate to be uploaded, including server certificate and client certificate, and upload or enter the certificate content to the platform according to the certificate format requirements.

- Server certificate: The user proves the identity of the server, and HTTPS checks whether the certificate sent by the server is signed by a center that he trusts. Deployed and configured on a Load Balancing to provide SSL server certificates and authentication for the websites of the Load Balancing backend service nodes. Both one-way authentication and two-way authentication require uploading the server certificate and private key content.
- Client certificate: The client CA public key certificate is used to verify the issuer of the client certificate, and the certificate provided by the client needs to be verified in HTTPS two-way authentication to successfully establish a connection. The website server verifies the signature of the client certificate with the CA certificate and rejects the connection if it does not pass validation. You only need to upload a client certificate and bind to a VServer listener for mutual authentication.

If a certificate needs to be used in multiple data centers at the same time, you need to create and upload certificates in multiple data centers at the same time.

### Certificate Format Requirements
When the SSL certificate is associated with a VServer listener, the platform will automatically read the certificate content in the file and load it into the VServer listener, so that the user's HTTPS application can be encrypted and decrypted through the SSL certificate.

The certificate file format supports `PEM` or `CRT` in the Linux environment, and does not support certificates in other formats, so you need to convert the certificate format before uploading. You can also create a certificate by directly entering the certificate content, and before uploading the certificate or entering the certificate content, you need to ensure that the certificate, certificate chain, and private key content meet the format requirements of the certificate.

#### Certificate issued by the root CA authority
If the certificate is unique from the root CA authority, the configured site can be considered trusted by accessing devices such as browsers without additional certificates. The certificate content format requirements are as follows:
- Begin with -----BEGIN CERTIFICATE----- and end with -----END CERTIFICATE-----.
- Each line is 64 characters, and the last line can be less than 64 characters long.
- The certificate content cannot contain spaces.

The format specification of the certificate issued by the root CA authority is as follows, you can refer to the following text content and certificate examples:
```
-----BEGIN CERTIFICATE-----
User certificate (BASE64 encoded)
-----END CERTIFICATE-----
-----BEGIN  CERTIFICATE-----
MIIFnDCCBISgAwIBAgIQD1pxAmxfzY+R4k4Ua1cVWDANBgkqhkiG9w0BAQsFADBy
MQswCQYDVQQGEwJDTjElMCMGA1UEChMcVHJ1c3RBc2lhIFRlY2hub2xvZ2llcywg
SW5jLjEdMBsGA1UECxMURG9tYWluIFZhbGlkYXRlZCBTU0wxHTAbBgNVBAMTFFRy
dXN0QXNpYSBUTFMgUlNBIENBMB4XDTE5MDQyMzAwMDAwMFoXDTIwMDQyMjEyMDAw
MFowHDEaMBgGA1UEAwwRKi51Y2xvdWRzdGFjay5jb20wggEiMA0GCSqGSIb3DQEB
AQUAA4IBDwAwggEKAoIBAQC2SDT5pJEQhRhQQ98vzuAvK1zUFMD1p1E3YyJGDISY
SINH38QTtVqWbZgmVkU6v2R1GrBz0iMfevO0/sjxefwHmiGYd1ytG9dm8D3fVZox
piST9hoIyjOFRstBLGXuxWSa2LdjVSePaFfxaN3UZLYY6MIHkdqxVZLhM4ANSLNr
PI6cRUZVBU29V3A2znkVEbx5dwKA3SGFVWfqjfzXqC+NTylLKb7H304BxspZlKDi
n+/aV/vSovVM7zg57AOtxjkSNzBDjdz+Ud3wqaT1O4vEG4tqqAnsIyJeaMueFti0
cjiMwLVsFsmV1eVSBiYWGO8U/YRFv+dNg4XG2MqYUFsRAgMBAAGjggKCMIICfjAf
BgNVHSMEGDAWgBR/05nzoEcOMQBWViKOt8ye3coBijAdBgNVHQ4EFgQUJYEWLiyn
YgqKaGaT8thKWAnuWfEwLQYDVR0RBCYwJIIRKi51Y2xvdWRzdGFjay5jb22CD3Vj
bG91ZHN0YWNrLmNvbTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUH
AwEGCCsGAQUFBwMCMEwGA1UdIARFMEMwNwYJYIZIAYb9bAECMCowKAYIKwYBBQUH
AgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwCAYGZ4EMAQIBMH0GCCsG
AQUFBwEBBHEwbzAhBggrBgEFBQcwAYYVaHR0cDovL29jc3AuZGNvY3NwLmNuMEoG
CCsGAQUFBzAChj5odHRwOi8vY2FjZXJ0cy5kaWdpdGFsY2VydHZhbGlkYXRpb24u
Y29tL1RydXN0QXNpYVRMU1JTQUNBLmNydDAJBgNVHRMEAjAAMIIBBAYKKwYBBAHW
eQIEAgSB9QSB8gDwAHYA7ku9t3XOYLrhQmkfq+GeZqMPfl+wctiDAMR7iXqo/csA
AAFqSaRkyQAABAMARzBFAiEAuovTHM3SEWQRyktGXvtm1hLHd7gxhPNzdzrzkJFX
rWMCIAPideB1BqUSUcpRME6NxIXJD7066ldWuSqgPkOiPtwLAHYAh3W/51l8+IxD
mV+9827/Vo1HVjb/SrVgwbTq/16ggw8AAAFqSaRl4wAABAMARzBFAiBiIpO59m6U
bmlmuQ8cL7WzoDkHiyE+UloEKZXiDpqCfQIhAPIKRdaJfh/5IZHFq31oJVd/TZ3g
pTQ6RpHe0BseSSefMA0GCSqGSIb3DQEBCwUAA4IBAQARaNWOJbAI7Rv6QPChPeWL
Mqryk+tOlterdxYZay6tr3Ea8VOqSS7YdVtvdkR1/k4k87H5AwCQT60/yu4N5J7M
Vkzmqo3tVQTtzVFo0SavgARY12XuU0jhG3LGFI0a43CgfMYMcZ0DiylhYUM48GWz
/axza5uangnIQxBwv+4KXGUfplJujv8WfBepeh+tqPgS8qCqn6e0+sdkUN7yHcA/
O24DiQajtMXG5nR6qHdZhRLCFRXRghYdvVKrkOVFogYqwa4dViyuP/6EFDkuMwDs
7XrxJIjL8qp9Lrw2sHN1F+USKhlPRaNBtWzDELf54zVgAIAeFUriqtER8ZWBWgp4
-----END CERTIFICATE-----
```

#### Certificates issued by intermediate authorities
If the certificate is issued by an intermediate CA authority, the certificate file obtained contains multiple certificates, and it is necessary to manually merge the server certificate and the intermediate certificate to fill in or upload it, commonly known as the certificate chain.

The splicing rules of the certificate chain are: put the first copy of the user certificate, put the second copy of the intermediate certificate, and there should be no blank line in the middle; Each line is 64 characters and the certificate content cannot contain spaces, and the last line can be less than 64 characters long, the format specification and certificate example are as follows:

```
-----BEGIN CERTIFICATE-----
User certificate (BASE64 encoded)
-----END CERTIFICATE-----
!!! There must be no empty rows in between!!!
-----BEGIN CERTIFICATE-----
User certificate (BASE64 encoded)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIFnDCCBISgAwIBAgIQD1pxAmxfzY+R4k4Ua1cVWDANBgkqhkiG9w0BAQsFADBy
MQswCQYDVQQGEwJDTjElMCMGA1UEChMcVHJ1c3RBc2lhIFRlY2hub2xvZ2llcywg
SW5jLjEdMBsGA1UECxMURG9tYWluIFZhbGlkYXRlZCBTU0wxHTAbBgNVBAMTFFRy
dXN0QXNpYSBUTFMgUlNBIENBMB4XDTE5MDQyMzAwMDAwMFoXDTIwMDQyMjEyMDAw
MFowHDEaMBgGA1UEAwwRKi51Y2xvdWRzdGFjay5jb20wggEiMA0GCSqGSIb3DQEB
AQUAA4IBDwAwggEKAoIBAQC2SDT5pJEQhRhQQ98vzuAvK1zUFMD1p1E3YyJGDISY
SINH38QTtVqWbZgmVkU6v2R1GrBz0iMfevO0/sjxefwHmiGYd1ytG9dm8D3fVZox
piST9hoIyjOFRstBLGXuxWSa2LdjVSePaFfxaN3UZLYY6MIHkdqxVZLhM4ANSLNr
PI6cRUZVBU29V3A2znkVEbx5dwKA3SGFVWfqjfzXqC+NTylLKb7H304BxspZlKDi
n+/aV/vSovVM7zg57AOtxjkSNzBDjdz+Ud3wqaT1O4vEG4tqqAnsIyJeaMueFti0
cjiMwLVsFsmV1eVSBiYWGO8U/YRFv+dNg4XG2MqYUFsRAgMBAAGjggKCMIICfjAf
BgNVHSMEGDAWgBR/05nzoEcOMQBWViKOt8ye3coBijAdBgNVHQ4EFgQUJYEWLiyn
YgqKaGaT8thKWAnuWfEwLQYDVR0RBCYwJIIRKi51Y2xvdWRzdGFjay5jb22CD3Vj
bG91ZHN0YWNrLmNvbTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUH
AwEGCCsGAQUFBwMCMEwGA1UdIARFMEMwNwYJYIZIAYb9bAECMCowKAYIKwYBBQUH
AgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwCAYGZ4EMAQIBMH0GCCsG
AQUFBwEBBHEwbzAhBggrBgEFBQcwAYYVaHR0cDovL29jc3AuZGNvY3NwLmNuMEoG
CCsGAQUFBzAChj5odHRwOi8vY2FjZXJ0cy5kaWdpdGFsY2VydHZhbGlkYXRpb24u
Y29tL1RydXN0QXNpYVRMU1JTQUNBLmNydDAJBgNVHRMEAjAAMIIBBAYKKwYBBAHW
eQIEAgSB9QSB8gDwAHYA7ku9t3XOYLrhQmkfq+GeZqMPfl+wctiDAMR7iXqo/csA
AAFqSaRkyQAABAMARzBFAiEAuovTHM3SEWQRyktGXvtm1hLHd7gxhPNzdzrzkJFX
rWMCIAPideB1BqUSUcpRME6NxIXJD7066ldWuSqgPkOiPtwLAHYAh3W/51l8+IxD
mV+9827/Vo1HVjb/SrVgwbTq/16ggw8AAAFqSaRl4wAABAMARzBFAiBiIpO59m6U
bmlmuQ8cL7WzoDkHiyE+UloEKZXiDpqCfQIhAPIKRdaJfh/5IZHFq31oJVd/TZ3g
pTQ6RpHe0BseSSefMA0GCSqGSIb3DQEBCwUAA4IBAQARaNWOJbAI7Rv6QPChPeWL
Mqryk+tOlterdxYZay6tr3Ea8VOqSS7YdVtvdkR1/k4k87H5AwCQT60/yu4N5J7M
Vkzmqo3tVQTtzVFo0SavgARY12XuU0jhG3LGFI0a43CgfMYMcZ0DiylhYUM48GWz
/axza5uangnIQxBwv+4KXGUfplJujv8WfBepeh+tqPgS8qCqn6e0+sdkUN7yHcA/
O24DiQajtMXG5nR6qHdZhRLCFRXRghYdvVKrkOVFogYqwa4dViyuP/6EFDkuMwDs
7XrxJIjL8qp9Lrw2sHN1F+USKhlPRaNBtWzDELf54zVgAIAeFUriqtER8ZWBWgp4
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIErjCCA5agAwIBAgIQBYAmfwbylVM0jhwYWl7uLjANBgkqhkiG9w0BAQsFADBh
MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
QTAeFw0xNzEyMDgxMjI4MjZaFw0yNzEyMDgxMjI4MjZaMHIxCzAJBgNVBAYTAkNO
MSUwIwYDVQQKExxUcnVzdEFzaWEgVGVjaG5vbG9naWVzLCBJbmMuMR0wGwYDVQQL
ExREb21haW4gVmFsaWRhdGVkIFNTTDEdMBsGA1UEAxMUVHJ1c3RBc2lhIFRMUyBS
U0EgQ0EwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCgWa9X+ph+wAm8
Yh1Fk1MjKbQ5QwBOOKVaZR/OfCh+F6f93u7vZHGcUU/lvVGgUQnbzJhR1UV2epJa
e+m7cxnXIKdD0/VS9btAgwJszGFvwoqXeaCqFoP71wPmXjjUwLT70+qvX4hdyYfO
JcjeTz5QKtg8zQwxaK9x4JT9CoOmoVdVhEBAiD3DwR5fFgOHDwwGxdJWVBvktnoA
zjdTLXDdbSVC5jZ0u8oq9BiTDv7jAlsB5F8aZgvSZDOQeFrwaOTbKWSEInEhnchK
ZTD1dz6aBlk1xGEI5PZWAnVAba/ofH33ktymaTDsE6xRDnW97pDkimCRak6CEbfe
3dXw6OV5AgMBAAGjggFPMIIBSzAdBgNVHQ4EFgQUf9OZ86BHDjEAVlYijrfMnt3K
AYowHwYDVR0jBBgwFoAUA95QNVbRTLtm8KPiGxvDl7I90VUwDgYDVR0PAQH/BAQD
AgGGMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjASBgNVHRMBAf8ECDAG
AQH/AgEAMDQGCCsGAQUFBwEBBCgwJjAkBggrBgEFBQcwAYYYaHR0cDovL29jc3Au
ZGlnaWNlcnQuY29tMEIGA1UdHwQ7MDkwN6A1oDOGMWh0dHA6Ly9jcmwzLmRpZ2lj
ZXJ0LmNvbS9EaWdpQ2VydEdsb2JhbFJvb3RDQS5jcmwwTAYDVR0gBEUwQzA3Bglg
hkgBhv1sAQIwKjAoBggrBgEFBQcCARYcaHR0cHM6Ly93d3cuZGlnaWNlcnQuY29t
L0NQUzAIBgZngQwBAgEwDQYJKoZIhvcNAQELBQADggEBAK3dVOj5dlv4MzK2i233
lDYvyJ3slFY2X2HKTYGte8nbK6i5/fsDImMYihAkp6VaNY/en8WZ5qcrQPVLuJrJ
DSXT04NnMeZOQDUoj/NHAmdfCBB/h1bZ5OGK6Sf1h5Yx/5wR4f3TUoPgGlnU7EuP
ISLNdMRiDrXntcImDAiRvkh5GJuH4YCVE6XEntqaNIgGkRwxKSgnU3Id3iuFbW9F
UQ9Qqtb1GX91AJ7i4153TikGgYCdwYkBURD8gSVe8OAco6IfZOYt/TEwii1Ivi1C
qnuUlWpsF1LdQNIdfbW3TSe0BhQa7ifbVIfvPWHYOu3rkg1ZeMo6XRU9B4n5VyJY
RmE=
-----END CERTIFICATE-----
```
#### RSA Private Key
When you upload a server certificate, you need to upload the private key content of the certificate at the same time.
- Begin with -----BEGIN RSA PRIVATE KEY----- and end with -----END RSA PRIVATE KEY-----.
- Each line is 64 characters, and the last line can be less than 64 characters long.
- The certificate content cannot contain spaces.

The format specification of the RSA private key content of the certificate is as follows, and you can refer to the following text content and certificate examples:
```
-----BEGIN RSA PRIVATE KEY-----
User certificate (BASE64 encoded)
-----END RSA PRIVATE KEY-----
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAtkg0+aSREIUYUEPfL87gLytc1BTA9adRN2MiRgyEmEiDR9/E
E7Valm2YJlZFOr9kdRqwc9IjH3rztP7I8Xn8B5ohmHdcrRvXZvA931WaMaYkk/Ya
CMozhUbLQSxl7sVkmti3Y1Unj2hX8Wjd1GS2GOjCB5HasVWS4TOADUizazyOnEVG
VQVNvVdwNs55FRG8eXcCgN0hhVVn6o3816gvjU8pSym+x99OAcbKWZSg4p/v2lf7
0qL1TO84OewDrcY5EjcwQ43c/lHd8Kmk9TuLxBuLaqgJ7CMiXmjLnhbYtHI4jMC1
bBbJldXlUgYmFhjvFP2ERb/nTYOFxtjKmFBbEQIDAQABAoIBAAbLqdftu+/A+n9R
jHJIlOCFThRlAq2V04gMVNSGNnJD78r/61wtvGcTxmKVgEa4qGraOB5VRPRxPcEv
Z3fjK5Nv+lUoDAczHMRsa+4Vz6YOstnmSKGvwhxzn3O6T0GHz+Ca+DlGjS9CPVcV
aQG4UHacxNEJ7byDO3LUW++C2JeEjg2LVLJt+jRwFIAwmk8XjM/jyOn5kCj2kvz6
w/yHbmwAac2mfA42CQN78o1bvEHlHH1cVDRHZ482pNlfp8WBGgCFWHBGeMpLOq9R
YXEt+SyJ84o80mTeR2DgswpW577uZLGfPyFUwKPc/XGZSzbfKX5L9ctGQrr4lsQ+
F2stOk0CgYEA5JyQ5S7qPf8YcczQfXblVueslDL56Zt1/KUdUQ4L9i2as+5LkiN+
7gVK8INhvYi8dRAqZxqXLqU5dXEWbkcnFbenjoQyPCfYI5lZE2+cZriLtO3I0YKP
nc9NwSE+gRR9kXbgGSiANJmGC5TxOU99hR7Nx6wUMAflatCaSmP5RbUCgYEAzB67
MOWzeVD8Eq/DhEE5N0o3hpyhiMy++A/LC8lAjFAi6ldc70zMYMUjfh+eTNK89Z6H
1z3xBaQMMlHAy8pU5uI9LOgm/xWaPNZ034Xx4fWftBnEGFtBgLfbNphoUz3Df5vK
XvXhjdKEkwevcLoZWHfNZIJnennlEV26Tk1DGW0CgYBYCKaPasqPRy2NnRZoSiG0
npBJnXu5ZsE/ogGxFdyrVxJs2YXGZ97YH7ek+KLpzr7rwWbiv02ai8udmwfNPZ8i
cM+YRPXnTlygEMxJfMBYmhZKfQrJCyLs3UiO55NfN5nHK2TOq1b7amdBDID71c17
Nsp9apl3iYLh6CSSIv95xQKBgCHmKKhiPYA0VuizkADy5BGunbIZaSpS9pQz60C1
16Z12Jaak7CaTIb1toNHtP6FMSSJg33Xp6OMLwpcUWyG2brOb+J5W6CZcdgQtbA5
ioZASJmcfdidry81WY6jmQ/Z/hG/ScijhSYMhD/20sgh3/u1ScMbdRv+CnDr4/kF
E9OxAoGBAK0mCJyyKNkWSdOg7fbBwYJKQ1BBZ6lh1gVpc7recSreNPRFWTd6l+cw
eCxXqbSJJw4oYYF4IoBX1fCfFd82engRkwmkDykGMwpomnJoZqjFhmVtDb81xRQL
pscHorV4flpOcSwg6b3jq0N6+PN85XI9XIFImXXHJqqKSFBBSPrf
-----END RSA PRIVATE KEY-----
```
If the rsa private key content is encrypted, such as the private key starting with '-----BEGIN PRIVATE KEY----- or -----BEGIN ENCRYPTED PRIVATE KEY-----, and ending with -----END PRIVATE KEY----- or -----END ENCRYPTED PRIVATE KEY-----', the contents of the certificate private key need to be converted. On a Linux system, the operation is as follows:

```
openssl rsa -in old_server_key.pem -out new_server_key.pemCopyErrorSuccess
```
#### Client Certificates
The client certificate format requirements are the same as those issued by the root CA authority, starting with -----BEGIN CERTIFICATE----- and ending with -----END CERTIFICATE-----, 64 characters per line, and the last line can be less than 64 characters. The following specifications are shown:
```
-----BEGIN CERTIFICATE-----
Client CA certificate (BASE64 encoded)
-----END CERTIFICATE-----
```
### Creating an SSL Certificate
The SSL certificate uploaded by the user is used to deploy the load balancing service of the HTTPS protocol, and you can upload server certificates and client certificates. When uploading the certificate, you need to check the format and validity of the SSL certificate, and if the content of the SSL certificate does not meet the format specifications, the SSL certificate cannot be successfully generated.
#### Creating a Server Certificate
You can switch to the SSL Certificates tab through the Load Balancing console, enter the SSL Certificate Management console, create an SSL certificate to enter the Upload Certificate wizard page, and select a server certificate to create a server certificate.

(1) The method of uploading the certificate file locally is created as shown in the following figure:

![1](/assets/images/user-guide/user-guide-88.png)

When uploading locally, you need to upload the user certificate file and the certificate private key file, where the user certificate file only supports CRT and PEM format files, and the certificate private key only supports uploading files in .key format.
- User certificate: The content of the user's authorization certificate, including information such as public key and signature, supports certificate chains, and is generally a file in .crt and .pem format.
- Certificate private key: Encrypts the private key content of the certificate, typically a file in .key format.

Click Upload Certificate to read the locally generated certificate file to the platform and confirm the certificate creation, during which the platform will detect the legitimacy of the certificate content.

(2) Manually enter the certificate content to create the following figure:

![1](/assets/images/user-guide/user-guide-89.png)

Manual input of the certificate also needs to enter the text content of the user certificate and the certificate private key, you need to refer to the format specification in the text box to enter the certificate content and private key content, confirm the certificate creation, during the creation process, the platform will detect the legitimacy of the certificate content.
#### Creating a Client Certificate
You can switch to the SSL Certificates tab through the Load Balancing console, enter the SSL Certificate Management console, create an SSL certificate to enter the Upload Certificate Wizard page, and select a client certificate to create a client certificate.

(1) The method of uploading the certificate file locally is created as shown in the following figure:

![1](/assets/images/user-guide/user-guide-90.png)

When uploading locally, you need to upload the client CA public key certificate file, and only CRT and PEM format files are supported. Click Upload Certificate to read the locally generated certificate file to the platform and confirm the certificate creation, during which the platform will detect the legitimacy of the certificate content.

(2) Manually enter the certificate content to create the following figure:

![1](/assets/images/user-guide/user-guide-91.png)

Manually entering the certificate also needs to enter the text content of the CA client certificate, you need to refer to the format specification in the text box to enter the certificate content and private key content, confirm the certificate creation, during the creation process, the platform will detect the legitimacy of the certificate content.