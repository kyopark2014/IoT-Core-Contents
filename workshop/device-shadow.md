# Device Shadow

여기서는 [Workshop: Devicd Shadow](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR/4/1)에 대해 설명합니다. [Device Shadow](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/device-shadow.md)를 활용하여 IoT 디바이스의 상태를 확인하고, 원하는 상태로 변경할 수 있습니다. 

## Shadow로 디바이스의 상태를 확인

1) shadow을 위하여 policy의 topic에 '$aws/things/${iot:Connection.Thing.ThingName}/shadow/\*'을 추가합니다. 

[Policy Hub Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/policyhub)에 접속합니다. 현재 Workshop에서 생성한 thing의 이름이 "Mything"이므로, "MyThing-Policy"를 선택하여 아래와 같이 업데이트 합니다. 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topic/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "arn:aws:iot:ap-northeast-2:677146750822:client/${iot:ClientId}"
    }
  ]
}
```

2) SDK의 sample/shadow.py를 수정합니다. 

"aws-iot-device-sdk-python-v2/samples/shadow.py"을 파일을 열어서, "desired"로 시작하는 부분을 주석처리합니다.

![noname](https://user-images.githubusercontent.com/52392004/192134627-314acfd4-614d-45d1-a1ac-aad654988e84.png)



아래와 같이 "Enter desired value"를 "Enter reported value"로 변경합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192134572-d29a5299-5463-4e6f-bb4e-faf82ac9fae4.png)

마찬가지로 아래 2개의 부분도 "Enter desired value"를 "Enter reported value"로 변경합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192134689-53b909d5-3367-450b-9c93-017be1fadfc1.png)

[수정된 shadow.py](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/workshop/src/shadow.py)에는 상기 수정이 반영된 파일을 확인할 수 있습니다. 

3) run_shadow.sh을 생성합니다. 

"run_basicpubsub.sh"을 복사하여 "run_shadow.sh"을 생성한 후에, shadow.py로 변경후 "--thing_name MyThing"울 추가하고, "--topic '$aws/rules/iotddb/iot/sensor' --count 0"부분을 삭제합니다.

이때의 "run_shadow.sh"의 예제는 아래와 같습니다.

```c
python3 aws-iot-device-sdk-python-v2/samples/shadow.py \
--endpoint samplel34rul5-ats.iot.ap-northeast-2.amazonaws.com \
--ca_file root-CA.crt \
--cert MyThing.cert.pem \
--key MyThing.private.key \
--client_id MyThing \
--thing_name MyThing
```

4) run_shadow.sh를 실행합니다. 

아래와 같이 실행후 desired value를 넣도록 메시지가 표시됩니다. 

```c
$ ./run_shadow.sh 
Connecting to anr3wll34rul5-ats.iot.ap-northeast-2.amazonaws.com with client ID 'MyThing'...
Connected!
Subscribing to Update responses...
Subscribing to Get responses...
Subscribing to Delta events...
Requesting current shadow state...
Launching thread to read user input...
Thing has no shadow document. Creating with defaults...
Changed local shadow value to 'off'.
Updating reported shadow value to 'off'...
Update request published.
Finished updating reported shadow value to 'off'.
Enter desired value: 
```

이때, white라고 입력합니다. 

```c
white
```

[Thing Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/thinghub)로 접속하여, "MyThing"을 선택합니다. 이후 아래처럼 [Device Shadows]에서 [Classic Shadow]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192135676-39a51551-0089-4450-9892-0c14d84e652b.png)

아래와 같이 state가 업데이트 된것을 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/192135766-196641c0-dc1e-4112-b54c-9ab176f3e9e8.png)


## Shadow로 디바이스의 상태를 변경

1) 아래와 같이 [MQTT Client](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test)로 이동하여, [Subscribe to a topic]에서 [Topic filter]로 "$aws/things/MyThing/shadow/update/+"로 입력하고, [Subscribe]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192135952-d89901ea-199c-49aa-a68c-9c697906aff2.png)

2) [MQTT Client](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test)에서, [Publish to a topic]에서 [Topic name]으로 "$aws/things/MyThing/shadow/update"로 입력한 후에 [Message payload]로 아래의 값을 입력합니다. 

```java
{
   "state":{
      "desired":{
         "color":"black"
      }
   },
   "version":1
}
```

이후 아래와 같이 [Publish]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192136207-4e93cdab-da33-495c-b18f-bac61a2f98c3.png)



## Reference

[Workshop - AWS IoT Core workshop for beginners](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR)
 
