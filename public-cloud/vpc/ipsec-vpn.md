---
layout: default
title: IPSecVPN
parent: VPC
grand_parent: Public Cloud
permalink: /public-cloud/vpc/ipsec-vpn/
nav_order: 1
---
# IPSec VPN 
The VPN gateway (IPSec VPN) provides secure and reliable encrypted communication services.
## Product introduction

VPN gateway service, which provides high-availability VPN service that can be tolerated by disasters, needs to cooperate with users in SCloud's VPC The user's local gateway and public network service are used together. Users can choose from a variety of encryption and authentication algorithms to ensure the reliability of the tunnel.

### Basic concepts of VPN gateway

#### Service structure of VPN gateway

The VPN gateway service is mainly composed of three parts:

VPN gateway: The VPN gateway in the public cloud on the SCloud side needs to be connected to the corresponding UVPC.

Guest Gateway: The gateway for customers in the local network

Tunnel: The tunnel connecting the VPN gateway and the customer gateway requires the customer to configure the corresponding algorithm and policy. Tunnels are built in the public network, and the quality of the network is affected by the public network.

#### VPNgateway terminology explained

| Noun | Illustrate |
| --- | --- |
| VPN gateway | The exit gateway of UVPC in the SCloud public cloud. |
| Guest gateway | On behalf of the gateway in the local network, the IP address, name and other information of the customer gateway are required on the console. |
| tunnel | To connect the channel between the customer gateway and the VPN gateway, the customer needs to set its encryption algorithm, authentication algorithm, key, etc. After installation, if one party is connected, the tunnel can be established. |
| EIP | An external elastic IP address is tied to the VPN to provide an external access address and bandwidth. |

#### Overview of VPN gateway features

| Function | Description |
| --- | --- |
| IKE certification support | Provides authentication for messages in the IKE negotiation process, supporting `md5`, `sha1`, and `sha2-256` Three authentication algorithms |
| IKE encryption support | Provides encryption protection for messages in the IKE negotiation process, supporting `3des`, `aes128`, and `AES192` and `AES256` four encryption algorithms |
| IKE DH Group | Specifies the `Diffie-Hellman` group used by IKE to exchange keys, which supports `1, 2, 5, 14, 15, 16` |
| ID type | To describe the endpoint identity of the VPN gateway, you can select automatic identification, IP address representation, or domain name representation |
| IPSec certification support | The authentication protection function provided by IPSec for user data supports two authentication algorithms: `md5` and `sha1` |
| IPSec encryption support | IPSec provides encryption and protection functions for user data, supporting `3DES`, `AES128`, `AES192` and `AES256` Four encryption algorithms |
| IPSec security protocol | IPSec supports AH and ESP two security protocols, AH only supports data authentication protection, ESP Authentication and encryption are supported, and the ESP protocol is recommended |
| PFS | PFS is a security feature in which one key is cracked without affecting the security of other keys, and supports DH groups of `1, 2, 5, 14, 15, 16` and off disable） |

#### The VPN gateway uses quotas

The default quota for each account number is as follows:

| Name | Quotas |
| --- | --- |
| VPN gateway | 5 |
| Guest gateway | 30 |
| Tunnels (single VPN gateway creation quota). | 20 |
| The number of peer CIDR segments per tunnel | 20 |
| The number of local network segments per tunnel | 10 |

## Product price
VPN gateway pricing is as follows:

There is no coupling relationship between the VPN gateway and the EIP instance, and the respective performance and billing cycle can be enjoyed exclusively.

| Product Specifications | Product Price | Product Performance | 
| -- | -- | -- | 
| Standard | 12.9 USD/month | 50 Mbps | 
| Enhanced | 34.41 USD/month |  |		