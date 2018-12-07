# UpdateGroup {#reference_yrd_b21_h2b .reference}

修改分组名称或标签。

## 请求参数 {#section_rr5_c21_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：UpdateGroup。|
|Name|String|是|分组名称 。支持中文、英文大小写、数字、下划线（\_）和连字符（-）。长度不超过10个汉字或20个字符。|
|Tags|String|否|分组标签，格式为：key:value。key：不可为空； 不可重复（具有唯一性）；仅支持大小写英文字母；长度不可超过20个字符。

value：不可为空 ；支持中文、英文大小写、数字、下划线（\_）和连字符（-）；长度不可超过10个汉字或20个字符。

|
|GroupId|Long|是|分组ID。|

## 返回参数 {#section_ajt_r21_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|

## 示例 {#section_iwp_w21_h2b .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateGroup
&GroupId=135992
&Name=newName
&Tags=AAA:AAA
&公共请求参数
```

**返回示例**

```
{
  "RequestId": "28F3F065-CD1C-4C49-BEB4-DD822B1EB999",
  "Success": true,
  "Code": "200"
}
```

