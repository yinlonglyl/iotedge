# 阿里云OSS服务访问 {#task_lcp_yd3_wgb .task}

本文以常用的访问阿里云对象存储（OSS）服务为例，详细讲解如何使用边缘函数计算访问阿里云服务，完成本地文件上传和云端文件下载能力。

-   已开通阿里云对象存储（OSS）服务，若未开通，请参考[开通OSS服务](https://help.aliyun.com/document_detail/31884.html)内容开通。
-   请您确保已根据[环境搭建](cn.zh-CN/用户指南/环境搭建/创建网关.md#)内容创建完成边缘实例。
-   根据[示例驱动](cn.zh-CN/用户指南/设备接入/示例驱动.md#)内容创建光照度传感器产品以及该产品下的LightSensor设备，并将设备分配至边缘实例。

设备数据上报云端后，会在物联网平台自动存储（默认存储30天），用户可以通过网页或者调用对应的云服务API，从外部访问到这些数据，进行二次业务开发。在此基础上，物联网边缘计算提供边缘函数计算能力，更方便地访问阿里云各种云服务，支持将设备在边缘端运行时生成的各类文件（统计报表、日志、图片、视频等）传输到云端，或从云端下载配置文件作为程序的输入来影响边缘端程序的行为。通过边缘函数计算访问OSS服务，可以选择永久存储设备数据。

使用边缘函数计算访问阿里云各种云服务的操作流程主要分为三步：

-   访问[阿里云官网](https://www.aliyun.com/)，开通对应的云服务，下载云服务SDK，将SDK和函数计算代码一起打包成.zip文件。
-   访问阿里云[函数计算控制台](https://www.aliyun.com/product/fc)，创建函数，上传.zip文件。
-   访问[物联网平台控制台](http://iot.console.aliyun.com/)，分配函数到边缘实例，部署函数到边缘网关运行。

1.  创建OSS Bucket。 
    1.  参考[创建存储空间](https://help.aliyun.com/document_detail/31885.html)中操作步骤创建存储空间，本示例中部分参数按如下表设置，其余参数可以使用默认值。 

        |参数|描述|
        |--|--|
        |Bucket 名称|设置为**le-fc-bucket**。|
        |区域|下拉选择该存储空间的数据中心。可以选择离您近的数据中心，访问速度更有保障。

|

    2.  在本地创建一个名为ossCloudFile.txt的txt格式的文件（文件内容可随意设置），参考[上传文件](https://help.aliyun.com/document_detail/31886.html)中的操作步骤，将该文件上传到已创建的**le-fc-bucket**存储空间中。 ossCloudFile.txt文件是为了后续测试通过函数下载OSS文件时使用。
2.  创建访问OSS的函数。 

    1.  下载访问OSS函数。该函数用于访问OSS，完成本地文件上传OSS和云端文件下载到本地的操作。 [accessAliOSS-code.zip](http://link-iot-edge-packet.oss-cn-shanghai.aliyuncs.com/fc-demo/accessAliOSS-code.zip) 
    2.  登录阿里云[函数计算控制台](https://www.aliyun.com/product/fc)。 如尚未开通该服务，请阅读并勾选**我已阅读并同意**内容，单击**立即开通**，开通服务。
    3.  单击**服务列表**后的“+”符号，根据界面提示配置参数后，单击**确定**创建一个服务。 其中，**服务名称**必须填写，此处设置为**EdgeFC**，其余参数可根据您的需求设置，也可以不设置。
    4.  创建服务成功后，在服务概览页面单击**函数列表**后的“+”符号，创建函数。 
    5.  选择函数模板，此处选择**空白函数**模板。 
    6.  在**触发器配置**中，选择**不创建触发器**，单击**下一步**。 
    7.  设置数据筛选函数的基础管理配置参数。 

        |参数|描述|
        |--|--|
        |所在服务|选择已创建的EdgeFC服务。|
        |函数名称|设置为accessAliOSS。|
        |运行环境|设置函数的运行环境，此示例中选择nodejs8。|
        |代码配置|选择**代码包上传**，上传已下载的[accessAliOSS-code.zip](http://link-iot-edge-packet.oss-cn-shanghai.aliyuncs.com/fc-demo/accessAliOSS-code.zip)代码包。|

        其余参数的值请根据您的需求，参见[函数计算](https://help.aliyun.com/product/50980.html?spm=a2c4g.11186623.2.8.7e6b1617Ezzl6L)设置，也可以不设置。

    8.  单击**下一步**，进入模板授权管理页面。此处无需设置，单击**下一步**。 
    9.  确认函数信息后，单击**创建**。 
    10. 在线编辑参数。 创建函数完成后，单击函数名称，在**代码执行**页签下选择**在线编辑**更改源码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/129670/155114582239340_zh-CN.png)

        其中，

        -   将<Your OSS Region\>替换为步骤[1](#)中创建空间存储时选择的区域信息，请从空间存储的**概览**页面查看，如下图示例所示。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/129670/155114582239352_zh-CN.png)

        -   将<Your OSS Bucket\>替换为步骤[1](#)中创建空间存储时设置的Bucket 名称，即**le-fc-bucket**。
    11. 登录[前提条件](#)中已完成的网关，执行如下命令生成本地测试文件，为样例代码中的本地上传文件功能准备一个名为localFile.txt的本地文件。 

        ```
        sudo echo "Hi, this is the file from edge GW." > /linkedge/run/localFile.txt
        ```

        **说明：** 在函数计算代码中访问网关宿主机的文件时，对于文件所在的路径是有约束的。在Link IoT Edge标准版（LE Standard）中，函数计算的代码是运行在与宿主机隔离的文件系统环境中，只有/linkedge/run目录是函数计算和宿主机环境共享并且拥有可读写权限。所以在测试时，请把测试文件的路径指到/linkedge/run目录下。

    12. 返回[函数计算控制台](https://www.aliyun.com/product/fc)，在accessAliOSS函数的**代码执行管理**页面中，**保存并执行**在线编辑的代码。 
    13. （可选）如需体验下载云端OSS文件到本地功能，可以accessAliOSS的**代码执行** \> **在线编辑**框中，注释掉第45行的`client.put`代码，去掉第48行开头的注释符"//"，调用OSS SDK的get方法（`client.get('ossCloudFile.txt', "/linkedge/run/fileFromCloud.txt");`），其中ossCloudFile.txt是步骤[1](#)中上传到**le-fc-bucket**存储空间的文件，/linkedge/run/fileFromCloud.txt是文件下载后保存在边缘端的文件名。 
     **样例代码解读**

    样例代码逻辑分为三步：

    1.  获取Credential，作为访问云服务的临时Token。

        ```
        credChain.resolvePromise()
        ```

    2.  填写您的OSS Region信息和OSS Bucket信息。

        ```
        region: '<Your OSS Region>',
        bucket: '<Your OSS Bucket>',
        ```

    3.  调用OSS SDK的put方法，将本地文件上传到OSS。

        ```
        client.put('fileFromEdge.txt', "/linkedge/run/localFile.txt");
        ```

        其中fileFromEdge.txt是文件上传后在云端保存的文件名，/linkedge/run/localFile.txt是本地需要上传的文件，两个文件名都可根据需求做修改，前提是本地文件必须存在。

3.  分配函数到边缘实例。 请先参考[云资源访问](cn.zh-CN/用户指南/云资源访问.md#)内容，在[RAM访问控制控制台](https://ram.console.aliyun.com)，为系统默认存在的**AliyunIOTAccessingFCRole**角色添加一个名为**AliyunOSSFullAccess**的访问OSS服务的权限。
    1.  登录[物联网平台控制台](http://iot.console.aliyun.com/)。 
    2.  左侧导航栏选择**边缘计算** \> **边缘实例**。 
    3.  在[前提条件](#)中完成的边缘实例右侧，单击**查看**。 
    4.  在实例详情页面，选择**设置**，单击**添加角色**，为边缘实例分配**AliyunIOTAccessingFCRole**角色。然后单击**确定**。 
    5.  在实例详情页面，选择**函数计算**，单击**分配函数**。 
    6.  在分配函数页面中，将已创建的访问OSS函数**accessAliOSS**分配到边缘实例中。 参数说明如下：

        |参数|描述|
        |--|--|
        |地域|选择您创建的服务所在的地域。|
        |服务|选择**EdgeFC**服务。|
        |函数|选择**accessAliOSS**函数。|
        |授权|选择**AliyunIOTAccessingFCRole**。|

    7.  单击**分配**，然后配置函数。 

        |参数|描述|
        |--|--|
        |运行模式|运行模式有两种。此处选择持续运行模式。程序部署后会立即执行。|
        |内存限制|函数运行可使用的内存资源上限，单位为MB。此处设置为512 MB。当函数使用内存超出该限制时，该函数计算程序会被强制重启。|
        |超时限制|函数收到事件后的最长处理时间，此处使用默认值5秒。如超过该时间函数仍未返回结果，该函数计算程序将会被强制重启。|
        |定时运行|单击开关按钮，打开定时运行，文本框中填入`* * * * *`。表示该函数会被定时触发运行，每分钟执行一次。Cron表达式详细信息请参考[详细表达式](https://yuque.antfin-inc.com/aone683673/link-iot-edge-doc/fcaccessoss)内容。|

        单击**确定**，至此您已将访问OSS函数分配到边缘实例中。

4.  部署边缘实例并查看设备的运行结果。 
    1.  在实例详情页面，单击右上角**部署**后在弹出框中单击**确定**，将子设备、函数计算下发到边缘端。 您可以通过单击**部署详情**来查看部署进度及结果。
    2.  实例部署成功约一分钟后，可以在[对象存储控制台](https://oss.console.aliyun.com/overview)，**le-fc-bucket** \> **文件管理**页面，可以看到fileFromEdge.txt文件已经成功上传。 可以单击文件右侧的**更多** \> **下载**，将文件下载到PC上查看文件内容。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/129670/155114582239380_zh-CN.png)

        至此您已经完整地体验了使用边缘函数计算实现阿里云OSS服务的访问功能。


如果在OSS文件管理页没有看到fileFromEdge.txt文件，可能有如下几种原因：

-   函数触发时间还没到：函数定时每分钟运行一次，如果刚部署完成，函数有可能还没有运行。请等待2分钟刷新页面查看。
-   网关本地系统时间不正确：本地时间可通过`date`命令查看，如果时间和网络时间差距过大，OSS会拒绝请求。
-   本地待上传文件不存在：如果在网关上执行`ls /linkedge/run/localFile.txt`命令，显示文件不存在，请参考[创建本地文件](#)内容创建。
-   OSS配置信息填写有误：请参考[在线编辑代码](#)内容填写正确的OSS配置信息。
-   如果上述配置都检查无误，可通过`cat /linkedge/run/logger/fc-base/accessAliOSS/log.INFO`命令查看函数运行日志，进行问题定位。

