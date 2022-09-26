# IoT Connectivity

## Connect / Disconnect Topic Event 

[Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)의 Connect/disconnect topic event을 이용하여 연결상태에 대한 event를 확인 할 수 있습니다. 

기본적으로 iot core setting에 disable되어 있고, 해당 이벤트 메시지를 수신할 때마다 추가 과금이 될 수 있으므로 목적에 맞게 사용합니다.

#### Event 정보

- connect event: $aws/events/presence/connected/clientId 

- Disconnect event: $aws/events/presence/disconnected/clientId 


#### Disconnect Reason

[Disconnect 되었을때에 아래의 Reason](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)을 이용하여 원인을 확인할 수 있습니다.

에러의 형태에는 AUTH_ERROR, CLIENT_INITIATED_DISCONNECT, CLIENT_ERROR, CONNECTION_LOST, SERVER_ERROR 등이 있으며, 예는 아래와 같습니다. 

```java
{
    "clientId": "186b5",
    "timestamp": 1573002340451,
    "eventType": "disconnected",
    "sessionIdentifier": "a4666d2a7d844ae4ac5d7b38c9cb7967",
    "principalIdentifier": "12345678901234567890123456789012",
    "clientInitiatedDisconnect": true,
    "disconnectReason": "CLIENT_INITIATED_DISCONNECT",
    "versionNumber": 0
}
```

#### 설정 

이벤트 메시지를 사용하려면 AWS IoT 콘솔의 설정(Settings) 탭으로 이동한 다음 이벤트 기반 메시지(Event-based messages) 섹션에서 이벤트 관리(Manage events)를 선택합니다. 관리할 이벤트를 지정할 수 있습니다.

<img width="732" alt="image" src="https://user-images.githubusercontent.com/52392004/192221816-ace6a1e0-7aca-45aa-8f0a-e676bcd61a94.png">


API 또는 CLI를 사용하여 게시할 이벤트 유형을 제어하려면 UpdateEventConfigurations API를 호출하거나 update-event-configurations CLI 명령을 사용합니다. 예:

```c
aws iot update-event-configurations --event-configurations "{\"THING\":{\"Enabled\": true}}"
```

<img width="560" alt="image" src="https://user-images.githubusercontent.com/52392004/192221550-364d8d23-81ba-44ed-ba9a-d2a3c8e085ea.png">



https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/iot-events.html#iot-events-settings-table


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

