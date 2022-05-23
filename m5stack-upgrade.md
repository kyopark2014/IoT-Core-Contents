# AWS Edukit(M5Stack) Upgrade


여기에서는 [AWS Edukit Workshop](https://edukit.workshop.aws/en/)에 따라 아래와 같이 M5Stack에 대한 업그레이드를 진행합니다.


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

<img width="485" alt="image" src="https://user-images.githubusercontent.com/52392004/169696896-6e9d7c75-bd78-48de-8cd2-d7c9356afeb1.png">

2) Visual Studio Code의 왼쪽 메뉴에서 [PlatformIO]를 선택 후, [Devices]를 선택후, 오른쪽에서 [Refresh]를 선택하면 아래와 같이 Device 정보를 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169672768-978ad794-a39b-4030-a42d-921e8465acf9.png)

여기서 "CP2104 USB to UART Bridge Controller"의 USB port name이 "/dev/cu.SLAB_USBtoUART" 임을 알 수 있습니다.


## 프로젝트 시작하기 

1) 아래와 같이 [PlatformIO] - [Open] - [Open Project]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673061-f2593512-d7da-4c4b-98da-3bd058fdc2f0.png)


2) 다운로드 받은 "Core2-for-AWS-IoT-Edukit"으로 이동하여, "Blinky-Hello-World"을 선택후에 [Open "Blinky-Hello-World"을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169726454-31675dde-fcb1-4ccb-8c84-7eab9af85086.png)


3) Device Provisioning 을 진행합니다. 

MQTT에 연결하기 위해서는 Device를 등록하여야 합니다. 

```c
$ cd Blinky-Hello-World
$ pio run -e core2foraws-device_reg -t register_thing
```

상기와 같이 등록시에 아래처럼 "AWS_IoT_registration_helper/output_files"에 인증과 관련된 파일들이 생성됩니다.

![noname](https://user-images.githubusercontent.com/52392004/169728933-1c352cf0-9553-430c-bb80-49fab7832fd3.png)

[IoT Core에 등록된 device 정보](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/thinghub)는 아래와 같습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169730291-d534d7b4-154c-43fe-977f-44311a284af1.png)

Certificates에 대한 정보는 아래와 같습니다. 여기서 Policy가 "Edukit_Policy"로 등록되어 있음을 알 수 있습니다.

![noname](https://user-images.githubusercontent.com/52392004/169730537-cbeda606-faed-4de9-950a-8b2b2fcdd36a.png)

Policy 내용은 아래와 같습니다.

```java
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:123456789012:client/${iot:Connection.Thing.ThingName}"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:123456789012:topic/${iot:Connection.Thing.ThingName}/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topic/$aws/things/${iot:Connection.Thing.ThingName}/streams/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topic/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/${iot:Connection.Thing.ThingName}/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/streams/*",
        "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:UpdateThingShadow",
        "iot:GetThingShadow"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:123456789012:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
      ]
    }
  ]
}
```



4) WiFi 설정을 아래와 같이 진행합니다. 

WiFi connection관련하여 "sdkconfig"의 값을 사용하는 WiFi 공유기(AP)의 값으로 변경합니다. 

```c
# AWS IoT EduKit Configuration
#
CONFIG_WIFI_SSID="AWSWorkshop"
CONFIG_WIFI_PASSWORD="IoTP$AK1t"
# end of AWS IoT EduKit Configuration
```


5) 아래와 같이 [PlatformIO] - [Miscellaneous] - [New Terminal]을 연 후에, "pio run --environment core2foraws"을 입력하고 [Enter]를 선택해서 Build를 진행합니다. 

```c
$ pio run --environment core2foraws
```

이후 아래와 같이 build가 수행됩니다. 

![image](https://user-images.githubusercontent.com/52392004/169673381-9627b281-f9d5-4df8-921c-0044e5afb93f.png)



6) complied된 firmware를 device에 upload 합니다.

```c
$ pio run --environment core2foraws --target upload --target monitor
```

이후 아래와 같이 upload가 진행됩니다.

![image](https://user-images.githubusercontent.com/52392004/169726765-66bc9310-63e0-4336-a234-89fb892d62a2.png)


업로드가 다 되면 아래와 같이 device가 재시작합니다.

![image](https://user-images.githubusercontent.com/52392004/169673493-f6cd70f8-dc9f-494b-b760-4f94fb8196a6.png)


7) 설치가 완료 되면, Console에서 아래와 같이 진행사항 확인이 가능합니다.

```c
␛[0;32mI (6029) WIFI: Device IP address: 192.168.68.110␛[0m
␛[0;32mI (6029) esp_netif_handlers: sta ip: 192.168.68.110, mask: 255.255.255.0, gw: 192.168.68.1␛[0m
␛[0;32mI (6059) MAIN: Connecting to AWS IoT Core at samplel34rul5-ats.iot.ap-northeast-2.amazonaws.com:8883␛[0m
␛[0;32mI (6079) aws_iot: Attempting to use device certificate from ATECC608␛[0m
␛[0;32mI (7869) MAIN: Successfully connected to AWS IoT Core!␛[0m
␛[0;32mI (7869) MAIN: Subscribing to '0123501CB56E162101/#'␛[0m
␛[0;32mI (8009) MAIN: Subscribed to topic '0123501CB56E162101/#'␛[0m
␛[0;32mI (8009) MAIN: 
****************************************
*  AWS client Id - 0123501CB56E162101  *
****************************************

␛[0m
␛[0;32mI (11279) MAIN: Subscribe callback␛[0m
␛[0;32mI (11279) MAIN: 0123501CB56E162101/      Hello from AWS IoT EduKit (QOS0) : 0 ␛[0m
␛[0;32mI (11299) MAIN: Subscribe callback␛[0m
␛[0;32mI (11299) MAIN: 0123501CB56E162101/      Hello from AWS IoT EduKit (QOS1) : 1 ␛[0m
␛[0;32mI (14409) MAIN: Subscribe callback␛[0m
␛[0;32mI (14409) MAIN: 0123501CB56E162101/      Hello from AWS IoT EduKit (QOS0) : 0 ␛[0m
␛[0;32mI (14429) MAIN: Subscribe callback␛[0m
␛[0;32mI (14429) MAIN: 0123501CB56E162101/      Hello from AWS IoT EduKit (QOS1) : 1 ␛[0m
␛[0;32mI (17549) MAIN: Subscribe callback␛[0m
␛[0;32mI (17549) MAIN: 0123501CB56E162101/      Hello from AWS IoT EduKit (QOS0) : 0 ␛[0m
```

이때, M5Stack은 아래와 같이 publish를 진행하고 있음을 확인 할 수 있습니다.

<img width="524" alt="image" src="https://user-images.githubusercontent.com/52392004/169728233-2e37c31d-98f8-4207-9384-d14321c55de7.png">



## Firmware 삭제 

다른 펌웨어를 설치하기 전에 또는 새로 빌드한 device 펌웨어의 문제로 무한 재부팅등의 오류가 발생하면 아래 명령어로 펌웨어를 제거합니다. 

```c
$ pio run --environment core2foraws --target erase
```

![image](https://user-images.githubusercontent.com/52392004/169729531-9af52413-7c70-4c93-8752-b447919129af.png)



## Trouble Shooting

#### Error message

```c
WARNING: There was an error checking the latest version of pip.
LibraryLoadError: Unable to find cryptoauthlib. You may need to reinstall:
  File "/Users/admin/.platformio/penv/lib/python3.8/site-packages/platformio/builder/main.py", line 192:
    env.SConscript(item, exports="env")
  File "/Users/admin/.platformio/packages/tool-scons/scons-local-4.3.0/SCons/Script/SConscript.py", line 597:
    return _SConscript(self.fs, *files, **subst_kw)
  File "/Users/admin/.platformio/packages/tool-scons/scons-local-4.3.0/SCons/Script/SConscript.py", line 285:
    exec(compile(scriptdata, scriptname, 'exec'), call_stack[-1].globals)
  File "/Users/admin/Documents/iot/edukit/Core2-for-AWS-IoT-EduKit/Blinky-Hello-World/register_thing.py", line 69:
    import certs_handler
  File "/Users/admin/Documents/iot/edukit/Core2-for-AWS-IoT-EduKit/Blinky-Hello-World/utilities/trustplatform/assets/python/certs_handler/__init__.py", line 1:
    from .certs_handler import *
  File "/Users/admin/Documents/iot/edukit/Core2-for-AWS-IoT-EduKit/Blinky-Hello-World/utilities/trustplatform/assets/python/certs_handler/certs_handler.py", line 18:
    from .create_cert_defs import *
  File "/Users/admin/Documents/iot/edukit/Core2-for-AWS-IoT-EduKit/Blinky-Hello-World/utilities/trustplatform/assets/python/certs_handler/create_cert_defs.py", line 30:
    from cryptoauthlib import *
  File "/Users/admin/.platformio/penv/lib/python3.8/site-packages/cryptoauthlib/__init__.py", line 23:
    raise error
  File "/Users/admin/.platformio/penv/lib/python3.8/site-packages/cryptoauthlib/__init__.py", line 20:
    load_cryptoauthlib()
  File "/Users/admin/.platformio/penv/lib/python3.8/site-packages/cryptoauthlib/library.py", line 105:
    raise LibraryLoadError('Unable to find cryptoauthlib. You may need to reinstall')
============================================================== [FAILED] Took 1.59 seconds ==============================================================

Environment             Status    Duration
----------------------  --------  ------------
core2foraws-device_reg  FAILED    00:00:01.587
========================================================= 1 failed, 0 succeeded in 00:00:01.587 =========================================================
```

#### 해결방안 

cryptoauthlib를 다시 설치해도 동일한 문제가 계속되면 아래와 같이 .platformio를 삭제후 처음부터 다시 설치해줍니다. 사내보안 시스템 등의 원인으로 platformio에서 다운로드에 실패할 수 있으니 주의 바랍니다. 

```c
$ cd
$ rm -rf .platformio
```
