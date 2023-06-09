---
layout: default
title: TerminateUHostInstance
parent: UHost
grand_parent: API
permalink: /api/uhost/TerminateUHostInstance
nav_order: 2
---
# Delete the cloud host - TerminateUHostInstance
## Brief introduction
Deletes the UHost instance in the specified data center.

`Expired resources that are not trials cannot be deleted; The delete operation can only be performed in the shutdown state`

## How to use
You can use any of the following methods to initiate an API request:

- Multilingual OpenSDK / Go / Python / Java /
- UAPI browser
- CloudShell cloud command line

## Definition
### Common parameters

| Parameter Name | Type | Description | Required |
| --- | --- | --- | --- |
| Action | string | The corresponding API command name, the current API is TerminateUHostInstance | Yes |
| PublicKey | string | User public key, which can be obtained from [Console](https://console.scloud.sg/uaccount/api_manage) | Yes |
| Signature | string | User signature generated based on public key and API command, see [Signature algorithm](https://docs.scloud.sg/api/common/signature-algorithm) | Yes |

### Request parameters

| Parameter Name | Type | Description | Required |
| --- | --- | --- | --- |
| Region | string | Region. See [List of Regions and Availability Zones](https://docs.scloud.sg/api/summary/regionlist) | Yes |
| Zone | string | Availability zone. See [Availability Zone List](https://docs.scloud.sg/api/summary/regionlist) | No |
| ProjectId | string | Project ID. Not filling in is the default item, and the sub-account must be filled in. | No |
| UHostId | string | UHost resource Id | Yes |
| ReleaseEIP | boolean | Whether to release the bound EIP when deleting the host. The default is false. | No |
| ReleaseUDisk | boolean | Whether to delete the mounted data disk when deleting the host. The default is false. | No |

### Response field

| Field Name | Type | Description | Required |
| --- | --- | --- | --- |
| RetCode | int | return status code, if it is 0, it means a successful return, if it is not 0, it means a failure | Yes |
| Action | string | Operation command name | Yes |
| Message | string | return error message, provide detailed description when RetCode is not 0 | No |
| InRecycle | string | It is used to judge whether to enter the recycle bin when the host is deleted. Put in Recycle Bin: "Yes", Delete Completely: "No". | Yes |
| UHostId | string | UHost instance Id | No |

## Example
### Example request

```
https://api.scloud.sg/?Action=TerminateUHostInstance

&Region=cn-bj2

&Zone=cn-bj2-04

&ProjectId=org-xxx

&UHostId=uhost-xxx

&ReleaseUDisk=true
```

### Example response

```
{

"Action": "TerminateUHostInstanceResponse",

"InRecycle": "No",

"RetCode": 0,

"UHostId": "uhost-xxx"

}
```