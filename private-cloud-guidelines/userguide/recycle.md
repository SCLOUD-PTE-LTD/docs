---
layout: default
title: Soft Delete
parent: User Guide
grand_parent: Private Cloud User Guides
permalink: /private-cloud/user-guides/soft-delete/
nav_order: 21
---
# 21. Recycle Bin

## 21.1 Overview of Recycling Bin

The recycle bin refers to a temporary retention area where resources are automatically released when deleted or overdue. Resources deleted by users include virtual machines, disks, EIPs, self-made images, etc., which will automatically enter the recycle bin after deletion.

Resources entered into the recycle bin will be retained by default for a certain period of time. The default retention time on the platform is 360000 seconds, and the retention time can be customized by the cloud platform administrator. **During the retention period, the resources can be recovered, renewed and destroyed. After the retention time expires, the resources will be completely destroyed and cannot be recovered.**

## 21.2 View Recycle Bin Resources

When resources on the cloud platform are manually deleted by the user or the cost expires, they will automatically enter the recycle bin for temporary storage. During the storage period, you can go to the recycle bin console to view the list of resources that have entered the recycle bin, as shown in the following figure:

![recycle](/assets/images/userguide/recycle.png)

The Recycle Bin resource list can be used to view the retained resource information that has been deleted or released under the current account, including resource ID, resource name, resource type, expiration time, deletion time, whether it is automatically destroyed, scheduled destruction time, and operation items.

- Resource Name/ID: The name and globally unique identifier of the current retained resource.
- Status: The current status of the resource, including deleted and being destroyed.
- Resource Type: The resource type of the current retained resources, including virtual machines, disks, public IPs, custom images, etc.
- Expiration time: refers to the expiration time of the current resource fee, only when the resource type is a chargeable resource, such as virtual machines, disks, and external IPs.
- Delete time: refers to the time when the current retained resources are manually deleted or expired due to cost and enter the recycle bin.
- Does it auto-destroy: Refers to whether the retained resources will be automatically destroyed during the retention period, and whether the resources will be automatically destroyed after the retention period can be set through the cloud platform management console.
  - If the global configuration of the cloud platform is set to automatically destroy the recycle bin resources, the resources will be automatically destroyed after the retention period.
  - If the global configuration of the Yun platform is set to not automatically destroy the resources in the recycle bin, the resources will be permanently stored in the recycle bin and can be manually recovered or destroyed.
- Destruction time: refers to the time when the current retained resources will be automatically destroyed, only when the cloud platform global configuration is set to recycle bin resources automatically destroyed.

The operation items on the list refer to the operations on individual resources, including recovery, renewal and immediate renewal, etc. The renewal operation is only valid for resources with billing type, and the resource list can be searched and filtered through the search box, supporting fuzzy search.

To facilitate the maintenance of recycling bin resources by tenants, batch operations on resources entering the recycling bin are supported, including batch recovery of resources, batch destruction of resources, and batch renewal of resources. Among them, batch renewal of resources will automatically renew for one period according to the billing method of the lake, such as resources charged by time, automatically renew for one hour; resources purchased by month will automatically renew for one month.

## 21.3 Restore Resources

Restoring resources refers to manually restoring resources that have been mistakenly deleted and entered into the recycle bin.

- If the resources are manually deleted by the user and there is no arrears, they can be directly restored by restoring the resources operation.
- If the resources are automatically entered into the recycle bin due to account arrears, when restoring the resources, you need to contact the cloud platform administrator to recharge the account, and then renew the resources through the "Renewal" operation, and then restore the resources.
- If the global auto renewal of resources is not enabled and the account balance is sufficient, the resources will enter the recycle bin after expiration. When restoring the resources, you need to first renew the resources through the "Renewal" operation, and then restore the resources.

Users can recover resources through the "**Restore**" option in the Recycle Bin resource list. If the resource fee has expired, it needs to be renewed before the recovery operation can be performed. The specific recovery operation is shown in the following figure:

![restore](/assets/images/userguide/restore.png)

After clicking Confirm, the current resource will be automatically restored to the resource list before it was deleted, and can be viewed through the relevant resource list. If the resource is in arrears, the interface will prompt the user that the resource is in arrears and needs to be recharged or renewed before the resource can be restored.

## 21.4 Renewal Resources

Renewal of resources refers to the renewal of the fee cycle of the resources, and only resources that need to be billed are supported for "**renewal**" operations. After the resources that are overdue or automatically enter the recycle bin due to expiration of fees are successfully renewed, they can be restored. The renewal cycle of resources varies according to the billing method.

- When resources are charged by the hour: one renewal operation can renew for 1 hour, and N renewal operations can renew for N hours.
- When resources are billed monthly: one renewal operation can renew for 1 month, N renewal operations can renew for N months.
- When resources are billed annually: one renewal operation can renew for 1 year, and N renewal operations can renew for a fee cycle of N years.

Users can renew resources through the "Renew" option in the retained resource list of the Recycle Bin. If the account is in arrears, contact the administrator to recharge the account before renewing. The specific renewal operation is shown in the following figure:

![renew](/assets/images/userguide/renew.png)

After clicking confirm, return to the Recycle Bin retention resource list, where you can view the expiration time of the renewed resource extended for one billing cycle in the list page.

## 21.5 Destroy resources

Destroying resources refers to manually destroying resources stored in the recycle bin, which cannot be recovered after being destroyed. It is necessary to confirm whether it is necessary to destroy the resources. The specific operation is shown in the following figure:

![destroy](/assets/images/userguide/destroy.png)

After clicking confirm, the resource will be cleared from the retained resource list and cannot be recovered, please operate with caution.

