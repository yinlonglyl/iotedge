# getThingsWithTags {#reference_wf4_czh_wgb .reference}

调用该接口根据标签获取设备列表。

## 限制说明 {#section_w1y_1dk_wgb .section}

若传入多个标签，仅返回满足所有标签条件的设备列表。

## getThingsWithTags\(params\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|dict|参数对象。需包含的必需参数，请参见表[params参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|payload|List|标签信息数组。标签是由\{"key":"value"\}组成的数组。|

|参数|类型|描述|
|:-|:-|:-|
|return|dict|调用成功，则返回满足条件的设备信息数组；否则返回驱动上报的出错信息。|

## 调用示例 {#section_mwt_xck_wgb .section}

以下示例查询标签为 `location: master bedroom` 的设备。

```
# -*- coding: utf-8 -*-
import lecoresdk

lesdk = lecoresdk.IoTData()

def handler(event, context):
  get_params = {"payload": [{"location":"master bedroom"}]}
  res = lesdk.getThingsWithTags(get_params)
  print(res)
  for device in res:
    print("device_%s: PK=%s DN=%s" % (res.index(device) + 1, device['productKey'], device['deviceName']))
  return 'OK'
```

