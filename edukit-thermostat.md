# AWS Edukit(M5Stack) - Thermostate

## PlatformIO IDE Extension 설치

Visual Studio Code에 PlatformIO IDE Extension이 설치되어 있지 않다면, [Visual Studio Code에 PlatformIO IDE Extension 설치 및 활용](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/edukit-platformio.md)에 따라 Visual Studio Code에서 [M5Stack](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/m5stack.md)을 디버깅할 수 있는 환경을 구성합니다. 


## Thermostate 설정

1) [Device 인증서 생성](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/certification.md)을 따라서 M5Stack을 위한 인증서를 생성합니다. 이 과정을 진행하면, "M5Stack.cert.pem", "M5Stack.private.key", "M5Stack.public.key", "AmazonRootCA1.cer"가 생성됩니다.

2) [Core2-for-AWS-IoT-EduKit](https://github.com/m5stack/Core2-for-AWS-IoT-EduKit)을 다운로드 하지 않았다면, 아래와 같이 Core2-for-AWS-IoT-EduKit을 다운로드 합니다. SMART THERMOSTAT은 Core2-for-AWS-IoT-EduKit의 하위 폴더에 있습니다.

```c
$ git clone https://github.com/m5stack/Core2-for-AWS-IoT-EduKit.git
```

3) Visual Studio Code를 실행하여 [File] - [Open Folder]를 선택 한 후, Core2-for-AWS-IoT-EduKit/Smart-Thermostate를 선택합니다. 

4) 아래와 같이 "sdkconfig"에서 "CONFIG_WIFI_SSID"에 사용하는 AP의 이름을 넣고, "CONFIG_WIFI_PASSWORD"에 패스워드를 입력합니다. M5Stack은 2.4GHz WiFi만을 지원하므로 선택하는 AP는 2.4GHz를 지원하는 AP의 SSID를 지정하여야 합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170207617-b76313fe-8313-4da7-807f-d415bb0f2a1a.png)


5) [IoT Core Endpoint](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/endpoint.md)에 따라 접속해야 하는 IoT Core의 Endpoint를 확인 합니다. 이후 아래와 같이 "sdkconfig"에서 "CONFIG_AWS_IOT_MQTT_HOST"에 IoT Core의 Endpoint를 입력합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170208495-680a41f2-8530-4e0b-8295-8243e93f387d.png)

6) 아래와 같이 [SMART-THERMOSTAT] - [main] - [certs]에 가면, 아래의 3개의 파일이 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/170208927-3fd07c1a-5ecc-4e3e-97a5-60eb92ea0144.png)

"aws-root-ca.pem"은 "AmazonRootCA1.cer", "certificate.pem.crt"은 "M5Stack.cert.pem", "private.pem.key"은 "M5Stack.private.key"와 동일한 파일이므로, 파일을 열어서 동일하게 복사하여 줍니다. 


7) 화씨(Fahrenheit)를 섭씨(Centigrade)로 변환합니다.

"main.c"에서 구해진 temperature는 화씨입니다.

```c
  MPU6886_GetTempData(&temperature);
  temperature = (temperature * 1.8)  + 32 - 50;   // Fahrenheit
```

아래 구분을 추가하여 섭씨로 변경합니다. 

```c
 temperature = (temperature - 32) * 0.5556; // Centigrade
```

8) 이벤트 생성 시점을 1초에서 10초로 변경합니다.

"main.c"에서는 아래와 같이 1초 간격으로 temperature를 측정하여, json으로 전송하고 있습니다. 온도의 변화에 비하여 1초 간격이 너무 빠르므로 아래와 같이 10초로 조정하였습니다.  

![noname](https://user-images.githubusercontent.com/52392004/170298699-8c930b15-8b74-4ded-a68a-c2855ff1ba52.png)

9) 이제 [PlatformIO]를 선택한 후 [PROJECT TASK]에서 "Build"와 "Uplaod and Monitor"를 순차적으로 선택하여, 빌드 및 펌웨어 업그레이드를 진행합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170210914-d1fc38d6-d80a-4d42-ab47-7bd9bf5af4d0.png)



## Thermostate 동작 결과

펌웨어 업그레이드 후에, Visual Studio Code의 Terminal에서는 아래처럼 로그로 device 상태가 IoT core로 10초마다 전송되는 것을 알 수 있습니다. 

```c
␛[0;32mI (634333) MAIN: Stack remaining for task 'aws_iot_task' is 2036 bytes␛[0m
␛[0;31mE (644333) MAIN: Update timed out␛[0m
␛[0;32mI (644533) MAIN: *****************************************************************************************␛[0m
␛[0;32mI (644533) MAIN: On Device: roomOccupancy false␛[0m
␛[0;32mI (644533) MAIN: On Device: hvacStatus STANDBY␛[0m
␛[0;32mI (644543) MAIN: On Device: temperature 19.472862␛[0m
␛[0;32mI (644543) MAIN: On Device: sound 6␛[0m
␛[0;32mI (644553) MAIN: Update Shadow: {"state":{"reported":{"temperature":19.472862,"sound":6,"roomOccupancy":false,"hvacStatus":"STANDBY"}}, "clientToken":"0123501CB56E162101-62"}␛[0m
␛[0;32mI (644563) MAIN: *****************************************************************************************␛[0m
```

이때 아래와 같이 M5Stack이 IoT Core로 publish를 하고 있는 것을 UI로 확인 할 수 있습니다. 

<img width="514" alt="image" src="https://user-images.githubusercontent.com/52392004/170211449-45fb6882-54e8-4f24-9dcf-0361641a94b5.png">


IoT Core로 publish되는 데이터는 아래와 같은 json입니다.

- Topic: $aws/things/0123501CB56E162101/shadow/update

```java
{
  "state": {
    "reported": {
      "temperature": 17.820343,
      "sound": 8,
      "roomOccupancy": false,
      "hvacStatus": "STANDBY"
    }
  },
  "clientToken": "0123501CB56E162101-77"
}
```

- Topic: $aws/things/0123501CB56E162101/shadow/update/accepted

```java
{
  "state": {
    "reported": {
      "temperature": 17.820343,
      "sound": 8,
      "roomOccupancy": false,
      "hvacStatus": "STANDBY"
    }
  },
  "metadata": {
    "reported": {
      "temperature": {
        "timestamp": 1653492409
      },
      "sound": {
        "timestamp": 1653492409
      },
      "roomOccupancy": {
        "timestamp": 1653492409
      },
      "hvacStatus": {
        "timestamp": 1653492409
      }
    }
  },
  "version": 11214,
  "timestamp": 1653492409,
  "clientToken": "0123501CB56E162101-77"
}
```


- Topic: $aws/things/0123501CB56E162101/shadow/update/documents

```java
{
  "previous": {
    "state": {
      "reported": {
        "temperature": 17.272562,
        "sound": 6,
        "roomOccupancy": false,
        "hvacStatus": "STANDBY"
      }
    },
    "metadata": {
      "reported": {
        "temperature": {
          "timestamp": 1653492399
        },
        "sound": {
          "timestamp": 1653492399
        },
        "roomOccupancy": {
          "timestamp": 1653492399
        },
        "hvacStatus": {
          "timestamp": 1653492399
        }
      }
    },
    "version": 11213
  },
  "current": {
    "state": {
      "reported": {
        "temperature": 17.820343,
        "sound": 8,
        "roomOccupancy": false,
        "hvacStatus": "STANDBY"
      }
    },
    "metadata": {
      "reported": {
        "temperature": {
          "timestamp": 1653492409
        },
        "sound": {
          "timestamp": 1653492409
        },
        "roomOccupancy": {
          "timestamp": 1653492409
        },
        "hvacStatus": {
          "timestamp": 1653492409
        }
      }
    },
    "version": 11214
  },
  "timestamp": 1653492409,
  "clientToken": "0123501CB56E162101-77"
}
```
