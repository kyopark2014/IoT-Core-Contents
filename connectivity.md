# IoT Connectivity

## Connect / Disconnect Topic Event 

[Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)를 이용하여 Connect/disconnect topic event을 이용하여 연결상태에 대한 event를 확인 할 수 있습니다. 이 event는 default가 enable이며, disable 할 수 없습니다. 


#### 연결 해제 원인

연결 해제 원인을 진단하려면 CloudWatch에서 AWSIotLogsV2 로그 그룹을 확인하여 로그 항목의 disconnectReason 필드에서 연결 해제 이유를 식별할 수 있습니다.

또한 AWS IoT의 수명 주기 이벤트 기능을 사용하여 연결 해제 이유를 식별할 수 있습니다. 수명 주기의 연결 해제 이벤트($aws/events/presence/disconnected/clientId)를 구독한 경우 연결 해제가 발생하면 AWS IoT에서 알림을 받게 됩니다. 알림의 disconnectReason 필드에서 연결 해제 이유를 확인할 수 있습니다.



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

#### 설정 방법

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



API 또는 CLI를 사용하여 게시할 이벤트 유형을 제어하려면 UpdateEventConfigurations API를 호출하거나 update-event-configurations CLI 명령을 사용합니다.

```c
aws iot update-event-configurations --event-configurations "{\"THING\":{\"Enabled\": true}}"
```

<img width="560" alt="image" src="https://user-images.githubusercontent.com/52392004/192221550-364d8d23-81ba-44ed-ba9a-d2a3c8e085ea.png">



https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/iot-events.html#iot-events-settings-table



connected/disconnected event를 enable 하기 위한 명령어는 아래와 같습니다.  

```java
aws iot update-event-configurations --event-configurations "{\"CERTIFICATE\":{\"Enabled\": true}}"
```

아래와 같이 상태 확인이 가능합니다. 

```java
aws iot describe-event-configurations
```

이때의 결과는 아래와 같습니다. 

```java
{
    "eventConfigurations": {
        "CA_CERTIFICATE": {
            "Enabled": false
        },
        "CERTIFICATE": {
            "Enabled": false
        },
        "JOB": {
            "Enabled": false
        },
        "JOB_EXECUTION": {
            "Enabled": false
        },
        "POLICY": {
            "Enabled": false
        },
        "THING": {
            "Enabled": false
        },
        "THING_GROUP": {
            "Enabled": false
        },
        "THING_GROUP_HIERARCHY": {
            "Enabled": false
        },
        "THING_GROUP_MEMBERSHIP": {
            "Enabled": false
        },
        "THING_TYPE": {
            "Enabled": false
        },
        "THING_TYPE_ASSOCIATION": {
            "Enabled": false
        }
    },
    "creationDate": "2022-05-07T23:38:55.809000+09:00",
    "lastModifiedDate": "2022-05-07T23:39:22.960000+09:00"
}
```

#### 주의사항 

Event message로는 서버에서는 연결되어 있는 것으로 착각하고 실제 디바이스 쪽에서는 연결이 끊어진 half connection은 잡아낼 수 없습니다. 결국 저 이벤트 메시지 catch하시더라도, 서버 측에서는 keep alive가 만료되어야 그것을 인지하고 저 이벤트 메시지를 보내 주게 됩니다. (keep alive 시간 조정)

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
