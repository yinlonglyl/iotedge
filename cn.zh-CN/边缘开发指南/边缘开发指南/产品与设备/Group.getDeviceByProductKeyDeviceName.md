# Group.getDeviceByProductKeyDeviceName {#concept_gp5_xgp_32b .concept}

## 功能介绍 {#section_nfz_vdf_h2b .section}

通过设备的ProductKey和DeviceName获取设备对象。

## 请求参数 {#section_dh5_djp_32b .section}

|名称|类型|描述|
|--|--|--|
|productKey|String|设备对应的产品唯一标识符。|
|deviceName|String|设备对应的设备名称。|
|callback|Function|[回调函数](#callback2)。|

**回调参数**

|名称|类型|描述|
|--|--|--|
|err|Error|错误对象，当有错误时Error非空。|
|device|Device|设备对象。|

## 使用示例 {#section_hv1_gjp_32b .section}

```
module.exports.handler = function(event, context, callback) {
    const linkedge = require("linkedge");
    var group = linkedge.getGroupByEvent(event);
    group.getDeviceByProductKeyDeviceName("a1lDXTP1NPK", "myDeviceName", function(err, device) {
        if(err){
        // 发生错误的逻辑            
        }else{
        // 未发生错误的逻辑            
        }
    });
};
```

