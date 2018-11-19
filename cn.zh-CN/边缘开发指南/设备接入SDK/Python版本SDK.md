# Python版本SDK {#concept_ycs_pcp_32b .concept}

本章为您介绍Python版本的SDK使用方法及相关API。Link IoT Edge提供Python版本的SDK，名称为lethingaccesssdk。Python版本开源的SDK源码请见[开源的Python库](https://github.com/aliyun/linkedge-thing-access-sdk-python)。

## Python版本设备接入SDK使用说明 {#section_o42_tbs_n2b .section}

在使用设备接入SDK前需要在您的代码库中导入import lethingaccesssdk。

## ThingCallback {#section_jyj_lgq_kfb .section}

首先根据真实设备，命名一个类（如Demo\_device）继承ThingCallback，然后在该类（Demo\_device）中实现[setProperties](#)、[getProperties](#)和[callService](#)三个函数。

## setProperties {#section_test1 .section}

**功能介绍**

设置具体设备属性函数。

**请求参数**

|名称|类型|描述|
|--|--|--|
|properties|dict|设置属性，取值格式为：```
{
    "property1": "value1", 
    "property2": "value2"
}
```

|

**返回参数**

|名称|类型|描述|
|--|--|--|
|code|int|若获取成功则返回LEDA\_SUCCESS，失败则返回错误码。|
|output|dict|数据内容自定义，例如：```
{
    "key1": xxx,
    "key2": yyy,
    ...
}
```

若无返回数据，则值为空`{}`。

|

## getProperties {#section_test2 .section}

**功能介绍**

获取具体设备属性的函数。

**请求参数**

|名称|类型|描述|
|--|--|--|
|keys|list|获取属性对应的名称，取值格式为：```
['key1', 'key2']
```

|

**返回参数**

|名称|类型|描述|
|--|--|--|
|code|int|若获取成功则返回LEDA\_SUCCESS，失败则返回错误码。|
|output|dict|返回值，例如：```
{
    'property1': xxx,
    'property2': yyy,
    ..}.
}
```

|

## callService {#section_c5h_wcx_tfb .section}

**功能介绍**

调用设备服务函数。

**请求参数**

|名称|类型|描述|
|--|--|--|
|name|str|设备服务名称。|
|args|dict|服务入参列表，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

**返回参数**

|名称|类型|描述|
|--|--|--|
|code|int|若获取成功则返回LEDA\_SUCCESS，失败则返回错误码。|
|output|dict|数据内容自定义，例如：```
{
    "key1": xxx,
    "key2": yyy,
    ...
}
```

若无返回数据，则值为空`{}`。

|

## ThingAccessClient\(config\) {#section_fcl_xcx_tfb .section}

**功能介绍**

设备接入客户端，您可以通过该客户端来主动上报设备属性或事件，也可被动接受云端下发的指令。

**请求参数**

|名称|类型|描述|
|--|--|--|
|config|dict|包括云端分配的ProductKey和deviceName。例如，

```
{
    "productKey": "xxx",
    "deviceName": "yyy"
}
```

|

## registerAndOnline\(ThingCallback\) {#section_gk2_g52_h2b .section}

**功能介绍**

将设备注册到边缘节点中并通知边缘节点上线设备。设备需要注册并上线后，设备端才能收到云端下发的指令或者发送数据到云端。

**请求参数**

|名称|类型|描述|
|--|--|--|
|ThingCallback|object|设备的callback对象。|

## reportEvent\(eventName, args\) {#section_zty_3jm_n2b .section}

**功能介绍**

主动上报设备事件。

**请求参数**

|名称|类型|描述|
|--|--|--|
|eventName|str|事件对应的名称，与您在产品定义中创建事件的名称一致。|
|args|dict|事件中包含的属性key与value，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

## reportProperties\(properties\) {#section_uhx_wv2_h2b .section}

**功能介绍**

主动上报设备属性。

**请求参数**

|名称|类型|描述|
|--|--|--|
|properties|dict|属性中包含的属性key与value，取值格式为：```
{
    "key1": "value1", 
    "key2": "value2"
}
```

|

## getTsl\(\) {#section_w4g_g52_h2b .section}

**功能介绍**

返回TSL字符串，数据格式与云端一致。

**返回参数**

```
TSL字符串
```

## online\(\) {#section_ath_g52_h2b .section}

**功能介绍**

通知边缘节点设备上线，该接口一般在设备离线后再次上线时使用。

## offline\(\) {#section_wq3_g52_h2b .section}

**功能介绍**

通知边缘节点设备已离线。

## unregister\(\) { .section}

**功能介绍**

移除设备和边缘节点的绑定关系。请谨慎使用该接口。

