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

SCloud ExternalElastic IP (EIP) is an Internet standard static IP Address. Serve the external elastic IP address with the cloud host UHost, load balancing ULB, NATgateway, etc.
The service can provide the services with the ability to access the external network.

## Billing method introduction

In the SCloud cloud platform,users can select different bandwidth usage patterns for each EIP in the account. At present, all regions in the cloud platform can provide **standard** bandwidth, **traffic** billing, shared **bandwidth**, and **Bandwidth** postpaid **billing** methods such as billing. For details on the rules for the recovery of overdue resources, see Recovery of expired resources.

### Bandwidth billing

When you purchase an elastic IP address for bandwidth billing, you should purchase an egress bandwidth limit at the same time. If you select this billing method, the public IP address (rawIP)is free of charge and does not count Traffic charges.

### Traffic billing

When purchasing elasticIP addresses, you can choose to set the "egress" bandwidth limit, according to the unit price of different regions IP production "outbound traffic" is billed after midnight every day, and the specific billing is carried out Information can be seen below the monitoring icon on the EIP Details page. If you select this billing method, the public IP address (naked-IP) is charged, excluding bandwidth fees with.

### Traffic billing restrictions

The restrictions will take effect at 00:00 on December 1, 2021

- The maximum bandwidth peak value supported by a single EIP is 300 Mbps, and the peak bandwidth value is not used as a service commitment The Nordic indicator is only used as a reference value and the peak value of the upper bandwidth limit. When there is a scramble for cash resources, the peak value of bandwidth may be limited.
- Under a single item, the sum of the actual bandwidth usage values of EIPs charged by all traffic in a single region is not more than 2 Gbps. If your service requires guaranteed or larger peak bandwidth, you must use the bandwidth billing EIP or shared bandwidth.

### Shared bandwidth

For more information about the shared bandwidth mode, see the Shared Bandwidth.
### Bandwidth postpaid fee

If you select this billing method, the public IP address (nakedIP) is free of charge, and only the bandwidth fee is charged. The actual bandwidth of EIP is limited to the peak bandwidth, and the peak bandwidth: the bottom guarantee The bandwidth ratio relationship is 5:1.

#### Fees

The fee is charged in two parts: minimum bandwidth fee + postpaid fee.

- The guaranteed bandwidth fee is charged when purchasing the postpaid bandwidth, and the price is consistent with the price list of ordinary bandwidth.

- The postpaid fee will be settled at 0:00 the next day, and the fee will be charged according to the average value of the bandwidth exceeding the minimum guarantee, that is, below The average value of the shaded points in the graph (one point every five minutes, the band value of the point is calculated as 0 below the guarantee.), higher than the guarantee, according to the bandwidth value-the guarantee).

#### Postpaid billing cases

For the bandwidth of 5M guarantee and peak value of 25M, 25 minutes were used,production Five billable bandwidth values are generated: `4, 5, 15, 15, and 15`.

The minimum bandwidth fee has been paid at the time of purchase, and the bandwidth value of the postpaid fee = `(0+0+10+10+10)/5 = 6M` (due to 4, 5 none of them exceed the guarantee of 5M, so it is 0, and when the value is 15, it is paid later. The fee billing value is `15-5 = 10M`). The fee to be paid is `6M x 7 credit/Mbps/day x 25 minutes = 0.729 credit`.

#### Pay attention to the bandwidth post-payment fee

- When the bandwidth is adjusted after the bandwidth payment, the minimum bandwidth value is adjusted, and the guaranteed bandwidth value is adjusted After consolidation, the corresponding peak value bandwidth will be automatically adjusted.

- The monitoring and control data of EIP - bandwidth utilization is calculated according to the peak bandwidth.

- The optional range of guaranteed bandwidth is 5-200 Mbps.

## Introduction to the line

- BGP: Conventional BGP line bandwidth. Supports billing methods such as bandwidth billing, traffic billing, and shared bandwidth.
- Boutique BGP: Optimize dedicated lines covering Chinese mainland traffic, avoid by passing international operators, and improve it Access quality. Available only in Hong Kong, it supports bandwidth billing and premium shared bandwidth.

## Internet bandwidth limit

### Ingress bandwidth is free of charge

When the purchased egress bandwidth is less than 50 Mbps, the ingress bandwidth is equal to 50 Mbps. When the purchased egress bandwidth is greater than or equal to 50 Mbps, the ingress bandwidth is equal to the egress bandwidth. (Ingress bandwidth: from Internet to SCloud; Egress bandwidth: from SCloud to the Internet).

## Apply for an elastic IP

Usually when applying for cloud host (UHost) or load balancing (ULB),at the same time An external elastic IP will be applied for and the IP will be associated with the requested source Proceed to tie-up.

In addition to this, it can be found in the **All Products** -\> **Base Network UNet** -\> **Elasticity IP** page, click **Apply EIP** button to apply.

In the application process, you can select the matching billing method and bandwidth according to the application type.

If you select Specify IP address, you can select an IPaddress that has not been occupied by the last week.

Click the **Buy Now** button, enter the payment page to confirm it, and the payment is completed After that, the newly applied elastic IP can be tied to the existing resources fixed.

## Bind/unbind external elasticIP addresses

Elastic IP and the resources to which it is bound(such as UHost) are independent in terms of resources. This is enough to ensure that a certain resource needs to be deleted, and the elasticity is preserved IP addresses in order to tie uplink to other sources.

There are now three portals for the operation of binding resources, specifically elastic IP list and elastic IP Details , as well as specific product pages.

### Binding operation

Take the elastic IP list entry as an example, pass through the status column and select **Untied State** and click the binding button to open the binding Pop-ups.

Then select the type of resource that needs to be tied, and you can use the drop-down list to list the specific type that needs to be tied Search for the source and click **OK**.

### Unbind operation

The unbind operation can also be performed by selecting the Bound bulletin the Elastic IP List status column ElasticIP,and click to unbind the operation. It is also possible to select multiple elasticIPs to bind the bullet elasticIPs are unbound in batches.

## Adjust the IP bandwidth

Bandwidth is a variable resource for elasticIP. For the bandwidth of elasticIP, users can upgrade and upgrade at will according to their needs,and can be serviced without stopping It takes effect in real time to achieve the elasticity of the network.

The bandwidth of an elastic IP can be divided in to two parts, one of which is selected when applying for an elasticIP, which is called Base bandwidth; The other part is the bandwidth package that can be developed when needed and effective in the future, bandwidth The sum of the package and the base bandwidth is the maximum amount of bandwidth that can actually be reached.

### Adjust the base bandwidth

You can check the current bandwidth of the IP address in the Monitoring and Control section of the ElasticIP Details page and a statistical view of traffic. And you can click the **Basic Information** on the left side of the page in the **Alarm Template** button to monitor the bandwidth usage The control detail set up with alarms to grasp the usage of resources at any time and adjust the purchased bandwidth in a timely manner.

**Note:**

Both the standard bandwidth mode and the elasticIP address of the traffic billing mode can adjust the base bandwidth. In the shared bandwidth mode, you do not need to adjust the elastic IP bandwidth of a singleIP If tuning is required, you can open the shared bandwidth through the tuning office.

### Bandwidth package management

Temporary adjustment of bandwidth in the form of planning tasks.

**Note:**

EIP that shares bandwidth and traffic billing does not **support** bandwidth plans.

## Set the host to actively access the egress

If a host has multiple external elastic IP addresses tied to it, if you need to specify an exit, you must introduce the external elastic IP exit Priority is a feature. This special performance is enough to meet the needs of some scenes, the host takes the initiative to visit the outside and determine the access The demand for exports.

On the ECS details page,click the **Network** tab to see the assets that are tied to it All external elasticIP addresses on the source.

Select the elasticIPthat needs to be used as active access to the outside world,and click Set as Export when When the icon after the IP address turns green,it means that the IPaddress has been created is the egressIP.

## Release elasticIP

When the elastic IPis no longer needed, it can be released directly in the console.

Please note that the released elasticIP is not tied to any source.

## Change the billing method of elasticIP addresses

For purchased elasticIP addresses, you can change the billing method in the operation menu. Select the pop-up IP address for which you want to change the billing method,and then click the right side In a single column, the Change billing method button is pressed.

In the pop-up window, click **Traffic** and **OK** to cut the existing bandwidth billing elasticIP addressSwitch to traffic billing.