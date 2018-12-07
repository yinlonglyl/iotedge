# ThingAccessClient {#concept_d2w_xcp_32b .concept}

## 功能介绍 {#section_a3w_ys2_h2b .section}

设备接入客户端，您可以通过该客户端来主动上报设备属性或事件，也可被动接受云端下发的指令。

## 请求参数 {#section_gqv_x2p_32b .section}

|名称|类型|描述|
|config|Object|元数据，用于配置该客户端。取值格式为：```
{
    "productKey": "Your Product Key", 
    "deviceName": "Your Device Name"
}
```

|
|callbacks|Object|回调函数对象。-   指定设置设备属性的回调参数，请参见[callbacks.setProperties](#section_ej1_g52_h2b)
-   指定获取设备属性的回调参数，请参见[callbacks.getProperties](#section_r22_5bn_j2b)
-   指定调用设备服务的回调参数，请参见[callbacks.callService](#section_wwf_5bn_j2b)

|

**回调说明**

```
callbacks: {
  setProperties: function(properties) {},
  getProperties: function(keys) {},
  callService: function(name, args) {}
}
```

## callbacks.setProperties {#section_ej1_g52_h2b .section}

**功能介绍**

设置具体设备属性的回调函数。通过回调函数，实现设置设备的属性。

**请求参数**

|名称|类型|描述|
|--|--|--|
|properties|Object|设置属性的对象，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

## callbacks.getProperties {#section_r22_5bn_j2b .section}

**功能介绍**

获取具体设备属性的回调函数。通过回调函数，实现获取设备的属性。

**请求参数**

|名称|类型|描述|
|--|--|--|
|keys|String\[\]|获取属性对应的名称，取值格式为：\['key1', 'key2'\]

|

## callbacks.callService {#section_wwf_5bn_j2b .section}

**功能介绍**

调用设备服务回调函数。通过回调函数，实现调动设备服务。

**请求参数**

|名称|类型|描述|
|--|--|--|
|name|String|设备服务名称。|
|args|Object|服务入参列表，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

