# Publish {#reference_zfy_ryh_wgb .reference}

调用该接口发布消息到自定义的Topic。

## publish\(params,callback\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|object|参数对象。需包含的必需参数，请参见下表[params参数说明](#)。|
|callback\(err\)|function|回调函数。遵循JavaScript标准实践。-   调用成功，err为null。
-   调用失败，err包含发生的错误信息。

|

|参数|类型|描述|
|:-|:-|:-|
|topic|String|接收消息的目标Topic。|
|payload|String|消息负载。|

## 调用示例 {#section_ucp_5gj_wgb .section}

以下示例中定义为每当有外部事件触发时，向`/hello/world`这个Topic 发送一条消息。

```
'use strict';

const leSdk = require('linkedge-core-sdk');
const iotData = new leSdk.IoTData();

exports.handler = function (event, context, callback) {
  var message = {
    topic: '/hello/world',
    payload: 'Hello World.',
  };
  iotData.publish(message, (err, data)=> {
    if (err == null) {
      console.log("-- Publish HelloWorld success.");
    }
    callback(err);
  });
};
```

