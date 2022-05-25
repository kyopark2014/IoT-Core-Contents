# AWS Edukit(M5Stack) - Thermostate

## PlatformIO IDE Extension 설치

Visual Studio Code에 PlatformIO IDE Extension이 설치되어 있지 않다면, [Visual Studio Code에 PlatformIO IDE Extension 설치 및 활용](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/edukit-platformio.md)에 따라 Visual Studio Code에서 [M5Stack](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/m5stack.md)을 디버깅할 수 있는 환경을 구성합니다. 


## Device Provisioning

1) [Authentification](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/Authentification.md)에 따라 접속해야 하는 IoT Core의 Endpoint를 확인 합니다. 

2) [Device 인증서 생성](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/certification.md)을 따라서 M5Stack을 위한 인증서를 생성합니다. 이 과정을 진행하면, "M5Stack.cert.pem", "M5Stack.private.key", "M5Stack.public.key", "AmazonRootCA1.cer"가 생성됩니다.

3) [Core2-for-AWS-IoT-EduKit](https://github.com/m5stack/Core2-for-AWS-IoT-EduKit)을 다운로드 하지 않았다면, 아래와 같이 Core2-for-AWS-IoT-EduKit을 다운로드 합니다. SMART THERMOSTAT은 Core2-for-AWS-IoT-EduKit의 하위 폴더에 있습니다.

```c
$ git clone https://github.com/m5stack/Core2-for-AWS-IoT-EduKit.git
```

4) Visual Studio Code를 실행하여 [File] - [Open Folder]를 선택 한 후, Core2-for-AWS-IoT-EduKit/Smart-Thermostate를 선택합니다. 

5) 아래와 같이 "sdkconfig"에서 "CONFIG_WIFI_SSID"에 사용하는 AP의 이름을 넣고, "CONFIG_WIFI_PASSWORD"에 패스워드를 입력합니다. M5Stack은 2.4GHz WiFi만을 지원하므로 선택하는 AP는 2.4GHz를 지원하는 AP의 SSID를 지정하여야 합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170207617-b76313fe-8313-4da7-807f-d415bb0f2a1a.png)

6) 아래와 같이 "sdkconfig"에서 [Authentification](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/Authentification.md)에서 확인한 IoT Core의 Endpoint를 입력합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170208495-680a41f2-8530-4e0b-8295-8243e93f387d.png)

7) 아래와 같이 [SMART-THERMOSTAT] - [main] - [certs]에 가면, 아래의 3개의 파일이 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/170208927-3fd07c1a-5ecc-4e3e-97a5-60eb92ea0144.png)

"aws-root-ca.pem"은 "AmazonRootCA1.cer", "certificate.pem.crt"은 "M5Stack.cert.pem", "private.pem.key"은 "M5Stack.private.key"와 동일한 파일이므로, 파일을 열어서 동일하게 복사하여 줍니다. 

8) 이제 [PlatformIO]를 선택한 후 [PROJECT TASK]에서 "Build"와 "Uplaod and Monitor"를 순차적으로 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170210914-d1fc38d6-d80a-4d42-ab47-7bd9bf5af4d0.png)

9) 정상적으로 
ㅅㅣㄹ행이 

```c
I (5584) wifi:AP's beacon interval = 102400 us, DTIM period = 1
␛[0;32mI (6544) WIFI: Device IP address: 192.168.0.6␛[0m
␛[0;32mI (6544) MAIN: Shadow Init␛[0m
␛[0;32mI (6544) MAIN: Shadow Connect␛[0m
␛[0;32mI (6544) esp_netif_handlers: sta ip: 192.168.0.6, mask: 255.255.255.0, gw: 192.168.0.1␛[0m
␛[0;32mI (6574) aws_iot: Attempting to use device certificate from ATECC608␛[0m
␛[0;32mI (8604) I2S: DMA Malloc info, datalen=blocksize=512, dma_buf_count=2␛[0m
␛[0;32mI (8604) I2S: PLL_D2: Req RATE: 44100, real rate: 90909.000, BITS: 16, CLKM: 11, BCK: 5, MCLK: 11.338, SCLK: 2909088.000000, diva: 64, divb: 21␛[0m
␛[0;32mI (8614) I2S: PLL_D2: Req RATE: 44100, real rate: 90909.000, BITS: 16, CLKM: 11, BCK: 5, MCLK: 11.338, SCLK: 2909088.000000, diva: 64, divb: 21␛[0m
␛[0;32mI (8854) MAIN: *****************************************************************************************␛[0m
␛[0;32mI (8854) MAIN: On Device: roomOccupancy false␛[0m
␛[0;32mI (8854) MAIN: On Device: hvacStatus STANDBY␛[0m
␛[0;32mI (8864) MAIN: On Device: temperature 57.139534␛[0m
␛[0;32mI (8864) MAIN: On Device: sound 66␛[0m
␛[0;32mI (8874) MAIN: Update Shadow: {"state":{"reported":{"temperature":57.139534,"sound":66,"roomOccupancy":false,"hvacStatus":"STANDBY"}}, "clientToken":"0123501CB56E162101-0"}␛[0m
␛[0;32mI (11014) MAIN: *****************************************************************************************␛[0m
␛[0;32mI (11014) MAIN: Stack remaining for task 'aws_iot_task' is 2036 bytes␛[0m
␛[0;32mI (12024) MAIN: Update accepted␛[0m
␛[0;32mI (12224) MAIN: *****************************************************************************************␛[0m
␛[0;32mI (12224) MAIN: On Device: roomOccupancy false␛[0m
␛[0;32mI (12224) MAIN: On Device: hvacStatus STANDBY␛[0m
␛[0;32mI (12234) MAIN: On Device: temperature 57.640759␛[0m
␛[0;32mI (12234) MAIN: On Device: sound 4␛[0m
␛[0;32mI (12244) MAIN: Update Shadow: {"state":{"reported":{"temperature":57.640759,"sound":4,"roomOccupancy":false,"hvacStatus":"STANDBY"}}, "clientToken":"0123501CB56E162101-1"}␛[0m
␛[0;32mI (12254) MAIN: *****************************************************************************************␛[0m
␛[0;32mI (12264) MAIN: Stack remaining for task 'aws_iot_task' is 2036 bytes␛[0m
␛[0;32mI (13274) MAIN: Update accepted␛[0m
```
