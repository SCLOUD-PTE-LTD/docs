---
layout: default
title: NAT
parent: VPC
grand_parent: Public Cloud
permalink: /public-cloud/vpc/nat/
nav_order: 2
---

# NAT gateway
## Introduction

NAT gateway is an enterprise-level VPC public network gateway that allows cloud resources in subnets that are not bound to elasticIP addresses You can configure port forwarding rules to enable cloud resources to provide services to the outside world.

## Mode setting

NAT gateways can be selected from normal mode or whitelist mode.

In normal mode, all cloud resources in the specified subnet that are not bound to elasticIP addresses can pass through the NATThe gateway accesses the Internet.

In the whitelist mode, only cloud resources in the subnet specified by the NAT gateway and defined in the whitelist can pass through the NAT gateway to go out of the external network 。

Before you change the NATnetwork to the white list mode, you can configure the white list in advance to ensure that you switch to the whitelist mode Does not affect normal business. In normal mode, the whitelist is configurable but does not take effect.

## Port forwarding

Users can configure port forwarding to map the internal network ports of cloud resources in a VPC to a NATgateway to make cloud resources matchable Outside to provide services.

Cloud resources in the specified subnet of the NAT gateway that have elastic IPaddresses bound will not appear in the port forwarding configuration list.

## Network export

You can set a cloud resource in a subnet to access the Internet through a specified EIP.

Provide default egress rules to support resources in the subnet to access the Internet through the responsible balancing mode or designatedEIP.

## Quota limits

| Name | Quotas |
| --- | --- |
| The number of EIPs that can be tied to NAT | 64 |
| Number of egress rules | 100 |
| Number of port forwardings | 100 |

## Products supported by NAT

Currently, the following types of sources are supported for whitelisting, egress rules, and port forwarding

| Name | Whitelist | Export rules | Port forwarding |
| --- | --- | --- | --- |
| UHOST cloud host | ✓ | ✓ | ✓ |
| UPHOST physical cloud host | ✓ | ✓ | ✓ |
| UDHost Private Zone | ✓ | — | ✓ |
| UHybrid hosts the cloud | — | — | — |
| UNI virtual NIC | ✓ | ✓ | ✓ |
| UAuditHost fort machine | ✓ | ✓ | ✓ |