---
layout: default
title: Signature Algorithm
parent: Common
grand_parent: API
permalink: /api/common/signature-algorithm
nav_order: 2
---
# Signature Algorithm
## Data assumptions
When generating a signature in an API request, you need to provide the key in the account, including PublicKey and PrivateKey, which can be obtained from the UAPI console.
In this case, it is assumed
```
PublicKey  = 'john.doe@example.com1296235120854146120'
PrivateKey = '46f09bb9fab4f12dfc160dae12273d5332b5debe'
```
You can use the above PublicKey and PrivateKey to debug your code, and when you get the consistent signature result (that is, your code is correct), you can replace it with your own PublicKey and PrivateKey and other API requests.
In this example, assume that the user request parameters are stringed as follows:
```
{
    "Action"     :  "DescribeUHostInstance",
    "Region"     :  "cn-bj2",
    "Limit"      :  10,
    "PublicKey"  :  "john.doe@example.com1296235120854146120"
}
```
The SHA1 signature of the signed string is generated, which is the value of the request parameter Signature.
According to the above algorithm, in this example, the calculated Signature is `CBA5CF5EC4D4233D206B1B54951E3787350A642F`.

## Construct the signature
### 1. Sort the request parameters in ascending order by name
```
{
    "Action"     :  "DescribeUHostInstance",
    "Limit"      :  10,
    "PublicKey"  :  "john.doe@example.com1296235120854146120",
    "Region"     :  "cn-bj2"
}
```
### 2. Construct the signed parameter string
The construction rules of the signed string are: signed string = all request parameters spliced (no HTTP escaping required). and concatenate the private key of the API key at the end of this signature string.
```
ActionDescribeUHostInstanceLimit10PublicKeyjohn.doe@example.com1296235120854146120Regioncn-bj246f09bb9fab4f12dfc160dae12273d5332b5debe
```

**Note**:

- For bool types, it should be encoded as true/false
- For floating-point types, if the fractional part is 0, only the integer part should be retained, such as 42.0 should be retained 42
- For floating-point types, you cannot use scientific notation

### 3. Calculate the signature
The string is signed using SHA1 encoding to generate the final signature.
