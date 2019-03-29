# Python版本SDK {#concept_ycs_pcp_32b .concept}

Link IoT Edge提供Python版本的SDK，名称为lethingaccesssdk。本章为您介绍Python版本的SDK使用方法及相关SDK。Python版本开源的SDK源码请见[开源的Python库](https://github.com/aliyun/linkedge-thing-access-sdk-python)。

## 安装和使用 {#section_p2z_rvz_bhb .section}

1.  您可以通过如下命令来安装SDK：

    ```
    pip3 install lethingaccesssdk
    ```

    **说明：** 您也可以通过物联网边缘计算提供的[开发工具](../../../../../cn.zh-CN/用户指南/函数计算/开发工具/更多用法.md#)使用该SDK来开发驱动。

2.  安装完成后，您可以根据SDK接口进行驱动开发。

    **说明：** 完成驱动开发后，直接运行会提示错误，必须通过物联网平台控制台，将已开发的驱动部署到网关中方可执行。部署驱动到网关的操作请参考[驱动开发](../../../../../cn.zh-CN/用户指南/设备接入/驱动开发.md#)。

    使用SDK开发驱动的示例代码片段如下所示：

    ```
    # -*- coding: utf-8 -*-
    import logging
    import time
    import lethingaccesssdk
    from threading import Timer
    
    
    # Base on device, User need to implement the getProperties, setProperties and callService function.
    class Temperature_device(lethingaccesssdk.ThingCallback):
        def __init__(self):
            self.temperature = 41
            self.humidity = 80
    
        def getProperties(self, input_value):
            '''
            Get properties from the physical thing and return the result.
            :param input_value:
            :return:
            '''
            retDict = {
                "temperature": 41,
                "humidity": 80
            }
            return 0, retDict
    
        def setProperties(self, input_value):
            '''
            Set properties to the physical thing and return the result.
            :param input_value:
            :return:
            '''
            return 0, {}
    
        def callService(self, name, input_value):
            '''
            Call services on the physical thing and return the result.
            :param name:
            :param input_value:
            :return:
            '''
            return 0, {}
    
    
    def thing_behavior(client, app_callback):
        while True:
            properties = {"temperature": app_callback.temperature,
                          "humidity": app_callback.humidity}
            client.reportProperties(properties)
            client.reportEvent("high_temperature", {"temperature": 41})
            time.sleep(2)
    
    try:
        infos = lethingaccesssdk.Config().getThingInfos()
        for info in infos:
            app_callback = Temperature_device()
            client = lethingaccesssdk.ThingAccessClient(info)
            client.registerAndonline(app_callback)
            t = Timer(2, thing_behavior, (client, app_callback))
            t.start()
    except Exception as e:
        logging.error(e)
    
    # don't remove this function
    def handler(event, context):
        return 'hello world'
    ```


## getConfig\(\) {#section_nmq_stg_chb .section}

获取驱动相关配置信息。

返回值为驱动配置信息。

## ThingCallback {#section_jyj_lgq_kfb .section}

首先根据真实设备，命名一个类（如Demo\_device）继承ThingCallback，然后在该类（Demo\_device）中实现setProperties、getProperties和callService三个函数。

**setProperties**

设置具体设备属性函数。

|名称|类型|描述|
|--|--|--|
|properties|Dict|设置属性，取值格式为： ```
{
    "property1": "value1", 
    "property2": "value2"
}
```

 |

|名称|类型|描述|
|--|--|--|
|code|Integer|若获取成功则返回0，失败则返回非0错误码。|
|output|Dict|数据内容自定义，例如： ```
{
    "key1": xxx,
    "key2": yyy,
    ...
}
```

 若无返回数据，则值为空`{}`。

 |

**getProperties**

获取具体设备属性的函数。

|名称|类型|描述|
|--|--|--|
|keys|List|获取属性对应的名称，取值格式为： ```
['key1', 'key2']
```

 |

|名称|类型|描述|
|--|--|--|
|code|Integer|若获取成功则返回0，失败则返回非0错误码。|
|output|Dict|返回值，例如： ```
{
    'property1': xxx,
    'property2': yyy,
    ..}.
}
```

 |

**callService**

调用设备服务函数。

|名称|类型|描述|
|--|--|--|
|name|String|设备服务名称。|
|args|Dict|服务入参列表，取值格式为： ```
{
    "key1": "value1", 
    "key2": "value2"
}
```

 |

|名称|类型|描述|
|--|--|--|
|code|Integer|若获取成功则返回0，失败则返回非0错误码。|
|output|Dict|数据内容自定义，例如： ```
{
    "key1": xxx,
    "key2": yyy,
    ...
}
```

 若无返回数据，则值为空`{}`。

 |

## ThingAccessClient\(config\) {#section_fcl_xcx_tfb .section}

设备接入客户端，您可以通过该客户端来主动上报设备属性或事件，也可被动接受云端下发的指令。

|名称|类型|描述|
|--|--|--|
|config|Dict|包括云端分配的ProductKey和deviceName。 例如，

```
{
    "productKey": "xxx",
    "deviceName": "yyy"
}
```

 |

**registerAndOnline\(ThingCallback\)**

将设备注册到网关中并通知网关上线设备。设备需要注册并上线后，设备端才能收到云端下发的指令或者发送数据到云端。

|名称|类型|描述|
|--|--|--|
|ThingCallback|Object|设备的callback对象。|

**reportProperties\(properties\)**

主动上报设备事件。

|名称|类型|描述|
|--|--|--|
|eventName|String|事件对应的名称，与您在产品定义中创建事件的名称一致。|
|args|Dict|事件中包含的属性key与value，取值格式为： ```
{
    "key1": "value1", 
    "key2": "value2"
}
```

 |

**reportEvent\(eventName, args\)**

主动上报设备属性。

|名称|类型|描述|
|--|--|--|
|properties|Dict|属性中包含的属性key与value，取值格式为： ```
{
    "key1": "value1", 
    "key2": "value2"
}
```

 |

**getTsl\(\)**

返回TSL字符串，数据格式与云端一致。

返回值：

```
TSL字符串
```

**online\(\)**

通知网关设备上线，该接口一般在设备离线后再次上线时使用。

**offline\(\)**

通知网关设备已离线。

**cleanup\(\)**

资源回收接口，您可以使用该接口回收您的资源。

**unregister\(\)**

移除设备和网关的绑定关系。请谨慎使用该接口。

## Config {#section_az2_wdh_chb .section}

驱动相关配置信息。

**Config\(\)**

基于当前驱动配置字符串构造新的Config对象。

**getThingInfos\(\)**

返回所有设备相关信息。

返回值说明如下：

返回设备用于连接Link IoT Edge的配置信息的封装。

|名称|类型|描述|
|--|--|--|
|ThingInfo|List|设备信息。|

|名称|类型|描述|
|--|--|--|
|productKey|String|产品唯一标识。|
|deviceName|String|设备名称。|
|custom|Object|设备自定义配置。|

