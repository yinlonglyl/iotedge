# getThingProperties {#reference_ejg_tyh_wgb .reference}

调用该接口获取指定设备的某项属性值。

## getThingProperties\(params, callback\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|object|参数对象。需包含的必需参数，请参见表[params参数说明](#)。|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。具体请参见表[callback参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|productKey|String|设备所属产品的ProductKey，创建产品时，物联网平台为该产品生成的唯一标识。|
|deviceName|String|设备的DeviceName，创建设备时生成的名称。|
|payload|Array| 包含要获取的设备属性的identifier数组。调用成功将返回该属性当前的对应值。

 在物联网平台控制台，设备所属产品详情页的功能定义页，单击**查看物模型**，即可查看各属性的identifier。

 |

|参数|类型|描述|
|:-|:-|:-|
|err|Error| -   调用成功，err为null。
-   调用失败，err包含发生的错误信息。

 |
|data|object|返回指定设备属性的值。|

## 调用示例 {#section_tcw_54j_wgb .section}

以下示例获取设备LightDev2的LightSwitch属性值（即开关状态）。

```
'use strict';

const leSdk = require('linkedge-core-sdk');
const iotData = new leSdk.IoTData();

const getPropertiesParams = {
  productKey: 'a1ZJTVsqj2y', // Please replace it with your Product Key.
  deviceName: 'LightDev2',   // Please replace it with your Device Name.
  payload: ['LightSwitch']   // The property defined in the Product TSL.
};
/* Promise wrapper for getThingProperties. */ 
function getThingProperties(params) {
  return new Promise((resolve, reject) => {
    iotData.getThingProperties(params, (err, data) => {
      err ? reject(err) : resolve(data);
    });
  });
}

exports.handler = function (event, context, callback) {
  getThingProperties(getPropertiesParams).then((data) => {
    console.log(data); // Return value example: { LightSwitch: 0 } 
    if (null == data.LightSwitch) {
      console.log("-- [Warn] No property LightSwitch return.");
    } else {
      console.log("-- [Success] LightSwitch = " + data.LightSwitch);
      if (data.LightSwitch == '0') {
        console.log("-- Light is off.");
      } else {
        console.log("-- Light is on.");
      }
    }
    callback(null);
  }).catch((err) => {
    console.log(err);
    callback(err);
  });
};
```

