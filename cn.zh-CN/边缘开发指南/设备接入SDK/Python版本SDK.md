# Python版本SDK {#concept_ycs_pcp_32b .concept}

本章为您介绍Python版本的SDK使用方法及相关API。Link IoT Edge提供Python版本的SDK，名称为lethingaccesssdk。

## Python版本设备接入SDK使用说明 {#section_o42_tbs_n2b .section}

在使用设备接入SDK前需要执行import lethingaccesssdk命令，将lethingaccesssdk（Python版本SDK）导入到您自己的代码库中。

如下示例展示了使用Python版本的SDK进行开发操作：

```
import lethingaccesssdk
import time

class Demo_device(lethingaccesssdk.ThingCallback):
  def __init__(self):
    self.LightSwitch = 1

  def callService(self, name, input_value):
    pass

  def getProperties(self, input_value):
    pass

  def setProperties(self, input_value):
    pass

app_callback = Demo_device()
client = lethingaccesssdk.ThingAccessClient("YourProductKey", "YourDeviceName")
client.registerAndonline(app_callback)

while True:
  time.sleep(3)
  propertiesDict={"LightSwitch":app_callback.LightSwitch}
  client.reportProperties(propertiesDict)

def handler(event, context):
  
  return 'hello world'
```

## ThingAccessClient {#section_qyh_4gm_n2b .section}

设备接入客户端，您可以通过该客户端来主动上报设备属性或事件，也可被动接受云端下发的指令。

**常量定义**

|名称|类型|描述|
|--|--|--|
|PRODUCT\_KEY|String|传给ThingAccessClient构造函数的键值，指定云端分配的productKey。|
|DEVICE\_NAME|String|传给ThingAccessClient构造函数的键值，指定云端分配的deviceName。|

## ThingAccessClient\(productKey，deviceName\) {#section_tcz_ydr_n2b .section}

**功能介绍**

构造函数，使用您自己设备的指定的ProductKey和DeviceName构造。

## ThingCallback {#section_jyj_lgq_kfb .section}

需要先实现一个类来继承ThingCallback，然后再实现[setProperties](#)、[getProperties](#)和[callService](#)三个函数。

## setProperties {#section_test1 .section}

**功能介绍**

设置具体设备属性函数。

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

## getProperties {#section_test2 .section}

**功能介绍**

获取具体设备属性的函数。

**请求参数**

|名称|类型|描述|
|--|--|--|
|keys|String\[\]|获取属性对应的名称，取值格式为：\['key1', 'key2'\]

|

## callService {#section_test3 .section}

**功能介绍**

调用设备服务函数。

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

## registerAndOnline\(ThingCallback object\) {#section_gk2_g52_h2b .section}

**功能介绍**

将设备注册到边缘节点中并通知边缘节点上线设备。设备需要注册并上线后，设备端才能收到云端下发的指令或者发送数据到云端。

## reportEvent\(eventName, args\) {#section_zty_3jm_n2b .section}

**功能介绍**

主动上报设备事件。

**请求参数**

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

## reportProperties\(properties\) {#section_uhx_wv2_h2b .section}

**功能介绍**

主动上报设备属性。

**请求参数**

|名称|类型|描述|
|--|--|--|
|properties|Object|属性中包含的属性key与value，取值格式为：```
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

## unregister\(\) {#section_hdh_g52_h2b .section}

**功能介绍**

从边缘计算节点移除设备。请谨慎使用该接口。

## online\(\) {#section_ath_g52_h2b .section}

**功能介绍**

通知边缘节点设备上线，该接口一般在设备离线后再次上线时使用。

## offline\(\) {#section_wq3_g52_h2b .section}

**功能介绍**

通知边缘节点设备已离线。

