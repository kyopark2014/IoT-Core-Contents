# AWS Edukit(M5Stack) Upgrade

## Visual Studio Code 환경 구성 

아래와 같이 Visual Studio Code의 Extentions에서 "PlatformIO IDE"를 설치합니다. 

![image](https://user-images.githubusercontent.com/52392004/169671706-b232090b-0c1a-4f8c-b31c-5f732729c48d.png)

이후, 왼쪽의 메뉴에서 "PlatformIO"를 선택하면 [PLATFORM QUICK ACCESS]로 진입 할 수 있습니다.

![noname](https://user-images.githubusercontent.com/52392004/169671783-b558e864-78ee-40f9-957a-50490050ad31.png)


## AWS Sample Download

아래와 같이 m5stack에 대한 sample을 git으로 다운로드 합니다.

```c
$ git clone https://github.com/m5stack/Core2-for-AWS-IoT-EduKit.git
```


## Device 연결

1) M5Stack을 컴퓨터와 연결합니다. 이때 자동으로 전원이 들어오지만, 안들어오는 경우에 Power on 버튼을 선택하여 Device를 켭니다. 

![image](https://user-images.githubusercontent.com/52392004/169696860-08630181-79d6-411b-8c48-3363342cc4d9.png)


2) Visual Studio Code의 왼쪽 메뉴에서 [PlatformIO]를 선택 후, [Devices]를 선택후, 오른쪽에서 [Refresh]를 선택하면 아래와 같이 Device 정보를 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169672768-978ad794-a39b-4030-a42d-921e8465acf9.png)

여기서 "CP2104 USB to UART Bridge Controller"의 USB port name을 복사해 놓습니다. 상기의 경우에는 "/dev/cu.SLAB_USBtoUART" 입니다. 


## 첫 프로젝트 다운로드 하기 

1) 아래와 같이 [PlatformIO] - [Open] - [Open Project]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673061-f2593512-d7da-4c4b-98da-3bd058fdc2f0.png)

2) 다운로드 받은 "Core2-for-AWS-IoT-Edukit"으로 이동하여, "Getting-Started"을 선택후에 [Open "Getting-Started"을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673127-a84f49e9-ca12-4261-8670-d9c14ff80022.png)

3) 아래와 같이 [PlatformIO] - [Miscellaneous] - [New Terminal]을 연 후에, "pio run --environment core2foraws"을 입력하고 [Enter]를 누릅니다.

이후 아래와 같이 build가 수행됩니다. 

![image](https://user-images.githubusercontent.com/52392004/169673381-9627b281-f9d5-4df8-921c-0044e5afb93f.png)

4) complied된 firmware를 device에 upload 합니다.

```c
$ pio run --environment core2foraws --target upload --target monitor
```

이후 아래와 같이 upload가 진행됩니다.

![image](https://user-images.githubusercontent.com/52392004/169673476-0e408af7-25d4-4c35-938c-4c3ae8087126.png)

업로드가 다 되면 아래와 같이 device가 재시작합니다.

![image](https://user-images.githubusercontent.com/52392004/169673493-f6cd70f8-dc9f-494b-b760-4f94fb8196a6.png)

재시후에 아래와 같은 QR Code가 보여집니다.

![noname](https://user-images.githubusercontent.com/52392004/169673610-c227b1e8-f2b2-4ae9-90c1-fc5b22217e83.png)

5) App에서 Android App

ESP Maker(https://play.google.com/store/apps/details?id=com.espressif.rainmaker)을 통해 Android 단말에서 device에 접속할 수 있습니다.

![image](https://user-images.githubusercontent.com/52392004/169607992-ad91bd7c-af2b-419f-8a25-c51dd85e74e6.png)




## Firmware 삭제 

다른 펌웨어를 설치하기 전에 또는 새로 빌드한 device 펌웨어의 문제로 무한 재부팅등의 오류가 발생하면 아래 명령어로 펌웨어를 제거합니다. 

```c
$ pio run --environment core2foraws --target erase
```

![image](https://user-images.githubusercontent.com/52392004/169673964-8b4deb0c-0c08-46e5-a573-696737b108f6.png)


## Firware 수정

"sdkconfig"의 값을 아래와 같이 변경합니다. 

### MQTT Server

```c
CONFIG_ESP_RMAKER_MQTT_HOST="a1p72mufdu6064-ats.iot.us-east-1.amazonaws.com"
```

### NTP Server

[아래의 표](https://zetawiki.com/wiki/%EA%B3%B5%EC%9A%A9_NTP_%EC%84%9C%EB%B2%84_%EB%AA%A9%EB%A1%9D)를 보고 적절한 NTP Server를 지정합니다.

<img width="484" alt="image" src="https://user-images.githubusercontent.com/52392004/169686035-eaef3c7a-f576-47e7-abbc-7b48c5d66bd8.png">

```c
CONFIG_ESP_RMAKER_SNTP_SERVER_NAME="pool.ntp.org"
```

### Timezone

기본값은 "Asia/Shanghai"입니다. 아래를 참조하여 "Asia/Seoul"로 변경합니다.

[Supported Timezone values](https://rainmaker.espressif.com/docs/time-service.html#supported-timezone-values)에서 Seoul을 검색할 수 있습니다.

```c
CONFIG_ESP_RMAKER_DEF_TIMEZONE="Asia/Seoul"
```

This list is a set of valid values for setting the timezone region string using esp_rmaker_time_set_timezone() API or the esp.param.tz parameter or the CONFIG_ESP_RMAKER_DEF_TIMEZONE config option.

For POSIX timezone, i.e. esp_rmaker_time_set_timezone_posix() or esp.param.tz_posix, you may use other values, not covered here, but adhering to the first and second formats in GNU libc documentation for TZ.


<img width="657" alt="image" src="https://user-images.githubusercontent.com/52392004/169685877-c6eca590-8112-4457-a23b-172f3bc993c0.png">

