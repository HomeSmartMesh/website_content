---
title: Timer Cam OV3660
description: ESP32 timer camera OV3660 with low power sleep. An Time lapse application sends images through MQTT to a raspberry pi image recorder
order: 11
date: 2020-12-26T00:00:00+09:00
image: /images/esp32/timer-cam-3mp.png
tags: [Board,Camera,LowPower]
features:
    - 4 MB Flash
    - 8 MB PSRAM
    - 3 Mega Pixels OV3660
    - Fisheye lens DFOV 120°
    - Ultra low power, sleep current 2μA
    - 270mAh battery
---

![Timer Cam 3MP](/images/esp32/timer-cam-3mp.png)

* 3 Mega Pixels OV3660
* Fisheye lens DFOV 120°
* Ultra low power, sleep current 2μA
* 270mAh battery
* PSRAM 8MB

## Links
:button[]{label="Fisheye Timer camera $20" link="https://m5stack.com/collections/m5-unit/products/esp32-psram-timer-camera-fisheye-ov3660" icon="new"}


:button[]{label="Schematics - ESP32_SUBSYS" link="https://m5stack.oss-cn-shenzhen.aliyuncs.com/resource/docs/schematic/TimerCAM/TimerCAM_A1-ESP32_SUBSYS.pdf" icon="new"}

:button[]{label="Schematics - PMS_UART" link="https://m5stack.oss-cn-shenzhen.aliyuncs.com/resource/docs/schematic/TimerCAM/TimerCAM_A2-PMS_UART.pdf" icon="new"}

## Platformio porting
### board definition
* local file : `boards\m5stack-timer-cam.json`
* should be added to the [platformio/platform-espressif32](https://github.com/platformio/platform-espressif32/tree/master/boards) repo
* still missing from v2.1.0

### pins declaration
* file required `pins_arduino.h`
* available on [pins_arduino.h@1.0.5](https://github.com/espressif/arduino-esp32/blob/master/variants/m5stack_timer_cam/pins_arduino.h) not yet released

## Applications
### wakeup
* [m5stack/Arduino wakeup example](https://github.com/m5stack/TimerCam-arduino/tree/master/examples/wakeup)
* [sep32home/Platformio wakeup example](https://github.com/ESP32Home/timercam_wakeup)

:::details{summary="Build info ..."}
```log
PACKAGES:
 - framework-arduinoespressif32 0.0.0+sha.b05bdf6
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
Dependency Graph
|-- <Timer-CAM> 0.0.1
```
:::


### webcam
* [m5stack/Arduino webcam example](https://github.com/m5stack/TimerCam-arduino/tree/master/examples/web_cam)
* [esp32home/Platformio webcam example](https://github.com/ESP32Home/timercam_webcam)


:::details{summary="Build info ..."}
Note `app_httpd` is part of the `Timer-CAM` lib
```log
PACKAGES:
 - framework-arduinoespressif32 0.0.0+sha.b05bdf6
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - tool-mkspiffs 2.230.0 (2.30)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
Dependency Graph
|-- <Timer-CAM> 0.0.1   
|-- <WiFi> 1.0
```
:::

### mqtt timer cam

![Camera Lapse App](/images/esp32/camera_lapse_app.png)


:button[]{link="https://github.com/ESP32Home/timercam_mqtt" label="ESP32 Program Github Repo" }

:button[]{link="https://github.com/HomeSmartMesh/raspi/tree/master/js/camera_lapse" label="Node.js record service Github Repo" }


:::details{summary="Json Conf ..."}
```json
{
    "mqtt" : {
        "host":"10.0.0.42",
        "port":1883,
        "client_id":"esp_camera_lapse"
    },
    "camera":{
        "frame_size":"VGA",
        "buffer_count":2,
        "jpeg_quality":90,
        "base_topic":"esp/camera",
        "battery_sleep":300,
        "usb_sleep":10
    }
}
```
:::


:::details{summary="Platformio ini file ..."}
```ini
[env]
board = m5stack-timer-cam
framework = arduino
lib_deps =
    Timer-CAM
    WiFi
    ArduinoJson@6.16.1
    617@2.4.7   #MQTT 256dpi/arduino-mqtt
lib_ldf_mode = deep+
[env:m5stack-timer-cam]
platform = espressif32
monitor_speed = 115200

platform_packages =
    platformio/framework-arduinoespressif32 @ https://github.com/espressif/arduino-esp32.git#1.0.5-rc4
```
:::

:::details{summary="Build info ..."}
```ini
PACKAGES:
 - framework-arduinoespressif32 0.0.0+sha.b05bdf6
 - tool-esptoolpy 1.30000.201119 (3.0.0)
 - toolchain-xtensa32 2.50200.80 (5.2.0)
LDF: Library Dependency Finder -> http://bit.ly/configure-pio-ldf
LDF Modes: Finder ~ deep+, Compatibility ~ soft
Found 31 compatible libraries
Scanning dependencies...
Dependency Graph
|-- <Timer-CAM> 0.0.1
|-- <WiFi> 1.0
|-- <ArduinoJson> 6.16.1
|-- <MQTT> 2.4.7
|-- <FS> 1.0
|-- <SPIFFS> 1.0
|   |-- <FS> 1.0
```
:::
