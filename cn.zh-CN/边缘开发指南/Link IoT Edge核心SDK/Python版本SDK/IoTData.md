# IoTData {#concept_v5s_5gp_32b .concept}

Link IoT Edge核心SDK同时允许开发人员使用Python编写FC函数。核心SDK源码已开源，详情请见[核心SDK开源的Python库](https://github.com/aliyun/linkedge-core-sdk-python)。

包含IoT操作相关API的类。

## publish\(params\) {#section_fw1_c3s_n2b .section}

发布指定消息。您可以通过消息路由功能进一步设置消息的流转路径。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含如下必选参数。-   topic：\{str\}，消息主题。
-   payload：\{str\}，消息负载。

|

## getThingProperties\(params\) {#section_l3r_lhd_kfb .section}

获取指定的设备属性。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含如下必选参数。-   productKey：\{str\}，设备的ProductKey，创建设备时生成。
-   deviceName：\{str\}，设备的DeviceName，创建设备时生成。
-   payload：\{list\}，包含设备属性的数组。

|
|return|dict|指定设备属性的键值。|

## setThingProperties\(params\) {#section_zjx_j3d_kfb .section}

设置指定的设备属性。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含如下必选参数。-   productKey：\{str\}，设备的ProductKey，创建设备时生成。
-   deviceName：\{str\}，设备的DeviceName，创建设备时生成。
-   payload：\{dict\}，包含设置给设备的属性键值。

|
|return|dict|成功则返回True，否则为发生的错误。|

## callThingService\(params\) {#section_sbq_p3d_kfb .section}

调用指定的设备服务。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含如下参数。-   productKey：\{str\} \(Required\)，设备的ProductKey，创建设备时生成。
-   deviceName：\{str\} \(Required\)，设备的DeviceName，创建设备时生成。
-   service：\{str\} \(Required\)，被调用的服务名。
-   payload：\{str|Bytes\}，传给服务的参数。

|
|return|dict|被调用服务的返回值。|

## getThingsWithTags\(params\) {#section_bl2_q3d_kfb .section}

获取满足所有标签的设备信息。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含如下必选参数。-   payload：\{list\}，一个由\{<标签名\>:<标签值\>\}组成的数组。

|
|return|list|满足条件的设备信息数组。|

