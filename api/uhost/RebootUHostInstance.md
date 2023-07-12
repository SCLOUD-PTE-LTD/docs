---
layout: default
title: RebootUHostInstance
parent: UHost
grand_parent: API
permalink: /api/uhost/RebootUHostInstance
nav_order: 3
---
# Reboot the host - RebootUHostInstance
## Brief introduction
To restart the UHost instance, you must specify the values of the data center and UHost ID parameters.

## How to use
You can use any of the following methods to initiate an API request:
- Multilingual OpenSDK / Go / Python / Java /
- UAPI browser
- CloudShell cloud command line

## Definition
### Common parameters

| Parameter Name | Type | Description | Required |
| --- | --- | --- | --- |
| Action | string | The corresponding API command name, the current API is RebootUHostInstance | Yes |
| PublicKey | string | User public key, which can be obtained from [Console](https://console.scloud.sg/uaccount/api_manage) | Yes |
| Signature | string | User signature generated based on public key and API command, see [Signature algorithm](https://docs.scloud.sg/api/common/signature-algorithm) | Yes |

### Request parameters

| Parameter Name | Type | Description | Required |
| --- | --- | --- | --- |
| Region | string | Region. See [List of Regions and Availability Zones](https://docs.scloud.sg/api/common/region-and-zone) | Yes |
| Zone | string | Availability zone. See [Availability Zone List](https://docs.scloud.sg/api/common/region-and-zone) | No |
| ProjectId | string | Project ID. Not filling in is the default item, and the sub-account must be filled in. | No |
| UHostId | string | UHost instance ID | Yes |
| DiskPassword | string | encrypted disk password | No |

### Response field

| Field Name | Type | Description | Required |
| --- | --- | --- | --- |
| RetCode | int | return status code, if it is 0, it means a successful return, if it is not 0, it means a failure | Yes |
| Action | string | Operation command name | Yes |
| Message | string | return error message, provide detailed description when RetCode is not 0 | No |
| UHostId | string | UHost instance ID | No |

## Example
### Example request

```
https://api.scloud.sg?Action=RebootUHostInstance
&Region=vn-sng
&Zone=vn-sng-01
&ProjectId=org-xxx
&UHostId=uhost-xxx
```

### Example response

```
{
"Action": "RebootUHostInstanceResponse",
"RetCode": 0,
"UHostId": "uhost-xxx"
}
```