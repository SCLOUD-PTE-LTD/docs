---
layout: default
title: UNI
parent: VPC
grand_parent: Public Cloud
permalink: /public-cloud/vpc/uni/
nav_order: 5
---

# Virtual NIC
This document describes only the functions related to the product page, such as virtual NICs and secondary IP addresses

## Explanation of terms
Default NIC: When the NIC feature is enabled on the host, the EIP and firewall that are originally bound to the ECS will be installed on the default NIC.

Custom NIC: A network card manually created in the console can be elastically bound to an ECM.

Secondary IP address: The console manually applies for a secondary IP address, which can be used in conjunction with the primary IP address and can be elastically bound to the ECM, and the number of applications can be related to the ECM configuration.

| No. | vCPU | The number of virtual NICs | Number of private (1 primary + 1 secondary) of a single NIC |
| -- | -- | -- | -- |
| 1 | 1 < vCPU < 2 | 2 | 6 |
| 2 | 2 < vCPU ≤ 4 | 3 | 10 |
| 3 | 4 < vCPU ≤ 8 | 4 | 15 |
| 4 | 8 < vCPU ≤ 16 | 8 | 20 |
| 5 | 16 < vCPU ≤ 64 | 8 | 30 |
| 6 | 64 < vCPU ≤ 128 | 15 | 30 |
| 7 | vCPU > 128 | 15 | 30 |

## FAQ
#### Why can't the configured multi-network card be connected?
In general, due to the configuration of the network card, you can check as follows:
- Check whether the NIC is bound to the ECS and configured on the ECS.
- Check whether the RPF is turned off.
- Whether the network card routing is correct.

If the above configuration is still not passable, you can provide:
- The quintuple from the source IP to the destination IP and each hop information.
- The binding relationship between the NIC and the host.
- Information about the subnet to which the resource belongs.
- Routing configuration for the NIC.

Leave it to the after-sales consultation to assist in viewing.

#### What should I do if the network card function is not enabled on the cloud host and I want to open the second network card?

If the NIC function is not enabled when the ECS is created, we recommend that you create a new host to use the NIC feature.