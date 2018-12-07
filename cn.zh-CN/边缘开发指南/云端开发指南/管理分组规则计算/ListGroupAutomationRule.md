# ListGroupAutomationRule {#reference_mjp_phg_h2b .reference}

查询分组的规则详情列表。

## 请求参数 {#section_ukl_shg_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：ListGroupAutomationRule。|
|GroupId|Integer​|是|​分组ID。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值100，默认值是10。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。默认值 1。|

## 返回参数 {#section_uss_23g_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|
|CurrentPage|Integer|当前页。|
|PageSize|Integer|每页记录数。|
|Total|Integer|总记录数。|
|AutomationRuleList|List|规则列表。具体参数请见下表。|

|参数|类型|描述|
|:-|:-|:-|
|JobStatus|Integer|Job 状态。-   0：不存在。
-   2：运行中。
-   3：已完成。
-   4：失败。
-   11：正在部署。
-   12：正在停止。
-   31：部署失败。
-   32：停止失败。

|
|AutomationRuleId|String|规则ID|
|RuleName|String|规则名称。|
|GmtCreate|String|创建时间。|
|IsExisted|Integer|规则计算是否存在。取值：1：是；0：否。|

## 示例 {#section_ff3_kkg_h2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListGroupAutomationRule
&GroupId=136588
&CurrentPage=1
&PageSize=15
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "1184867D-778A-4F06-869E-5311B7D7FCAD",
    "Data": {
        "PageSize": 15,
        "CurrentPage": 1,
        "Total": 1,
        "AutomationRuleList": {
            "GroupSceneInfo": [
                {
                    "IsExisted": 1,
                    "JobStatus": 2,
                    "AutomationRuleId": "8d97902797b74cb297be46a832bc45d3",
                    "GmtCreate": "2018-06-20 19:50:03",
                    "RuleName": "automation_rule"
                }
            ]
        }
    },
    "Code": "200",
    "Success": true
}
```

