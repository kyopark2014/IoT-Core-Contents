# MQTT Persistent Session

## Persistent Session

- Broker가 Complexity를 가져가게 되며, 세션정보를 기억합니다.
- Client에서 CONNECT 메시지를 보낼때, cleanSession flag을 0(false)로 하여 설정하게 됩니다.
- Client는 persistent session이 있다면, connection acknowledged(CONNACK)에 sessionPresent attribute를 사용합니다. sessionPersent가 1이면, 저장된 메시지가 전달됩니다. 
- 모든 client library가 지원하고 있습니다. 
- Broker의 persistent session을 가지고 있는 client가 offline이 되면, QoS1과 QoS1인 메시지를 Queue에 저장합니다. 이때, QoS0인 메시지는 Queue에 저장되지 않습니다. 
- IoT Core는 QoS0, QoS1만 사용할 수 있으므로, QoS1 메시지만 Queue에 저장할수 있습니다. 
- 연결이 끊겼을때에도 Session data(예: clientID)를 이용하여 Queue에 저장합니다. 

## Best practices

#### Clean Session

- Client가 publish만 하는 경우
- 메시지 손실이 acceptable한 경우

#### Persistent Session

- Subscriber가 메시지를 잃어 버리면 안되는 경우
- Broker가 subscription 정보를 저장해야 하는 경우



## 단점

- 어떤 장치의 상태를 offline에서 계속 바꾼다면, queue에 쌓이는 메시지는 계속 과금되게 되고, online이후에 메시지를 가져갈때에도 과금이 발생하게 됩니다. (shadow대비 비용이 발생함) 
- MQTT는 queue에 저장된 메시지에 기본적으로 expire 값을 가지고 있지 않음 

## Reference

[MQTT Essentials - Part 8 | Persistent Session and Message Queueing](https://www.youtube.com/watch?v=2ETj1fM7-ZA)


[MQTT persistent sessions](https://docs.aws.amazon.com/iot/latest/developerguide/mqtt.html)
