# getThingProperties {#reference_onk_zyh_wgb .reference}

调用该接口获取指定设备的某项属性值。

## getThingProperties\(params\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|dict|请求参数对象。需包含的必需参数，请参见下表[params参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|productKey|String|设备所属产品的ProductKey，创建产品时，物联网平台为该产品生成的唯一标识。|
|deviceName|String|设备的DeviceName，创建设备时生成的名称。|
|payload|List| 包含要获取的设备属性的identifier数组。调用成功将返回该属性当前的对应值。

 在物联网平台控制台，设备所属产品详情页的功能定义页，单击**查看物模型**，即可查看各属性的identifier。

 |

|参数|类型|描述|
|:-|:-|:-|
|return|dict|调用成功，则返回要获取的设备属性的值；否则返回驱动上报的出错信息。|

## 调用示例 {#section_tcw_54j_wgb .section}

以下示例获取设备LightDev2的LightSwitch属性值（即开关状态）。

```
# -*- coding: utf-8 -*-
import lecoresdk

lesdk = lecoresdk.IoTData()

def handler(event, context):
  get_params = {"productKey": "a1ZJTVsqj2y", # Please replace it with your Product Key.
                "deviceName": "LightDev2",   # Please replace it with your Device Name.
                "payload": ["LightSwitch"]}  # The property defined in the Product TSL.
  res = lesdk.getThingProperties(get_params)
  print(res)
  if 'LightSwitch' in res.keys():
    print("-- [Success] LightSwitch = %s" % res['LightSwitch'])
    if res['LightSwitch'] == '0':
      print("-- Light is off.")
    else:
      print("-- Light is on.")
  else:
    print("-- [Warn] No property LightSwitch return.");
  return 'OK'
```

