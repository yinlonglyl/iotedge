# getThingsWithTags {#reference_txl_wyh_wgb .reference}

调用该接口根据标签获取设备列表。

## 说明 {#section_w1y_1dk_wgb .section}

若传入多个标签，仅返回满足所有标签条件的设备列表。

## getThingsWithTags\(params, callback\) {#section_fhl_1gj_wgb .section}

|参数|类型|描述|
|:-|:-|:-|
|params|object|参数对象。需包含的必需参数，请参见表[params参数说明](#)。|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。具体请参见表[callback参数说明](#)。|

|参数|类型|描述|
|:-|:-|:-|
|payload|Array| 包含标签信息。一个由\{<标签名\>:<标签值\>\}组成的数组。

 |

|参数|类型|描述|
|:-|:-|:-|
|err|Error| -   调用成功，err为null。
-   调用失败，err包含发生的错误信息。

 |
|data|Array|返回满足标签条件的设备信息数组。|

## 调用示例 {#section_mwt_xck_wgb .section}

以下示例查询标签为 `location: master bedroom` 的设备。

```
'use strict';

const leSdk = require('linkedge-core-sdk');
const iotData = new leSdk.IoTData();

const deviceTags = {
  payload: [{'location': 'master bedroom'}],
};
/* Promise wrapper for getThingsWithTags. */
function getThingsWithTags(params) {
  return new Promise((resolve, reject) => {
    iotData.getThingsWithTags(params, (err, things) => {
      err ? reject(err) : resolve(things);
    });
  });
}

exports.handler = function (event, context, callback) {
  getThingsWithTags(deviceTags).then((things) => {
    console.log(JSON.stringify(things));
    for(var i=0; i<things.length; i++) {
　    console.log('-- productKey='+things[i]["productKey"] + ', deviceName='+things[i]["deviceName"]);
    }
    callback(null);
  }).catch((err) => {
    console.log(err);
    callback(err);
  });
};
```

