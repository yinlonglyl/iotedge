# callThingService {#reference_z1f_vyh_wgb .reference}

调用该接口调用指定设备的某项服务。

## callThingService\(params, callback\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|object|参数对象。需包含的必需参数，请参见表[params参数说明](#)。|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。具体请参见表[callback参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|productKey|String|设备所属产品的ProductKey，创建产品时，物联网平台为该产品生成的唯一标识。|
|deviceName|String|设备的DeviceName，创建设备时生成的名称。|
|service|String| 被调用的服务identifier。

 在物联网平台控制台，设备所属产品详情页的功能定义页，单击**查看物模型**，即可查看各服务的identifier。

 |
|payload|String|Buffer| 包含指定服务的输入参数的identifier和值。

 可以通过**查看物模型**，查看服务的输入参数identifier和取值范围。

 |

|参数|类型|描述|
|:-|:-|:-|
|err|Error| -   调用成功，err为null。
-   调用失败，err包含发生的错误信息。

 |
|data|object|被调用服务的返回值。|

## 调用示例 {#section_tcw_54j_wgb .section}

以下示例调用设备LightDev2的服务turn。

```
'use strict';

const leSdk = require('linkedge-core-sdk');
const iotData = new leSdk.IoTData();

const callServiceParams = {
  productKey: 'a1ZJTVsqj2y', // Please replace it with your Product Key.
  deviceName: 'LightDev2',   // Please replace it with your Device Name.
  service: 'turn',           // The service defined in the Product TSL.
  payload: {'LightSwitch': 0},
};
/* Promise wrapper for callThingService. */
function callThingService(params) {
  return new Promise((resolve, reject) => {
    iotData.callThingService(params, (err, data) => {
      err ? reject(err) : resolve(data);
    });
  });
}

exports.handler = function (event, context, callback) {
  callThingService(callServiceParams).then((data) => {
    console.log("-- Call service Success. Return " + JSON.stringify(data));
    callback(null);
  }).catch((err) => {
    console.log(err);
    callback(err);
  });
};
```

