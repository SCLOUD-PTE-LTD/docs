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
- The Apple review channel is a special type of acceleration configuration. It does not need to be bound with an acceleration line. It needs to pay a fixed fee of 96 credit per month, which can be paid on time/monthly/yearly.

### Billing method of the accelerated line
- Currently supports bandwidth prepaid
- The bandwidth prepayment method is to charge in advance according to the purchased bandwidth, which can be paid by time/month/year. After deleting resources, you will no longer be billed and you will be refunded for the unused portion. If you deprecate the acceleration line but do not delete the resources, arrears orders may be generated, resulting in unnecessary financial losses.

### PathX Resource Renewal Order Instructions
At present, the renewal of PathX accelerated line and accelerated configuration is completed together. 

For example: you purchased a 2mb line from China to Los Angeles. Monthly renewal order price = bandwidth price 2 of a certain line in the console + acceleration configuration price (the monthly fixed fee is 60 credit without discount) and the number of acceleration configurations bound to this line.

### Specific acceleration line price
The unit price of prepaid bandwidth is different for different lines. For more information, please consult your account manager or online technical support