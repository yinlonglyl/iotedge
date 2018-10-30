# 本地对接TSDB {#task_gmf_kgn_j2b .task}

 [TSDB](https://help.aliyun.com/product/54825.html)是阿里云提供的时序时空数据库，目前已经推出了边缘版本。您可通过函数计算能力在本地对接TSDB。TSDB 实例支持通过 HTTP 协议进行数据写入、查询等操作，详情请参见[TSDB详细HTTP接口文档](https://help.aliyun.com/document_detail/61263.html)。

以下示例将为您介绍如何通过函数计算连接时序时空数据库TSDB。

1.  使用以下示例代码，在[函数计算控制台](https://fc.console.aliyun.com/)创建一个函数。 

    ```
    
    module.exports.handler = function(event, context, callback) {
    
    console.log('write data to tsdb ...');
    
    var http = require("http"); 
    
    
    var myDate = new Date();
    
    
    deviceData = JSON.parse(event);
    //写入TSDB的数据格式和数据体
    var data = [{ "metric" : "MeasuredIlluminance", 
    "timestamp": myDate.getTime(), 
    "value" : deviceData.payload.MeasuredIlluminance.value, 
    "tags" : { "devicetype":"lightSensor"} }]; 
    
    
    data = JSON.stringify(data); 
    
    
    // 写入请求体 
    var opt = { 
    host:'localhost', 
    port:'8246', 
    method:'POST', 
    path:'/api/put?summary', 
    headers:{ 
    "Content-Type": 'application/json', 
    "Content-Length": data.length 
    } 
    } 
    
    
    var body = ''; 
    var req = http.request(opt, function(res) { 
    console.log("response: " + res.statusCode); 
    res.on('data',function(data){ 
    body += data; 
    }).on('end', function(){ 
    console.log(body)
    callback(null, '结束'); 
    }); 
    }).on('error', function(e) { 
    console.log("error: " + e.message); 
    callback(null, '结束'); 
    }) 
    req.write(data); 
    req.end(); 
    }
    
    ```

2.  在[物联网平台控制台](https://iot.console.aliyun.com/)边缘实例页面，单击**增加实例**，然后创建一个边缘实例。 
3.  在已创建的实例的实例详情页面，选择**函数计算**，然后为该实例分配第一步中创建的函数。 

    **说明：** 为边缘实例配置函数时，选择运行模式为**按需运行**。


