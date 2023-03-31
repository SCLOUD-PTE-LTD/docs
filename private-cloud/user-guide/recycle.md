---  
layout: default
title: Recycle Bin
parent: User Guide
grand_parent: Private Cloud
permalink: /private-cloud/user-guide/recycle-bin/
nav_order: 15
---
# Recycle Bin
## Overview of Recycle Bin
The recycle bin refers to the temporary reserved area where resources are deleted or automatically released in arrears. Resources deleted by users, including virtual machines, disks, EIPs, self-made images, etc., will automatically enter the recycle bin after deletion.

The resources entering the recycle bin will be reserved for a default time. The default retention time of the platform is 360,000 seconds. The cloud platform administrator can set a custom retention time. Resources can be restored, renewed, and destroyed during the retention period. After the retention time expires, the resources will be completely destroyed and cannot be restored.

## View Recycle Bin Resources
When cloud platform resources are manually deleted by the user or the fee expires, they will automatically enter the recycle bin for temporary storage. During the retention period, you can go to the recycle bin console to view the list of resources that have entered the recycle bin, as shown in the following figure:

![1](/assets/images/user-guide/user-guide-151.png)

Through the resource list of the recycle bin, you can view the information of retained resources that have been deleted or released under the current account, including resource ID, resource name, resource type, expiration time, deletion time, whether to automatically destroy, scheduled destruction time, and operation items:

- Resource name/ID: the name and globally unique identifier of the currently reserved resource;
- Resource type: the resource type of the currently reserved resource, including virtual machine, hard disk, external network IP, self-made image, etc.;
- Expiration time: refers to the expiration time of the current resource fee, which is only valid when the resource type is a resource that needs to be billed, such as virtual machine, disk, and external network IP;
- Deletion time: refers to the time when the current retained resource is manually deleted or the fee expires and enters the recycle bin;
- Whether to automatically destroy: Refers to whether the currently retained resources will be automatically destroyed during the retention period. You can set whether to automatically destroy resources after the retention period through the cloud platform management console:
  - If the cloud platform is configured globally to automatically destroy resources in the recycle bin, the resources will be automatically destroyed after the retention period is reached;
  - If the cloud platform is configured globally so that the recycle bin resources are not automatically destroyed, the resources will remain in the recycle bin permanently, and the resources can be restored or destroyed manually;
- Destruction time: refers to the time when the current reserved resources will be automatically destroyed, and it is only valid when the cloud platform is configured globally to automatically destroy resources in the recycle bin.

The operation item on the list refers to the operation on a single resource, including recovery, renewal, and immediate renewal. The renewal operation is only valid when the resource type is a resource that needs to be billed. You can search and filter the resource list through the search box. Support fuzzy search.

To facilitate tenants' maintenance of recycle bin resources, batch operations on resources entering the recycle bin are supported, including batch restoration of resources, batch destruction of resources, and batch renewal of resources. Among them, batch renewal resources will be automatically renewed for one cycle according to the billing method of Zihu. For example, resources billed by the hour will be automatically renewed for one hour; resources purchased on a monthly basis will be automatically renewed for one month.

## Restoring resources
Restoring resources refers to manually restoring resources that have been accidentally deleted and entered the recycle bin.

- If the resource is manually deleted by the user and there is no arrears, it can be restored directly through the restore resource operation;
- If the resource is automatically entered into the recycle bin due to account arrears, when restoring the resource, you need to contact the cloud platform administrator to recharge the account, and after renewing the resource through the "renewal" operation, restore the resource;
- If the resource automatic renewal is not enabled globally and the account balance is sufficient, the resource will automatically enter the recycle bin after it expires. When restoring the resource, you need to renew the resource through the "Renewal" operation first, and then restore the resource.

Users can restore resources through the "Restore" operation item in the recycle bin reserved resource list. If the resource resource fee has expired, it needs to be renewed before the restoration operation can be performed. The specific recovery operation is shown in the figure below:

![1](/assets/images/user-guide/user-guide-152.png)

After clicking OK, the current resource will automatically restore to the resource list before deletion, which can be viewed through the related resource list. If the resource is in arrears, the interface will prompt the user that the resource is in arrears and needs to be recharged or renewed before the resource can be restored.

## Renewal Resources
Renewing resources refers to renewing the fee cycle of resources, and only supports the "renewal" operation for resources that need to be billed. The recovery operation can only be performed after the resources that are automatically entered into the recycle bin due to arrears or due fees are successfully renewed. The cycle of resource renewal varies according to the billing method:

- When resources are billed by the hour: one renewal operation can be renewed for 1 hour, and N renewal operations can be renewed for N hours;
- When resources are billed on a monthly basis: One subscription renewal operation can be renewed for 1 month, and N renewal operations will be renewed for N months;
- When resources are billed on an annual basis: One resource renewal operation can be renewed for 1 year, and N renewal operations are the renewal fee cycle of N years.

Users can perform resource renewal operations through the "Renewal" operation item in the recycle bin reserved resource list. If the account is in arrears, it is necessary to contact the administrator to recharge the account before proceeding with the renewal operation. The specific renewal operation is shown in the figure below:

![1](/assets/images/user-guide/user-guide-153.png)

After clicking OK, you will return to the list of reserved resources in the recycle bin. On the list page, you can check that the expiration time of the renewed resources has been extended by one billing cycle.

## Destroying resources
Destroying resources refers to manually destroying resources left in the recycle bin. Resources cannot be recovered after being destroyed. It is necessary to confirm whether it is necessary to destroy resources. The specific operation is shown in the figure below:

![1](/assets/images/user-guide/user-guide-154.png)

After clicking OK, the resource will be cleared from the retained resource list and cannot be restored. Please operate with caution.