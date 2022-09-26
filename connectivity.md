# IoT Connectivity

## Connect / Disconnect Topic Event 

[Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html)의 Connect/disconnect topic event을 이용하여 연결상태에 대한 event를 확인 할 수 있습니다. 

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

## FleetIndex Query

[FleetIndex Query](https://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html)로 특정 Thing이 online 상태인지 여부를 확인하거나 online 또는 offline 여부를 확인할 수 있습니다. 대신 FleetIndex의 connectivity 항목을 enable 해놓아야 합니다.

<img width="911" alt="image" src="https://user-images.githubusercontent.com/52392004/192209667-e6994903-9490-4746-8d3e-bd44e627f437.png">




## Last Will and Testament (LWT) 

[Monitor AWS IoT connections in near-real time using MQTT LWT](https://aws.amazon.com/ko/blogs/iot/monitor-aws-iot-connections-in-near-real-time-using-mqtt-lwt/)을 활용할 수 있습니다. 


<img src="https://user-images.githubusercontent.com/52392004/192209753-475dc7d5-b6c2-4b8e-b359-30361ff2b64e.png" width="600">


## Reference 

[Monitor AWS IoT connections in near-real time using MQTT LWT](https://aws.amazon.com/ko/blogs/iot/monitor-aws-iot-connections-in-near-real-time-using-mqtt-lwt/)

[AWS IoT Device SDK v2 for Python](https://github.com/aws/aws-iot-device-sdk-python-v2)

