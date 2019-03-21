# 使用Connector架构完成设备接入 {#concept_apc_r1m_zgb .concept}

本文档介绍驱动（设备接入模块）的Connector架构模式。Connector是一种结构清晰又灵活的模式，方便您快速构建驱动。我们推荐您使用Connector架构模式构建驱动程序。

Connector架构模式目前只适用于Node.js和Python的设备接入SDK。

## 概述 {#section_rmx_1yz_zgb .section}

Connector模式架构图如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135370/155313872140270_zh-CN.png)

在Connector架构模式中，驱动程序由4个部分组成：

-   ThingAccessClient

    此类由设备接入SDK提供，提供多个方法与Link IoT Edge交互，包括数据上行和数据下行。同时接受外部传入ThingAccessClientCallbacks类型回调函数，在收到Link IoT Edge的下行数据时调用回调接口。Connector架构中ThingAccessClientCallbacks的实现类是Connector类。

-   Connector

    Connector架构核心组件。对外，Connector组件提供connect和disconnect接口，并接受外部注入Thing接口。对内，Connector组件实现ThingAccessClientCallbacks接口，并在构建ThingAccessClient对象时传入，以建立与Link IoT Edge的连接，并在收到回调指令时转发指令到设备。

-   Thing

    对物理设备接口提供封装，负责与设备交互，方便Connector组件调用，对外提供面向对象的API。Thing在这里只是一个统称，接入具体设备时为具体设备抽象类，如Light（表示灯设备）。

-   Entry

    驱动程序主入口，将会获取驱动配置，初始Thing组件和Connector组件，最终调用Connector组件的connect方法连接设备和Link IoT Edge。也可调用disconnect方法断开设备与Link IoT Edge的连接。


Connector组件是Connector架构中最重要的组件，它通过组合的方式将设备抽象接口（Thing）和Link IoT Edge抽象接口（ThingAccessClient）关联起来，因此而得名Connector。

UML类图如下所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/135370/155313872140278_zh-CN.png)

## 操作步骤 {#section_drv_tc1_1hb .section}

**Light**

本示例演示一个模拟灯的驱动程序设计，更多详细信息可参考[Python版本](https://github.com/aliyun/linkedge-thing-access-sdk-python/tree/master)。

1.  抽象模拟灯类。通过设置isOn属性的true和false来标识模拟灯的开和关。

    示例代码如下：

    ```
    /**
     * A virtual light which can be turned on or off by updating its
     * <code>isOn</code> property.
     */
    class Light {
      constructor() {
        this._isOn = true;
      }
    
      get isOn() {
        return this._isOn;
      }
    
      set isOn(value) {
        return this._isOn = value;
      }
    ```

2.  实现Connector。代码主要包含如下功能：

    -   构造函数接收设备的配置参数和设备抽象对象，内部构造ThingAccessClient以便与Link IoT Edge交互。
    -   实现ThingAccessClientCallbacks的3个回调方法，并在回调方法中调用设备对象接口与设备交互。
    -   提供connect方法和disconnect方法。其中在connect方法里连接Link IoT Edge，在disconnect方法里断开设备与Link IoT Edge的连接。
    示例代码如下：

    ```
    /**
     * The class combines ThingAccessClient and the thing that connects
     * to Link IoT Edge.
     */
    class Connector {
      constructor(config, light) {
        this.config = config;
        this.light = light;
        this._client = new ThingAccessClient(config, {
          setProperties: this._setProperties.bind(this),
          getProperties: this._getProperties.bind(this),
          callService: this._callService.bind(this),
        });
      }
    
      /**
       * Connects to Link IoT Edge and publishes properties to it.
       */
      connect() {
        registerAndOnlineWithBackOffRetry(this._client, 1)
          .then(() => {
            return new Promise(() => {
              // Publish properties to Link IoT Edge platform.
              const properties = { 'LightSwitch': this.light.isOn ? 1 : 0 };
              this._client.reportProperties(properties);
            });
          })
          .catch(err => {
            console.log(err);
            return this._client.cleanup();
          })
          .catch(err => {
            console.log(err);
          });
      }
    
      /**
       * Disconnects from Link IoT Edge and stops publishing properties to it.
       */
      disconnect() {
        this._client.cleanup()
          .catch(err => {
            console.log(err);
          });
      }
    
      _setProperties(properties) {
        console.log('Set properties %s to thing %s-%s', JSON.stringify(properties),
          this.config.productKey, this.config.deviceName);
        if ('LightSwitch' in properties) {
          var value = properties['LightSwitch'];
          var isOn = value === 1;
          if (this.light.isOn !== isOn) {
            // Report new property to Link IoT Edge if it changed.
            this.light.isOn = isOn;
            if (this._client) {
              properties = {'LightSwitch': value};
              console.log(`Report properties: ${JSON.stringify(properties)}`);
              this._client.reportProperties(properties);
            }
          }
          return {
            code: RESULT_SUCCESS,
            message: 'success',
          };
        }
        return {
          code: RESULT_FAILURE,
          message: 'The requested properties does not exist.',
        };
      }
    
      _getProperties(keys) {
        console.log('Get properties %s from thing %s-%s', JSON.stringify(keys),
          this.config.productKey, this.config.deviceName);
        if (keys.includes('LightSwitch')) {
          return {
            code: RESULT_SUCCESS,
            message: 'success',
            params: {
              'LightSwitch': this.light.isOn ? 1 : 0,
            }
          };
        }
        return {
          code: RESULT_FAILURE,
          message: 'The requested properties does not exist.',
        }
      }
    
      _callService(name, args) {
        console.log('Call service %s with %s on thing %s-%s', JSON.stringify(name),
          JSON.stringify(args), this.config.productKey, this.config.deviceName);
        return {
          code: RESULT_FAILURE,
          message: 'The requested service does not exist.',
        };
      }
    }
    ```

3.  获取配置信息，并初始化Connector架构组件。

    -   调用getConfig获取驱动配置。
    -   调用getThingInfos获取设备信息及配置。
    -   初始化Connector组件。
    -   调用connect连接Link IoT Edge。
    示例代码如下：

    ```
    // Get the config which is auto-generated when devices are bound to this driver.
    getConfig()
      .then((config) => {
        // Get the device information from config, which contains product key, device
        // name, etc. of the device.
        const thingInfos = config.getThingInfos();
        thingInfos.forEach((thingInfo) => {
          const light = new Light();
          // The ThingInfo format is just right for connector config, pass it directly.
          const connector = new Connector(thingInfo, light);
          connector.connect();
        });
      });
    ```


**LightSensor**

本示例演示一个模拟光照度传感器的驱动程序设计。

1.  抽象模拟光照度传感器类。此处模拟光照度传感器有外部监听时会自动运行，在重置外部监听后会停止运行。

    示例代码如下：

    ```
    /**
     * A virtual light sensor which starts to publish illuminance between 100
     * and 600 with 100 delta changes once someone listen to it.
     */
    class LightSensor {
      constructor() {
        this._illuminance = 200;
        this._delta = 100;
      }
    
      get illuminance() {
        return this._illuminance;
      }
    
      // Start to work.
      start() {
        if (this._clearInterval) {
          this._clearInterval();
        }
        console.log('Starting light sensor...');
        const timeout = setInterval(() => {
          // Update illuminance and delta.
          let delta = this._delta;
          let illuminance = this._illuminance;
          if (illuminance >= 600 || illuminance <= 100) {
            delta = -delta;
          }
          illuminance += delta;
          this._delta = delta;
          this._illuminance = illuminance;
    
          if (this._listener) {
            this._listener({
              properties: {
                illuminance,
              }
            });
          }
        }, 2000);
        this._clearInterval = () => {
          clearInterval(timeout);
          this._clearInterval = undefined;
        };
        return this._clearInterval;
      }
    
      stop() {
        console.log('Stopping light sensor ...');
        if (this._clearInterval) {
          this._clearInterval();
        }
      }
    
      listen(callback) {
        if (callback) {
          this._listener = callback;
          // Start to work when some one listen to this.
          this.start();
        } else {
          this._listener = undefined;
          this.stop();
        }
      }
    }
    ```

2.  实现Connector。

    -   构造函数接收设备的配置参数和设备抽象对象，内部构造ThingAccessClient以便与Link IoT Edge交互。
    -   实现ThingAccessClientCallbacks的3个回调方法，并在回调方法中调用设备对象接口与设备交互。
    -   提供connect方法和disconnect方法。其中在connect方法里连接Link IoT Edge，在disconnect方法里断开设备与Link IoT Edge的连接。
    示例代码如下：

    ```
    /**
     * The class combines ThingAccessClient and the thing that connects to Link IoT Edge.
     */
    class Connector {
      constructor(config, lightSensor) {
        this.config = config;
        this.lightSensor = lightSensor;
        this._client = new ThingAccessClient(config, {
          setProperties: this._setProperties.bind(this),
          getProperties: this._getProperties.bind(this),
          callService: this._callService.bind(this),
        });
      }
    
      /**
       * Connects to Link IoT Edge and publishes properties to it.
       */
      connect() {
        registerAndOnlineWithBackOffRetry(this._client, 1)
          .then(() => {
            return new Promise(() => {
              // Running..., listen to sensor, and report to Link IoT Edge.
              this.lightSensor.listen((data) => {
                const properties = {'MeasuredIlluminance': data.properties.illuminance};
                console.log(`Report properties: ${JSON.stringify(properties)}`);
                this._client.reportProperties(properties);
              });
            });
          })
          .catch(err => {
            console.log(err);
            return this._client.cleanup();
          })
          .catch(err => {
            console.log(err);
          });
      }
    
      /**
       * Disconnects from Link IoT Edge.
       */
      disconnect() {
        // Clean the listener.
        this.lightSensor.listen(undefined);
        this._client.cleanup()
          .catch(err => {
            console.log(err);
          });
      }
    
      _setProperties(properties) {
        console.log('Set properties %s to thing %s-%s', JSON.stringify(properties),
          this.config.productKey, this.config.deviceName);
        return {
          code: RESULT_FAILURE,
          message: 'The property is read-only.',
        };
      }
    
      _getProperties(keys) {
        console.log('Get properties %s from thing %s-%s', JSON.stringify(keys),
          this.config.productKey, this.config.deviceName);
        if (keys.includes('MeasuredIlluminance')) {
          return {
            code: RESULT_SUCCESS,
            message: 'success',
            params: {
              'MeasuredIlluminance': this.lightSensor.illuminance,
            }
          };
        }
        return {
          code: RESULT_FAILURE,
          message: 'The requested properties does not exist.',
        }
      }
    
      _callService(name, args) {
        console.log('Call service %s with %s on thing %s-%s', JSON.stringify(name),
          JSON.stringify(args), this.config.productKey, this.config.deviceName);
        return {
          code: RESULT_FAILURE,
          message: 'The requested service does not exist.',
        };
      }
    }
    ```

3.  获取配置信息，并初始化Connector架构组件。

    -   调用getConfig获取驱动配置。
    -   调用getThingInfos获取设备信息及配置。
    -   初始化Connector组件。
    -   调用connect连接Link IoT Edge。
    示例代码如下：

    ```
    // Get the config which is auto-generated when devices are bound to this driver.
    getConfig()
      .then((config) => {
        // Get the device information from config, which contains product key, device
        // name, etc. of the device.
        const thingInfos = config.getThingInfos();
        thingInfos.forEach((thingInfo) => {
          const lightSensor = new LightSensor();
          // The ThingInfo format is just right for connector config, pass it directly.
          const connector = new Connector(thingInfo, lightSensor);
          connector.connect();
        });
      });
    ```


