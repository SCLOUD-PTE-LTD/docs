---
layout: default
title: IPSecVPN
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/IPSec-vpn/
nav_order: 12
---
# IPSecVPN Service
## Product Introduction
### Background
When you use the cloud platform to deploy and manage application services, some services are deployed on the IDC data center environment intranet or third-party public/private cloud platforms, such as web services are deployed on public cloud platforms, and applications and databases are deployed in private clouds, building a hybrid deployment environment of public cloud and private cloud.

In hybrid cloud application scenarios, the private network of the two ends can be directly connected through a dedicated line, and the network reliability and performance can be better guaranteed. However, due to the high cost of private lines, which is only suitable for some services that require high network latency, in order to save costs and establish point-to-point network communication with third-party platforms, the cloud platform provides VPN gateway-IPSecVPN connection service capabilities, allowing the resources of the VPC subnet on the platform to directly communicate with hosts on the internal network of the third-party platform, and also provide connection services between different VPC networks of the platform.

### Product Overview
IPSec VPN is a tunneling technology encrypted by the IPSec protocol, a framework of security standards defined by the Internet Engineering Task Force (IETF), which provides a secure tunnel for two private networks on the Internet and secures the connection through encryption. For IPSec, see RFC2409 (IKE—Internet Key Exchange Internet Key Exchange Protocol) and RFC4301 (IPSec Architecture).

Cloud platform IPSecVPN service is an Internet-based network connection service, which uses IPSec (Internet Protocol Security) secure encrypted channel to realize the secure and reliable connection between enterprise data centers, office networks and platform VPC private networks, and can also use VPN gateways to establish encrypted intranet connections between VPCs. The gateway service is a high-availability architecture that can be tolerated by disasters, supports users to select multiple encryption and authentication algorithms, and provides VPN connection health detection and connection logs to ensure the reliability, security, and management of tunnel connections.

With IPSecVPN, you can connect VPC private networks in on-premises data centers, enterprise branch offices, and private cloud platforms over encrypted channels, or you can use them for encrypted connections between different VPCs. Peer devices or systems only need IKEv1 or IKEv2 that support IPSec to interconnect with the platform's VPN gateway, such as a generic network device or a server configured with IPSecVPN.

### Logical Architecture
VPN gateway IPSecVPN service consists of three parts: VPN gateway, peer gateway, and VPN tunnel connection.

![1](/assets/images/product-functional-architecture-16.jpeg)

- VPN gateway <br/>
The platform-side VPC network establishes an egress gateway for IPSecVPN connection, and connects with the IPSecVPN of the peer gateway by associating VPC and external IP addresses to establish secure and reliable encrypted network communication between the platform's private network and external networks (such as IDC, public cloud, and private cloud).
- Peer gateway <br/>
The public IP address of the IPSecVPN gateway running on the external network, that is, the IP address of the gateway that is tunneled to the VPN gateway of the private cloud platform, and the gateway address that supports NAT forwarding.
- VPN tunnel <br/>
Connect the encrypted tunnel of the VPN gateway and the peer gateway, and combine the corresponding encryption authentication algorithms and policies to establish a tunnel connection for encrypted communication between the VPC and the external VPC.

A VPN gateway has and must be associated with one VPC network and one public IP address, which corresponds to the peer gateway and connects through the VPN tunnel. IPSecVPN supports point-to-multipoint connectivity, which enables VPN gateways and peer gateways to have one-to-one or one-to-many connections, that is, a VPN gateway can tunnel with multiple peer gateways at the same time. VPN tunnels allow encrypted communication between multiple VPC subnets of the platform and multiple CIDR blocks of the peer network through the tunnel, and the CIDR blocks of the platform VPC subnet cannot overlap with the network of the peer network (the overlap of the local and peer subnets will affect the normal communication of the network).

![1](/assets/images/product-functional-architecture-17.jpg)

As shown in the example shown in the preceding figure, the VPC network in the cloud platform already has two subnets, subnet1 (`192.168.1.0/24`) and subnet2 (`192.168.2.0/24`). There are two intranet CIDR blocks in the remote IDC data center, subnet3 (`192.168.3.0/24`) and subnet4 (`192.168.4.0/24`).
- The VPN gateway of the private cloud platform is bound to a VPC subnet and uses the public IP address as the network egress and the peer gateway of the remote data center.
- The gateway of the remote data center platform is bound to the data center subnet and uses another public IP address as the network egress and the peer gateway of the private cloud platform.
- The VPN gateways at both ends establish IPSecVPN tunnels separately, use the same pre-shared key and encryption authentication policy, and establish a VPN connection tunnel after the first IKE authentication and the second stage IPSec authentication.
- The subnets of the two end networks communicate with the subnets of the peer network through VPN tunnels to open up the internal networks across data centers and cloud platforms to build a hybrid cloud environment.

IPSecVPN tunnels are built and run in Internet networks, and the bandwidth, network congestion, and network jitter of the public network directly affect the quality of VPN network communication.

### VPN tunnel establishment
When establishing an IPSecVPN secure tunnel, you need to establish a SA (Security Association Security Alliance) between the two gateways. SA is the foundation of IPSec and is the agreement between communication gateways on connection conditions, such as network authentication protocol (AH, ESP), protocol encapsulation mode, encryption algorithm (DES, 3DES and AES), authentication algorithm, negotiation mode (main mode and brutal mode), shared secret and key lifetime, etc. The establishment of an SA security consortium requires that the same conditions be agreed upon and configured on both gateways to ensure that the SA can secure two-way traffic traffic on both gateways.

Standard IPSecVPN establishes SAs in both manual configuration and IKE auto-negotiation, and the private cloud platform VPN gateway service uses the IKE protocol to establish SAs. Built on a framework defined by `ISAKMP` (Internet Security Association and Key Management Protocol), the IKE protocol has a set of self-protection mechanisms that securely authenticate identities, exchange and distribute keys over insecure networks, and provide IPSec with auto-negotiation of exchange keys and establishment of SA services.
- Identity authentication: Support pre-shared-key authentication to confirm the identity of both ends of the communication, and encrypt and transmit the identity data after the key is generated to achieve security protection of identity data.
- Exchange and key distribution: The DH (Diffie-Hellman) algorithm is a public-key algorithm in which two ends of the communication calculate a shared key by exchanging some data without transmitting the key.

IKE performs key negotiation and establishes an SA for IPSec in two phases:
- The first stage: the two ends of the communication establish a channel that has passed identity authentication and security protection with each other, that is, establish an IKE SA, the role is to verify each other's identities between the two ends, and negotiate the IKE SA, protecting the IPSec SA negotiation process in the second stage. IKE V1 and V2 versions are supported, with V1 supporting two IKE switching methods, Main Mode and Aggressive Mode.
 - The second stage: use the IKE SA established in the first stage to negotiate security services for IPSec, that is, negotiate specific SAs for IPSec, and establish IPSec SAs for the secure transmission of final IP data.

IKE negotiates the establishment of an SA for IPSec and gives the established parameters and generated keys to IPSec, which uses the SA established by the IKE protocol to encrypt or authenticate the final IP packet. Through the IKE protocol, IPSecVPN can provide dynamic authentication and key distribution between ends, and reduce the complexity of manually configuring parameters by automatically establishing IPSec parameters. At the same time, because each SA in the IKE protocol needs to run the DH exchange process, it can effectively ensure that the keys used by each SA are not related to each other, and increase the security of the VPN tunnel.

After the VPN tunnel is successfully connected, it automatically delivers routes to the peer subnet for the local subnet associated with the VPC, so that requests from the local subnet to access the remote VPC are forwarded through the VPN gateway and tunnel, completing the opening of the entire link.

### VPN Tunnel Parameters
IPSecVPN tunnel SA negotiation requires configuration of appropriate parameter information, including tunnel basic information, pre-shared key, IKE policy, and IPSec policy configuration information. During the establishment process, the VPN at both ends needs to ensure that the pre-shared key, IKE policy, and IPSec policy configuration are consistent, the IKE policy specifies the encryption and authentication algorithm of the IPSec tunnel during the negotiation phase, and the IPSec policy specifies the protocol and encryption authentication algorithm used by IPSec in the data transmission phase. The specific parameter information is shown in the following table:

(1) Basic information
- Name/Comment: A name and comment for the VPN tunnel connection.
VPN gateway: The VPN gateway attached to the VPN tunnel, that is, the VPN gateway where the tunnel runs on the cloud platform.
- Peer gateway: The peer gateway attached to the VPN tunnel, that is, the Internet egress IP address of the peer gateway, such as the VPN gateway of the IDC data center.
- Local CIDR block: The subnet in the VPC network where the VPN gateway resides that needs to communicate with the peer network (such as IDC data center), such as `192.168.1.0/24`. The local CIDR block is used for second-stage negotiation and cannot overlap with the peer CIDR block.
- Peer CIDR Block: The subnet in the IDC data center or third-party cloud platform that needs to communicate with the local CIDR block VPN, such as `192.168.2.0/24`. The peer CIDR block is used for second-stage negotiation and cannot overlap with the local CIDR block.

(2) Pre-shared key
- Pre Shared Key: The secret key of the IPSecVPN connection, which is used for the negotiation of the VPN connection, and the keys of the local end and the peer need to be consistent during the VPN connection negotiation process.

(3) IKE policy
- Version: The version of the IKE key exchange protocol, which supports V1 and V2. V2 simplifies the negotiation process of SA and is more suitable for multi-segment scenarios, so we recommend that you choose V2.
- Authentication algorithm: Provides authentication for packets during IKE negotiation, and supports md5, sha1, and sha2-256 authentication algorithms.
- Encryption algorithm: Provides encryption protection for packets during IKE negotiation, and supports four encryption algorithms: 3des, AES128, AES192, and AES256.
- Negotiation mode: IKE v1's negotiation mode, which supports main mode and aggressive mode.
  - The main mode goes through three two-way exchange phases (6 messages) of SA exchange, key exchange, and authentication during IKE negotiation, while brute mode only needs to go through two exchange phases (3 messages) of SA generation/key exchange and authentication.
  - Since brute mode key exchange together with identity authentication cannot provide identity protection, the negotiation process of main mode is more secure, and the security of information transmission is consistent after successful negotiation.
  - The main mode is suitable for scenarios where the public IP addresses of the devices on both ends are fixed, and brute mode is suitable for scenarios where NAT traversal is required and the IP address is not fixed.
- DH group: Specifies the Diffie-Hellman algorithm used when IKE exchanges keys, the security and exchange time of key exchange increases with the expansion of DH group, supporting 1, 2, 5, 14, 24.
  - 1: DH group using the 768-bit Modular Exponential (MODP) algorithm.
  - 2: DH group with 1024-bit MODP algorithm.
  - 5: DH group with 1536-bit MODP algorithm.
  - 14: DH group with 2048-bit MODP algorithm.
  - 24: 2048-bit MODP algorithm with 256-bit prime-order subgroups DH group.
- Self-Identity: The identity of the VPN gateway, used for IKE Phase 1 negotiation. Supports IP addresses and FQDNs (Full Domain Names).
- Peer ID: The identity of the peer gateway for IKE Phase 1 negotiation. Supports IP addresses and FQDNs
- Lifetime: The time-to-live of the first stage SA, after which the SA is renegotiated, such as 86400 seconds.

(4) IPSec policy

- Secure transmission protocol: IPSec supports AH and ESP security protocols, AH only supports data authentication protection, ESP supports authentication and encryption, we recommend using ESP protocol.
- IPSec authentication algorithm: The authentication protection function provided for the second stage of user data, supporting md5 and sha1 authentication algorithms.
- IPSec encryption algorithm: Provides encryption protection for second-stage user data, supports 3des, aes128, aes192, and aes256 encryption algorithms, and is not available when using the AH security protocol.
- PFS DH Group: The PFS (Perfect Forward Secrecy) feature is a security feature that means that one key is cracked without affecting the security of other keys. PFS features the Diffie-Hellman key exchange algorithm negotiated in the second phase, supported DH groups that support 1, 2, 5, 14, 24 and Disable, and Disable for clients that do not support PFS.
- Lifetime: The time-to-live of the second-stage SA, after which the SA is renegotiated, such as 86400 seconds.

### Application scenarios
VPN Gateway IPSecVPN service is an Internet-based network connection service that enables secure and reliable connection between enterprise data centers, office networks and platform VPCs through IPSec secure encrypted tunnels, and users can also use VPN gateways to establish encrypted intranet connections between VPCs. The gateway service is a high-availability architecture that can be tolerated, supports users to select multiple encryption and authentication algorithms, and provides VPN connection health detection and connection logs, which can meet different application scenarios.
- VPC connection to on-premises data center: IPSecVPN service connects private network hosts in on-premises data centers with virtual resources of VPC networks to build a hybrid cloud service model.
- VPC to public cloud VPC connection: Connect the virtual resources of third-party public cloud VPC private network and private cloud VPC network through IPSecVPN service to build a multi-cloud hybrid service model.
- Connection of VPC to third-party private private network: Connect the VPC private network of the third-party private cloud with the virtual resources of the SCloudStack VPC network through IPSecVPN to build a multi-cloud hybrid service model.
- VPC-to-VPC connection: Connect a VPC with another VPC network through the IPSecVPN service to achieve VPC connectivity.

## Usage Process
Before using the VPN gateway IPSec service, you need to clarify the scenario and deploy the VPN and connections according to different scenarios:
- Tenants create local VPC networks and subnets as needed, and deploy virtual machines in the subnets.
- Tenants specify parameters such as the VPC network, public IP address, security group, and other parameters where the VPN gateway resides to create a highly available VPN gateway.
- The tenant creates a peer gateway based on the IP address of the peer gateway.
- Tenants deploy IPSec VPN tunnels based on their needs by specifying VPN tunnel basic parameters, pre-shared keys, IKE policies, and IPSec policies.
- The user configures the VPN for the remote gateway device with consistent VPN tunnel parameters. (Remote gateway device refers to the VPN routing device of the IDC data center, the VPN gateway different from the local VPC, or the VPN gateway of a third-party cloud platform.)
- Configure routes that require communication hosts in VPC according to your needs, and you do not need to configure routes if routes can be automatically delivered.
- Test network connectivity, such as virtual machines in the local VPC subnet ping IP addresses in the remote VPC network to verify that the communication is normal.

Typically, the IKE protocol uses UDP's ports 500 and 4500 for communication, and IPSec's AH and ESP protocols use protocols 51 or 50 to work, respectively, so in order to ensure the normal operation of IKE and IPSec, you need to ensure that the gateway device or firewall applying IKE and IPSec configuration has opened traffic to the above ports and protocols.

IPSecVPN service is an Internet-based encrypted communication service, and you need to confirm that both gateways have fixed or NAT Internet IP addresses before using IPSecVPN.

## VPN Gateway
Users create a VPN gateway based on network planning requirements to establish IPSecVPN tunnel connection with the peer gateway and provide a secure and encrypted VPN exclusive network tunnel.
### Creating a VPN Gateway
When you create a VPN gateway, you need to specify the model, VPC network, subnet, public IP address, security group, VPN gateway name and remarks, and you can enter the VPN Gateway resource console through IPSecVPN in the navigation bar, and enter the creation wizard page through Create VPN Gateway, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-107.png)

(1) Select and configure the VPN gateway base configuration and network setup information:
- Model: The cluster type of the host where the VPN gateway instance is located, customized by the platform administrator, such as x86 models and ARM models, and the instance created by the ARM model is an ARM version of IPSecVPN gateway instance, which is suitable for domestic chips, servers, and operating systems.
- Name/Comment: The name and remarks of the VPN gateway.
- VPC network: The VPC network served by the VPN gateway, that is, the VPN gateway provides IPSecVPN communication services only to the resources in the selected VPC, and only supports adding subnets of the same VPC network to the local gateway of the associated tunnel.
- Subnet: The subnet where the VPN gateway instance is located, it is generally recommended to choose a subnet with a sufficient number of available IPs.
- Internet IP address: The public IP address used by the VPN gateway, that is, the local gateway address of the peer gateway to establish the IPSecVPN tunnel, and only the external IP address bound to the same data center and with a default route is supported.

(2)After selecting and configuring the above information, you can select the purchase quantity and payment method, confirm the order amount, and click "Buy Now" to create the VPN gateway:
- Purchase quantity: Create VPN gateway instances in batches according to the selected configuration and parameters, and only one VPN gateway instance can be created at a time.
- Payment method: Select the billing method of the VPN gateway, which supports three modes: timely, annual, and monthly, and you can choose the appropriate payment method according to your needs.
- Total cost: The user selects the VPN gateway resource to display the cost according to the payment method.

After confirming the order, click Buy Now, after clicking Buy Now, you will return to the VPN gateway resource list page, where you can view the VPN gateway creation process, usually displaying the status of "Creating" first, and converting to "Running" after successful creation.

Allows you to create multiple VPN gateways under a VPC and tunnel subnets under a VPC with different peer gateways, so as to achieve communication scenarios in which different subnets under the same VPC and different subnets on the peer side are tunneled.

### Viewing VPN Gateways
Go to the VPN Gateway resource console in the navigation pane to view the list of VPN gateway resources, and you can go to the details page by the name and ID on the list to view the overview and monitoring information of the VPN gateway.

#### List of VPN gateways
The VPN gateway list can view the resource information of all VPN gateways under the current account, including name, resource ID, VPC, subnet, public IP address, number of tunnels, creation time, expiration time, billing method, status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-108.png)

- Name/ID: The name and globally unique identifier of the VPN gateway.
- VPC network: The VPC network served by the VPN gateway, that is, the VPN gateway provides IPSecVPN communication services only to the resources in the selected VPC, and only supports adding subnets of the same VPC network to the local gateway of the associated tunnel.
- Subnet: The subnet where the VPN gateway instance is located.
- Internet IP: The public IP address used by the VPN gateway, that is, the local gateway address of the peer gateway to establish the IPSecVPN tunnel. When tunneling the current gateway with a remote data center or cloud platform, you must specify the IP address or the address after SNAT as the peer gateway IP address.
- Number of tunnels: The number of tunnels that have been created on the current VPN gateway.
- Status: The running status of the VPN gateway, including creating, running, and deleting.

The action items on the list refer to the deletion operations of a single VPN gateway instance, and you can search and filter the list of Load Balancing resources through the search box, supporting approximate search .

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all VPN gateway resources owned by the current user as an Excel table. You can also delete VPN gateways in bulk.

#### VPN Gateway Details
On the VPN gateway resource list, click Name to go to the overview page to view the details of the current VPN gateway instance, as shown on the overview page:

![1](/assets/images/user-guide/user-guide-109.png)

(1) Basic information

For the basic information of the VPN gateway, including name, ID, VPC network, subnet, Internet IP, number of tunnels, billing method, status, creation time, expiration time, and alarm template information, you can click the button on the right side of the alarm template to modify the alarm template associated with the VPN gateway.

(2) Monitoring information

Monitoring charts and information related to VPN gateway instances, including NIC in/out bandwidth, NIC in/out packet volume, and VPN gateway outbound bandwidth usage, supports viewing monitoring data for 1 hour, 6 hours, 12 hours, 1 day, and custom time.

### Modifying the Name and Remarks
Modify the name and comment of the VPN gateway resource to perform operations in any state. You can modify it by clicking the Edit button to the right of each VPN gateway name on the VPN gateway resource list page.

### Modifying the Alarm Template
Modify the alarm template to configure the monitoring data of the VPN gateway, and trigger alarms when the indicators and thresholds defined by the alarm template fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure the network communication between the VPN gateway and its services.

You can modify the alarm template through the action items on the VPN gateway details overview page, and select the new VPN gateway alarm template in the Modify Alarm Template wizard to modify it.

### Deleting the VPN Gateway
You can delete unwanted VPN gateway instances through the console or APIs, and automatically unbind the bound public IP addresses when you delete them. You can only delete a gateway that is not associated with any VPN tunnel, and you need to delete the tunnel connection associated with the VPN gateway before deleting it.

After the VPN gateway is deleted, it is directly destroyed, so make sure that the VPN gateway has no traffic access requests before deletion, otherwise it may affect the business access.

### VPN Gateway Renewal
You can manually renew the VPN gateway, and the renewal operation applies only to the resource itself, and does not renew additional resources associated with the resource, such as bound public IP resources. After the additional associated resources expire, they are automatically unbound from the VPN gateway, and you need to renew the relevant resources in a timely manner to ensure normal service usage.

When the VPN gateway is renewed, the renewal period will be charged according to the renewal period, which matches the billing method of the resource, and if the billing method of the VPN gateway is Hours, the renewal period can be selected from 1 to 24 hours; If the billing method of the VPN gateway is Monthly, you can choose from January to November for the renewal period. If the billing method of the VPN gateway is Annual, the renewal period is 1 to 5 years.

## Peer Gateways
The public IP address of the IPSecVPN gateway running on the external network, that is, the gateway that tunnels to the VPN gateway of the private cloud platform. The peer gateway can be thought of as the VPN gateway IP address of a third-party private cloud platform, IDC data center, and public cloud platform that establishes a VPN connection to the current platform.

### Creating a Peer Gateway
To create a peer gateway, you need to specify the public IP address of the peer gateway. Since the IPSecVPN service is an Internet-based encrypted communication service, you need to confirm that both gateways have fixed or NAT Internet IP addresses before using it. After entering the IP address being determined, the user can create a peer gateway that is used to create a tunnel connection.

![1](/assets/images/user-guide/user-guide-110.png)

If the remote VPN gateway uses a private network address, you must provide a fixed public IP address after the private network address is SNAT. If the address after the SNAT of the remote network is a non-fixed public IP address, such as an IP address pool, the peer gateway is entered as `0.0.0.0`, which means that a tunnel connection is established with any peer gateway IP address, and the connection can be established when the authentication algorithm, key, local subnet and peer subnet are consistent, so that the two end networks can transmit NAT through IPSecVPN communication.

### Viewing Peer Gateways
In the VPN Gateway resource console, you can switch to the peer gateway to view the resource information of all peer gateways under the current account, including the name, ID, public IP address, number of tunnels, creation time, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-111.png)

- Public IP address: refers to the public IP address of the peer gateway, specify that the tunnel created by the peer gateway will initiate VPN connection requests with this IP address as the peer IP address, and ensure that the IP address is the IP address of the remote VPN gateway.
- Number of tunnels: The number of tunnels that have been created on the current peer gateway.

The operation items on the list refer to the deletion operations of a single peer gateway instance, and you can search and filter the peer gateway resource list through the search box, supporting approximate search .

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all peer gateway resources owned by the current user as an Excel table. You can also delete the peer gateway in batches.

### Modifying the Name and Remarks
Modify the name and remarks of the peer gateway resource to perform operations in any state. You can modify it by clicking the Edit button to the right of each peer gateway name on the peer gateway resource list page.

### Deleting the Peer Gateway
You can delete unwanted peer gateway instances through the console or API, only you can delete peer gateways that are not associated with any VPN tunnels, and you need to delete the tunnel connection associated with the peer gateway before deletion.

![1](/assets/images/user-guide/user-guide-112.png)

After the peer gateway is deleted, it is directly destroyed, so make sure that the peer gateway has no traffic access requests before deletion, otherwise the service access may be affected.

## VPN tunnels
Connect the encrypted tunnel of the VPN gateway and the peer gateway, and combine the corresponding encryption authentication algorithms and policies to establish a tunnel connection for encrypted communication between the VPC and the external VPC, and a maximum of 20 VPN tunnels can be created for a single VPN gateway or peer gateway. When you use a VPN tunnel to connect to a remote gateway, you need to have some prerequisites:

- The gateway device of the remote data center or cloud platform needs to support the IKEv1 and IKEv2 versions of the protocol, such as Huawei, H3C, Shanshi, Shengfu, Cisco ASA, Juniper and other brands of routers or firewall settings, or IPSecVPN servers built using Linux systems.
- The remote data center, cloud gateway, or IPSecVPN server must be configured with a fixed or SNAT-translated public IP address.
- Remote data centers and local cloud platforms must open UDP 500, UDP 4500, UDP 50, and UDP 51 ports on the network to ensure normal communication between IKE and IPSec.
- The CIDR blocks that connect the cloud platform to the remote data center cannot be repeated or overlapped.

### VPN Tunnel Configuration Process
1. Create VPN gateways and peer gateways on the local cloud platform;
2. Create a VPN tunnel using the created VPN gateway and peer gateway on the local cloud platform;
3. Configure VPN gateways and related tunnel configurations in remote data centers, third-party cloud platforms, or public cloud platforms;
4. Wait for the gateways at both ends to negotiate with SA and establish a VPN connection;
5. Create a virtual machine with the same VPC as the VPN gateway and the designated local CIDR block on the local cloud platform, and ping a host in the remote network intranet to verify network connectivity.

### Creating a VPN Tunnel
User-created VPN tunnels are used to connect VPN gateways and peer gateways, and support for configuring IKE and IPSec policies. When you create a VPN tunnel, you need to specify the basic configuration, pre-shared key, IKE policy, and IPSec policy.

You can enter the VPN Tunnels tab through the IPSecVPN resource console to create a VPN tunnel, and the basic configuration of the Create Tunnel Wizard page is shown in the following figure:

![1](/assets/images/user-guide/user-guide-113.png)

(1) Select the basic configuration and pre-shared key information for the pro-configuration VPN tunnel:
- Name/Comment: A name and comment for the VPN tunnel connection.
- VPN gateway: The VPN gateway attached to the VPN tunnel, that is, the VPN gateway where the tunnel runs on the cloud platform, can also be referred to as the local gateway.
- Peer gateway: The peer gateway attached to the VPN tunnel, that is, the Internet egress IP address of the peer gateway, such as the VPN gateway of the IDC data center.
- Local CIDR block: The subnet in the VPC network where the VPN gateway resides that needs to communicate with the peer network (such as IDC data center), such as `192.168.1.0/24`. The local CIDR block is used for the second phase of negotiation, and cannot overlap with the peer CIDR block, and you can only select the subnet CIDR block to which the VPN gateway belongs to the VPC.
- Peer CIDR Block: The subnet in the IDC data center or third-party cloud platform that needs to communicate with the local CIDR block VPN, such as `192.168.2.0/24`.<br/> The peer CIDR block is used for the second stage of negotiation, cannot overlap with the local CIDR block, and supports the configuration of multiple CIDR segments, each CIDR segment is separated by a carriage return, and up to 20 peer-to-peer CIDR blocks are supported.
- Pre-shared key: The secret key of the IPSecVPN connection, which is used for the negotiation of the VPN connection, and during the VPN connection negotiation, ensure that the keys of the local and peer sides are consistent. Consists of a-z, A-Z, numbers, special characters, but cannot contain '?' and spaces, which are 128 characters long.

Note: When establishing tunnel configuration at the peer end, from the perspective of the peer gateway device, the local CIDR block and the peer CIDR block need to be swapped when configuring the CIDR segment.

(2) Configure the IKE policy for phase 1 and the IPSec policy for phase 2 of VPN tunnel connection negotiation as required. When establishing a connection, ensure that the IKE policy on both ends is consistent (local and peer identities are configured opposite on the peer side). Typically, you select the default value to create a tunnel, and you only need to use the same configuration parameters to connect the two tunnels through gateways at both ends when establishing the tunnel configuration on the peer side.

![1](/assets/images/user-guide/user-guide-114.png)

**IKE Policy:**

- Version: The version of the IKE key exchange protocol, which supports V1 and V2. V2 simplifies the negotiation process of SA and is more suitable for multi-segment scenarios, so we recommend that you choose V2. If the peer VPN device supports only V1, you must select the V1 version to create it.
- Authentication algorithm: Provides authentication for packets during IKE negotiation, supports md5, sha1, and sha2-256 authentication algorithms, and defaults to sha1.
- Encryption algorithm: Provides encryption protection for packets during IKE negotiation, supports four encryption algorithms: 3des, AES128, AES192, and AES256, and the default algorithm is AES128.
- Negotiation mode: IKE v1's negotiation mode, which supports main mode and aggressive mode.
  - The main mode goes through three two-way exchange phases (6 messages) of SA exchange, key exchange, and authentication during IKE negotiation, while brute mode only needs to go through two exchange phases (3 messages) of SA generation/key exchange and authentication.
  - Since brute mode key exchange together with identity authentication cannot provide identity protection, the negotiation process of main mode is more secure, and the security of information transmission is consistent after successful negotiation.
  - The main mode is suitable for scenarios where the public IP addresses of the devices on both ends are fixed, and brute mode is suitable for scenarios where NAT traversal is required and the IP address is not fixed.
- DH group: Specify the Diffie-Hellman algorithm used when IKE exchanges keys, the security and exchange time of key exchange increases with the expansion of DH group, support 1, 2, 5, 14, 24, the default value is 2, the higher the value, the higher the computing performance.
  - 1: DH group using the 768-bit Modular Exponential (MODP) algorithm.
  - 2: DH group with 1024-bit MODP algorithm.
  - 5: DH group with 1536-bit MODP algorithm.
  - 14: DH group with 2048-bit MODP algorithm.
  - 24: 2048-bit MODP algorithm with 256-bit prime-order subgroups DH group.
- Self-identification: The identity of the VPN gateway, which is used for IKE negotiation in the first stage, supports IP address and FQDN (full name domain name), and defaults to the external IP address of the VPN gateway.
- Peer ID: The identity of the peer gateway for IKE Phase 1 negotiation. Supports IP address and FQDN (full domain name), which defaults to the IP address of the peer gateway. If the peer is in NAT passthrough mode (the IP address of the peer gateway is 0.0.0.0), you need to revise the identification IP to the real peer gateway IP address, which is the local gateway IP address configured in the peer VPN gateway device.
- Lifetime: The survival time of the first stage SA, after the lifetime period is exceeded, the SA will be renegotiated, the value range is 3600~86400, and the default value is 86400 seconds.

Note: When establishing a tunnel configuration at the peer end, from the perspective of the peer gateway device, the local identity and the peer ID need to be switched when configuring the identity.

**IPSec policy**

- Secure transmission protocol: IPSec supports AH and ESP security protocols, AH only supports data authentication protection, ESP supports authentication and encryption, we recommend using ESP protocol.
- IPSec authentication algorithm: The authentication protection function provided for the second stage of user data supports md5 and sha1 algorithms, and the default is sha1.
- IPSec encryption algorithm: The encryption protection function provided for the second stage of user data, supports four encryption algorithms: 3des, AES128, AES192, and AES256, which is AES128 by default and is not available when using AH security protocol.
- PFS DH Group: The PFS (Perfect Forward Secrecy) feature is a security feature that means that one key is cracked without affecting the security of other keys. PFS features the Diffie-Hellman key exchange algorithm negotiated in the second phase, supported DH groups support 1, 2, 5, 14, 24 and Disable, the default value is 2, Disable is suitable for clients that do not support PFS.
- Lifetime: The survival time of the second-stage SA, after the lifetime period is exceeded, the SA will be renegotiated, the value range is 3600~86400, and the default value is 86400 seconds.


(3) After selecting and configuring the above information, click Create Now to create an IPSecVPN tunnel, and return to the tunnel list page to view the tunnel creation process and VPN connection process.

During the creation process, the resource status of the VPN tunnel is Creating, and after the tunnel is successfully created, the resource status flow changes to Running. After successful creation, the platform will connect VPN with the peer gateway according to the parameters configured in the tunnel, that is, negotiate the stage of IPSecVPN two SAs, the connection status of the VPN tunnel is "connected", and after the connection status is changed to "connected", it proves that the VPN tunnel has been successfully connected. If the VPN tunnel connection fails, it will be retried, and if 3 retries still fail, it will show that Phase 1 failed or Phase 2 failed, and the system will retry every 12 seconds after the connection failure.

When the connection status of the VPN tunnel is connected, the platform automatically delivers network routes to the peer CIDR block in the associated local VPC subnet including the virtual machine in the tunnel configuration to ensure network connectivity.

### Viewing VPN Tunnels
After the VPN tunnel is created, you can enter the VPN Gateway console through the navigation bar, switch to the VPN Tunnels tab to view the tunnel resource list, and enter the details page by the name and ID on the list to view the detailed configuration information and monitoring information of the VPN tunnel.
#### VPN tunnel list
VPN tunnel list can view the resource list information of all tunnels under the current account, including name, resource ID, VPN gateway, peer gateway, creation time, resource status, connection status, and operation items, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-115.png)

- Name/ID: The name and globally unique identifier of the VPN tunnel.
- VPN gateway: The name and IP address of the VPN gateway associated with the VPN tunnel.
- Peer gateway: The name and IP address of the peer gateway associated with the VPN tunnel.
- Creation Time: The time the VPN tunnel was created.
- Resource status: The resource health status of the VPN tunnel, including creating, running, and deleting.
Connection Status: The connection status of the VPN tunnel, including Connected, Connected, Phase 1 Failed, and Stage 2 Failed.
  - Phase 1 failure means that the VPN tunnel fails to negotiate the first IKE SA, and you need to check whether the VPN gateway IP, peer gateway IP, peer CIDR block, local CIDR block, pre-shared key, and IKE configuration parameters of the two end tunnels are consistent.
  - Phase 2 failure usually means that the IKE SA negotiation of Phase 1 has been successful, but the IPSec SA negotiation of Phase 1 fails, and you need to check whether the IPSec configuration parameters of the second stage of the tunnel at both ends are consistent.

The operation items on the list refer to operations on a single tunnel, including downloading tunnel configurations and deleting tunnel configurations, and you can search and filter the tunnel resource list through the search box, supporting approximate search .

In order to facilitate the statistics and maintenance of resources by tenants, the platform supports downloading the list of all tunnel resources owned by the current user as an Excel table. You can also delete tunnels in batches.

#### VPN Tunnel Details
On the VPN tunnel resource list, click Name or ID to go to the overview page to view the detailed configuration and monitoring information of the current VPN tunnel:

![1](/assets/images/user-guide/user-guide-116.png)

(1) Basic information

The basic information of the VPN tunnel, including resource ID, name, VPN gateway, peer gateway, resource status, connection status, creation time, and alarm template information, can be modified by clicking the button on the right side of the alarm template to modify the alarm template associated with the load balancer.

(2) Network segment information

The configured CIDR block matching information of the VPN tunnel, including the local CIDR block and the peer CIDR block, only shows that the CIDR segment can communicate through the connection established by the VPN tunnel. You can modify the local and peer CIDR blocks by clicking the Edit button, see Modifying the Tunnel CIDR Block Policy.

(3) IKE configuration information

IKE configuration information of the VPN tunnel, including pre-shared key, IKE version, negotiation mode, IKE authentication algorithm, IKE encryption algorithm, DH group, local identity, peer identity, and lifetime information. The IKE configuration information can be modified via the Edit button, as described in Modifying IKE Policies .

(4) IPSec configuration information

IPSec configuration information for the VPN tunnel, including secure transport protocol, IPSec authentication algorithm, IPSec encryption algorithm, PFS DH group, and lifetime information. The IKE configuration information can be modified via the Edit button, see Modifying IPSec Policies .

(5) Monitoring information

Monitoring charts and information related to Load Balancing instances, including tunnel ingress bandwidth, tunnel out/out packet volume, and tunnel health status, can view monitoring data for 1 hour, 6 hours, 12 hours, 1 day, and custom times.

You can also view the connection status of the tunnel directly through the monitoring chart of tunnel health, if the data in the chart is all 1 for connected, and 0 for the country that is connected or failed.

#### Downloading the VPN Tunnel Configuration
You can download tunnel configuration information to your local computer, and refer to the downloaded configuration file to configure VPN tunnels in remote data centers or cloud platforms. The downloaded configuration file is in conf format and includes the entire VPN tunnel VPN gateway, peer gateway, local network segment, peer gateway, pre-shared key, and policy-related configuration information for IKE and IPSec.

You can download the configuration file through the [Download Tunnel Configuration] button on the tunnel list, and after clicking it, a .conf file will be downloaded locally, and the relevant configurations of the current tunnel will be listed in the file:
```
conn ipsvpn_tunnel-MQcK-cVMR
    keyingtries=3
    authby=psk
    auto=start
    type=tunnel
    pfs=no

keyexchange=ikev2

# IPSecProtocol

esp=aes128-sha1-modp1024!

ike=aes128-sha1-modp1024!
    ikelifetime=86400s
    lifetime=86400s
    left=192.168.93.46
    leftid=192.168.93.46
    leftsubnet=172.16.1.0/24
    leftnexthop=%defaultroute
    right=106.75.234.78
    rightid=106.75.234.78
    rightsubnet=10.0.192.0/20
    rightnexthop=%defaultroute
    dpdaction=hold
    dpddelay=8s
    dpdtimeout=13s
```
- keyexchange represents the version of IKE used by the current tunnel as V2.
- IKE and ESP represent authentication and encryption algorithms for IKE policies and IPSec policies, respectively.
- left and right represent the public IP addresses of the VPN gateway and peer gateway, respectively, and the VPN gateway in the example configuration file uses a private network address to communicate with the peer gateway 106.75.234.78 through SNAT.
- leftid and rightid represent the home and peer identities in the IKE configuration item.
- leftsubnet and rightsubnet represent the local network segment and the peer network segment, respectively.

If you need to configure VPN for remote data centers or cloud platforms based on this profile, from the perspective of peer gateway devices, local gateway and local network block refer to their own gateway devices and network segments, and peer gateway and peer network block refer to the VPN gateway and VPC subnet of the private platform, so when configuring the peer gateway, you need to adjust the VPN gateway & peer gateway, local network block & peer gateway, and local identity & peer identifier.

### Modifying the Tunnel CIDR Segment Policy
You can modify the CIDR block policy of the tunnel according to your business requirements, such as adding the local CIDR block or reducing the CIDR block of the peer endpoint. You can modify the CIDR block policy by using the CIDR block information on the tunnel details overview page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-117.png)

You can customize and modify the local CIDR block and the peer CIDR block, and the local CIDR block and the peer CIDR block are not allowed to be duplicated or overlapped.

- The local CIDR block allows you to select only the subnet CIDR block contained in the VPC to which the VPN gateway belongs.
- Supports entering multiple peer CIDR segments, one CIDR segment per line, which must conform to the input specifications of the CIDR segment.
- Up to 20 peer CIDR segments are supported within the same tunnel.

After confirming the modification, the platform will automatically reconnect the tunnel, that is, the connection status of the tunnel is Connected, and when the status flows to Connected, it means that the configuration modification is successful, and the platform will automatically send the route to the virtual machine in the associated subnet with the destination of the peer CIDR block to the virtual machine in the associated subnet, so that the virtual machine can communicate with the peer CIDR block.

Under the same VPC, only one rule is allowed for the local CIDR block and the peer CIDR block, that is, the tunnel associated with two VPN CIDR blocks of the same VPC cannot have the same CIDR block policy, otherwise routing delivery may be affected and network interruption may occur.

### Modifying the IKE Policy Configuration
When the network configuration changes or the tunnel connection status fails with Phase 1, you can reconnect by verifying and modifying the IKE policy configuration. The IKE policy can be modified through IKE on the tunnel details overview page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-118.png)

You can modify all configuration parameters of the pre-shared key and IKE policy, and the pre-shared key, IKE version, negotiation mode (IKEv1), authentication algorithm, encryption algorithm, DH group, local identity, and peer ID of the tunnel at both ends must be consistent, and the lifetime cycle can be inconsistent.
If the peer gateway uses 0.0.0.0, it is usually recommended to configure the private IP address of the peer gateway, as long as the identities configured at both ends are consistent, you can connect normally.

After confirming the modification, the platform automatically reconnects the tunnel, that is, the connection status of the tunnel is Connected, and the status flows to Connected, indicating that the IKE policy has been successfully modified.

### Modifying the IPSec Policy Configuration
When the network configuration changes or the tunnel connection status is Phase 2 Failure, you can reconnect by verifying and modifying the IPSec policy configuration. You can modify IPSec policies through IPSec on the tunnel details overview page, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-119.png)

All configuration parameters of the IPSec policy can be modified, and the secure transport protocol, authentication algorithm, encryption algorithm, and PFS DH group of the tunnel at both ends must be consistent, and the lifetime can be inconsistent.

After the modification is confirmed, the platform automatically reconnects the tunnel, that is, the connection status of the tunnel is Connected, and the status is transferred to Connected, indicating that the IPSec policy has been successfully modified.

### Modifying the Name and Remarks
Modify the name and comment of the VPN tunnel resource to operate in any state. You can modify this by clicking the Edit button to the right of each VPN tunnel name on the VPN tunnel resource list page.

### Modifying the Alarm Template
Modifying the alarm template configures the monitoring data of the VPN tunnel, and through the indicators and thresholds defined in the alarm template, you can trigger alarms when the indicators related to the VPN tunnel fail or exceed the indicator thresholds, notify relevant personnel to handle the fault, and ensure the network communication between the VPN gateway and services.

You can modify the alarm template through the action items on the VPN tunnel details overview page, and select a new VPN tunnel alarm template in the Modify Alarm Template wizard to modify it.

### Deleting the VPN Tunnel
Users can delete unwanted VPN tunnels through the console or API, and the VPN tunnel will be automatically broken after deletion, and the routes configured in the local subnet virtual machine will be cleared.

![1](/assets/images/user-guide/user-guide-120.png)

After the VPN tunnel is deleted, it is directly destroyed, so make sure that there are no traffic access requests in the VPN tunnel before deletion, otherwise service access may be affected.

