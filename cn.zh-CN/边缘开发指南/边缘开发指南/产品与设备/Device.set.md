# Device.set {#concept_txz_1hp_32b .concept}

## 功能介绍 {#section_c2c_wdf_h2b .section}

设置设备属性。

## 请求参数 {#section_bbw_rjp_32b .section}

|名称|类型|描述|
|--|--|--|
|key|String|属性名称。|
|value|Object|根据您定义的产品属性值类型确定Object具体类型。|
|callback|Function|[回调函数](#callback5)。|

**回调参数**

|名称|类型|描述|
|--|--|--|
|err|Error|错误对象，当有错误时Error非空。|
|status|Boolean|设置属性结果。true表示设置成功，false表示设置失败。|

## 使用示例 {#section_zqn_tjp_32b .section}

```
module.exports.handler = function(event, context, callback) {
    const linkedge = require("linkedge");
    var group = linkedge.getGroupByEvent(event);
    group.getDeviceByProductKeyDeviceName("a1lDXTP1NPK", "myDeviceName", function(err, device) {
        if(err){
        // 发生错误的逻辑            
        }else{
            // 未发生错误的逻辑  
            device.set("temperature",12,function(err, status){ 
                // 如果需要获取结果可以通过status进行判断}
                );          
        }
    });};
```

