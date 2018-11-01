# IoTData {#concept_v5s_5gp_32b .concept}

Link IoT Edge核心SDK允许开发人员使用Node.js编写FC函数。

包含IoT操作相关API的类。

## publish\(params, callback\) {#section_fw1_c3s_n2b .section}

发布指定消息。您可以通过消息路由功能进一步设置消息的流转路径。

|名称|类型|描述|
|--|--|--|
|params|Object|参数对象，包含如下必选参数。-   topic：\{String\}，消息主题。
-   payload：\{String\}，消息负载。

|
|callback\(err\)|function|回调函数。遵循JavaScript标准实践。成功则err为null，否则err包含发生的错误。|

## getThingProperties\(params, callback\) {#section_l3r_lhd_kfb .section}

获取指定的设备属性。

|名称|类型|描述|
|--|--|--|
|params|Object|参数对象，包含如下必选参数。-   productKey：\{String\}，设备的ProductKey，创建设备时生成。
-   deviceName：\{String\}，设备的DeviceName，创建设备时生成。
-   payload：\{Array\}，包含设备属性的数组。

|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。-   err：\{Error\}，成功则为null，否则为发生的错误。
-   data：\{Object\}，指定设备属性的键值。

|

## setThingProperties\(params, callback\) {#section_zjx_j3d_kfb .section}

设置指定的设备属性。

|名称|类型|描述|
|--|--|--|
|params|Object|参数对象，包含如下必选参数。-   productKey：\{String\}，设备的ProductKey，创建设备时生成。
-   deviceName：\{String\}，设备的DeviceName，创建设备时生成。
-   payload：\{Object\}，包含设置给设备的属性键值。

|
|callback\(err\)|function|回调函数。遵循JavaScript标准实践。-   err：\{Error\}，成功则返回null，否则为发生的错误。

|

## callThingService\(params, callback\) {#section_sbq_p3d_kfb .section}

调用指定的设备服务。

|名称|类型|描述|
|--|--|--|
|params|Object|参数对象，包含如下参数。-   productKey：\{String\} \(Required\)，设备的ProductKey，创建设备时生成。
-   deviceName：\{String\} \(Required\)，设备的DeviceName，创建设备时生成。
-   service：\{String\} \(Required\)，被调用的服务名。
-   payload：\{String|Buffer\}，传给服务的参数。

|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。-   err：\{Error\}，成功则返回null，否则为发生的错误。
-   data：被调用服务的返回值。

|

## getThingsWithTags\(params, callback\) {#section_bl2_q3d_kfb .section}

获取满足所有标签的设备信息。

|名称|类型|描述|
|--|--|--|
|params|Object|参数对象，包含如下必选参数。-   payload：\{Array\}，一个\{<标签名\>:<标签值\>\}组成的数组。

|
|callback\(err, data\)|function|回调函数。遵循JavaScript标准实践。-   err：\{Error\}，成功则返回null，否则为发生的错误。
-   data：\{Array\}，满足条件的设备信息数组。

|

