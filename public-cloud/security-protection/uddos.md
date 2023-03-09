---
layout: default
title: UDDoS
parent: Security Protection
grand_parent: Public Cloud
permalink: /public-cloud/security-protection/uddos/
nav_order: 2
---
# UDDoS - DDoS attack protection 
UDDoS (SCloud DDoS) DDoS attack protection is a DDoS attack protection product for customers. It includes two protection methods, one is through the anti-DDoS data room, and the other is to directly resist DDoS attacks (cleaning) on IP bandwidth.

| | Introduction | Advantages | Shortcoming |
| --- | --- | --- | --- |
| Anti-DDoS| Provide DDoS for registered domain names or origin server IPs (including non-SCloud elastic public IP addresses). Attack and defense. When the user's domain name or origin server IP address (including non-SCloud elastic public IP address). When suffering from a large amount of DDoS attacks, you can use the Anti-DDoS Pro IP proxy origin server IP address to face users. Hide the IP address of the origin server to divert attack traffic to the Anti-DDoS Pro IP address to ensure the stable and normal operation of the origin server | It can resist larger attacks and support up to 1.2T of defense capability. It can also support SCloud and non-SCloud origin server IPs. | It is necessary to replace the new IP, the operation is more complicated, and it takes a certain amount of time to switch to Anti-DDoS. |
| Cleaning | Cleaning is a self-developed product that provides DDoS attack protection for SCloud's elastic extranet IP After years of offensive and defensive experience, it adopts a variety of defense strategies to support defense network layer attacks, such as TCP Message attack, SYN flood attack, ACK flood attack, etc | There is no need to change the IP, and it is cut and changed in seconds after being attacked, and the response is lower. | It cannot resist larger attacks, and the maximum can currently support 10G defense. Only SCloud elastic public IP addresses are supported, but non-SCloud IP addresses are not supported. |