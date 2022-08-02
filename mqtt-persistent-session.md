# MQTT Persistent Session

## Persistent Session

- Broker가 Complexity를 가져가게 되며, 세션정보를 기억합니다.
- cleanSession flag을 false로 하여 설정하게 됩니다.
- 모든 client library가 지원하고 있습니다. 
- 연결이 끊겼을때에도 Session data(예: clientID)를 이용하여 Queue에 저장합니다. 

## 단점

- 어떤 장치의 상태를 offline에서 계속 바꾼다면, queue에 쌓이는 메시지는 계속 과금되게 되고, online이후에 메시지를 가져갈때에도 과금이 발생하게 됩니다. (shadow대비 비용이 발생함) 


## Reference

[MQTT Essentials - Part 8 | Persistent Session and Message Queueing](https://www.youtube.com/watch?v=2ETj1fM7-ZA)
