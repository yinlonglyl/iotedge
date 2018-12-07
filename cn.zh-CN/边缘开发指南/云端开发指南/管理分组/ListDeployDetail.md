# ListDeployDetail {#reference_qmt_xkg_h2b .reference}

查询分组部署详情列表。

## 请求参数 {#section_yhm_llg_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：ListDeployDetail。|
|GroupId|Long|是|​分组ID。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值100，默认值是10。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。默认值 1。|

## 返回参数 {#section_tfh_nng_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|
|Data|[Data](#table_jlf_xng_h2b)|调用成功时，返回的数据。具体见下表。|

|参数|类型|描述|
|:-|:-|:-|
|CurrentPage|Integer|当前页码。|
|PageSize|Integer|每页记录数。|
|Total|Integer|总记录数。|
|DeployDetailList|List|分组部署详情列表。见下表 [DeployDetailList](#table_o5g_d4g_h2b)。|

|参数|类型|描述|
|:-|:-|:-|
|GroupId|Integer|分组ID。|
|Id|String|标识。|
|JobStatus|Integer|Job 状态。-   0：不存在。
-   2：运行中。
-   3：已完成。
-   4：失败。
-   11：正在部署。
-   12：正在停止。
-   31：部署失败。
-   32：停止失败。

|
|Type|String|类型。|
|Name|String|名称。|

## 示例 {#section_n4v_j4g_h2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListDeployDetail
&GroupId=136588
&CurrentPage=1
&PageSize=15
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "E67A8584-EFD3-4B76-AA86-C9C21339C19A",
    "Data": {
        "DeployDetailList": {
            "DeployDetailDTO": [
                {
                    "JobStatus": 0,
                    "Type": 1,
                    "Id": "29927d2d2a924d2e88d0e389612779c6",
                    "GroupId": 136588,
                    "Name": "rule1"
                },
                {
                    "JobStatus": 0,
                    "Type": 1,
                    "Id": "764fce393cf84a03b5921c4b04bb1c32",
                    "GroupId": 136588,
                    "Name": "rule2"
                }
            ]
        },
        "PageSize": 15,
        "CurrentPage": 1,
        "Total": 4
    },
    "Code": "200",
    "Success": true
}
```

