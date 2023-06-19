# vscode中PlatformIO的安装及使用

[toc]



## 过程记录



### 安装

[PlatformIO IDE for VSCode — PlatformIO 最新文档](https://docs.platformio.org/en/latest/integration/ide/vscode.html)



## 代码分析



### cmake相关



#### idf_component_register & target_compile_options

```\
idf_component_register(SRCS "hello_world_main.c"
                    INCLUDE_DIRS "")

target_compile_options(${COMPONENT_LIB} PRIVATE "-Wno-format")
```

这段代码是 ESP-IDF（Espressif IoT Development Framework）中用于注册组件的代码。

首先，`idf_component_register` 函数被调用，它用于注册当前组件并指定其相关信息。在这个例子中，被注册的组件包含一个源文件 "hello_world_main.c"，该源文件将被编译并链接到组件库中。`INCLUDE_DIRS` 参数用于指定组件的头文件目录，这里设置为空，表示不包含额外的头文件目录。

接下来，`target_compile_options` 函数被调用，它用于为当前组件库添加编译选项。在这个例子中，`-Wno-format` 选项被添加到当前组件库的编译选项中。`-Wno-format` 是一个编译器选项，用于禁用对格式化字符串的警告。通过将此选项添加到组件的编译选项中，可以忽略与格式化字符串相关的警告。

总结起来，这段代码的作用是注册一个组件，并为该组件库添加了编译选项 `-Wno-format`，用于禁用格式化字符串的警告。这样在构建过程中，编译器将不会发出关于格式化字符串的警告信息。



## BUG与注意事项



### 安装后打开报错

>Error: ERROR: Exception: Traceback (most recent call last): File "C:\Users\ace\.platformio\penv\lib\site-packages\pip\_vendor\urllib3\response.py", line 438, in _error_catcher yield File "C:\Users\ace\.platformio\penv\lib\site-packages\pip\_vendor\urllib3\response.py", line 519, in read data = self._fp.read(amt) if not fp_closed else b"" File "C:\Users\ace\.platformio\penv\lib\site-packages\pip\_vendor\cachecontrol\filewrapper.py", line 62, in read data = self.__fp.read(amt) File "C:\Users\ace\.platformio\python3\lib\http\client.py", line 463, in read n = self.readinto(b) File "C:\Users\ace\.platformio\python3\lib\http\client.py", line 507, in readinto ...

解决：电脑连接手机热点，直连即可通过，数据量不大

参考链接：[Visual Studio Code PlatformIo IDE 新建项目下载慢的解决办法_platformio创建工程太慢_我是一座离岛的博客-CSDN博客](https://blog.csdn.net/ngl272/article/details/124776171?ops_request_misc=%7B%22request%5Fid%22%3A%22168672234116800227412023%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=168672234116800227412023&biz_id=0&utm_medium=distribute.pc_chrome_plugin_search_result.none-task-blog-2~all~first_rank_ecpm_v1~times_rank-12-124776171-null-null.nonecase&utm_term=PlatformIO &spm=1018.2226.3001.4187)



### ~~新建项目慢（会报错，弃用该方式）~~

按照官方手册操作 [ESP-IDF 和 ESP32-DevKitC 入门：调试、单元测试、项目分析 — PlatformIO 最新文档](https://docs.platformio.org/en/latest/tutorials/espressif32/espidf_debugging_unit_testing_analysis.html#tutorial-espressif32-espidf-debugging-unit-testing-analysis) 

新建esp工程时一直转圈，

解决：电脑连接手机热点，直连即可通过，数据量不大



### esp32 demo编译问题



根据 [使用VScode开发ESP32，PlatformIO开发ESP32_platformio 上传 超时 esp_【ql君】qlexcel的博客-CSDN博客](https://blog.csdn.net/qlexcel/article/details/121527415) 的demo例程成功实现运行

demo例程的位置 "C:\Users\ace\Documents\PlatformIO\Projects\230615-100954-espidf-hello-world"

编译下载后重启就能获取到串口的正确输出数据



### cmake使用注意

使用cmake不能直接在vscode里面直接调用gcc编译器，platformio的工程编译是会起一个终端窗口然后执行相应的配置文件去编译，与单纯的编译c文件不同。

#### message打印

直接使用STATUS、WARNING不会打印信息，直接被忽略；使用FATAL_ERROR可以打印，但是编译任务会终止

```
MESSAGE(FATAL_ERROR "$ENV{IDF_PATH}: " $ENV{IDF_PATH}) 
include($ENV{IDF_PATH}/tools/cmake/project.cmake)
```

打印信息

```
-- Configuring incomplete, errors occurred!
See also "C:/Users/ace/Documents/PlatformIO/Projects/230615-100954-espidf-hello-world/.pio/build/esp32dev/CMakeFiles/CMakeOutput.log".

CMake Error at CMakeLists.txt:5 (MESSAGE):
  C:/Users/ace/.platformio/packages/framework-espidf:
  C:/Users/ace/.platformio/packages/framework-espidf
```



1. `STATUS`: 输出信息作为构建过程中的状态消息。通常用于显示构建过程中的重要信息。

2. `WARNING`: 输出警告信息。通常用于提醒开发者注意潜在的问题或不推荐的做法。

3. `AUTHOR_WARNING`: 输出作者警告信息。与 `WARNING` 类似，但更适用于库或框架的作者向使用者发出警告。

4. `SEND_ERROR`: 输出错误信息，并停止构建过程。用于指示严重的错误，无法继续构建。

5. `FATAL_ERROR`: 输出致命错误信息，并终止构建过程。通常用于指示无法恢复的错误，需要终止构建。

6. `DEPRECATION`: 输出废弃警告信息，提示某些功能或接口已经被废弃，建议使用其他替代方案。



### win不需要添加环境变量 IDF_PATH





### 使用platformio 提供的 esp32 的examples

拉取git仓库 [platformio/platform-espressif32: Espressif 32: development platform for PlatformIO (github.com)](https://github.com/platformio/platform-espressif32)

使用vscode打开 platform-espressif32\examples\espidf-hello-world文件夹

编译，报错

>Error: [WinError 2] ϵͳ�Ҳ���ָ�����ļ���: 'e:\\git_projects\\platform-espressif32\\examples\\espidf-hello-world\\.pio\\build\\esp32dev\\bootloader\\.cmake\\api\\v1\\reply\\target-erase_flash-620c286d63bad3f2ed9d.json' 

根据提示删除 platform-espressif32\\examples\\espidf-hello-world\\.pio\\build\\esp32dev\\bootloader\\.cmake\\api\\v1\\reply下的所有文件，然后重新编译即可



### PlatformIO的esp32的教程网页文档例程代码有问题

[Get started with ESP-IDF and ESP32-DevKitC: debugging, unit testing, project analysis — PlatformIO latest documentation](https://docs.platformio.org/en/latest/tutorials/espressif32/espidf_debugging_unit_testing_analysis.html#adding-code-to-the-generated-project)

改：

```
/* WiFi softAP Example

  This example code is in the Public Domain (or CC0 licensed, at your option.)

  Unless required by applicable law or agreed to in writing, this
  software is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
  CONDITIONS OF ANY KIND, either express or implied.
*/

#include <string.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_system.h"
#include "esp_wifi.h"
#include "esp_event.h"
#include "esp_log.h"
#include "nvs_flash.h"

#include "lwip/err.h"
#include "lwip/sys.h"

#define EXAMPLE_ESP_WIFI_SSID "mywifissid"
#define EXAMPLE_ESP_WIFI_PASS "mywifipass"
#define EXAMPLE_MAX_STA_CONN (3)

static const char *TAG = "wifi softAP";

static void wifi_event_handler(void *arg, esp_event_base_t event_base,
                              int32_t event_id, void *event_data)
{
  if (event_id == WIFI_EVENT_AP_STACONNECTED)
  {
    wifi_event_ap_staconnected_t *event = (wifi_event_ap_staconnected_t *)event_data;
    // MACSTR
    // ESP_LOGI(TAG, "station " , " join, AID=%d", MAC2STR(event->mac), event->aid); 
  }
  else if (event_id == WIFI_EVENT_AP_STADISCONNECTED)
  {
    wifi_event_ap_stadisconnected_t *event = (wifi_event_ap_stadisconnected_t *)event_data;
    // MACSTR
    // ESP_LOGI(TAG, "station " , " leave, AID=%d", MAC2STR(event->mac), event->aid);
  }
}

void wifi_init_softap()
{
//   tcpip_adapter_init();
  ESP_ERROR_CHECK(esp_event_loop_create_default());

  wifi_init_config_t cfg = WIFI_INIT_CONFIG_DEFAULT();
  ESP_ERROR_CHECK(esp_wifi_init(&cfg));

  ESP_ERROR_CHECK(esp_event_handler_register(WIFI_EVENT, ESP_EVENT_ANY_ID, &wifi_event_handler, NULL));

  wifi_config_t wifi_config = {
      .ap = {
          .ssid = EXAMPLE_ESP_WIFI_SSID,
          .ssid_len = strlen(EXAMPLE_ESP_WIFI_SSID),
          .password = EXAMPLE_ESP_WIFI_PASS,
          .max_connection = EXAMPLE_MAX_STA_CONN,
          .authmode = WIFI_AUTH_WPA_WPA2_PSK},
  };
  if (strlen(EXAMPLE_ESP_WIFI_PASS) == 0)
  {
    wifi_config.ap.authmode = WIFI_AUTH_OPEN;
  }

  ESP_ERROR_CHECK(esp_wifi_set_mode(WIFI_MODE_AP));
  ESP_ERROR_CHECK(esp_wifi_set_config(ESP_IF_WIFI_AP, &wifi_config));
  ESP_ERROR_CHECK(esp_wifi_start());

  ESP_LOGI(TAG, "wifi_init_softap finished. SSID:%s password:%s",
          EXAMPLE_ESP_WIFI_SSID, EXAMPLE_ESP_WIFI_PASS);
}

void app_main()
{
  // Initialize NVS
  esp_err_t ret = nvs_flash_init();
  if (ret == ESP_ERR_NVS_NO_FREE_PAGES || ret == ESP_ERR_NVS_NEW_VERSION_FOUND)
  {
    ESP_ERROR_CHECK(nvs_flash_erase());
    ret = nvs_flash_init();
  }
  ESP_ERROR_CHECK(ret);

  ESP_LOGI(TAG, "ESP_WIFI_MODE_AP");
  wifi_init_softap();
}
```
