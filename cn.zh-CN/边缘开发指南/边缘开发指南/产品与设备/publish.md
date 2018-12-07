# publish {#concept_dhg_wgp_32b .concept}

## 功能介绍 {#section_ihx_vdf_h2b .section}

发布消息。您可以通过消息路由，配置该消息流转的目的地。

## 请求参数 {#section_kxv_php_32b .section}

|名称|类型|描述|
|--|--|--|
|topic|String|消息的主题。-   如果您发送的数据需在边缘流转，则topic命名规则为：支持英文大小写、数字、斜线、下划线和短划线，长度不超过256个字符。
-   如果您发送的数据需要上云端，则topic命名规则请参考[Topic数据格式](../../../../cn.zh-CN/用户指南/规则引擎/Topic数据格式.md#)。

|
|payload|String|发布的消息体，数据内容由您自行决定。|
|callback|Function|[回调函数](#callback1)。|

**回调参数**

|名称|类型|描述|
|--|--|--|
|err|Error|错误对象，当有错误时Error非空。|
|status|Boolean|发布消息的结果。true表示发送成功，false表示发送失败。|

## 返回参数 {#section_pfp_23p_32b .section}

无。

## 使用示例 {#section_z1t_f3p_32b .section}

```
//发送消息
linkedge.publish("/xxx/topic", "{\"payload\" : \" xxx_test\"}",
function(err, status) {
    if (!err && status) {
        log.LOGI(TAG, "sendSignal xxx_topic ok");
    }
});
```

