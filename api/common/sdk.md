---
layout: default
title: SDK Usage
parent: Common
grand_parent: API
permalink: /api/common/sdk-usage
nav_order: 4
---
# SDK Sample Code Usage
## Steps
### Step 1

Enter the UAPI product and select the API to be used: https://console.scloud.sg/uapi/ucloudapi/Computing 

### Step 2

Fill in the relevant fields. Take Obtain Elastic IP Information-DescribeEIP as an example to query the EIP resource list of a project in the main region of Singapore in the account.

![1](/assets/images/public-cloud-user-guides/guide-19.png)

### Step 3

View [Sample Code], select the SDK language, and you can get the SDK Demo code of the language (Go, PHP, Python, Javascript).

![1](/assets/images/public-cloud-user-guides/guide-20.png)

### Step 4

Obtain the public and private keys of the account: https://docs.scloud.sg/api/common/api-keys 

### Step 5

Replace the PublicKey and PrivateKey in [Sample Code]

### Step 6

Copy the code and run it locally to get the corresponding query results.

<b>(1) For GO SDK Demo, please refer to the following steps:</b>

Note: For more help, you can view the GO SDK documentation

- Save the request code as `main.go`
- Execute `go mod init main`
- Execute `go mod tidy`
```go
go run ./main.go
```

Explanation:
- If you use go mod and Goland IDE at the same time, please search for `vgo` in Settings and enable `vgo` support;
- If you use go mod and `GOPATH` at the same time, please note that `go mod init/tidy` cannot be executed under `GOPATH`, just remove the project from `GOPATH`

<b>(2) For the Python SDK Demo, please refer to the following steps:</b>

Explanation:

For more help, you can view the [Python2 SDK](https://ucloud.github.io/ucloud-sdk-python2/) documentation and [Python3 SDK](https://ucloud.github.io/ucloud-sdk-python3/) documentation

Save the request code as `main.py`

```python
pip install ucloud-sdk-python3 (if using python2, execute pip install ucloud-sdk-python2)
python ./main.py
```
