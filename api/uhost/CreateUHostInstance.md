---
layout: default
title: CreateUHostInstance
parent: UHost
grand_parent: API
permalink: /api/uhost/CreateUHostInstance
nav_order: 1
---
# Create a cloud host - CreateUHostInstance
## Brief introduction
Create a UHost instance.

## How to use
You can use any of the following methods to initiate an API request:
- Multilingual OpenSDK / Go / Python / Java /
- UAPI browser
- CloudShell cloud command line

## Definition
### Common parameters

| Parameter name | Type |  | Required |
| --- | --- | --- | --- |
| Action | string | The corresponding API instruction name, the current API is `CreateUHostInstance` | Yes |
| PublicKey | string | The user's public key, available from [Console](https://console.scloud.sg/uaccount/api_manage) fetch | Yes |
| Signature | string | According to the public key and API The user signature generated by the directive is found in [Signature algorithm](https://docs.scloud.sg/api/common/signature-algorithm) | Yes |

### Required parameters

| Parameter Name | Type | Description | Required |
| --- | --- | --- | --- |
| Region | string | Region. See [List of Regions and Availability Zones](https://docs.scloud.sg/api/common/region-and-zone) | Yes |
| Zone | string | Availability zone. See [Availability Zone List](https://docs.scloud.sg/api/common/region-and-zone) | Yes |
| ProjectId | string | Project ID (example: `org-04ibqy`), This field, by default, is empty and is only required when the account is a Sub-Account | No |
| ImageId | string | Image ID. | Yes |
| Disks.N.IsBoot | string | Whether it is a system disk. Enumeration value: True, it is the system disk; False, it is the data disk (default). One and only one disk in the Disks array is the system disk. | Yes |
| Disks.N.Type | string | Disk type.  | Yes |
| Disks.N.Size | int | Disk size, in GB. | Yes |
| Disks.N.BackupType | string | Disk backup scheme. Enumeration value: NONE, no backup DATAARK, data ark SNAPSHOT, snapshot The backup mode supported by the current disk, default value: NONE | No |
| Disks.N.Encrypted | boolean | [The function is only available in some availability zones, please consult technical support for details] Whether the disk is encrypted. encryption: true, no encryption: false Encryption must pass in the corresponding KmsKeyId, the default value is false | No |
| Disks.N.KmsKeyId | string | [The function is only available in some availability zones, please consult technical support for details] kms key id. Required when selecting an encrypted disk. | No |
| Disks.N.CouponId | string | Cloud disk coupon id. Not applicable to system disk/local disk. Please inquire through the DescribeCoupon interface, or log in to the user center to view | No |
| LoginMode | string | Host login mode. Password (default option): Password, Key: KeyPair. | Yes |
| Password | string | UHost password. The password needs to be encoded with base64, for example: # echo -n Password1 \| base64UGFzc3dvcmQx. | No |
| Name | string | UHost instance name. Default: UHost. | No |
| Tag | string | Business group. Default: Default (Default means no grouping). | No |
| ChargeType | string | Charging mode. The enumerated values are: Year, paid annually; Month, paid monthly; Dynamic, prepaid by the hour Postpay, paid by the hour (support shutdown without charge, currently only supported by some availability areas, please contact your account manager) Preemptive billing is preemptive instance (internal testing phase) The default is monthly payment | No |
| Quantity | int | Purchase duration. Default: value 1. This parameter is not required for hourly purchases (Dynamic/Postpay). When paying monthly, this parameter is passed as 0, which means the purchase is until the end of the month. | No |
| CPU | int | Number of virtual CPU cores. Optional parameters: 1-64 (refer to the console for the corresponding relationship between specific models and CPUs). Default value: 4. | No |
| Memory | int | Memory size. Unit: MB. Range: [1024, 262144], the value is a multiple of 1024 (refer to the console for the optional range). Default: 8192 | No |
| GpuType | string | GPU type, enumerated value ["K80", "P40", "V100", "T4","T4A", "T4S","2080Ti","2080Ti-4C", "1080Ti", "T4/4", "MI100", "V100S",2080","2080TiS","2080TiPro","3090","A100"], required when MachineType is G | No |
| GPU | int | Number of GPU cores. Only GPU models support this field (the optional range is related to MachineType+GpuType) | No |
| NetCapability | string | Network enhancement feature. Enumerated value: Normal, not enabled; Super, enabled network enhancement 1.0; Ultra, enabled network enhancement 2.0 (refer to the official website document for details) | No |
| HotplugFeature | boolean | Hot plug feature. True is enabled, False is not enabled, and the default is False. | No |
| VPCId | string | VPC ID. The default is the default VPC of the current region. | No |
| SubnetId | string | Subnet ID. The default is the default subnet of the current region. | No |
| PrivateIp.N | string | [Array] Specifies the internal network IP when creating the cloud host. If no value is passed, the IP under the current subnet will be randomly assigned. Example of calling method: PrivateIp.0=x.x.x.x. Currently only one intranet IP is supported. | No |
| SecurityGroupId | string | Firewall ID, default: Web recommended firewall. For how to query SecurityGroupId. | No |
| IsolationGroup | string | Hardware isolation group id. Available through DescribeIsolationGroup. | No |
| AlarmTemplateId | int | Alarm template id, if the alarm template id is passed and the alarm template id is correct, the alarm template will be bound. The failure to bind the alarm template will only have a log in the background, and will not affect the process of creating a host, nor will it report an error on the front end. | No |
| MachineType | string | Cloud host machine type (V2.0), in this field and field UHostType, only one of the fields is required. Enumeration value ["N", "C", "G", "O", "OS", "OM", "OPRO", "OMAX", "O.BM", "O.EPC"]. | No |
| MinimalCpuPlatform | string | Minimum cpu platform, enumerated value ["Intel/Auto", "Intel/IvyBridge", "Intel/Haswell", "Intel/Broadwell", "Intel/Skylake", "Intel/ Cascadelake", "Intel/CascadelakeR", "Intel/IceLake", "Amd/Epyc2", "Amd/Auto", "Ampere/Auto", "Ampere/Altra"], the default value is "Intel/Auto". | No |
| MaxCount | int | The maximum number of hosts created this time, the value range is [1,100], and the default value is 1. | No |
| NetworkInterface.N.EIP.Bandwidth | int | [If EIP is bound, this parameter is required] The external network bandwidth of the elastic IP, the unit is Mbps. The shared bandwidth mode must specify 0M bandwidth, and the non-shared bandwidth mode must specify Specify non-0Mbps bandwidth. The bandwidth range of non-shared bandwidth in each region is as follows: traffic billing [1-300], bandwidth billing [1-800] | No |
| NetworkInterface.N.EIP.PayMode | string | Elastic IP billing mode. Enumeration value: `Traffic`, traffic billing; `Bandwidth`, bandwidth billing; `ShareBandwidth`, shared bandwidth mode. `Free`: free bandwidth mode, default is `Bandwidth` | No |
| NetworkInterface.N.EIP.ShareBandwidthId | string | Bound shared bandwidth Id, only valid when PayMode is ShareBandwidth | No |
| NetworkInterface.N.EIP.OperatorName | string | [If EIP is bound, this parameter is required] The line of the elastic IP. Enumerated value: International: International BGP: Bgp The line parameters allowed in each region are as follows: cn-sh1: Bgp cn-sh2: Bgp cn-gd: Bgp cn-bj1: Bgp vn-sng: Bgp hk: International us-ca: International th-bkk: International kr-seoul:International us-ws:International ge-fra:International sg:International tw-kh:International. Other overseas routes are International | No |
| NetworkInterface.N.EIP.CouponId | string | Current EIP voucher id. Please query through the DescribeCoupon interface, or log in to the user center to view. | No |
| NetworkInterface.N.CreateCernetIp | boolean | Apply for and bind an External Network EIP. True means to apply for and bind, False means not to apply for binding, and the default is False. Currently, only models with HPC features are supported. | No |
| UserData | string | User-defined data. This field can be filled in when the image supports the Cloud-init Feature. Note: 1. The total data size does not exceed 16K; 2. Use base64 encoding | No |
| AutoDataDiskInit | string | Whether the data disk needs to be automatically partitioned and mounted. This field can be filled in when the image supports the "Cloud-init" Feature. Values `On` auto mount (default) `Off` no auto mount. | No |
| KeyPairId | string | KeyPairId Key pair ID, this item is required when LoginMode is KeyPair. | No |
| Features.UNI | boolean | ENI features. This feature takes effect only when the permission bit of the ENI is enabled. By default, false is disabled, and true is enabled. It is only compatible with NetCapability Normal. | No |
| CouponId | string | Host voucher ID. Please query through the DescribeCoupon interface, or log in to the user center to view | No |

### Response field

| Field Name | Type | Description | Required |
| --- | --- | --- | --- |
| RetCode | int | return status code, if it is 0, it means a successful return, if it is not 0, it means a failure | Yes |
| Action | string | Operation command name | Yes |
| Message | string | return error message, provide detailed description when RetCode is not 0 | No |
| UHostIds | array[string] | UHost instance Id collection | No |
| IPs | array[string] | [Batch creation will not return] IP information | No |

## Example
### Example request

```
https://api.scloud.sg?Action=CreateUHostInstance
&Region=IrPRKVru
&Zone=YxunehQG
&ProjectId=SGGYxkiC
&ImageId=WTrQVOVc
&Disks.N.IsBoot=RprtMlkM
&Disks.N.Type=cTNMVMgN
&Disks.N.Size=4
&LoginMode=ZDxcwBvI
&Password=HjhrLmyV
&Name=kWFfXPdF
&Tag=mZmpwPll
&ChargeType=SVUvekbj
&Quantity=7
&UHostType=xESfaKXW
&CPU=7
&Memory=9
&GpuType=wcdlufLM
&GPU=4
&StorageType=hidVBsZq
&BootDiskSpace=9
&DiskSpace=9
&NetCapability=UAaqyFEH
&TimemachineFeature=yes
&HotplugFeature=true
&DiskPassword=ZhNHQpUa
&NetworkId=CUvjmHLR
&VPCId=INGnMbev
&SubnetId=BzArPHsE
&PrivateIp.N=kINtqkDh
&PrivateMac=kpjksQBp
&SecurityGroupId=jbPxuRXI
&HostType=EMqnGIZZ
&InstallAgent=mOfkpefj
&ResourceType=KCqMZlIl
&Disks.N.BackupType=LiQiEorj
&IsolationGroup=MyYUcDKC
&Disks.N.Encrypted=false
&Disks.N.KmsKeyId=rwVpvLJi
&Disks.N.CouponId=PvRWpaRb
&AlarmTemplateId=2
&MachineType=MJsjIQFC
&MinimalCpuPlatform=VCokccwd
&MaxCount=5
&NetworkInterface.N.EIP.Bandwidth=9
&NetworkInterface.N.EIP.PayMode=JujSAqbO
&NetworkInterface.N.EIP.ShareBandwidthId=kHfdnNWd
&NetworkInterface.N.EIP.OperatorName=WNXiYXjc
&NetworkInterface.N.EIP.CouponId=GFzoXEOo
&SetId=1
&HostIp=VxccviHh
&NetworkInterface.N.IPv6.ShareBandwidthId=nYbcxXrf
&UserData=wTkIjHzw
&Disks.N.Id=YnyPnaOj
&AutoDataDiskInit=MynHIYht
&NetworkInterface.N.IPv6.Address=wzQVOnfd
&CharacterName=rDPtrUlu
&PromotionId=iIzhvRBn
&ImageOsName=XojzwILg
&PodId=qwVrfmTT
&BillActivityId=5
&BillActivityRuleId=3
&RestrictMode=kRfUiaKA
&Volumes.N.Type=FKimwmGX
&Volumes.N.IsBoot=zjLJQmIe
&Volumes.N.Size=5
&Volumes.N.VolumeId=lYyhJqrV
&Volumes.N.CouponId=oiISGCSv
&HpcEnhanced=false
&NetworkInterface.N.CreateCernetIp=true
&KeyPairId=gYzfeRZI
&SecGroupId.N=shbyPlMw
&Features.UNI=PCaQdRus
&CouponId=LIgqlYLP
&GpuFsx=false
&UfsMountId=owebqtsq
&UFSMountId=zFLnxyoU
&SecGroupId.N.Priority=DMnmhjIz
&SecurityMode=ykuINtPs
&Disks.N.BackupMode=rgqxGmlQ
&Disks.N.CustomBackup.Journal=wHuWPXvg
&Disks.N.CustomBackup.Hour=rrczngzO
&Disks.N.CustomBackup.Day=sQHbfMoJ
&UDSetId=fdlEmzzf
&UDHostId=MuOlWCEV
&HostBinding=true
&HostBinding=true
```

### Example response

```
{
  "Action": "CreateUHostInstanceResponse",
  "IPs": [
    "iUkWSFjX"
  ],
  "MountedUFSId": "qfAGtiIv",
  "RetCode": 0,
  "UHostIds": [
    "GLKEpaXW"
  ]
}
```