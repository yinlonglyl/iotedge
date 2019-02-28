# setThingProperties {#reference_ffd_1zh_wgb .reference}

调用该接口为指定设备设置某项属性值。

## setThingProperties\(params\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|dict|请求参数对象。需包含的必需参数，请参见下表[params参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|productKey|String|设备所属产品的ProductKey，创建产品时，物联网平台为该产品生成的唯一标识。|
|deviceName|String|设备的DeviceName，创建设备时生成的名称。|
|payload|dict| 包含属性identifier和要设置的属性值。

 在物联网平台控制台，设备所属产品详情页的功能定义页，单击**查看物模型**，即可查看各属性的identifier和取值范围。

 |

|参数|类型|描述|
|:-|:-|:-|
|return|dict|调用成功，则返回true；否则返回驱动上报的出错信息。|

## 调用示例 {#section_szs_crj_wgb .section}

以下示例设置设备LightDev的LightSwitch属性值为0（即“关”状态）。

```
# -*- coding: utf-8 -*-
import lecoresdk

lesdk = lecoresdk.IoTData()

def handler(event, context):
  set_params = {"productKey": "a1ZJTVsqj2y", # Please replace it with your Product Key.
                "deviceName": "LightDev",    # Please replace it with your Device Name.
                "payload": {"LightSwitch":0}}# The property defined in the Product TSL.
  res = lesdk.setThingProperties(set_params)
  print(res)
  return 'OK'
```

