# Group.getDevicesByProductKey {#concept_bdx_ygp_32b .concept}

## 功能介绍 {#section_l31_wdf_h2b .section}

通过设备的ProductCode获取设备列表。

## 请求参数 {#section_lkr_3jp_32b .section}

|名称|类型|描述|
|--|--|--|
|productKey|String|设备对应的产品唯一标识符。|
|callback|Function|[回调函数](#callback3)。|

**回调参数**

|名称|类型|描述|
|--|--|--|
|err|Error|错误对象，当有错误时Error非空。|
|deviceList|Map<Device\>|设备列表。|

## 使用示例 {#section_kkr_kjp_32b .section}

```
module.exports.handler = function(event, context, callback) {
    const linkedge = require("linkedge");
    var group = linkedge.getGroupByEvent(event);
    group.getDevicesByProductKey("a1lDXTP1NPK", function(err, device) {
        if(err){
        // 发生错误的逻辑            
        }else{
        // 未发生错误的逻辑            
        }
    });
};
```

