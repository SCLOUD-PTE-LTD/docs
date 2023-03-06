---
layout: default
title: ULB
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/ulb/
nav_order: 2
---
# ULB - SCloud Load Balancer
## Introduction to ULB
ULB (SCloud Load Balancer) is a load balancing service provided by SCloud, which can provide traffic distribution functions based on network packets or proxy for multiple hosts or other service instances. 

In a high-concurrency service environment, ULB is used to build a service cluster composed of multiple service nodes. Service clusters can scale the processing and fault tolerance of services, and automatically eliminate the impact of a single service node failure on the overall service, improving service availability.

ULB supports HTTP, HTTPS protocols (Nginx-like or HAproxy-like) for Layer 7; Layer 4 protocols support TCP protocol and UDP protocol (LVS-like).

## ULB composition
ULB services are mainly composed of the following three parts:
* ULB Service Instance (SCloud LoadBalancer): Used to receive traffic and distribute traffic.
* Virtual Server/Listener (VServer): A listener, each VServer is a set of load-balanced front-end port configurations.
* Service node (RealServer/Backend): The cloud resource where the backend actually processes the request.

![4f99935ea27846559c12661ac55d34f1](https://user-images.githubusercontent.com/124770063/223073275-501c65f3-7ae3-435e-865f-147ce1a65c62.png)

## Overview of features

| Product function | Extranet ULB | Intranet ULB | Description |
| --- | --- | --- | --- |
| Layer 4 forwarding(TCP/UDP). | ✓ | ✓ |
| Layer 7 forwarding(HTTP/HTTPS). | ✓ | ✓ |
| Load balancing algorithm | Polling, source address hash,weighted polling, minimal connection, primary/standby | Polling, source address hash, consistent hash, minimal connection, primary/standby |
| Health checks | ✓ | ✓ | According to the rules, the back-end service node health-check is carried out, and the abnormal service node is automatically isolated, once the problem is found, Quickly switch issues to ensure service availability. |
| Will keep talking | ✓ | ✓ | Support will beheld, and users can forward their requests to the same backend service node. |
| Cross-zone disaster recovery | ✓ | ✓ | You can tie backend service nodes in different zones to achieve cross-zone disaster recovery |
| Extranet fireproof wall | ✓ | — | Only the proxy type support is requested and only effective for Layer7 traffic |
| Domain name forwarding | ✓ | ✓ | Supports forwarding traffic to different backend nodes by access domain name andURL |
| Certificate management | ✓ | ✓ | Supports HTTPS certificate management |
| SSL Offloading | ✓ | ✓ | Supports HTTPS SSL Offloading |
| WebSocket | ✓ | ✓ | Supported only when monitoring the TCPprotocol |
| IPv6 address support | ✓ | — | Supports forwardingIPv6traffic |
| Mount the hybrid cloud node | ✓ | ✓ | Proxy type support is requested only |
| ULB logs | ✓ | ✓ | Proxy type support is requested only |
| HTTP/2 | Ticket support | Ticket support | HTTP/2 is supported |
| Redirect | — | — | Currently, HTTP access redirects toHTTPSare not supported |
| Two-way authentication | — | — | Currently, HTTPS mutual authentication is not supported |

## Technical architecture

(SCloud Load Balancer) provides traffic distribution capabilities to ensure service scalability and high availability. It supports two scenarios: intranet and external network, and supports two forwarding modes: request proxy and packet forwarding. 

The following will introduce the request agent (hereinafter referred to as ULB7) and the newspaper respectively The basic architecture of the text forwarding mode (hereinafter referred to as ULB4). 

### Intranet ULB4
The intranet ULB4 is self-developed based on `DPDK` technology. A single server can provide more than 30 million parallel connections, 10 million pps, 10G line-speed forwarding capability. Cluster deployment allows at least four servers in a single cluster. `ECMP + BGP` is used for high availability. 
The intranet ULB4 adopts a forwarding mode similar to DR. The schematic diagram of intranet load balancing forwarding is as follows:
 
As shown in the figure above, the ULB4 cluster announces the same VIP (fictitious IP) through the access switcher connected to it, the access switch is equipped with `ECMP` algorithm, which can load balance traffic to multiple ULB servers, thus constituting ULB4 cluster. When some servers in the ULB4 cluster have forwarding exceptions, BGP reports them Text forwarding will also stop forwarding, and the ULB4 server will be removed from the server cluster within three seconds, thus guaranteed at the same time, the ULB4 cluster health check module will also issue an alarm to make the engineer Intervention. 

In addition, the servers of the same ULB4 cluster are distributed across zones, ensuring that the ULB4 cluster is highly available across zones. There is a module in ULB4 that is responsible for the health check of back-end nodes (currently only supported TCP/UDP port probe), and reported to ULB4Manager and ULB4 forwarding server. 

After the ULB4 forwarding server receives the service packet of the client, it will be in the state from the state Select one of the normal back-end nodes, modify the destination MAC and then tunnel to the back-end nodes, pass The source IP address and destination IP address remain unchanged. 

In ULB4 mode, the back-end node must be tied to ULB4 at the LO port The VIP (fictitious IP) address, and monitor the service, in order to be correct Process the packet and send the return packet directly to the client. This is a typical DR process, so the intranet ULB4 can directly see the source IP of the client.
### Extranet ULB4

The external network ULB4 is similar to the internal network ULB4, and it is also self-developed based on `DPDK` technology. A single server can provide more than 30 million parallel connections and 10 million pps , 10G line-speed forwarding capability. Cluster deployment allows at least four servers in a single cluster. `ECMP+ BGP` is used for high availability. Similarly, it uses a forwarding pattern similar to DR. The schematic diagram of load balancing forwarding on the Internet is as follows:
 
Unlike the intranet ULB4, external network traffic comes in from the public network. The client's access to ULB4 traffic enters the SCloud POP point and enters the `UVER` ( SCloud Virtual Edge Router). 

`UVER` is SCloud's self-developed public network traffic measurement center, which can be obtained from the service library Know all the next hop information of the EIP, drain the traffic through BGP, and tunnel the EIP traffic to the corresponding next hop. A ULB4 EIP falls to all servers in the ULB4 cluster, so `UVER` will do this Some traffic is sent to each server in the ULB4 cluster according to the consistent hash algorithm. The subsequent process is similar to the intranet ULB4. In the Backend node, the ULB's EIP needs to be tied to the LO port and monitored Listen to the service, and the return message will be sent directly to `UVER` and returned to the client over the Internet. 

In the external network ULB4, the cluster health check module will periodically detect the survival status of the server, if If a problem is found with the server, `UVER` will be notified to remove the abnormal server, so as to ensure it Certificate high availability. Similarly, ULB4 clusters on the Internet are highly available across zones. 
### Intranet ULB7
ULB7 is based on Haproxy, and a single example can support more than 40w pps and 2Gbps , and at least 400,000 parallel connections. The architecture is as follows:

![1](https://user-images.githubusercontent.com/124770063/223077834-4f007ccb-2adb-4594-9680-802fc1d4f8f7.png)

The intranet ULB7 adopts cluster deployment, with at least four servers in a single cluster. The tenants share the server on the bottom layer, but use Docker for resource isolation and CPU of isolation. Unlike the DR mode used by ULB4, ULB7 uses Proxy mode (i.e. Full NAT mode). 

After receiving the request from the client, the intranet ULB7 connects the client to the ULB7 IP Transfer, converted into ULB7 proxy IP to Backend Connection of the actual IP address. Therefore, the Backend (service node) cannot directly see the client ip, only through `X-Forwarded-For` (HTTP). pattern to get. 

The intranet ULB7 uses `ECMP+ BGP` to achieve high availability, and the intranet ULB7 server is connected to the upstream through Quagga The switcher establishes a BGP connection. Multiple servers in the same cluster will send the same VIP (virtual IP) to the uplink exchange machine declare. In this way, the uplink switching machine balances traffic to each service in the cluster according to the `ECMP` algorithm on the machine. When an exception occurs on a server, BGP is interrupted within three seconds, kicking the failed server out of the cluster for verification Services can still work normally. 
### Extranet ULB7
ULB7 is based on Haproxy, and a single example can support more than 40w pps and 2Gbps , and at least 400,000 parallel connections. ULB uses the affinity of the CPU to achieve core isolation and resource control. 
 
Unlike the DR mode used by ULB4, ULB7 uses Proxy mode, also known as Full NAT mode. After receiving the client's request, ULB7 connects the client to the ULB7 EIP Transfer, converted into ULB7 proxy IP (proxy IP) and Backend server The connection of the actual IP address. As a result, Backend cannot see the client ip directly. In addition, the node health check module is integrated in the ULB7 process, so there is no need for additional sections Click the Health Check module. 

Similarly, in the external network ULB7, the cluster health check module will periodically detect the survival status of the server , if a problem is found with the server, UVER will be notified and the service will be abnormal Server rejection, thus ensuring high availability. Similarly, ULB7 clusters on the Internet are highly available across zones. 

### Mode comparison
Compared with ULB7, ULB4 has stronger forwarding ability and is suitable for scenes that pursue forwarding performance. ULB7, on the other hand, can process seven layers of data SSL offloading, domain name forwarding, path forwarding and other functions, and back-end nodes are not required VIP (virtual IP) configuration. 

## Performance indicators

ULB performance indicators
* New connections per second (CPS): The number of new connections per second, and the ability to handle new connections. 
* Maximum number of simultaneous connections: The total number of connections per second, and the ability to realize and process connections. 
* Packet processing per second (PPS): The number of packets forwarded per second, which is reflected in the packet forwarding rate. 
* Maximum throughput: The bandwidth that can be supported. 
* Requests per second (QPS): The number of queries per second, QPS is not the core of ULB Target, the real consumption of performance is CPS. Short connection case, QPS=CPS; Long connection case, QPS > CPS. 

```
Note: The following performance indicators are theoretically maximum performance under simple and ideal flow rates, and here are some limitations and explanations.

1. Packet forwarding ULB can achieve the performance shown in the following table, please request to proxy ULB in a single VPC Examples of shared resources.

2. After enabling the ULB log management function, there will be a 10% performance loss.

3. SSL cps is obtained under the algorithm conditions of TLSv1.2, ECDHE-RSA-AES256-GCM-SHA384, 2048, 256, and the algorithm with more performance may not meet the following performance indicators.

4. ULB examples are deployed in cluster mode, and the actual example is also multi-forward mode on cluster stand-alone machines, so the traffic from unbalanced sources, especially the traffic from a single source, Maximum performance will not be achieved.

5. Please request the maximum number of concurrent connections of the agent ULB refers to the number of connections and ULB from the customer end to ULB To the sum of the connections to RServer, the monitoring shows only the customer end to ULB The number of connections on this side.
```

| Products | Mode | New Connections per second (cps) | Maximum parallel connection (pcs) | Maximum throughput (bps) | Packets per second (pps) |
| --- | --- | --- | --- | --- | --- |
| Extranet ULB | Packet forwarding | 600,000 | 100,000,000 | 30G | 18,000,000 |
| Extranet ULB | Proxy request | 40,000 (4,000 ssl) | 300,000 | 800M | 400,000 |
| Intranet ULB | Packet forwarding | 600,000 | 100,000,000 | 30G | 21,000,000 |
| Intranet ULB | Proxy request | 40,000（4,000 ssl） | 300,000 | 800M | 400,000 |

## Routing configuration

* To be able to correctly forward data to the backend service node, you need to ensure ULB

* The firewall/route of the backend service node can be sufficient to allow access to the corresponding network segment, otherwise it may cause this The host cannot provide services normally.

### HTTP header restrictions

ULB's default header size is 8K. When configuring, make sure that the size of the HTTP header does not exceed 8K. This will cause the ULB to not work properly.

### Quota limits

| | Packet forwarding ULB | Proxy Request ULB |
| --- | --- | --- |
| The number of EIPs can be tied | 20 | 100 |
