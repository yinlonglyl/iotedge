# 基于阿里云Cloud Shell搭建环境 {#concept_j5n_pr1_1hb .concept}

本文介绍基于阿里云云命令行（Cloud Shell），快速将网关连接到物联网边缘计算控制台，并将网关数据上传至云端。Cloud Shell是阿里云提供的网页版命令行工具。您可以在任意浏览器上运行云命令行管理阿里云资源。

然而在Cloud Shell中操作时，若连续15分钟不做任何操作，系统会自动退出，并清空您保存在Cloud Shell上的资料，因此不建议您使用Cloud Shell作为开发生产环境，仅用于快速了解并无任何门槛地使用Link IoT Edge。在开始操作本章内容前，请您仔细阅读[Cloud Shell介绍及使用限制](https://help.aliyun.com/document_detail/90256.html)。

Cloud Shell仅支持搭建v1.8.2及以上版本的Link IoT Edge标准版（LE Standard）软件包。

## 创建边缘实例和网关 {#section_bjt_byb_bhb .section}

1.  [物联网平台控制台](http://iot.console.aliyun.com/)，选择**边缘计算** \> **边缘实例**。
2.  创建一个边缘实例。
    1.  单击**新增实例**，在弹出窗口中设置**实例名称**。
    2.  在**网关产品**后单击**新建网关产品**，为实例创建网关。

        物联网边缘计算中的网关，承载边缘计算能力，每个实例必须分配一个网关设备，并且该网关设备同一时间只能被分配到一个边缘实例。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723637158_zh-CN.png)

    3.  在新建产品页面中，设置网关产品参数，然后单击**完成**。

        物联网边缘计算中的新建网关产品功能继承物联网平台**设备管理** \> **产品**中，高级版产品的功能。已自动为您简化创建适合物联网边缘计算中使用的网关产品步骤。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723637159_zh-CN.png)

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

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723637160_zh-CN.png)

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

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723737161_zh-CN.png)

5.  系统跳转到网关设备的设备详情页面，在设备详情页面获取网关设备的设备证书（ProductKey、DeviceName、DeviceSecret），用于后续启动网关。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723737164_zh-CN.png)


## 启动Link IoT Edge {#section_qbn_hyb_bhb .section}

1.  以阿里云账号登录 [Cloud Shell](https://shell.aliyun.com/)。
2.  在授权提示窗口中，单击**确认**授权Cloud Shell获取临时的会话访问密钥。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135995/155263723740466_zh-CN.png)

3.  在挂载存储空间提示窗口中，选择**暂不创建**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135995/155263723740467_zh-CN.png)

    等待15 ~ 30秒后系统成功登录Cloud Shell，操作界面如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135995/155263723740399_zh-CN.png)

4.  执行如下命令，查看Cloud Shell运行环境的CPU。

    ```
    cat /proc/cpuinfo
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135995/155263723740400_zh-CN.png)

    CPU为Intel E5-2682，因此使用Link IoT Edge标准版（LE Standard）x86\_64架构安装包。

5.  执行如下命令下载适用于x86\_64架构的Link IoT Edge软件包。

    ```
    wget http://link-iot-edge-packet.oss-cn-shanghai.aliyuncs.com/x86-64-linux-gnu/link-iot-edge-x86-64-v1.8.2.tar.gz
    ```

    **说明：** 上述命令中使用的下载软件包地址为x86\_64系统v1.8.2版本软件包地址，其他高于v1.8.2版本的LE Standard软件包下载地址请见[发布历史](../cn.zh-CN/产品简介/发布历史.md#)。

6.  执行如下命令解压并安装Link IoT Edge软件包。

    ```
    tar xzvf link-iot-edge-x86-64-v1.8.2.tar.gz -C /tmp/ && mv /tmp/linkedge/* /linkedge && rm -rf /tmp/linkedge
    ```

7.  配置网关初始化参数到安全存储。

    ```
    cd /linkedge/gateway/build/script
    ./set_gw_triple.sh {YourProductKey} {YourDeviceName} {YourDeviceSecret}
    ```

    **说明：** 请将\{YourProductKey\} \{YourDeviceName\} \{YourDeviceSecret\}替换为已在本地保存的网关设备的设备证书信息。

    例如，网关设备证书信息为ProductKey：a1\*\*\*\*\*\*gs、DeviceName：gateway、DeviceSecret：2Px\*\*\*\*\*\*\*\*\*\*\*\*\*\*H1S，则执行的实际命令如下：

    ```
    ./set_gw_triple.sh a1******gs gateway 2Px**************H1S
    ```

8.  启动Link IoT Edge核心服务。

    ```
    cd /linkedge/gateway/build/script
    ./iot_gateway_start.sh
    ```

9.  执行如下命令查看Link IoT Edge核心服务的运行状态。

    ```
    ./iot_gateway_status.sh
    ```

    若系统显示如下信息，表示Link IoT Edge核心服务启动成功。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135995/155263723740402_zh-CN.png)

    您也可以在[物联网控制台](http://iot.console.aliyun.com/)，选择**边缘计算** \> **边缘实例**，在已创建好的边缘实例右侧单击**查看**进入**实例详情**页面，查看网关状态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103166/155263723737203_zh-CN.png)

10. 在实例详情页面中，查看CPU使用率、内存使用率、存储使用率以及实例进程需要授权访问阿里云云监控（CloudMonitor）服务。
    1.  请参考[云资源访问](../cn.zh-CN/用户指南/云资源访问.md#)内容，在[RAM控制台](https://ram.console.aliyun.com)，创建授信**IoT物联网**的服务角色，并为该角色添加名为**AliyunCloudMonitorFullAccess**的访问云监控服务的权限。
    2.  在**实例信息**页签下打开**云监控状态**，如下图所示。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723737199_zh-CN.png)

    3.  在实例进程后，单击**查看**，可查看实例各个进程的信息及CPU、内存使用率。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155263723737200_zh-CN.png)

11. 您可以在**实例详情**页面，网关名称右侧的操作栏中单击**远程连接**或者**远程文件管理**，方便您远程控制网关设备或对网关设备上的文件进行管理。详细说明请参见[远程运维管理](../cn.zh-CN/用户指南/远程运维管理.md#)。

