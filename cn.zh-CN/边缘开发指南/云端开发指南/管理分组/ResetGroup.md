# ResetGroup {#reference_vsd_rt2_h2b .reference}

重置分组中的网关。返回结果中success为true，只代表生成重置分组的指令成功，并不代表分组中的网关已重置成功。

## 请求参数 {#section_trw_st2_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：ResetGroup。|
|GroupId|Long​|是|​分组ID。|

## 返回参数 {#section_usy_c52_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|

## 示例 {#section_vmt_bv2_h2b .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=ResetGroup
&GroupId=135992
&公共请求参数
```

**返回示例**

```
{
  "RequestId": "9A6B2507-D10C-4E94-BDA1-3D300181C8DA",
  "Success": true,
  "Code": "200"
}
```

