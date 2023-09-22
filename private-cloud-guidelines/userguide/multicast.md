---
layout: default
title: Multicast
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/multicast/
nav_order: 29
---
## 29. Multicast

## 29.1 Introduction to multicast

### 29.1.1 Multicast Overview
As a communication method alongside Unicast and Broadcast, Multicast technology can effectively solve the problem of single-point transmitting and multi-point receiving, thus realizing efficient data transmission from point to multi-point in the network, which can save a lot of network bandwidth and reduce network load.

The multicast feature of the network can be used to conveniently provide some new value-added services, including online live streaming, network TV, tele-education, telemedicine, Internet radio, real-time video conferencing and other information service areas of the Internet.

Multicast is a one-to-many communication mode between hosts. Multicast is a technology that allows one or more multicast sources to send the same message to multiple receivers. A multicast source sends a message to a specific multicast address, which differs from a unicast address in that it does not belong to a specific host, but to a group of hosts. A multicast address represents a group to which all receivers that need to receive multicast packets join.

### 29.1.2 Multicast Group Overview

A set identified by an IP multicast address. Any user host (or other receiving device) that joins a multicast group becomes a member of that group and can identify and receive multicast data sent to that multicast group.

### 29.1.3 Multicast Source Overview
A multicast source can send data to multiple multicast groups at the same time, and multiple multicast sources can send messages to one multicast group at the same time. Multicast source usually does not need to join multicast group, the source DR is responsible for managing the registration of multicast source and the establishment of SPT (Shortest Path Tree).

### 29.1.4 Multicast Group Membership Overview
All hosts that join a multicast group become members of that multicast group. Membership in a multicast group is dynamic, and hosts can join or leave a multicast group at any time. Multicast group members can be widely distributed anywhere in the network.

### 29.1.5 Multicast Router Overview
A router or switch that supports Layer 3 multicast functionality. Multicast routers are capable of providing not only multicast routing functions, but also management functions for multicast group members on the end segments connected to users.

### 29.1.6 IPv4 Multicast Addresses
The IANA (Internet Assigned Numbers Authority) allocates class D address space for IPv4 multicast use. 32 bits of IPv4 addresses are available, and the highest 4 bits of class D addresses are 1110, so the address range is from 224.0.0.0 to 239.255.255.255, as shown in the following table See the following table for the classification and meaning:

  | Address Range | Meaning |
  -----------------------------------------------------|--------------------------------------------------------------------------- -------------------------------------------------|
  | 224.0.0.0 to 224.0.0.255 | Permanent group address. IANA is a routing protocol reserved IP address (also called reserved group address) used to identify a specific group of network devices for routing protocols, topology lookup, etc. It is not used for multicast forwarding. |IANA
  | 224.0.1.0 ~ 231.255.255.255 233.0.0.0 ~ 238.255.255.255 | ASM multicast address, valid network-wide. Note: Among them, 224.0.1.39 and 224.0.1.40 are reserved addresses and are not recommended to be used.                                           |
  | 232.0.0.0 to 232.255.255.255 | SSM multicast address by default, valid throughout the network.                                                                                      |
  | 239.0.0.0 ~ 239.255.255.255 | Local management group address, valid only within the local management domain. Reusing the same local management group address in different management domains will not cause conflicts.                                |# 29.1.7

### 29.1.7 Multicast Forwarding Mechanism
In the multicast model, the destination address field of an IP message is a multicast group address, and the multicast source transmits information to the group of hosts identified by this destination address. Therefore, multicast routers on the forwarding path often need to forward multicast messages received from one incoming interface to multiple outgoing interfaces in order to deliver multicast messages to the receiving sites in each direction.
* In order to ensure the transmission of multicast messages across the network, it is necessary to rely on unicast routing tables or routing tables provided separately for multicast use (e.g., MBGP routing tables) to guide forwarding;
* In order to handle the same multicast message received by the same device on different interfaces from different peers, an RPF (Reverse Path Forwarding) check on the incoming interface of a multicast message is required to decide whether to forward or discard the message. the RPF check mechanism is the basis for most multicast routing protocols for multicast forwarding.

## 29.2 Creating Multicast

Cloud platform users can create multicast by specifying VPC, multicast group IP, multicast group port, sender machine, receiver machine, project group, multicast name and other related basic information.

You can access the [Multicast] resource console through the navigation bar, and enter the wizard page through "**Create**" as shown below:

![createFS](/assets/images/userguide/createmulticast.png)

1. Select and configure the basic configuration for multicast:

* Rule Name/Remarks: the name and remarks of the multicast application, the name must be specified when applying
* Multicast group IP: Multicast IP only supports the IP within the 224.0.0.0/4 network segment
* Multicast group port: Multicast group port needs to be specified by user, the default range of port is 10000-64999
* vpc: VPC network must be selected when creating multicast, that is, select the network to join
* Sender: The sender and receiver are virtual machines under the same VPC, and the sender is a single virtual machine
* Receiver: The sender and receiver are virtual machines under the same VPC, and the receiver can be multiple virtual machines
* The number of multicast group rules that can be added by a single vpc is limited to 9, and the number of receivers of a multicast group is limited to 9

## 29.3 Multicast List

You can view the multicast resource list by entering the multicast console through the navigation bar.

Multicast list can view the list information of all multicast resources under the current account, including name, resource ID, status, VPC, multicast group IP, multicast group port, sender, receiver, project group, creation time, update time and operation items, as shown in the following figure:

![FSlist](/assets/images/userguide/multicastlist.png)

- Name: the name of the multicast resource;
- Resource ID: the resource ID of the multicast as a globally unique identifier;
- Status: the status of the multicast resource, including available, deleted, etc;
- Multicast group IP: need to be specified by the user, multicast IP only supports IPs in the 224.0.0.0/4 network segment
- Multicast group port: need to be specified by user, the default range of port is 10000-64999
- Sender: the same as the sending machine under vpc, can send multicast messages as multicast source
- Receiver: the same as the receiving machine under vpc, can receive multicast messages

## 29.4 Update Multicast Rules

Update the rules of multicast, add or delete the receiver's machine. It can be modified by clicking the "Update" button on the right side of the multicast list name, as follows:

![modifyFSname](/assets/images/userguide/modifymulticast.png)

## 29.5 Deleting Multicast

Users can delete multicast from the console, which supports batch delete operation for multicast. This can be done through "**delete**" in the multicast list action item, as shown in the following figure:

![FSrm](/assets/images/userguide/multicastrm.png)




