# Client {#concept_mvj_1dd_kfb .concept}

包含函数计算相关API的类。

## invoke\_function\(params\) {#section_wxc_xjd_kfb .section}

调用指定函数。

-   params：\{Object\} 参数对象，包含：
    -   functionId：\{String\} \(Required\) 被调用函数的唯一ID。
    -   invocationType：\{String\} 调用类型。同步为`Sync`，异步为`Async`。默认为同步。
    -   payload：\{String|Bytes\} 参数信息作为函数的输入。
-   return：被调函数的返回值。

