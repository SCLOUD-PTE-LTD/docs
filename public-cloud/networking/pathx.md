---
layout: default
title: PathX
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/pathx/
nav_order: 6
---
# PathX
## Product introduction
Global Dynamic Acceleration (PathX) is a network acceleration product dedicated to improving the quality of global access to applications.

After using PathX, with the help of forwarding clusters distributed all over the world, users in various regions can achieve nearby access, and forward requests back to the source site through PathX, effectively avoiding problems such as slow response and packet loss caused by cross-border network congestion.

## Function introduction
### Multi-port acceleration
An accelerated domain name can perform accelerated forwarding for multiple ports of the origin site at the same time.

### Multi-protocol acceleration
Support TCP, UDP layer 4 forwarding, and HTTP, HTTPS layer 7 forwarding; use TCP to support HTTP HTTPS HTTP2 Websocket and other scenarios; use UDP to support QUIC protocol, chat room and other applications

### Multi-region acceleration
If there are multiple regions that need to be accelerated, multiple acceleration lines can be bound to one acceleration configuration, and the acceleration domain name remains unchanged.

### The acceleration effect is obvious
Effectively improve the stability of cross-border and trans-continental networks, and reduce network delays by an average of 20%

### Zero packet loss during peak hours
Using SCloud's self-developed overseas routing optimization technology and the advantages of the global backbone network, it supports intelligent link scheduling and greatly optimizes link packet loss.

## Product advantages
### Elasticity
Multiple acceleration configurations can share the bandwidth of one acceleration line, pay on demand, and use it immediately.

### Stabilize
Forwarding back to source traffic load balancing, multiple lines are redundant.

### Simple
Acceleration configuration can be completed within 1 minute, saving operation and maintenance work.

### Safety
Provides multi-G level defense capabilities for free and isolates tenants.

### Transparent
Install the SCloud self-developed TOA module on the source server, and you can see the real IP of the visitor in the business log.

### Monitor
Provide bandwidth usage data by region to facilitate operational analysis of traffic source distribution

### Alarm
The alarm strategy is flexible, and the bandwidth usage is automatically notified.

## Glossary
### Source site IP, source site domain name
The IP or domain name that needs to be accelerated can be selected. In order to flexibly expand the scale of the origin server, it is recommended to fill in the domain name.

### Business location
The region where the origin server is located. For example, if the origin is in Los Angeles, the business location is the United States.

### Acceleration area
The area that needs to be covered by the business (the distance from the source station is far away). For example, if the source station is in the United States and needs to cover Chinese users, the acceleration area is China.

### cname domain name
The nearest access domain name provided by the accelerated configuration instance. You need to change the cname record of the domain name of the origin site to this domain name in the acceleration zone through DNS intelligent resolution service, so that users in the acceleration zone can access nearby.

### Acceleration line
The acceleration line refers to the line instance from the acceleration area to the business location (the location of the source station). One acceleration line can be bound to multiple acceleration configurations (with the same location of origin). If the origin is located in China and the acceleration area is the United States, then the acceleration line to be purchased is "Los Angeles to China or Washington to China".

### Accelerate configuration
Configure the origin server (IP/domain name) to be accelerated, support multiple service ports, specify the location of the origin, and select the purchased acceleration line.

## Price
### Accelerated Configuration Price
- Acceleration configuration can be bound to multiple acceleration lines, and you need to pay a fixed fee of 60 credit * N per month. N is equal to the number of lines bound to the acceleration configuration, and the renewal time follows the bound acceleration line resources.
- 
### Billing method of the accelerated line
- Currently supports bandwidth prepaid
- The bandwidth prepayment method is to charge in advance according to the purchased bandwidth, which can be paid by time/month/year. After deleting resources, you will no longer be billed and you will be refunded for the unused portion. If you deprecate the acceleration line but do not delete the resources, arrears orders may be generated, resulting in unnecessary financial losses.

### PathX Resource Renewal Order Instructions
At present, the renewal of PathX accelerated line and accelerated configuration is completed together. 

For example: you purchased a 2mb line from China to Los Angeles. Monthly renewal order price = bandwidth price 2 of a certain line in the console + acceleration configuration price (the monthly fixed fee is 60 credit without discount) and the number of acceleration configurations bound to this line.

### Specific acceleration line price
The unit price of prepaid bandwidth is different for different lines. For more information, please consult your account manager or online technical support.

## FAQ
### Relationship between acceleration configuration and acceleration line
1. Bandwidth sharing function: an acceleration line can be bound by multiple acceleration configurations, and these acceleration configurations share the bandwidth of the acceleration line;
2. One acceleration configuration can bind multiple acceleration lines.
3. Deleting the acceleration configuration will not affect the acceleration line, and the acceleration line still exists;
4. If the acceleration configuration is bound to the acceleration line, the acceleration line cannot be deleted; if there is no acceleration configuration on the acceleration line, the acceleration line can be deleted.

### Why do I see a large number of fixed IP addresses accessing the origin server?
These addresses are the IP addresses of the accelerated cluster forwarding nodes, which serve two functions:

1. Transmit your business data traffic (the real IP of the visitor can be obtained through the toa module)
2. Do regular health checks on the origin server
The health check of the origin server is to detect the availability of the origin through the principle of TCP three-way handshake, which is a short connection. For UDP ports, health checks are not currently supported.

> Note: The cloud service provider or IDC security policy related to the origin site may be sensitive to a large number of short TCP connections in a short period of time.

### How to check PathX's back-to-source IP (forwarding node IP near the source site)?
Please check it on the PathX resource details page, which can be used as a reference when setting the whitelist on the origin site. 

These IPs will change in case of ddos attack or computer room network shutdown. 

The IP obtained by accelerated domain name resolution is the IP of the access point close to the client side, and it will change in the event of a ddos attack or the shutdown of the computer room network.

### Can non-SCloud servers use global dynamic acceleration?
Yes, as long as the server is reachable by public network routing.

### Is there any difference between global dynamic acceleration and CDN acceleration?
CDN accelerates access to resource caches at edge nodes, and the cached objects are static media resources. The entire link is on the public network, and the cross-border back-to-source line is not stable. Leading foreign CDN manufacturers have made in-depth optimization of the back-to-source network. Affected by policies, the number of nodes in some areas is limited and other products are needed for assistance.

Global application acceleration optimizes the quality of the transnational (continental) network from the client to the source site. Relying on SCloud network scheduling capabilities, it controls packet loss and delay. It does not support application data caching, and each request will access the source site to obtain resource data. Suitable for payment, login, chat, long connection and other scenarios. At the same time, it supports websocket, http and other application layer protocols, terminates the tcp connection in advance on the side close to the client, and optimizes the end-to-end long connection, which can greatly increase the link speed.
### Can HTTP(s) websites or API scenarios be used?
1. It can be used, and the global dynamic acceleration supports tcp transparent transmission back to the source. Configure TCP port 80 or 443 on the console, the certificate is still deployed on your business server, no other settings are required.
2. If your business scenario requires Offloading SSL nearby, and use the HTTP protocol to return to the source, you can use the HTTPS-HTTP method in PathX layer 7 port forwarding.
3. If you use HTTP(s) requests, it is not convenient to install the TOA module on the source site and you want to obtain the real client IP, you can use PathX HTTPS-HTTPS or HTTP-HTTP forwarding.

### What is multi-site access?
Take the acceleration from North China, East China, and South China to Hong Kong as an example. After the acceleration is created, users in North China, East China, and South China access the same acceleration domain name, but the resolved IPs are different. These IPs correspond to the entrances of pathx in the three regions. That is, when users in different regions access the pathx acceleration domain name, the IP of the acceleration entrance closest to the user will be resolved.

### After the resource is used for a period of time, the accelerated domain name + port of PathX or GlobalSSH suddenly cannot be accessed normally, but the source site + port can be accessed normally

Test with curl will generally report "curl: (56) Failure when receiving data from the peer" Test with telnet After the prompt connection is successful, immediately receive "Reset By Remote Peer" Important! focus! focus! Test with nc, it will prompt that the connection is established successfully.

1. Check whether the source site has security policy settings, such as Alibaba Cloud's Security Knight (the cloud shield will automatically block if the user does not make any configurations, and the whitelist IP needs to be enabled to protect the CDN trust vendor), fail2ban, etc. If so, you can download it from The console resource details page obtains pathx or globalssh export ip and adds it to the whitelist.
2. The above cannot solve your problem, please submit a ticket to consult your origin server provider, whether there is a security policy to automatically block unknown source IP.
3. Check system parameter settings <br/>
`net.ipv4.tcp_timestamps = 1 net.ipv4.tcp_tw_recycle = 0 net.ipv4.tcp_tw_reuse = 1`

Enabling tcp_tw_recycle is enough to recycle TCP connections (effective when used as a client pressure test or when sending a large number of requests to the outside). Because tcp_tw_recycle was designed earlier, it did not consider that NAT technology has become popular in the public network today, which will lead to NAT (such as Internet Cafe, 4G, WIFI) The connection of the user part fails. Currently this parameter is basically obsolete. To increase the speed of tcp recovery can also be achieved by reducing the time for tcp to wait for timeout.

### Can the source site be modified?
Beginning in July 2020, the PathX accelerated source site supports modification. On the accelerated configuration details page, you can modify it to a new source site IP or domain name, and the original accelerated domain name and port remain unchanged. Note that after modifying the source site, a short service interruption may occur, requiring the client to reconnect.

### Is it possible to retrieve the pathX arrears after recovery?
It cannot be retrieved after recycling. At this time, the resource is marked for deletion in our resource and billing system, and the resource needs to be recreated.

### Can pathX configure bandwidth alarms?
It can be configured in the "Alarm Template" of the acceleration line details page, and supports bandwidth absolute value and bandwidth usage alarms. On the acceleration line details page and acceleration configuration page, the drop-down list can select different acceleration areas to facilitate the location of entrances with high bandwidth ratios.

### PathX's current limiting measures
PathX limits traffic according to the purchased bandwidth. When the real-time bandwidth (the maximum value of outgoing bandwidth and incoming bandwidth) exceeds the purchased bandwidth, it will trigger current limiting. 

Current limiting will cause increased packet loss rate and connection interruption. The time-limited flow of purchased bandwidth will be automatically released. Please configure the bandwidth usage alarm on the line details page. 

PathX bandwidth monitoring data needs to be dynamically collected and calculated, and there is a 1-2 minute lag in computer rooms distributed around the world, resulting in a short period of time. The actual bandwidth will greatly exceed the purchased bandwidth, and the current limit command will cause serious loss after the bandwidth report is completed. package case.

### Explanation of Capability Restrictions Related to Acceleration Configuration
1. Number of acceleration areas: unlimited;
2. Number of ports: 50;
3. Layer 4 and Layer 7 protocol support: http(s) websocket(s), layer 4 forwarding can be extended to http2, `quic`, `ipsec-vpn`, `rtmp`, rtc and other protocols, ssl-vpn needs non-standard support, and does not support ftp access.
4. Layer 4 port forwarding: on ordinary clusters, the number of concurrent connections is limited to 10,000;
5. Layer 7 port forwarding: On ordinary clusters, a single source IP (client) is limited to 100 concurrent connections, the maximum number of concurrent connections is 4000, and HTTPS requests will be lower; high-performance clusters are supported, and you can contact us at any time if necessary.

### How to handle load balancing when the origin server domain name and origin server have multiple IPs?
1. If the domain name of the source site filled in is resolved to multiple IPs, a health check will be performed on all source site IP+port combinations (excluding UDP protocol ports for the time being) every 30s, and the source site nodes that fail the health check will be kicked out;

2. The newly added connection request of the client is distributed among multiple source station IP nodes in a polling manner;