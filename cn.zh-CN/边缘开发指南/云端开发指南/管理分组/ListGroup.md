# ListGroup {#reference_ksj_cmz_g2b .reference}

分页查询分组列表。

## 请求参数 {#section_xgc_j5z_g2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：ListGroup。|
|Name|String|否|分组名称。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值40，默认值是10。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。默认值 1。|

## 返回参数 {#section_ukl_jvz_g2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|
|Data|[Data](#table_wpr_tvz_g2b)|调用成功时，返回的数据。具体见下表。|

|参数|类型|描述|
|:-|:-|:-|
|PageSize|Integer|每页记录数。|
|Total|Integer|总记录数。|
|CurrentPage|Integer|当前页码。|
|GroupList|List|分组数据类别。具体参见下表 [GroupList](#table_skk_pwz_g2b)。|

|参数|类型|描述|
|:-|:-|:-|
|Status|Integer|状态。取值：-   0：不存在 \(not exist\)。
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
|Name|String|名称。|
|GroupId|Long|分组ID。|
|Tags|String|分组标签。|

## 示例 {#section_cdm_sxz_g2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListGroup
&PageSize=10
&CurrentPage=1
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"42C32E79-F3A1-4AD5-B80E-CC0F0221EE5D",
    "Data":{
        "PageSize":15,
        "GroupList":{
            "GroupInfo":[
                {
                    "Status":0,
                    "GmtCreate":"2018-06-13 16:23:44",
                    "GmtModified":"2018-06-13 16:23:44",
                    "Tags":"tag1:val1,tag2:val2",
                    "Name":"group1",
                    "GroupId":136722
                },
                {
                    "Status":0,
                    "GmtCreate":"2018-06-13 16:04:08",
                    "GmtModified":"2018-06-13 16:14:28",
                    "Tags":"tag1:val1,tag2:val2",
                    "Name":"分组测1528877048098",
                    "GroupId":136718
                }
            ]
        },
        "CurrentPage":1,
        "Total":82
    },
    "Code":"200",
    "Success":true
}
```

