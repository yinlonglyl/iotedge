# ListGroupDevice {#reference_dxq_ly2_h2b .reference}

分页查询分组中的设备。

## 请求参数 {#section_n3v_hz2_h2b .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：ListGroupDevice。|
|GroupId|Long|是|​分组ID。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值100，默认值是10。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。默认值 1。|

## 返回参数 {#section_ows_sz2_h2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。200表示成功，其他返回码表示错误。|
|Data|[Data](#table_bhy_2bf_h2b)|调用成功时，返回的数据。具体参数见下表。|

|参数|类型|描述|
|:-|:-|:-|
|PageSize|Integer|每页记录数。|
|CurrentPage|Integer|当前页。|
|Total|Integer|总记录数。|
|GroupDeviceList|List|具体参数，请见下表 [GroupDeviceList](#table_rg5_nbf_h2b)。|

|参数|类型|描述|
|:-|:-|:-|
|LastOnlineTime|String|最新上线时间。|
|Status|String|设备状态。-   inactive：未激活。
-   online：在线 。
-   offline：离线。
-   disable：无效。

|
|ProductName|String|产品名称。|
|ProductKey|String|产品Key。|
|DeviceName|String|设备名称。|

## 示例 {#section_zs2_wbf_h2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListGroupDevice
&GroupId=134429
&CurrentPage=1
&PageSize=10
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "DF3247F9-AC12-4121-840C-F917C128C9B4",
    "Data": {
        "PageSize": 15,
        "CurrentPage": 1,
        "Total": 2,
        "GroupDeviceList": {
            "GroupDevice": [
                {
                    "Status": "inactive",
                    "ProductName": "product1",
                    "ProductKey": "a1XXXXXXXXX",
                    "DeviceName": "K7WHfrRAvhKPMKZXXXXX"，
                },
                {
                    "Status": "offline",
                    "LastOnlineTime": "2018-06-12 09:40:59",
                    "ProductName": "product2",
                    "ProductKey": "a1XXXXXXXXX",
                    "DeviceName": "SU8FbXAtoe8LqPDXXXXX"
                }
            ]
        }
    },
    "Code": "200",
    "Success": true
}
```

