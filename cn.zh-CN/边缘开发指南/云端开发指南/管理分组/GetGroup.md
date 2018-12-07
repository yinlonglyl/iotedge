# GetGroup {#reference_hkj_vb1_h2b .reference}

查询分组的具体信息。

## 请求参数 {#section_kl1_xb1_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetGroup。|
|GroupId|Long|是|分组ID。|

## 返回参数 {#section_arx_jc1_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|
|Data|[Data](#table_mmf_sc1_h2b)|调用成功过时返回的数据，具体定义参见下表。|

|参数|类型|描述|
|:-|:-|:-|
|Status|Integer|状态。-   0：不存在 \(not exist\)。
-   2：运行中 \(running\)。
-   3：已完成 \(finished\)。
-   4：失败 \(failed\)。
-   11：部署中 \(deploying\)。
-   12：停止 \(stopping\)。
-   31：部署失败 \(deploy failed\)。
-   32：停止失败 \(stop failed\)。

|
|GmtCreate|String|创建时间。|
|GmtModified|String|修改时间。|
|Tags|String|分组标签。|
|Name|String|分组名称。|
|GroupId|Long|分组ID。|

## 示例 {#section_tty_fd1_h2b .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=GetGroup
&GroupId=135992
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "C3730D33-AD3A-439F-8756-E7564BA13549",
    "Data": {
        "Status": 0,
        "GmtCreate": "2018-06-21 14:24:37",
        "GmtModified": "2018-06-21 14:24:37",
        "Tags": "key:value",
        "Name": "group"
        "GroupId": 136800
    },
    "Code": "200",
    "Success": true
}
```

