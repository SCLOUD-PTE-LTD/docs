---
layout: default
title: ResetULHostInstancePassword
parent: ULightHost
grand_parent: API
permalink: /api/ulhost/ResetULHostInstancePassword
nav_order: 6
---
# ResetULHostInstancePassword
## Introduction
Reset the administrator password of the lightweight application cloud host.

> This operation requires the UHost instance to be closed.

## Instructions
You can choose any of the following methods to initiate an API request:

Multilingual OpenSDK / Java /

CloudShell cloud command line

## Definition
### Common parameter

| Parameter name | Type |  | Required |
| --- | --- | --- | --- |
| Action | string | The corresponding API instruction name, the current API is `ResetULHostInstancePassword` | Yes |
| PublicKey | string | The user's public key, available from [Console](https://console.scloud.sg/uaccount/api_manage) fetch | Yes |
| Signature | string | According to the public key and API The user signature generated by the directive is found in [Signature algorithm](https://docs.scloud.sg/api/common/signature-algorithm) | Yes |

### Request parameter


### Response field 


## Example
### Example request
```
https://api.scloud.sg?Action=ResetULHostInstancePassword
&Region=vn-sng
&ProjectId=CAUPAisq
&ULHostId=jCauOLoz
&Password=AgZVFGfu

```
### Example response
```
{
  "Action": "ResetULHostInstancePasswordResponse",
  "RetCode": 0,
  "ULHostId": "NcJIRRyi"
}
```