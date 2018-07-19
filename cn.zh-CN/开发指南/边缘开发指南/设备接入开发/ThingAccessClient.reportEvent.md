# ThingAccessClient.reportEvent {#concept_c3j_qdp_32b .concept}

## 功能介绍 {#section_wgj_wv2_h2b .section}

主动上报设备事件。

## 请求参数 {#section_fvn_vfp_32b .section}

|名称|类型|描述|
|--|--|--|
|eventName|String|事件对应的名称，与您在产品定义中创建事件的名称一致。|
|args|Object|事件中包含的属性key与value，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

