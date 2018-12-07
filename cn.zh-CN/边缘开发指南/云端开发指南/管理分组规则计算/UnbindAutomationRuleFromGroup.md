# UnbindAutomationRuleFromGroup {#reference_o33_jzg_h2b .reference}

从分组中解绑规则。

## 请求参数 {#section_ybz_kzg_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：UnbindAutomationRuleFromGroup。|
|GroupId|Long​|是|​分组ID。|
|AutomationRuleId|String|否|规则ID。|

## 返回参数 {#section_prn_szg_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|

## 示例 {#section_gg1_xzg_h2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UnbindAutomationRuleFromGroup
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

