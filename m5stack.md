## AWS Edukit

[M5Stack AWS](https://aws.amazon.com/ko/about-aws/whats-new/2020/12/introducing-aws-iot-edukit/)은 IoT Application을 개발할때 필요한 IoT hardwaware kit 입니다. 

- 주요센서: 제어용 터치 스크린, 온도, 가속도계, 자이로, 마이크

- Espressif ESP32 microcontroller 

- FreeRTOS, Arduino, MicroPython을 지원

- 구매방법: Amazon.com 또는 M5Stack 스토어

![image](https://user-images.githubusercontent.com/52392004/169607592-5604d6c9-743f-4f55-8a30-8e4aa7b1881e.png)






## M5Stack Core2
[M5Stack Core2](https://docs.platformio.org/en/latest/boards/espressif32/m5stack-core2.html?utm_source=platformio&utm_medium=piohome)

Platform Espressif 32: Espressif Systems is a privately held fabless semiconductor company. They provide wireless communications and Wi-Fi chips which are widely used in mobile devices and the Internet of Things applications.

![image](https://user-images.githubusercontent.com/52392004/169607434-d96325e0-1cd9-432f-91eb-bc2cfbd9cf82.png)


* ESP32 16MB

* USB-C interface for connection, power etc

* Capacitive colour touch screen

* Microphone

* Speaker

* Vibration motor (kind of like your smart phone when it vibrates)

* RGB lights either side of the unit

* Led light

* Micro SD card slot

* Battery - 500mAh lithium

* 2.4Ghz Wifi


```c
$ pio run --environment core2foraws

$ pio run --environment core2foraws --target upload --target monitor

Compressed 1521216 bytes to 879603...
Writing at 0x00020000... (1 %)
Writing at 0x00024000... (3 %)
Writing at 0x000ec000... (96 %)
Writing at 0x000f0000... (98 %)
Writing at 0x000f4000... (100 %)
Wrote 1521216 bytes (879603 compressed) at 0x00020000 in 15.7 seconds (effective 777.4 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
==================================== [SUCCESS] Took 87.87 seconds ====================================
--- Available filters and text transformations: colorize, debug, default, direct, esp32_exception_decoder, hexlify, log2file, nocontrol, printable, send_on_enter, time
--- More details at https://bit.ly/pio-monitor-filters

--- Available ports:
---  1: /dev/cu.Bluetooth-Incoming-Port 'n/a'
---  2: /dev/cu.SLAB_USBtoUART 'CP2104 USB to UART Bridge Controller'
---  3: /dev/cu.usbserial-02430F5F 'CP2104 USB to UART Bridge Controller'
---  4: /dev/cu.wlan-debug   'n/a'
--- Enter port index or full name: 4
```  

## AWS Edukit Upgrade

[AWS Edukit(M5Stack)](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/m5stack-upgrade.md)에서 업그레이드 방법에 대해 설명합니다.

[AWS Edukit Workshop](https://edukit.workshop.aws/en/)에는 Edukit 사용에 대한 여러 사례를 설명합니다.





## MQTT

AWS Edukit은 아래와 같이 MQTT Protocol을 이용하여 AWS IoT Core와 통신합니다. [MQTT](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/mqtt.md)에서 상세하게 MQTT에 대해 설명합니다.

![mqtt](https://user-images.githubusercontent.com/52392004/169608426-054b8204-94f6-4e34-af77-a57974e39a7c.png)


## Android App

ESP Maker(https://play.google.com/store/apps/details?id=com.espressif.rainmaker)을 통해 Android 단말에서 device에 접속할 수 있습니다.

![image](https://user-images.githubusercontent.com/52392004/169607992-ad91bd7c-af2b-419f-8a25-c51dd85e74e6.png)
