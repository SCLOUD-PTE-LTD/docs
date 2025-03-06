---
layout: default
title: USMC
parent: SCloud Server Migration Center
grand_parent: Public Cloud
permalink: /public-cloud/multi-cloud-migration/usmc/
nav_order: 2
---
# USMC
## Concept
### What is USMC?
USMC (SCloud Server Migration Center) is a product designed to assist users in migrating physical servers, virtual machines, and cloud hosts from other cloud service providers or IDC (Internet Data Centers) to SCloud’s public cloud.

## Product Advantages
### Simple and Easy to Use
- The USMC client is lightweight and requires no installation.
- Supports configuration of filtering rules, excluding files/directories on Linux and drive letters on Windows.
- Provides one-click script execution.

### Zero Downtime Migration
- On the source machine, USMC only performs read and copy operations, allowing the customer’s business to continue running normally during migration.

### Resume from Breakpoints
- During migration, users can pause or resume transmission at any time via the console.
- If network fluctuations cause the agent to go offline, the transfer progress can be restored after restarting the agent.

### Incremental Migration
- Supports incremental migration tasks. After the initial full migration is completed, periodic incremental updates ensure a smooth business transition.

### MAC/IP Retention
- The target host’s MAC address can remain unchanged (via API), preventing the invalidation of certain software licenses.
- The target host’s internal IP can remain unchanged (by pre-creating a VPC), avoiding the need for business network reconfiguration.

### High Security
- Data transmission uses RSA asymmetric encryption.
- Supports transmission via internal networks or dedicated lines.

## Supported Scenarios

### Source and Destination

USMC currently only supports migration to SCloud public cloud UHost and does not support migration to SCloud physical cloud or lightweight cloud instances.

|       Source       |    Destination    | Task Plan Type |
| :---------------: | :---------------: | :------------: |
| IDC Physical Machine/VMware | SCloud Public Cloud | IDC |
| SCloud Public Cloud/Physical Cloud | SCloud Public Cloud | IDC |
| Alibaba Cloud | SCloud Public Cloud | Alibaba Cloud |
| Tencent Cloud | SCloud Public Cloud | Tencent Cloud |
| Huawei Cloud | SCloud Public Cloud | Huawei Cloud |
| Amazon AWS | SCloud Public Cloud | Amazon |
| Other Cloud Providers | SCloud Public Cloud | IDC |

> Choosing the appropriate task plan type will improve the migration experience. However, OS adaptation for specific cloud providers may take time. If a specific cloud provider is not supported, selecting "IDC" may be an alternative. See the next section for details.

### Supported Operating Systems
USMC currently supports x86_64 systems only and does not support 32-bit or ARM-based systems.

#### Linux

| Source OS Version | Architecture | UEFI Support | Cloud-init Support | Cloud Provider Adaptation |
| :--------------: | :----------: | :----------: | :----------------: | :------------------------: |
| CentOS 5 | x86_64 | No | No | |
| CentOS 6 | x86_64 | No | Yes | Alibaba Cloud / Huawei Cloud |
| CentOS 7 | x86_64 | No | Yes | Alibaba Cloud / Tencent Cloud / Huawei Cloud |
| CentOS 8 | x86_64 | No | Yes | Alibaba Cloud / Huawei Cloud |
| CentOS Stream 8 | x86_64 | No | Yes | |
| Ubuntu 12 | x86_64 | No | No | |
| Ubuntu 14 | x86_64 | No | No | Alibaba Cloud |
| Ubuntu 16 | x86_64 | No | Yes | Alibaba Cloud / Huawei Cloud |
| Ubuntu 18 | x86_64 | No | Yes | Alibaba Cloud / Huawei Cloud |
| Ubuntu 20 | x86_64 | No | Yes | Alibaba Cloud |
| Ubuntu 22 | x86_64 | No | Yes | Alibaba Cloud / Amazon |
| Debian 7 | x86_64 | No | No | Alibaba Cloud / Amazon |
| Debian 9 | x86_64 | No | Yes | |
| Debian 10 | x86_64 | No | Yes | |
| Debian 11 | x86_64 | No | Yes | |
| openSUSE 12 | x86_64 | No | No | |

#### Windows

| Source OS Version | Architecture | UEFI Support | Cloud-init Support |
| :--------------: | :----------: | :----------: | :----------------: |
| Windows 2003 | x86_64 | No | No |
| Windows 2008 | x86_64 | Yes | No |
| Windows 2008R2 | x86_64 | Yes | No |
| Windows 2012 | x86_64 | Yes | No |
| Windows 2012R2 | x86_64 | Yes | No |
| Windows 2016 | x86_64 | Yes | No |
| Windows 2019 | x86_64 | Yes | No |
| Windows 2022 | x86_64 | Yes | No |

### Supported Regions

USMC supports a total of 22 regions, including 6 in China, 8 in Asia-Pacific, 3 in the Americas, and 5 in Europe, Africa, and the Middle East.

| Region Group | Destination Region | Internal Network/Dedicated Line Support | Internal Network Service Address |
| :----------: | :----------------: | :-------------------------------------: | :------------------------------: |
| China | North China 1 | Yes | 10.10.211.172:5260 |
| China | North China 2 | Yes | 100.65.182.127:5260 |
| China | Shanghai 2 | Yes | 10.23.251.99:5260 |
| China | Guangzhou | Yes | 10.13.241.209:5260 |
| China | Taipei | No | |
| China | Hong Kong | Yes | 10.8.198.54:5260 |
| Asia-Pacific | Singapore | No | |
| Asia-Pacific | Tokyo | No | |
| Asia-Pacific | Seoul | No | |
| Asia-Pacific | Jakarta | No | |
| Asia-Pacific | Bangkok | No | |
| Asia-Pacific | Ho Chi Minh City | No | |
| Asia-Pacific | Manila | No | |
| Asia-Pacific | Mumbai | No | |
| Americas | Los Angeles | No | |
| Americas | Washington D.C. | No | |
| Americas | São Paulo | No | |
| Europe, Africa, Middle East | Lagos | No | |
| Europe, Africa, Middle East | Frankfurt | No | |
| Europe, Africa, Middle East | Moscow | Yes | 10.39.221.47:5260 |
| Europe, Africa, Middle East | London | No | |
| Europe, Africa, Middle East | Dubai | No | |

> USMC depends on cloud disk services. If a specific availability zone runs out of disk capacity or is not enabled, migration will not be possible.

## Constraints and Limitations

- Disk Size Limits: The system disk should be ≤1000GB, and data disks should be ≤8000GB, with at least 500MB of available space. (For Linux exceeding these limits, please contact technical support.)
- No In-Memory Data Migration: USMC does not migrate memory data, so products like Redis or other in-memory databases cannot be migrated.
- Database Migration Considerations: If the machine contains MySQL, PostgreSQL, MongoDB, or TiDB services, it is recommended to use UDTS to migrate the database first or stop the service/business before migration.
- Hostname Not Migrated: USMC does not migrate hostnames. If the machine runs services that depend on the hostname, such as RabbitMQ or Kubernetes (K8S), please handle them manually.
- RAID & LVM Not Supported: Linux machines with RAID disk arrays or LVM volumes are not supported for migration.
- Shared File Systems: If the machine contains a shared file system such as NFS or NAS, it is recommended to unmount them before starting migration.
- Linux Read Permission Requirements: Ensure the directories have sufficient read permissions, and preferably run the agent as the root user.
- Possible MAC/IP/SID Changes: After migration, the server's MAC, IP, or SID may change, which could cause application license invalidation. Users must resolve this issue themselves.

## Billing Information

The USMC product itself is free of charge. However, during the migration process, a temporary intermediate instance is created under the user's account to facilitate migration. The costs associated with this instance are borne by the user.

- UHost: The intermediate instance is a Fast UHost VM with the same configuration as the source machine. It runs continuously during migration and is charged based on CPU/Memory usage. Once the migration is complete, the instance is automatically reinstalled to become the target machine.
- UDisk: USMC provisions the necessary disks based on the source machine's information and mounts them to the intermediate instance for data transfer. These disks are charged based on storage capacity.
- EIP: If the migration is performed over the public internet, the intermediate instance will be assigned an Elastic IP (EIP). Users can choose between bandwidth-based or data transfer-based billing. Once the migration is complete, the EIP can be unbound and released if no longer needed.

## Frequently Asked Questions
### How to exclude files during Linux migration?
Before executing `go2SCloud.sh`, add the following line to `agent_config.conf`:
```
excludeRulesFile=”/root/myRules.txt”
```
The `myRules.txt` file contains exclusion rules, formatted as follows:

| Rule          | Description                                           |
|--------------|-------------------------------------------------------|
| *.o         | Exclude all files ending with `.o`                     |
| /data/*/test | Exclude `test` files or folders under `/data/*/`      |
| /foo        | Exclude `foo` file or folder in the root directory     |
| /foo//bar | Exclude `bar` files or folders inside `/foo/` and subdirectories |
| foo/        | Exclude all folders named `foo`                        |
| test.txt    | Exclude all files named `test.txt`                     |
| testdd      | Exclude all files or folders named `testdd`            |

### How to exclude drive letters (e.g., D and F) in Windows migration?

Before executing `go2SCloud.bat`, add the following line to `agent_config.conf`:
```
winExclude=“D,F”
```
> Note: Multiple partitions on the same disk are not supported.

### Can USMC migrate GPU machines?

Yes, but the console does not provide a direct option. Users must use the API to create migration tasks manually.

> Note: SCloud requires a specific CPU/Memory-to-GPU ratio. If you are unfamiliar with this, please contact technical support.

### What factors affect the migration speed?

The actual transfer speed is primarily determined by:

1. Source machine's outbound bandwidth (your server).
2. Target machine's inbound bandwidth (Step 3 - Host Configuration).
3. Bandwidth limits set in Step 3 - Migration Configuration (the smallest of these values will be the bottleneck).

Other influencing factors:

- Linux Migration: Uses file-based transfer. The actual occupied space and the presence of many small files can slow down migration.
- Windows Migration: Uses block-based transfer. The transfer speed is proportional to the actual occupied disk space, provided the disk is not fragmented.
- Cross-region migrations (e.g., migrating from Alibaba Cloud Hangzhou to SCloud Hong Kong) may be significantly slower due to network latency and instability.

> If migration is taking too long, consider improving network quality.

### Can USMC retain the MAC address?

Yes, but this feature is not enabled in the console. Users can either call the API or request technical support.

### I want to migrate my machine to Moscow, but the API specified an O-type machine, and the actual target became N-type. Why?

The Moscow region does not support O-type instances and also does not support security groups.

### The Supported Scenarios page lists Ubuntu 22 as supported, but without Alibaba Cloud adaptation. Can I still migrate?

Yes, you can select IDC as the migration source when creating the migration task.

> USMC optimizes widely used third-party cloud systems (e.g., removing Alibaba Cloud monitoring agents) to enhance the migration experience.

If your cloud provider is not listed, try creating a migration source as IDC. If issues arise, contact technical support.

> For Windows, you can select any migration source.

### The Supported Scenarios page does not list Debian 12. Can I still migrate?

No, Debian 12 is not supported. You may contact technical support to request future support.

### Can I migrate consumer editions of Windows (e.g., Windows 7/8/10)?

Currently, consumer Windows versions are not officially supported. You may contact technical support.

### After starting the migration script, the agent does not appear online in the console. What should I do?

First, try refreshing the page. If the agent still does not appear, follow the Migration Guide to check network requirements.

### After migration, I cannot log in with the password set during task creation. What should I do?

This is usually caused by security policies in the source OS (e.g., Windows blocking `bat` or `ps1` scripts).

Try logging in using the original source machine password or contact technical support.

### My source machine was CentOS 7.8, but the console shows CentOS 8.4. Why?

Your migration task is not yet complete. The intermediate machine uses CentOS 8 by default. After completion, it will be replaced with the target machine.

> USMC uses fixed intermediate images: CentOS 8 for Linux and Windows Server 2019 for Windows.

### After starting the migration task, the progress remains at 0% for a long time. What should I check?

Ensure that the firewall/security group allows port 22.

### After migration, I rebooted and found a new kernel. Can I switch back to the old one?

For better compatibility, USMC installs a SCloud-customized Linux 4.19 kernel.

> Switching to the old kernel may cause boot failures.

### My migration progress has been stuck at 67% for a long time. Why?

Check the task details to see if auto-incremental migration is enabled. If the incremental interval is very small, the console will keep showing progress at `67%`.

> The console does not support disabling auto-incremental migration. Please contact technical support to disable it.