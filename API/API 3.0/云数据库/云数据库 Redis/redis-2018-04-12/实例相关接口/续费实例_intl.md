﻿## 1. API Description

API domain name: redis.tencentcloudapi.com.

Renew an instance

Default API request rate limit: 20 requests/sec.

Note: This API supports financial availability zones. As financial availability zones and non-financial availability zones are isolated, if the common parameter Region specifies a financial availability zone (e.g., ap-shanghai-fsi), it is necessary to specify a domain name with the financial availability zone too, preferably in the same region as specified in Region, such as redis.ap-shanghai-fsi.tencentcloudapi.com.



## 2. Input Parameters

The following list of request parameters lists only the API request parameters and some common parameters. For the complete list of common parameters, see [Common Request Parameters](/document/api/239/20005).

| Parameter name | Required | Type | Description |
|---------|---------|---------|---------|
| Action | Yes | String | Common parameter; the value for this API: RenewInstance |
| Version | Yes | String | Common parameter; the value for this API: 2018-04-12 |
| Region | Yes | String | Common parameters; for details, see the [List of Regions](/document/api/239/20005#.E5.9C.B0.E5.9F.9F.E5.88.97.E8.A1.A8) supported by the product. |
| Period | Yes | Integer | Length of purchase in months |
| InstanceId | Yes | String | Instance ID |

## 3. Output Parameters

| Parameter name | Type | Description |
|---------|---------|---------|
| DealId | String | Deal ID |
| RequestId | String | The unique request ID which is returned for each request. The RequestId for the current request needs to be provided during troubleshooting. |

## 4. Sample

### Sample 1. Request Example

#### Input Sample Code

```
https://redis.tencentcloudapi.com/?Action=RenewInstance
&Period=12
&InstanceId=crs-5a4py64p
&<Common request parameter>
```

#### Output Sample Code

```
{
  "Response": {
    "DealId": "6954225",
    "RequestId": "495d8e9f-61bf-465e-aa4e-14be54a9095a"
  }
}
```


## 5. Developer Resources

### API Explorer

**This tool provides various capabilities such as online call, signature verification, SDK code generation, and quick API retrieval that significantly reduce the difficulty of using TencentCloud API.**

* [API 3.0 Explorer](https://console.cloud.tencent.com/api/explorer?Product=redis&Version=2018-04-12&Action=RenewInstance)

### SDK

TencentCloud API 3.0 comes with a set of complementary development toolkits (SDKs) that support multiple programming languages and make it easier to call the APIs.

* [Tencent Cloud SDK 3.0 for Python](https://github.com/TencentCloud/tencentcloud-sdk-python)
* [Tencent Cloud SDK 3.0 for Java](https://github.com/TencentCloud/tencentcloud-sdk-java)
* [Tencent Cloud SDK 3.0 for PHP](https://github.com/TencentCloud/tencentcloud-sdk-php)
* [Tencent Cloud SDK 3.0 for Go](https://github.com/TencentCloud/tencentcloud-sdk-go)
* [Tencent Cloud SDK 3.0 for NodeJS](https://github.com/TencentCloud/tencentcloud-sdk-nodejs)
* [Tencent Cloud SDK 3.0 for .NET](https://github.com/TencentCloud/tencentcloud-sdk-dotnet)

### TCCLI

* [Tencent Cloud CLI 3.0](https://cloud.tencent.com/document/product/440/6176)

## 6. Error Codes

Only the error codes related to the API business logic are listed below. For other error codes, see [Common Error Codes](/document/api/239/15694#.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81).

| Error Code | Description |
|---------|---------|
| InvalidParameter.EmptyParam | Parameter is empty. |
| InvalidParameter.PermissionDenied | The API has no CAM permissions. |
| LimitExceeded.PeriodExceedMaxLimit | The requested length of purchase is more than 3 years and exceeds the maximum value. |
| LimitExceeded.PeriodLessThanMinLimit | The length of purchase is invalid. It must be at least one month. |
| ResourceInUse.InstanceBeenLocked | The instance is locked by another process. |
| ResourceNotFound.InstanceNotExists | No Redis instance found by the serialId. |
| ResourceUnavailable.AccountBalanceNotEnough | The request order number does not exist. |
| ResourceUnavailable.InstanceDeleted | The instance has already been reclaimed. |
