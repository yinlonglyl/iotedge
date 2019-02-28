# setThingProperties {#reference_n4j_5yh_wgb .reference}

调用该接口为指定设备设置某项属性值。

## setThingProperties\(params, callback\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|object|参数对象。需包含的必需参数，请参见表[params参数说明](#)。|
|callback\(err\)|function|回调函数。遵循JavaScript标准实践。-   调用成功，err为null。
-   调用失败，err包含发生的错误信息。

|

|参数|类型|描述|
|:-|:-|:-|
|productKey|String|设备所属产品的ProductKey，创建产品时，物联网平台为该产品生成的唯一标识。|
|deviceName|String|设备的DeviceName，创建设备时生成的名称。|
|payload|Object| 包含属性identifier和要设置的属性值。

 在物联网平台控制台，设备所属产品详情页的功能定义页，单击**查看物模型**，即可查看各属性的identifier和取值范围。

 |

## 调用示例 {#section_szs_crj_wgb .section}

以下示例设置设备LightDev2的LightSwitch属性值为1（即“开”状态）。

```
'use strict';

const leSdk = require('linkedge-core-sdk');
const iotData = new leSdk.IoTData();

const setPropertiesParams = {
  productKey: 'a1ZJTVsqj2y', // Please replace it with your Product Key.
  deviceName: 'LightDev2',   // Please replace it with your Device Name.
  payload: {'LightSwitch': '1'} // The property defined in the Product TSL.
};
/* Promise wrapper for setThingProperties. */ 
function setThingProperties(params) {
  return new Promise((resolve, reject) => {
    iotData.setThingProperties(params, (err, data) => {
      err ? reject(err) : resolve(data);
    });
  });
}

exports.handler = function (event, context, callback) {
  setThingProperties(setPropertiesParams).then((data) => {
    console.log("-- Set Property Success. Return " + JSON.stringify(data));
    callback(null);
  }).catch((err) => {
    console.log(err);
    callback(err);
  });
};
```

