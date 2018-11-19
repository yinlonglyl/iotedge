# Client {#concept_mvj_1dd_kfb .concept}

包含函数计算相关API的类。

## invoke\_function\(params\) {#section_wxc_xjd_kfb .section}

调用指定函数。

|名称|类型|描述|
|--|--|--|
|params|dict|参数对象，包含的参数请见下表[params参数说明](#)。|
|return|dict|被调函数的返回值。|

|名称|类型|描述|
|--|--|--|
|functionId|str|被调用函数的唯一ID。您还可以通过serviceName与functionName同时指定被调函数，无需使用functionId。|
|serviceName|str|服务名，必须与functionName共同指定被调函数。您也可以通过functionId指定被调函数，无需使用serviceName。|
|functionName|str|函数名，必须与serviceName共同指定被调函数。您也可以通过functionId指定被调函数，无需使用functionName。|
|invocationType|str|调用类型。同步为`Sync`，异步为`Async`。默认为同步。|
|payload|str|bytes|参数信息作为函数的输入。|

