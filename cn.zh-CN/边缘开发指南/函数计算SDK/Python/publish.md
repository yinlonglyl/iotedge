# publish {#reference_qnw_xyh_wgb .reference}

调用该接口发布消息到自定义的Topic。

## publish\(params\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|dict|参数对象。需包含的必需参数，请参见下表[params参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|topic|String|接收消息的目标Topic。|
|payload|String|消息负载。|

|参数|类型|描述|
|:-|:-|:-|
|return|dict|调用成功，则返回成功信息；否则返回出错信息。**说明：** 返回的成功信息仅表示接口调用成功，不代表消息发送成功。

|

## 调用示例 {#section_ucp_5gj_wgb .section}

以下示例中定义每当有外部事件触发时，向`/topic/hello`这个Topic 发送一条消息。

```
# -*- coding: utf-8 -*-
import lecoresdk

lesdk = lecoresdk.IoTData()

def handler(event, context):
  pub_params = {"topic": "/topic/hello",
              "payload": "Hello World"}
  lesdk.publish(pub_params)
  print("Publish message to /topic/hello.")
  return 'Hello world'
```

