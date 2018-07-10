# BindAutomationRuleToGroup {#reference_v5m_nyg_h2b .reference}

绑定规则至分组。

## 请求参数 {#section_rn1_pyg_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：BindAutomationRuleToGroup。|
|GroupId|Long​|是|​分组ID。|
|AutomationRuleId|String|是|规则ID。|

## 返回参数 {#section_ecr_yyg_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|​阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|Integer|接口返回码。200表示成功，其他返回码表示错误。|

## 示例 {#section_nh2_fzg_h2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BindAutomationRuleToGroup
&GroupId=134429
&AutomationRuleId=7fc7b181a9be43ef938ad58f67d70d4b
&公共请求参数
```

**返回示例**

```
{
  "RequestId": "D832E764-A75C-474E-B9C3-AA75E9D579E6",
  "Success": true,
  "Code": "200"
}
```

