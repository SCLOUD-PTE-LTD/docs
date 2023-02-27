---
layout: default
title: UNet
parent: Networking
grand_parent: Public Cloud
permalink: /public-cloud/networking/unet/
nav_order: 1
---

# Elastic IP addresses on the Internet
## Product introduction

SCloud ExternalElastic IP (EIP)is an Internet standard static IP Address. Serve the external elastic IP address with the cloud host UHost, load balancing ULB, NATgateway, etc.
The service can provide the services with the ability to access the external network.

## Billing method introduction

In the SCloud cloud platform,users can select different bandwidth usage patterns for each EIP in the account. At present, all regions in the cloud platform can provide **standard** bandwidth, **traffic** billing **,** shared **bandwidth** , and **Bandwidth** postpaid **billing** methods such as billing. For details on the rules for the recovery of overdue resources, see Recovery of expired resources.

### Bandwidth billing

When you purchase an elastic IP address for bandwidth billing, you should purchase an egress bandwidth cap at the same time. If you select this billing method, the public IP address (rawIP)is free of charge and does not count Traffic charges.

### Traffic billing

When purchasing elasticIP addresses, you can choose to set the "egress" bandwidth cap, according to the unit price of different regions IP production "outbound traffic" is billed after midnight every day, and the specific billing is carried out Information can be seen below the monitoring icon on the EIP Details page. If you select this billing method, the public IP address (naked-IP) is charged, excluding bandwidth fees with.

**Traffic billing restrictions**

The restrictions will take effect at 00:00 on December 1, 2021

- The maximum bandwidth peak value supported by a single EIP is 300 Mbps, and the peak bandwidth value is not used as a service commitment The Nordic indicator is only used as a reference value and the peak value of the upper bandwidth limit. When there is a scramble for cash resources, the peak value of bandwidth may be limited.
- Under a single item, the sum of the actual bandwidth usage values of EIPs charged by all traffic in a single region is not more than 2 Gbps. If your service requires guaranteed or larger peak bandwidth, you must use the bandwidth billing EIP or shared bandwidth.

### Shared bandwidth

For more information about the shared bandwidth mode, see the Shared Bandwidth.
### Bandwidth postpaid fee

If you select this billing method, the public IP address (nakedIP) is free of charge, and only the bandwidth fee is charged. The actual bandwidth of EIP is limited to the peak bandwidth, and the peak bandwidth: the bottom guarantee The bandwidth ratio relationship is 5:1.

#### Fees

The fee is charged in two parts: minimum bandwidth fee + postpaid fee.

(1) The guaranteed bandwidth fee is charged when purchasing the postpaid bandwidth, and the price is consistent with the price list of ordinary bandwidth.

(2) The postpaid fee will be settled at 0:00 the next day, and the fee will be charged according to the average value of the bandwidth exceeding the minimum guarantee, that is, below The average value of the shaded points in the graph (one point every five minutes, the band value of the point is calculated as 0 below the guarantee.), higher than the guarantee, according to the bandwidth value-the guarantee).

#### Postpaid billing cases

For the bandwidth of 5M guarantee and peak value of 25M, 25 minutes were used,production Five billable bandwidth values are generated: 4, 5, 15, 15, and 15.

The minimum bandwidth fee has been paid at the time of purchase, and the bandwidth value of the postpaid fee= (0+0+10+10+10)/5=6M (due to 4,5 None of them exceed the guarantee of 5M, so it is 0, and when the value is 15, it is paid later The fee billing value is 15-5=10M). The fee to be paid is 6M \* 7credit/Mbps/day \* 25minutes= 0.729 credit.

#### Pay attention to the bandwidth post-payment fee

1. When the bandwidth is adjusted after the bandwidth payment, the minimum bandwidth value is adjusted, and the guaranteed bandwidth value is adjusted After consolidation, the corresponding peak value bandwidth will be automatically adjusted.

2. The monitoring and control data of EIP - bandwidth utilization is calculated according to the peak bandwidth.

3. The optional range of guaranteed bandwidth is 5-200 Mbps.

## Introduction to the line

- BGP: Conventional BGP line bandwidth. Supports billing methods such as bandwidth billing, traffic billing, and shared bandwidth.
- Boutique BGP: Optimize dedicated lines covering Chinese mainland traffic, avoid by passing international operators, and improve it Access quality. Available only in Hong Kong, it supports bandwidth billing and premium shared bandwidth.

## Internet bandwidth limit

### Ingress bandwidth is free of charge

When the purchased egress bandwidth is less than 50 Mbps, the ingress bandwidth is equal to 50 Mbps. When the purchased egress bandwidth is greater than or equal to 50 Mbps, the ingress bandwidth is equal to the egress bandwidth. (Ingress bandwidth: from Internet to SCloud; Egress bandwidth: from SCloud to the Internet. ）