---
layout: default
title: Lab 4
parent: Tenant Administrator
grand_parent: Private Cloud Labs
permalink: /private-account-labs/tenant-admin/lab-4
nav_order: 4
---
# Lab 4: Connect 2 VPC using Network Interconnection
## Introduction
In the previous lab ([Lab 3: VPC and Subnet in advanced](https://docs.scloud.sg/private-account-labs/tenant-admin/lab-3)), we already learn how to create VPC, Subnet and control the communication between resources (VM) in a single VPC. 

In this lab, let extend this scenario to multiple VPC and see how SCloudStack can support the communication between resources (VM) in different VPC?

![1](/assets/images/labs/lab-47.png)

Following services will be used in this labs: 

- Network: VPC, Subnet, VPC Gateway, VPC Interconnection
- Compute: VM
- Storage: Cloud Disk 

## Prerequisite
- An account with Tenant Administrator role. 
- Your tenant need to be in Active status.

Please contact your System Administrator to get your credential.
## Guidelines
Before start, you need to access the SCloudStack console dashboard, if you don't know how to do that, visit [Lab 1: Getting Started with SCloudStack Console](https://docs.scloud.sg/private-account-labs/tenant-admin/lab-1).

### 1. Create your VPC

Step 1: From the Tenant Admin dashboard, open the left-side bar and click in VPC menu under Network group.

![1](/assets/images/labs/lab-7.png)

Step 2: Click **Create** to open the creation form.

![1](/assets/images/labs/lab-8.png)

Step 3: Enter VPC information as below image: 

![1](/assets/images/labs/lab-48.png)

- VPC Name: vpc-1
- VPC Network Segment: `10.0.1.0/24`

Leave other fields as default value and click **Confirm** to create VPC.

Create another VPC named `vpc-2` with below information:

- VPC Name: vpc-1
- VPC Network Segment: `10.0.2.0/24`

Step 4: Ensure that 2 VPC are created in the VPC list.

![1](/assets/images/labs/lab-49.png)

### 2. Create your Subnet

Step 1: From VPC list screen in above screen (step 4), click in `vpc-1` to navigate to VPC detail page. 

In the VPC detail page, the panel on the right-hand side is the Subnet management area. Click in the **Create** button to open Subnet creation form of current VPC.

Step 2: Enter Subnet information as below image:

![1](/assets/images/labs/lab-50.png)

- Subnet Name: subnet-1
- Subnet Network Segment: `10.0.1.0/26`

Leave other fields as default value and click **Confirm** to create Subnet.

Next, repeat above step in `vpc-2` to create a subnet in `vpc-2`.

![1](/assets/images/labs/lab-51.png)

- Subnet Name: subnet-2
- Subnet Network Segment: `10.0.2.0/26`

After these steps, review and make sure each VPC has 1 Subnet created.

### 3. Create your VM

Step 1: Go to side-bar menu, click in VM item under Compute group.

![1](/assets/images/labs/lab-14.png)

Step 2: In the VM creation page, enter information as the following images: 

![1](/assets/images/labs/lab-15.png)

Basic Information: 
- Boot source: Image

![1](/assets/images/labs/lab-16.png)

Basic Settings: 
- Cluster: leave as default value.
- Image: select Debian 10.12 as OS image.
- Specifications: 1C2G (1 vCore CPU, 2GB Ram).
- Disk: leave as default value (1 40GB system disk, 1 10GB data disk).

![1](/assets/images/labs/lab-52.png)

Network Settings: 
- VPC: vpc-1.
- Subnet: subnet-1
- Leave other fields as default value.

![1](/assets/images/labs/lab-53.png)

Manage Settings: 
- VM Name: vm-1
- Login Method: Password
- Administrator Password: click Randomly Generate to get a random password or set your own desired password (note the password somewhere, we will need it in the next step).

Step 3: Click Buy Now in the top-right panel to create your VM.

Step 4: Repeat above steps to create `vm-2` in `subnet-2`, configurations should be the same, except the subnet selection under Network Settings section & VM Name under Manage Settings section.

VM-2
- VM Name: vm-2
- VPC: vpc-2
- Subnet: subnet-2
- 
Step 5: Ensure 2 VM are available in the VM list.

![1](/assets/images/labs/lab-54.png)

After this step, we already set up an infrastructure with  2 VPC, each VPC has 1 subnet and 1 VM as the diagram in the introduction section.

### 4. Test the connection between VM in 2 VPC.
From the VM list above, we can grab the IP address of each VM as below (noted that IP address allocated randomly from the subnet IP range, your IP supposed to be different): 
- vm-1: `10.0.1.2`
- vm-2: `10.0.2.2`

VPC is the core of the virtualized network, providing internal network services for cloud platform virtual machines, including network broadcast domains, subnets (IP network segments), IP addresses, etc., which are the foundation of all NFV virtual network functions. A VPC is a container of subnets, which are absolutely isolated between different VPCs to ensure network isolation and security.

Use the console login to access vm-1 (click Console login button in the VM listing page), new browser tab will be opened and you will need to enter credential to access the VM (username will be `root` and the password is defined at VM creation step).

After successfully access to vm-1 via console login, ping vm-2 as below image.

![1](/assets/images/labs/lab-55.png)

As you can see, the network is unreachable as 2 VPC are isolated and have no connection by default.

### 5. Connect 2 VPC using Gateway & Interconnection.

In this step, lets connect vpc-1 and vpc-2 so that vm-1 in vpc-1 can communicate with vm-2 in vpc-2.

Step 1: Enable VPC Gateway for both VPC.

![1](/assets/images/labs/lab-49.png)

In the VPC list, click in the "Enable VPC Gateway" button and click Confirm in the popup.

![1](/assets/images/labs/lab-56.png)

Apply same steps for `vpc-2`, after enable Gateway for both VPC, the list should look like  below (noted the Disable VPC Gateway button).

![1](/assets/images/labs/lab-57.png)

Step 2: Connect 2 VPC using network interconnection.

Go to vpc-1 detail page by click in vpc name, and in the detail page, select Network Interconnection tab.

![1](/assets/images/labs/lab-58.png)

Click "Unify" button and enter information as below image: 

![1](/assets/images/labs/lab-59.png)

- Connect scenes: VPC interconnect
- Opposite VPC: vpc-2

Then click confirm to finish the process, you should be able to see vpc-2 information under vpc-1 network interconnection tab like below image:

![1](/assets/images/labs/lab-60.png)

Now let go to the vm-1 console login again and ping to vm-2, as we already connected 2 VPC, the ping command should work now: 

![1](/assets/images/labs/lab-61.png)

## Conclusion
Congrats, by far we already learn how to create multiple VPC and connect them using VPC Gateway and Network Interconnection. 
If you want to learn more on these topics, check out below documents:

- [VPC](https://docs.scloud.sg/private-cloud/user-guide/vpc/)




