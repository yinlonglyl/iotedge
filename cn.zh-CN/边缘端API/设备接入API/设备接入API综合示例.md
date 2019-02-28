# 设备接入API综合示例 {#concept_xjf_j2p_32b .concept}

本文展示一段使用设备接入API，开发驱动的代码片段。设备使用thing标识，拥有一个temperature的只读属性，在连接到边缘计算节点后，每隔2秒向平台上报属性和高温事件。

## C版本 {#section_zf1_zgq_yfb .section}

```
/*
 * Copyright (c) 2014-2018 Alibaba Group. All rights reserved.
 * License-Identifier: Apache-2.0
 *
 * Licensed under the Apache License, Version 2.0 (the "License"); you may
 * not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *    http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
 * WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */

#include <stdlib.h>
#include <stdio.h>
#include <assert.h>
#include <string.h>
#include <signal.h>
#include <pthread.h>
#include <cJSON.h>
#include <unistd.h>
#include <sys/prctl.h>

#include "log.h"
#include "le_error.h"
#include "leda.h"

#ifdef __cplusplus
extern "C" {
#endif

#define LED_TAG_NAME            "demo_led"
#define MAX_DEVICE_NUM          32

static int g_dev_handle_list[MAX_DEVICE_NUM] = 
{
    INVALID_DEVICE_HANDLE
};
static int  g_dev_handle_count = 0;

static int  g_LedRunState      = 0;

static int get_properties_callback_cb(device_handle_t dev_handle, 
                               leda_device_data_t properties[], 
                               int properties_count, 
                               void *usr_data)
{
    int i = 0;

    for (i = 0; i < properties_count; i++)
    {
        log_i(LED_TAG_NAME, "get_property %s: ", properties[i].key);

        if (!strcmp(properties[i].key, "temperature"))
        {
            sprintf(properties[i].value, "%d", 30); /* 作为演示，填写模拟数据 */
            properties[i].type = LEDA_TYPE_INT;
            log_i(LED_TAG_NAME, "%s\r\n",  properties[i].value);
        }
    }

    return LE_SUCCESS;
}

static int set_properties_callback_cb(device_handle_t dev_handle, 
                               const leda_device_data_t properties[], 
                               int properties_count, 
                               void *usr_data)
{
    int i = 0;

    for (i = 0; i < properties_count; i++)
    {
        log_i(LED_TAG_NAME, "set_property type:%d %s: %s\r\n", properties[i].type, properties[i].key, properties[i].value);
    }

    return LE_SUCCESS;
}

static int call_service_callback_cb(device_handle_t dev_handle, 
                               const char *service_name, 
                               const leda_device_data_t data[], 
                               int data_count, 
                               leda_device_data_t output_data[], 
                               void *usr_data)
{
    int i = 0;
 
    log_i(LED_TAG_NAME, "service_name: %s\r\n", service_name);
 
    for (i = 0; i < data_count; i++)
    {
        log_i(LED_TAG_NAME, "    input_data %s: %s\r\n", data[i].key, data[i].value);
    }

    output_data[0].type = LEDA_TYPE_INT;
    sprintf(output_data[0].key, "%s", "temperature");
    sprintf(output_data[0].value, "%d", 50); /* 作为演示，填写模拟数据 */

    return LE_SUCCESS;
}

static int parse_driverconfig_and_onlinedevice(const char* driver_config)
{
    char                        *productKey     = NULL;
    char                        *deviceName     = NULL;
    cJSON                       *root           = NULL;
    cJSON                       *object         = NULL;
    cJSON                       *item           = NULL;
    cJSON                       *result         = NULL;
    leda_device_callback_t      device_cb;
    device_handle_t             dev_handle      = -1;

    root = cJSON_Parse(driver_config);
    if (NULL == root)
    {
        log_e(LED_TAG_NAME, "driver_config parser failed\r\n");
        return LE_ERROR_INVAILD_PARAM;
    }

    object = cJSON_GetObjectItem(root, "deviceList");
    cJSON_ArrayForEach(item, object)
    {
        if (cJSON_Object == item->type)
        {
            result = cJSON_GetObjectItem(item, "productKey");
            productKey = result->valuestring;
            
            result = cJSON_GetObjectItem(item, "deviceName");
            deviceName = result->valuestring;

            /*  parse user custom config 
                因为本demo主要介绍驱动开发流程和SDK接口的使用，所以并没有完整的实现设备和驱动的连接。实际应用场景中这部分是
                必须要实现的，要实现驱动和设备的连接，开发者可以通过在设备关联驱动配置流程时通过自定义配置来配置设备连接信息，
                操作流程可以参考如下链接
                https://help.aliyun.com/document_detail/85236.html?spm=a2c4g.11186623.6.583.64bd3265NR9Ouo
                
                如果用户在自定义配置中填写内容为
                { 
                    "ip" : "192.168.1.199",
                    "port" 9999
                }

                则实际生成结果为
                "custom" : { 
                    "ip" : "192.168.1.199",
                    "port" 9999
                }

                下面给出解析自定义配置的示例代码，具体如下
                cJSON          *custom = NULL;
                char           *ip     = NULL;
                unsigned short port    = 65536;

                custom = cJSON_GetObjectItem(item, "custom");
                result = cJSON_GetObjectItem(custom, "ip");
                ip = result->valuestring;
                
                result = cJSON_GetObjectItem(custom, "port");
                port = (unsigned short)result->valueint;

                获取到设备ip和port后即可连接该设备，连接成功则注册上线设备，否则继续下一个配置的解析
            */

            /* register and online device */
            device_cb.get_properties_cb            = get_properties_callback_cb;
            device_cb.set_properties_cb            = set_properties_callback_cb;
            device_cb.call_service_cb              = call_service_callback_cb;
            device_cb.service_output_max_count     = 5;

            dev_handle = leda_register_and_online_by_local_name(productKey, deviceName, &device_cb, NULL);
            if (dev_handle < 0)
            {
                log_e(LED_TAG_NAME, "product:%s device:%s register failed\r\n", productKey, deviceName);
                continue;
            }
            g_dev_handle_list[g_dev_handle_count++] = dev_handle;
            log_i(LED_TAG_NAME, "product:%s device:%s register success\r\n", productKey, deviceName);
        }
    }

    if (NULL != root)
    {
        cJSON_Delete(root);
    }

    if (0 == g_dev_handle_count)
    {
        log_e(LED_TAG_NAME, "current driver no device online\r\n");
    }

    return LE_SUCCESS;
}

static void *thread_device_data(void *arg)
{
    int i = 0;

    prctl(PR_SET_NAME, "demo_led_data");
    while (0 != g_LedRunState)
    {
        for (i = 0; i < g_dev_handle_count; i++)
        {
            /* report device properties */
            leda_device_data_t dev_proper_data[1] = 
            {
                {
                    .type  = LEDA_TYPE_INT,
                    .key   = {"temperature"},
                    .value = {"20"}
                }
            };
            leda_report_properties(g_dev_handle_list[i], dev_proper_data, 1);

            /* report device event */
            leda_device_data_t dev_event_data[1] = 
            {
                {
                    .type  = LEDA_TYPE_INT,
                    .key   = {"temperature"},
                    .value = {"50"}
                }
            };
            leda_report_event(g_dev_handle_list[i], "high_temperature", dev_event_data, 1);
        }

        sleep(5);
    }

    pthread_exit(NULL);

    return NULL;
}

static void ledSigInt(int sig)
{
    if (sig && (SIGINT == sig))
    {
        if (0 != g_LedRunState)
        {
            log_e(LED_TAG_NAME, "Caught signal: %s, exiting...\r\n", strsignal(sig));
        }

        g_LedRunState = -1;
    }

    return;
}

int main(int argc, char** argv)
{
    int         size            = 0;
    char        *driver_config  = NULL;
    pthread_t   thread_id;
	struct      sigaction sig_int;

    /* listen SIGINT singal */
	memset(&sig_int, 0, sizeof(struct sigaction));
    sigemptyset(&sig_int.sa_mask);
    sig_int.sa_handler = ledSigInt;
    sigaction(SIGINT, &sig_int, NULL);

    /* set log level */
    log_init(LED_TAG_NAME, LOG_FILE, LOG_LEVEL_DEBUG, LOG_MOD_VERBOSE);

    log_i(LED_TAG_NAME, "demo startup\r\n");

    /* init sdk */
    if (LE_SUCCESS != leda_init(leda_get_module_name(), 5))
    {
        log_e(LED_TAG_NAME, "leda_init failed\r\n");
        return LE_ERROR_UNKNOWN;
    }

    /* take driver config from config center */
    size = leda_get_config_size(leda_get_module_name());
    if (size >0)
    {
        driver_config = (char*)malloc(size);
        if (NULL == driver_config)
        {
            return LE_ERROR_ALLOCATING_MEM;
        }

        if (LE_SUCCESS != leda_get_config(leda_get_module_name(), driver_config, size))
        {
            return LE_ERROR_INVAILD_PARAM;
        }
    }

    /* parse driver config and online device */
    if (LE_SUCCESS != parse_driverconfig_and_onlinedevice(driver_config))
    {
        log_e(LED_TAG_NAME, "parse driver config or online device failed\r\n");
        return LE_ERROR_UNKNOWN;
    }
    free(driver_config);

    /* report device data */
    if (0 != pthread_create(&thread_id, NULL, thread_device_data, NULL))
    {
        log_e(LED_TAG_NAME, "create device data thread failed\r\n");
        return LE_ERROR_UNKNOWN;
    }

    /* keep driver running */
    prctl(PR_SET_NAME, "demo_led_main");
    while (0 != g_LedRunState)
    {
        sleep(5);
    }

    /* exit sdk */
    leda_exit();
    log_i(LED_TAG_NAME, "demo exit\r\n");

	return LE_SUCCESS;
}

#ifdef __cplusplus
}
#endif
```

## Node.js版本 {#section_wlk_g52_h2b .section}

```
/*
 * Copyright (c) 2018 Alibaba Group Holding Ltd.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

'use strict';

const {
  RESULT_SUCCESS,
  RESULT_FAILURE,
  ThingAccessClient,
} = require('linkedge-thing-access-sdk');

var driverConfig;
try {
  driverConfig = JSON.parse(process.env.FC_DRIVER_CONFIG)
} catch (err) {
  throw new Error('The driver config is not in JSON format!');
}
var configs = driverConfig['deviceList'];
if (!Array.isArray(configs) || configs.length === 0) {
  throw new Error('No device is bound with the driver!');
}

var args = configs.map((config) => {
  var self = {
    config,
    thing: {
      temperature: 41,
    },
    callbacks: {
      setProperties: function (properties) {
        // Usually, in this callback we should set properties to the physical thing and
        // return the result. Here we just return a failed result since the properties
        // are read-only.
        console.log('Set properties %s to thing %s-%s', JSON.stringify(properties),
          config.productKey, config.deviceName);
        // Return an object representing the result in the following form or the promise
        // wrapper of the object.
        return {
          code: RESULT_FAILURE,
          message: 'failure',
        };
      },
      getProperties: function (keys) {
        // Usually, in this callback we should get properties from the physical thing and
        // return the result. Here we return the simulated properties.
        console.log('Get properties %s from thing %s-%s', JSON.stringify(keys),
          config.productKey, config.deviceName);
        // Return an object representing the result in the following form or the promise
        // wrapper of the object.
        if (keys.includes('temperature')) {
          return {
            code: RESULT_SUCCESS,
            message: 'success',
            params: {
              temperature: self.thing.temperature,
            }
          };
        }
        return {
          code: RESULT_FAILURE,
          message: 'The requested properties does not exist.',
        }
      },
      callService: function (name, args) {
        // Usually, in this callback we should call services on the physical thing and
        // return the result. Here we just return a failed result since no service
        // provided by the thing.
        console.log('Call service %s with %s on thing %s-%s', JSON.stringify(name),
          JSON.stringify(args), config.productKey, config.deviceName);
        // Return an object representing the result in the following form or the promise
        // wrapper of the object
        return new Promise((resolve) => {
          resolve({
            code: RESULT_FAILURE,
            message: 'The requested service does not exist.',
          })
        });
      }
    },
  };
  return self;
});

args.forEach((item) => {
  var client = new ThingAccessClient(item.config, item.callbacks);
  client.setup()
    .then(() => {
      return client.registerAndOnline();
    })
    .then(() => {
      // Push events and properties to LinkEdge platform.
      return new Promise(() => {
        setInterval(() => {
          var temperature = item.thing.temperature;
          if (temperature > 40) {
            client.reportEvent('high_temperature', {'temperature': temperature});
          }
          client.reportProperties({'temperature': temperature});
        }, 2000);
      });
    })
    .catch(err => {
      console.log(err);
      return client.cleanup();
    })
    .catch(err => {
      console.log(err);
    });
});

module.exports.handler = function (event, context, callback) {
  console.log(event);
  console.log(context);
  callback(null);
};
```

## Python版本 {#section_rnc_kpx_kfb .section}

```
# -*- coding: utf-8 -*-
import lethingaccesssdk
import time
import json
import os
import logging


# Base on device, user need to implement the callService, getProperties and getProperties function.
class Temperature_device(lethingaccesssdk.ThingCallback):
  def __init__(self):
    self.temperature = 41

  def callService(self, name, input_value):
    return -1, {}

  def getProperties(self, input_value):
    if input_value[0] == "temperature":
      return 0, {input_value[0]: self.LightSwitch}
    else:
      return -1, {}

  def setProperties(self, input_value):
    if "temperature" in input_value:
      self.LightSwitch = input_value["temperature"]
      return 0, {}

try:
  driver_conf = json.loads(os.environ.get("FC_DRIVER_CONFIG"))
  if "deviceList" in driver_conf and len(driver_conf["deviceList"]) > 0:
    device_list_conf = driver_conf["deviceList"]
    device = device_list_conf[0]
    app_callback = Temperature_device()
    client = lethingaccesssdk.ThingAccessClient(device)
    client.registerAndonline(app_callback)
    while True:
      time.sleep(2)
      if app_callback.temperature > 40:
        client.reportEvent('high_temperature', {'temperature': app_callback.temperature})
        client.reportProperties({'temperature': app_callback.temperature})
except Exception as e:
  logging.error(e)

# don't remove this function
def handler(event, context):
  return 'hello world'
```

