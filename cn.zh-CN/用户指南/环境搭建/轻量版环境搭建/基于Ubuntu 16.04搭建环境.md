# 基于Ubuntu 16.04搭建环境 {#concept_s4l_hft_lgb .concept}

本文介绍基于Ubuntu 16.04系统搭建Link IoT Edge轻量版本（LE Lite）运行环境的方法。

## 前提条件 {#section_ppn_nby_dhb .section}

-   Link IoT Edge的远程登录功能依赖设备的SSH服务，请确保设备上已经开启了SSH服务。SSH的详细信息可参考[OpenSSH的使用](https://help.ubuntu.com/lts/serverguide/openssh-server.html.en?spm=a2c4g.11186623.2.11.3ce2427aDPqjvW&file=openssh-server.html.en)。
-   请确保设备的本地环回端口已启用，即在设备上执行ping 127.0.0.1命令时，返回结果正常。

## 创建边缘实例和网关 {#section_jt5_12y_dhb .section}

1.  [物联网平台控制台](http://iot.console.aliyun.com/)，选择**边缘计算** \> **边缘实例**。
2.  创建一个边缘实例。
    1.  单击**新增实例**，在弹出窗口中设置**实例名称**。
    2.  在**网关产品**后单击**新建网关产品**，为实例创建网关。

        物联网边缘计算中的网关，承载边缘计算能力，每个实例必须分配一个网关设备，并且该网关设备同一时间只能被分配到一个边缘实例。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155356637037158_zh-CN.png)

    3.  在新建产品页面中，设置网关产品参数，然后单击**完成**。

        物联网边缘计算中的新建网关产品功能继承物联网平台**设备管理** \> **产品**中，高级版产品的功能。已自动为您简化创建适合物联网边缘计算中使用的网关产品步骤。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155356637037159_zh-CN.png)

        参数说明如下：

        |参数|说明|
        |--|--|
        |产品名称|为网关产品设置名称，用于后续的查询及识别网关产品。支持中文、英文字母、数字和下划线，长度限制4~30，一个中文汉字算2位。|
        |所属分类|选择品类，为该产品定义[物模型](../cn.zh-CN/用户指南/产品与设备/物模型/概述.md#)。可选择为：

        -   **自定义品类**：需根据实际需要，定义产品功能。
        -   任一既有功能模板。

选择任一物联网平台预定义的品类，快速完成产品的功能定义。选择产品模板后，您可以在该模板基础上，编辑、修改、新增功能。

若您需要的网关没有特殊功能定义，建议您选择**自定义品类**。

|
        |产品描述|可输入文字，用来描述产品信息。字数限制为100。可以为空。|

        产品创建成功后，页面自动跳转回新增实例页面，并且**网关产品**参数下自动分配了刚创建的网关产品。

    4.  在新增实例页面，单击**网关设备**后的**新建网关设备**为网关产品添加设备。

        物联网边缘计算中的新建网关设备功能继承物联网平台**设备管理** \> **设备**的功能。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155356637037160_zh-CN.png)

    5.  根据界面提示设置参数后，单击**确认**。

        参数说明如下：

        |参数|描述|
        |--|--|
        |产品|系统已自动关联上一步创建的网关产品。|
        |设备名称|为该网关设备命名。设备名称需保持产品内唯一。如不填写，系统将自动生成。**说明：** 设备名称支持大写字母\[A-Z\]、小写字母\[a-z\]、数字\[0-9\]和下划线（\_）。且不能以下划线开头或结尾。

|

    6.  （可选）在新增实例页面，单击**新增标签**，可以设置实例标签。通过标签您可以更加有效地归类及识别实例。您也可以不设置标签。
3.  实例参数设置完成后，单击**确定**，至此您已创建边缘实例和网关。
4.  在**实例详情** \> **实例信息**页面，**网关**页签下，单击网关名称右侧的**查看**，获取网关设备信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155356637037161_zh-CN.png)

5.  系统跳转到网关设备的设备详情页面，在设备详情页面获取网关设备的设备证书（ProductKey、DeviceName、DeviceSecret），用于后续启动网关。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155356637137164_zh-CN.png)


## 启动Link IoT Edge {#section_tmr_hb5_jgb .section}

1.  登录您Ubuntu 16.04系统的机器。
2.  下载Link IoT Edge软件包。

    执行如下命令下载适用于Ubuntu 16.04系统的Link IoT Edge轻量版（LE Lite）软件包。

    ```
    wget http://link-iot-edge-packet.oss-cn-shanghai.aliyuncs.com/x86-64-linux-gnu/link-iot-edge-lite-x86-64-v1.8.2.tar.gz
    ```

    **说明：** 以上命令中使用的下载软件包地址为Ubuntu 16.04系统v1.8.2版本软件包地址，其他版本LE Lite软件包下载地址请见[发布历史](../cn.zh-CN/产品简介/发布历史.md#) 。

3.  以root用户执行以下命令解压并安装Link IoT Edge轻量版软件包。

    ```
    sudo tar zxvf link-iot-edge-lite-x86-64-v1.8.2.tar.gz -C /
    ```

4.  以root用户执行以下命令启动Link IoT Edge。

    ```
    sudo /linkedge/gateway/build/script/iot_gateway_start_lite.sh {YourProductKey} {YourDeviceName} {YourDeviceSecret}
    ```

    **说明：** 请将\{YourProductKey\} \{YourDeviceName\} \{YourDeviceSecret\}替换为已在本地保存的网关设备的设备证书信息。

    例如，网关设备证书信息为ProductKey：a1\*\*\*\*\*\*gs、DeviceName：gateway、DeviceSecret：2Px\*\*\*\*\*\*\*\*\*\*\*\*\*\*H1S，则执行的实际命令如下：

    ```
    sudo /linkedge/gateway/build/script/iot_gateway_start_lite.sh a1******gs gateway 2Px**************H1S
    ```

    若系统显示如下信息，表示Link IoT Edge核心服务启动成功。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/104167/155356637137295_zh-CN.png)

    您也可以在[物联网控制台](http://iot.console.aliyun.com/)，选择 **边缘计算** \> **边缘实例** ，在已创建好的边缘实例右侧单击**查看**进入**实例详情**页面，查看网关状态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103166/155356637137203_zh-CN.png)

5.  您可以在**实例详情**页面，网关名称右侧的操作栏中单击**远程连接**或者**远程文件管理**，方便您远程控制网关设备或对网关设备上的文件进行管理。详细说明请参见[远程运维管理](../cn.zh-CN/用户指南/远程运维管理.md#)。

## 使用systemd管理Link IoT Edge {#section_rm4_vkc_kgb .section}

-   下载service文件。

    ```
    wget http://remote-access-oxs.oss-cn-shanghai.aliyuncs.com/%E8%84%9A%E6%9C%AC/LinkIoTEdgeLite.service
    ```

-   将service文件拷贝到/etc目录。

    ```
    sudo cp LinkIoTEdgeLite.service /etc/systemd/system/LinkIoTEdgeLite.service
    ```

-   启动Link IoT Edge。

    ```
    sudo systemctl start LinkIoTEdgeLite.service
    ```

-   停止Link IoT Edge。

    ```
    sudo systemctl restart LinkIoTEdgeLite.service
    ```

-   开机自启动Link IoT Edge。

    ```
    sudo systemctl enable LinkIoTEdgeLite.service
    ```


## 下一步 {#section_t1n_j5z_2hb .section}

环境搭建完成后，您可以参考[设备接入](../cn.zh-CN/用户指南/设备接入/设备接入简介.md#)章节内容，把您的设备接入到物联网边缘计算。

