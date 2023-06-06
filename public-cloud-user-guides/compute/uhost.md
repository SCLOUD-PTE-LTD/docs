---
layout: default
title: UHost
parent: Compute
grand_parent: Public Cloud User Guides
permalink: /public-cloud-user-guides/compute/uhost/
nav_order: 1
---
# UHost - Cloud Host
## 1. How to create a new VM via SCloud console?
Step 1: Open your web browser and navigate to SCloud public cloud console at: https://passport.scloud.sg/#login 

![1](/assets/images/public-cloud-user-guides/guide-1.png)

- Enter your username (email)
- Enter your password

Then click login button.

Step 2: In the console dashboard, select `All Products` menu, then select `UHost` under computing section.

![1](/assets/images/public-cloud-user-guides/guide-2.png)

Step 3: In the UHost Compute Instance management console, click `Create UHost` button.

![1](/assets/images/public-cloud-user-guides/guide-3.png)

Step 4: In the UHost creation form, select `Recommended Configuration` and configure as below image.

![1](/assets/images/public-cloud-user-guides/guide-4.png)

- Region Selection: Ho Chi Minh City
- Model Selection: Select model based on your need (Example: 1Core2Gb).
- Operating System: Select CentOS, Ubuntu or Windows
- Leave it as default value: 1Mb network bandwidth and firewall allowed HTTP, HTTPS, RDP, SSH.
- Login Setting: Set the password to access your VM
- Instance Name: `my-demo-instance`

Step 5: If you need to customize your instance in advanced, you can toggle to `Customized Configuration`

![1](/assets/images/public-cloud-user-guides/guide-5.png)

**Region selection**: select any region that matched your need (Choose a location close to your customers to reduce network latency).

![1](/assets/images/public-cloud-user-guides/guide-6.png)

**Model Configuration**: Select between `Outstanding`, `GPU`, `General` and `High frequency` model, you can also customize the VM CPU & Memory for different purposes.

![1](/assets/images/public-cloud-user-guides/guide-7.png)
**Operating System Selection**: Select your desired OS (CentOS, Ubuntu, Windows, Debian, RedHat, Rocky).

![1](/assets/images/public-cloud-user-guides/guide-8.png)
**Disk Setting**: Leave it as default value or add/remove the data/os disks.

![1](/assets/images/public-cloud-user-guides/guide-9.png)
**Network Setting**: Configure the VPC, Subnet and Public IP Address.

![1](/assets/images/public-cloud-user-guides/guide-10.png)
**Login Setting**: Set your instance name & password for the root account (you will need it later to access your host).

Step 6: Review the pricing and click `Purchase Now` to buy your VM.

![1](/assets/images/public-cloud-user-guides/guide-11.png)

Step 7: Review your order and click `Submit my order` to finalize the process.

![1](/assets/images/public-cloud-user-guides/guide-12.png)

Step 8: Wait for a while and you will be redirected to the UHost Compute Instance, your host (VM) should be listed here.

