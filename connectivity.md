# IoT Connectivity

### Connect / Disconnect Topic Event 

[수명 주기 이벤트 (Lifecycle events)](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)를 이용하여 Connect/disconnect topic event을 이용하여 연결상태에 대한 event를 확인 할 수 있습니다. 이 event는 default가 enable이며, disable 할 수 없습니다. 이와 관련된 Event Topic은 아래와 같습니다. 

- connect event: $aws/events/presence/connected/clientId 

- Disconnect event: $aws/events/presence/disconnected/clientId 


### 연결 해제 원인

연결 해제 원인은 [IoT Core의 설정에서 Logs을 enable](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/settings/logging)했다먄, CloudWatch에서 AWSIotLogsV2 로그 그룹을 확인하여 로그 항목의 disconnectReason 필드에서 연결 해제 이유를 식별할 수 있습니다.

또한, AWS IoT의 수명 주기 이벤트(Lifecycle events) 기능을 활용하여 연결 해제 이유를 식별할 수 있습니다. 수명 주기의 연결 해제 이벤트($aws/events/presence/disconnected/clientId)를 구독한 경우 연결 해제가 발생하면 AWS IoT에서 알림을 받게 됩니다. 알림의 disconnectReason 필드에서 연결 해제 이유를 확인할 수 있습니다. 에러의 형태에는 AUTH_ERROR, CLIENT_INITIATED_DISCONNECT, CLIENT_ERROR, CONNECTION_LOST, SERVER_ERROR 등이 있습니다.


### 수명 주기 이벤트 확인 방법

이벤트 메시지 수신 Policy에 아래와 같이 '$aws/events/\*'를 추가합니다. 

```java
{
    "Version":"2012-10-17",
    "Statement":[{
        "Effect":"Allow",
        "Action":[
            "iot:Subscribe",
            "iot:Receive"
        ],
        "Resource":[
            "arn:aws:iot:region:account:/$aws/events/*"
        ]
    }]
}
```

이후 아래처럼 '$aws/events/presence/connected/clientId'을 구독하면, connected event를 확인할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/192277910-ccfbaca2-6259-4b67-a840-36308c0d300e.png)

또한, 아래처럼 '$aws/events/presence/disconnected/clientId'을 구독하면, disconnected event를 확인할 수 있는데, 여기서, 테스트를 위해 client를 강제 종료하였으므로, "CLIENT_INITIATED_DISCONNECT"인 Reson을 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/192278792-afc6ece9-6cc7-4ba4-bb3d-58f33b4bad4a.png)

### 주의사항 

서버와 클라이언트간 TCP 연결이 갑자기 끊어지는 경우에, 때로는 keep alive가 만료될때까지 disconnected event message을 생성할 수 없습니다. 이 경우에 때로는 keep alive 시간 조정이 필요할 수 있습니다. (기본 keep alive timer는 1200초 입니다.)




## FleetIndex Query

[FleetIndex Query](https://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)로 특정 Thing이 online 상태인지 여부를 확인하거나 online 또는 offline 여부를 확인할 수 있습니다. 대신 FleetIndex의 connectivity 항목을 enable 해놓아야 합니다.

<img width="911" alt="image" src="https://user-images.githubusercontent.com/52392004/192209667-e6994903-9490-4746-8d3e-bd44e627f437.png">




## Last Will and Testament (LWT) 

[Monitor AWS IoT connections in near-real time using MQTT LWT](https://aws.amazon.com/ko/blogs/iot/monitor-aws-iot-connections-in-near-real-time-using-mqtt-lwt/)을 활용할 수 있습니다. LWT는 MQTT protocol에 있는 표준 Method로 디바이스의 끊어짐을 인지하고 알림을 만들 수 있습니다. 

<img src="https://user-images.githubusercontent.com/52392004/192209753-475dc7d5-b6c2-4b8e-b359-30361ff2b64e.png" width="600">

이때의 state event의 예는 아래와 같습니다. 

```java
{
  "state": {
    "reported": {
      "last_will": "yes",
      "trigger_action": "on",
      "client_id": "lwtThing"
        }
    }
}
```

keep-alive 주기를 적절히 조정합니다. 세션이 exprie되면, AWS IoT Core로부터 Last Will Testament (LWT) trigger가 생성됩니다. 


## Reference 

[Monitor AWS IoT connections in near-real time using MQTT LWT](https://aws.amazon.com/ko/blogs/iot/monitor-aws-iot-connections-in-near-real-time-using-mqtt-lwt/)

[AWS IoT Device SDK v2 for Python](https://github.com/aws/aws-iot-device-sdk-python-v2)

[Troubleshooting device fleet disconnects](https://docs.aws.amazon.com/iot/latest/developerguide/ota-troubleshooting-fleet-disconnects.html)
