# C版本SDK {#concept_am2_t1s_xfb .concept}

本章为您介绍C版本的SDK使用方法及相关API。Link IoT Edge提供C版本的SDK，名称为link-iot-edge-access-sdk-c。C版本开源的SDK源码请见[开源C库](https://github.com/aliyun/link-iot-edge-access-sdk-c)。

## get\_properties\_callback {#section_n2l_rbs_xfb .section}

```
/*
     * 获取属性的回调函数, Link IoT Edge 需要获取某个设备的属性时, SDK 会调用该接口间接获取到数据并封装成固定格式后回传给 Link IoT Edge.
     * 开发者需要根据设备id和属性名找到设备, 将属性值获取并以@device_data_t格式返回.
     *
     * @dev_handle:         Link IoT Edge 需要获取属性的具体某个设备.
     * @properties:         开发者需要将属性值更新到properties中.
     * @properties_count:   属性个数.
     * @usr_data:           注册设备时, 用户传递的私有数据.
     * 所有属性均获取成功则返回LE_SUCCESS, 其他则返回错误码(参考错误码宏定义).
     * */
typedef int (*get_properties_callback)(device_handle_t dev_handle, 
                                       leda_device_data_t properties[], 
                                       int properties_count, 
                                       void *usr_data);
```

## set\_properties\_callback {#section_vhl_gcs_xfb .section}

```
/*
     * 设置属性的回调函数, Link IoT Edge 需要设置某个设备的属性时, SDK 会调用该接口将具体的属性值传递给应用程序, 开发者需要在本回调
     * 函数里将属性设置到设备.
     *
     * @dev_handle:         Link IoT Edge 需要设置属性的具体某个设备.
     * @properties:         Link IoT Edge 需要设置的设备的属性名和值.
     * @properties_count:   属性个数.
     * @usr_data:           注册设备时, 用户传递的私有数据.
     * 
     * 若获取成功则返回LE_SUCCESS, 失败则返回错误码(参考错误码宏定义).
     * */
typedef int (*set_properties_callback)(device_handle_t dev_handle, 
                                       const leda_device_data_t properties[], 
                                       int properties_count, 
                                       void *usr_data);
```

## call\_service\_callback {#section_b4m_gcs_xfb .section}

```
/*
     * 服务调用的回调函数, Link IoT Edge 需要调用某个设备的服务时, SDK 会调用该接口将具体的服务参数传递给应用程序, 开发者需要在本回调
     * 函数里调用具体的服务, 并将服务返回值按照设备 tsl 里指定的格式返回. 
     *
     * @dev_handle:   Link IoT Edge 需要调用服务的具体某个设备.
     * @service_name: Link IoT Edge 需要调用的设备的具体某个服务名.
     * @data:         Link IoT Edge 需要调用的设备的具体某个服务参数, 参数与设备 tsl 中保持一致.
     * @data_count:   Link IoT Edge 需要调用的设备的具体某个服务参数个数.
     * @output_data:  开发者需要将服务调用的返回值, 按照设备 tsl 中规定的服务格式返回到output中.
     * @usr_data:     注册设备时, 用户传递的私有数据.
     * 
     * 若获取成功则返回LE_SUCCESS, 失败则返回错误码(参考错误码宏定义).
     * */
typedef int (*call_service_callback)(device_handle_t dev_handle, 
                                     const char *service_name, 
                                     const leda_device_data_t data[], 
                                     int data_count, 
                                     leda_device_data_t output_data[], 
                                     void *usr_data);
```

## leda\_init {#section_a3n_gcs_xfb .section}

```
/*
 * 模块初始化, 模块内部会创建工作线程池, 异步执行云端下发的指令, 工作线程数目通过worker_thread_nums配置.
 *
 * module_name:         模块名称.
 * worker_thread_nums : 线程池工作线程数.
 *
 * 阻塞接口, 成功返回LE_SUCCESS, 失败返回错误码.
 */
int leda_init(const char *module_name, int worker_thread_nums);
```

## config\_changed\_callback {#section_hzn_gcs_xfb .section}

```
/*
 * 设备配置变更回调.
 *
 * module_name:   模块名称.
 * config:        配置信息.
 *
 * 阻塞接口, 成功返回LE_SUCCESS, 失败返回错误码.
 */
typedef int (*config_changed_callback)(const char *module_name, const char *config);
```

## leda\_register\_config\_changed\_callback {#section_tq4_gcs_xfb .section}

```
/*
 * 注册设备配置变更回调.
 *
 * module_name:    模块名称.
 * config_cb:      配置变更通知回调接口.
 *
 * 阻塞接口, 成功返回LE_SUCCESS,失败返回错误码.
 */
int leda_register_config_changed_callback(const char *module_name, config_changed_callback config_cb);
```

## leda\_register\_and\_online\_by\_device\_name {#section_ijp_gcs_xfb .section}

```
/*
 * 通过已在云平台注册的device_name, 注册设备并上线设备, 申请设备唯一标识符.
 *
 * 设备默认注册后即上线.
 *
 * product_key: 产品pk.
 * device_name: 云平台注册的device_name, 可以通过配置文件导入.
 * device_cb:   设备回调函数结构体，详细描述@leda_device_callback.
 * usr_data:    设备注册时传入私有数据, 在回调中会传给设备.
 *
 * 阻塞接口, 返回值设备在Link IoT Edge本地唯一标识, >= 0表示有效, < 0 表示无效.
 *
 */
device_handle_t leda_register_and_online_by_device_name(const char *product_key, const char *device_name, leda_device_callback_t *device_cb, void *usr_data);
```

## leda\_register\_and\_online\_by\_local\_name {#section_scq_gcs_xfb .section}

```
/*
 * 注册设备并上线设备, 申请设备唯一标识符, 若需要注册多个设备, 则多次调用该接口即可.
 *
 * 设备默认注册后即上线.
 *
 * product_key: 产品pk.
 * local_name:  由设备特征值组成的唯一描述信息, 必须保证同一个product_key时，每个待接入设备名称不同.
 * device_cb:   设备回调函数结构体，详细描述@leda_device_callback.
 * usr_data:    设备注册时传入私有数据, 在回调中会传给设备.
 *
 * 阻塞接口, 返回值设备在Link IoT Edge本地唯一标识, >= 0表示有效, < 0 表示无效.
 *
 */
device_handle_t leda_register_and_online_by_local_name(const char *product_key, const char *local_name, leda_device_callback_t *device_cb, void *usr_data);
```

## leda\_online {#section_tmr_gcs_xfb .section}

```
/*
 * 上线设备, 设备只有上线后, 才能被 Link IoT Edge 识别.
 *
 * dev_handle:  设备在Link IoT Edge本地唯一标识.
 *
 * 阻塞接口,成功返回LE_SUCCESS, 失败返回错误码.
 */
int leda_online(device_handle_t dev_handle);
```

## leda\_offline {#section_khs_gcs_xfb .section}

```
/*
 * 下线设备, 假如设备工作在不正常的状态或设备退出前, 可以先下线设备, 这样Link IoT Edge就不会继续下发消息到设备侧.
 *
 * dev_handle:  设备在Link IoT Edge本地唯一标识.
 *
 * 阻塞接口, 成功返回LE_SUCCESS,  失败返回错误码.
 *
 */
int leda_offline(device_handle_t dev_handle);
```

## leda\_report\_properties {#section_xzs_gcs_xfb .section}

```
/*
 * 上报属性, 设备具有的属性在设备能力描述 tsl 里有规定.
 *
 * 上报属性, 可以上报一个, 也可以多个一起上报.
 *
 * dev_handle:          设备在Link IoT Edge本地唯一标识.
 * properties:          @leda_device_data_t, 属性数组.
 * properties_count:    本次上报属性个数.
 *
 * 阻塞接口, 成功返回LE_SUCCESS,  失败返回错误码.
 *
 */
int leda_report_properties(device_handle_t dev_handle, const leda_device_data_t properties[], int properties_count);
```

## leda\_report\_event {#section_tmn_cfq_yfb .section}

```
/*
 * 上报事件, 设备具有的事件上报能力在设备 tsl 里有约定.
 *
 * 
 * dev_handle:  设备在Link IoT Edge本地唯一标识.
 * event_name:  事件名称.
 * data:        @leda_device_data_t, 事件参数数组.
 * data_count:  事件参数数组长度.
 *
 * 阻塞接口, 成功返回LE_SUCCESS,  失败返回错误码.
 *
 */
int leda_report_event(device_handle_t dev_handle, const char *event_name, const leda_device_data_t data[], int data_count);
```

## leda\_get\_tsl\_size {#section_ml5_gcs_xfb .section}

```
/*
 * 获取配置相关信息长度.
 *
 * product_key:   产品pk.
 *
 * 阻塞接口, 成功返回product_key对应的tsl长度, 失败返回0.
 */
int leda_get_tsl_size(const char *product_key);
```

## leda\_get\_tsl {#section_lcv_gcs_xfb .section}

```
/*
 * 获取配置相关信息.
 *
 * product_key:  产品pk.
 * tsl:          输出物模型, 需要提前申请好内存传入.
 * size:         tsl长度, 该长度通过leda_get_tsl_size接口获取, 如果传入tsl比实际配置内容长度短, 会返回LE_ERROR_INVAILD_PARAM.
 *  
 * 阻塞接口, 成功返回LE_SUCCESS, 失败返回错误码.
 */
int leda_get_tsl(const char *product_key, char *tsl, int size);
```

## leda\_get\_config\_size {#section_lvv_gcs_xfb .section}

```
/*
 * 获取配置信息长度.
 *
 * module_name:  模块名称
 *
 * 阻塞接口, 成功返回config长度, 失败返回0.
 */
int leda_get_config_size(const char *module_name);
```

## leda\_get\_config {#section_e4w_gcs_xfb .section}

```
/*
 * 获取配置信息.
 *
 * module_name:  模块名称
 * config:       配置信息, 需要提前申请好内存传入.
 * size:         config长度, 该长度通过leda_get_config_size接口获取, 如果传入config比实际配置内容长度短, 会返回LE_ERROR_INVAILD_PARAM.
 *  
 * 阻塞接口, 成功返回LE_SUCCESS, 失败返回错误码.
 */
int leda_get_config(const char *module_name, char *config, int size);
```

## leda\_get\_device\_handle {#section_g1y_gcs_xfb .section}

```
/*
 * 获取设备句柄.
 *
 * product_key: 产品pk.
 * device_name: 设备名称.
 *
 * 阻塞接口, 成功返回device_handle_t, 失败返回小于0数值.
 */
device_handle_t leda_get_device_handle(const char *product_key, const char *device_name);
```

## leda\_exit {#section_rsy_gcs_xfb .section}

```
/*
 * 模块退出.
 *
 * 模块退出前, 释放资源.
 *
 * 阻塞接口.
 */
void leda_exit(void);
```

## leda\_get\_module\_name {#section_qnz_gcs_xfb .section}

```
/*
 * 获取驱动模块名称.
 *
 * 阻塞接口, 成功返回驱动模块名称, 失败返回NULL.
 */
char* leda_get_module_name(void);
```

