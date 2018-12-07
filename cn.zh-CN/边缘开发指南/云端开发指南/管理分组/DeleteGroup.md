# DeleteGroup {#reference_lk2_hr2_h2b .reference}

删除分组。

## 请求参数 {#section_r51_kr2_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：DeleteGroup。|
|GroupId|Long​|是|​分组ID。|

## 返回参数 {#section_jmd_tr2_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|

## 示例 {#section_rsl_ds2_h2b .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteGroup
&GroupId=135992
&公共请求参数
```

**返回示例**

```
{
  "RequestId": "711BE7A9-7AE7-4087-96CE-86016BD23D17",
  "Success": true,
  "Code": "200"
}
```

