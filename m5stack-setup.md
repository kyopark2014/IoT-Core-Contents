# M5Stack의 설정관련 

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





재시후에 아래와 같은 QR Code가 보여집니다.

![noname](https://user-images.githubusercontent.com/52392004/169673610-c227b1e8-f2b2-4ae9-90c1-fc5b22217e83.png)

5) App에서 Android App

ESP Maker(https://play.google.com/store/apps/details?id=com.espressif.rainmaker)을 통해 Android 단말에서 device에 접속할 수 있습니다.

![image](https://user-images.githubusercontent.com/52392004/169607992-ad91bd7c-af2b-419f-8a25-c51dd85e74e6.png)
