# 基于Ubuntu 16.04搭建环境 {#concept_i25_b4n_lgb .concept}

本文介绍基于Ubuntu 16.04系统，如何快速将网关连接到物联网边缘计算控制台，并将网关数据上传至云端。

Link IoT Edge标准版软件包支持在Ubuntu 16.04 ~ Ubuntu 18.04系统上运行，并在下列平台上进行测试和验证：

|架构|操作系统|
|--|----|
|x86\_64|Ubuntu 16.04 ~ Ubuntu 18.04|
|ARMv7|Ubuntu 16.04 ~ Ubuntu 18.04|
|ARMv8（AArch64）|Ubuntu 16.04 ~ Ubuntu 18.04|

尽管Link IoT Edge可以在其它版本的Ubuntu操作系统上运行，但为了最佳的稳定性和安全性，建议您选择在官方支持的系统版本上运行。接下来，我们将基于x86\_64 Ubuntu 16.04的平台上为您介绍Link IoT Edge标准版安装部署的方法。Ubuntu16.04~18.04系统在其它架构上的标准版软件包您可以在[发布历史](../cn.zh-CN/产品简介/发布历史.md#)中获取。

## 准备工作 {#section_srr_y4m_lgb .section}

准备Ubuntu 16.04系统的PC或者虚拟机，并且该PC或者虚拟机能够正常连接网络。如果您不熟悉Ubuntu 16.04的安装，可参考[Ubuntu 16.04安装指南](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop-1604#0)。

## 环境设置 {#section_wzw_y3t_jgb .section}

在x86\_64 Ubuntu 16.04机器上需要设置Link IoT Edge运行所依赖的环境。

1.  在x86\_64 Ubuntu 16.04机器的本地终端窗口或者SSH终端窗口执行以下命令，下载环境检查工具并运行：

    ```
    wget http://iotedge-web.oss-cn-shanghai.aliyuncs.com/public/testingTool/link-iot-edge_env-check.sh
    sudo chmod +x ./link-iot-edge_env-check.sh
    sudo ./link-iot-edge_env-check.sh
    ```

    **说明：** link-iot-edge\_env-check.sh脚本在Link IoT Edge支持的平台需要以root权限运行并需要以下Linux系统命令：printf、echo、cat、ls、expr、grep、test、uname、set、head、sort、cut、uniq、xargs、ifconfig。

2.  按照运行环境检查工具的提示在您的机器上安装所有必需的依赖项，当检查工具成功运行完成后，返回如下图信息，表示Link IoT Edge能够在您的机器上成功运行。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103167/155073190538243_zh-CN.png)


## 创建边缘实例和网关 {#section_xzy_nxm_lgb .section}

1.  [物联网平台控制台](http://iot.console.aliyun.com/)，选择**边缘计算** \> **边缘实例**。
2.  创建一个边缘实例。
    1.  单击**新增实例**，在弹出窗口中设置**实例名称**。
    2.  在**网关产品**后单击**新建网关产品**，为实例创建网关。

        物联网边缘计算中的网关，承载边缘计算能力，每个实例必须分配一个唯一的网关产品及设备。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190537158_zh-CN.png)

    3.  在新建产品页面中，设置网关产品参数，然后单击**完成**。

        物联网边缘计算中的新建网关产品功能继承物联网平台**设备管理** \> **产品**中，高级版产品的功能。已自动为您简化创建适合物联网边缘计算中使用的网关产品步骤。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637159_zh-CN.png)

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

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637160_zh-CN.png)

    5.  根据界面提示设置参数后，单击**确认**。

        参数说明如下：

        |参数|描述|
        |--|--|
        |产品|系统已自动关联上一步创建的网关产品。|
        |设备名称|为该网关设备命名。设备名称需保持产品内唯一。如不填写，系统将自动生成。**说明：** 设备名称支持大写字母\[A-Z\]、小写字母\[a-z\]、数字\[0-9\]和下划线（\_）。且不能以下划线开头或结尾。

|

        在新增实例页面，单击**新增标签**，可以设置实例标签。通过标签您可以更加有效的去归类及识别实例。您也可以不设置标签。

3.  实例参数设置完成后，单击**确定**，至此您已创建边缘实例和网关。
4.  在**实例详情** \> **实例信息**页面，**网关**页签下，单击网关名称右侧的**查看**，获取网关设备信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637161_zh-CN.png)

5.  系统跳转到网关设备的设备详情页面，在设备详情页面获取网关设备的设备证书（ProductKey、DeviceName、DeviceSecret），用于后续启动网关。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637164_zh-CN.png)


## 启动Link IoT Edge {#section_tmr_hb5_jgb .section}

1.  登录您的x86\_64 Ubuntu 16.04机器。
2.  安装Link IoT Edge软件包。

    执行如下命令下载适用于x86\_64 Ubuntu 16.04系统的Link IoT Edge软件包。

    ```
    wget http://link-iot-edge-packet.oss-cn-shanghai.aliyuncs.com/x86-64-linux-gnu/link-iot-edge-x86-64-v1.8.1.tar.gz
    ```

    **说明：** 以上命令中使用的下载软件包地址为x86\_64 Ubuntu 16.04系统v1.8.1版本软件包地址，其他版本LE Standard软件包下载地址请见[发布历史](../cn.zh-CN/产品简介/发布历史.md#)。

3.  以root用户执行以下命令解压并安装Link IoT Edge软件包。

    该命令在网关设备的根目录中创建 /linkedge/gateway目录， 并将软件包安装到/linkedge/gateway/build目录。

    ```
    sudo tar xzvf link-iot-edge-x86-64-v1.8.1.tar.gz -C /
    ```

4.  配置网关初始化参数到安全存储。

    ```
    cd /linkedge/gateway/build/script
    sudo ./set_gw_triple.sh {YourProductKey} {YourDeviceName} {YourDeviceSecret}
    ```

    **说明：** 请将\{YourProductKey\} \{YourDeviceName\} \{YourDeviceSecret\}替换为已在本地保存的网关设备的设备证书信息。

    例如，网关设备证书信息为ProductKey：a1\*\*\*\*\*\*gs、DeviceName：gateway、DeviceSecret：2Px\*\*\*\*\*\*\*\*\*\*\*\*\*\*H1S，则执行的实际命令如下：

    ```
    sudo ./set_gw_triple.sh a1******gs gateway 2Px**************H1S
    ```

5.  启动Link IoT Edge核心服务。

    ```
    cd /linkedge/gateway/build/script
    sudo ./iot_gateway_start.sh
    ```

6.  执行如下命令查看Link IoT Edge核心服务的运行状态。

    ```
    sudo ./iot_gateway_status.sh
    ```

    若系统显示如下信息，表示Link IoT Edge核心服务启动成功。

    ![](https://cdn.nlark.com/lark/0/2018/png/30427/1542789699540-67ca7bcd-5838-4134-9354-033e51c498d4.png)

    您也可以在[物联网控制台](http://iot.console.aliyun.com/)，选择**边缘计算** \> **边缘实例**，在已创建好的边缘实例右侧单击**查看**进入**实例详情**页面，查看网关状态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/103166/155073190637203_zh-CN.png)

7.  在实例详情页面中，查看CPU使用率、内存使用率、存储使用率以及实例进程需要授权访问阿里云云监控（CloudMonitor）服务。
    1.  请参考[云资源访问](../cn.zh-CN/用户指南/云资源访问.md#)内容，在[RAM访问控制控制台](https://ram.console.aliyun.com)，创建授信**IoT物联网**的服务角色，并为该角色添加名为**AliyunCloudMonitorFullAccess**的访问云监控服务的权限。
    2.  在**实例信息**页签下打开**云监控状态**，如下图所示。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637199_zh-CN.png)

    3.  在实例进程后，单击**查看**，可查看实例各个进程的信息及CPU、内存使用率。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/102593/155073190637200_zh-CN.png)

8.  您可以在**实例详情**页面，网关名称右侧的操作栏中单击**远程连接**或者**远程文件管理**，方便您远程控制网关设备或对网关设备上的文件进行管理。详细说明请参见[远程运维管理](../cn.zh-CN/用户指南/远程运维管理.md#)。

## 使用systemd管理Link IoT Edge {#section_ix2_2ms_lgb .section}

你可以使用systemd来管理Link IoT Edge服务的启动（start）、停止（stop）和查看状态（status）。

Link IoT Edge的systemd service如下所示：

```
[Unit]
Description=Link IoT Edge

[Service]
Type=forking
Restart=on-failure
ExecStart=/linkedge/gateway/build/script/iot_gateway_start.sh
ExecReload=/linkedge/gateway/build/script/iot_gateway_start.sh
ExecStop=/linkedge/gateway/build/script/iot_gateway_stop.sh

[Install]
WantedBy=multi-user.target
```

您可运行如下命令下载该service文件，并拷贝到/etc/systemd/system/目录。

```
wget http://iotedge-web.oss-cn-shanghai.aliyuncs.com/public/testingTool/LinkIoTEdge.service
sudo cp LinkIoTEdge.service /etc/systemd/system/LinkIoTEdge.service
```

可以使用如下命令启动或者重启服务。

-   启动命令：`sudo systemctl start LinkIoTEdge.service`
-   重启命令：`sudo systemctl restart LinkIoTEdge.service`

可以使用如下命令停止服务。

```
sudo systemctl stop LinkIoTEdge.service
```

可以使用如下命令设置开机自动启动服务。

```
sudo systemctl enable LinkIoTEdge.service
```

