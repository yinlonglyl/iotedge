# Device.$\{method\} {#concept_ugm_chp_32b .concept}

## 功能介绍 {#section_jrd_wdf_h2b .section}

调用设备服务。

## 请求参数 {#section_xn5_vjp_32b .section}

|名称|类型|描述|
|--|--|--|
|intputParam1|Object|根据您定义的产品的服务入参类型决定|
|intputParam2|Object|根据您定义的产品的服务入参类型决定|
|……|Object|请求参数个数由您的定义决定。|
|callback|Function|[回调函数](#callback6)。|

**回调参数**

|名称|类型|描述|
|--|--|--|
|err|Error|错误对象，当有错误时Error非空。|
|value|Object|服务返回参数。|

## 使用示例 {#section_qjd_xjp_32b .section}

```
module.exports.handler = function(event, context, callback) {
    const linkedge = require("linkedge");
    var group = linkedge.getGroupByEvent(event);
    group.getDeviceByProductKeyDeviceName("a1lDXTP1NPK", "myDeviceName", function(err, device) {
        if(err){
        // 发生错误的逻辑            
        }else{
            // 未发生错误的逻辑  
            // 假设您定义的函数名称为open，无参数
            device.open(function(err, value){ 
              var outputParam = value
            });  
        }
    });};
```

