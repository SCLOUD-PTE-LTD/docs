---
layout: default
title: UDDoS
parent: Security Protection
grand_parent: Public Cloud
permalink: /public-cloud/security-protection/uddos/
nav_order: 2
---
# UDDoS - DDoS attack protection 
## Introduction
### UDDoS (SCloud DDoS) 
DDoS attack protection is a DDoS attack protection product for customers. It includes two protection methods, one is through the anti-DDoS data room, and the other is to directly resist DDoS attacks (cleaning) on IP bandwidth.

| | Introduction | Advantages | Shortcoming |
| --- | --- | --- | --- |
| Anti-DDoS| Provide DDoS for registered domain names or origin server IPs (including non-SCloud elastic public IP addresses). Attack and defense. When the user's domain name or origin server IP address (including non-SCloud elastic public IP address). When suffering from a large amount of DDoS attacks, you can use the Anti-DDoS Pro IP proxy origin server IP address to face users. Hide the IP address of the origin server to divert attack traffic to the Anti-DDoS Pro IP address to ensure the stable and normal operation of the origin server | It can resist larger attacks and support up to 1.2T of defense capability. It can also support SCloud and non-SCloud origin server IPs. | It is necessary to replace the new IP, the operation is more complicated, and it takes a certain amount of time to switch to Anti-DDoS. |
| Cleaning | Cleaning is a self-developed product that provides DDoS attack protection for SCloud's elastic extranet IP After years of offensive and defensive experience, it adopts a variety of defense strategies to support defense network layer attacks, such as TCP Message attack, SYN flood attack, ACK flood attack, etc | There is no need to change the IP, and it is cut and changed in seconds after being attacked, and the response is lower. | It cannot resist larger attacks, and the maximum can currently support 10G defense. Only SCloud elastic public IP addresses are supported, but non-SCloud IP addresses are not supported. |

### Introduction to DDoS Attacks
Denial of Service Attack (Abbreviation: DoS), also known as flood attack, is a network attack method whose purpose is to exhaust the network or system resources of the target computer, temporarily interrupt or stop the service, and cause its Not available to target customers.

Distributed Denial of Service attack (Abbreviation: DDoS), is that hackers use two or more compromised computers on the network as "zombies" to launch denial of service attacks on specific targets.

### Introduction to Anti-Advanced Principles
The high-defense service is a value-added protection service launched for the situation that Internet servers (including non-SCloud hosts) are unavailable after being attacked by a large-traffic DDoS attack. This service can provide customers with protection against DDoS, CC and other attacks, and can protect against SYN Flood, ACK Flood, UDP Flood, ICMP Flood, connection exhaustion attacks, DNS Request/Response Flood, HTTP Get/Post Flood, etc. Layer 3 to 7 s attack.


![1](/assets/images/ddos-architecture.jpeg)

Normally, users directly access the source site.

After configuring the Anti-Defense service, the customer switches the business to Anti-Defense (for services with a domain name, resolve the domain name to the IP or CNAME of Anti-Advanced; for services without a domain name, replace the business IP with Anti-Defense IP), and then all public network The traffic will first go to the high-defense computer room. The high-defense computer room will clean and filter the attack traffic, and forward the normal traffic to the source station IP, so as to ensure that the source station provides stable and normal services.

> Remark:<br/>
User refers to the user who accesses the customer's business.<br/>
Customers refer to users who purchase Anti-Advanced.

## Product advantages
Anti-DDoS (UADS) is a value-added paid product against DDoS attacks launched by SCloud for all users. This product can provide users with the following high-quality services:

### Complex wall-penetrating attack protection
In offense and defense, it is often not just a simple attack, but can be directly filtered through the machine. In order to ensure the protection effect, the high-defense computer room uses its own special equipment to support the protection of various ACK, SYN, connection exhaustion, and CC through the firewall. In addition, the effect of protecting against complex wall penetration attacks is achieved through the following three points:

### Customized exclusive strategy
7X24 expert support
Optional watermark protection
Ultra-large flow protection
Mainland China (Mainland) can provide a maximum DDoS attack protection capability of `1.2Tbps` for a single user, and Hong Kong, Macao and Taiwan regions support a protection capability of up to `400Gbps`, which can easily resist large-traffic DDoS attacks.

### Hide user service resources
Anti-DDoS Pro can replace and hide user sites. Using the high-defense IP as the front of the source site prevents attackers from finding the user's network resources and increases the security of the source site.

### Elastic protection
Provide a billing method combining "guaranteed protection + elastic protection" to reduce daily security costs for users. When necessary, the elastic protection value can be adjusted on-demand on the console, which takes effect in seconds and does not require any new equipment. At the same time, there is no need to make any adjustments in the business, and the service is not interrupted throughout the process.

When the attack traffic exceeds the guaranteed protection value, the user will continue to be protected to ensure uninterrupted business and will be charged according to the actual attack volume of the day.

## FAQ

1. What should I do if I want to improve the security of the host?<br/>
It is recommended to use host intrusion detection products, which are currently free to install and use.

1. After the EIP is blocked, how to check the attack situation?<br/>
It can be seen in Cloud Security Center->Security Event->Network Attack.

1. Is there free cleaning service in the server?<br/>
Yes, the basic protection is a free service, which is enabled by default, and the free cleaning capabilities of the server refer to the cleaning capabilities of the server.

1. What is the trigger condition for EIP cleaning?<br/>
If the incoming packet volume of EIP exceeds the cleaning threshold, it will be cleaned. After cleaning, abnormal traffic will be filtered. The default cleaning threshold of the server refers to the cleaning capability of the server.

1. What is the trigger condition for EIP blocking?<br/>
If the incoming traffic of EIP exceeds the blocking threshold, it will be blocked. After blocking, the incoming traffic cannot reach the cloud host.

1. After the EIP is blocked, why are there still a few areas that can still be pinged?<br/>
Because blocking is implemented on the carrier's backbone network, there may be cases where machines in the MAN can still be pinged through after blocking.

1. Can the blocking threshold of EIP be increased?<br/>
It can be adjusted appropriately according to the purchased bandwidth and the bandwidth of the server. For details, please contact technical support or account manager.

1. Can the cleaning threshold of EIP be increased?<br/>
It can be adjusted on the console by yourself, and the maximum support can be adjusted to `500kpps`. If you have higher package requirements, please contact technical support or account manager.

1.   What are the countermeasures after EIP is blocked?<br/>
If it is an attack, it is recommended to purchase high-defense for defense. If it is not an attack, you can apply for unblocking and increase the blocking threshold.

1.   How to report the crime after being attacked?<br/>
Report the case to the local network monitoring department, and provide relevant information according to the requirements of the network monitoring department. Then the network monitoring department judges whether it meets the standards for filing a case, and enters the network monitoring processing process. <br/>
After the case is formally filed, SCloud will cooperate with the contact person of the network monitoring department to provide attack evidence (traffic graph, attack events, packet capture information, etc.).

1.   The cloud host is attacked by small traffic, why not protect it?<br/>
Since the basic protection is a public DDoS protection service, it does not protect against small traffic attacks (less than the cleaning threshold). It is recommended to optimize server performance and install host intrusion detection products to deal with small traffic attacks.

1.   After the EIP is blocked, why the cloud host traffic monitoring does not see large traffic data?<br/>
Because the traffic monitoring system of DDoS protection is deployed at the network boundary of the server (with a granularity of 2 seconds), it is located on the upper layer of the cloud host, so in general, the traffic monitoring data of DDoS protection will be larger than the traffic data seen on the cloud host.

1.   The cloud host has not been used yet, why is it attacked by DDoS?<br/>
As long as the business is connected to the external network for communication, there is a risk of being attacked by DDOS.

1.   The cloud host is attacked, what is the other party attacking?<br/>
Generally, attacks target IP addresses or services.

1.   Why do I still receive udp attack traffic after the cloud host is configured to prohibit udp traffic?<br/>
As long as the IP can be accessed from the external network, DDoS attacks can be launched, but the policy configuration is at the host boundary, which cannot prevent the attack traffic from reaching the server network boundary.

1.   If I am not under attack, why is the purge triggered?<br/>
Because when the inbound traffic exceeds the cleaning threshold, traffic cleaning will be triggered. If you do not want to be cleaned, you can increase the flow cleaning threshold.

1.   Why is the blocking triggered when downloading the business normally?<br/>
Because there will be an instantaneous burst of traffic during downloading, and when the incoming traffic exceeds the blocking threshold, blocking will be triggered. If you do not want to be blocked, it is recommended to limit the speed or apply for an increase in the blocking threshold.