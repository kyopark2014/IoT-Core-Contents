# Message Queuing Telemetry Transport(MQTT)

## Summary 

- publish-subscribe 통신 모델 기반의 메시지 전송 프로토콜

- ISO/IEC PRF 20922

- OASIS 에서 Spec에 대한 유지 보수를 담당. (2020년 기준) 최신 버전은 5.0, 가장 널리 쓰이는 버전은 3.1.1 (ISO 표준으로 등록)


- 비동기식: 송신자는 메시지를 전송한 이후 그에 대한 메시지 수신자의 응답을 기다리지 않고 즉시 다음 작업을 수행

- 주로 Machine to Machine (M2M) 과 Internet of Things (IoT) 컨텍스트와 같이 리소스가 제한된 환경을 포함해 다양한 상황에서 사용

- 실시간으로 데이터 처리시 문제점 : 
. 지연: 환자 측이 보낸 변화한 상태 정보를 의사 측이 아직 전송받지 못한 상황에서 의사 측이 보낸 이전 상태에 대한 처방을 환자 측이 적용한다면 문제
. 순서보장


- MQTT에서 메시지를 발행한 송신자 클라이언트는 수신자 클라이언트가 자신이 보낸 메시지를 제대로 받았는지 확인할 수 있는 별도의 방법이 존재하지 않는다. 따라서, 송신자 클라이언트가 발행한 메시지를 해당 토픽을 구독 중인 수신자 클라이언트들이 전송받은 이후, 수신자 클라이언트는 자신이 메시지를 받았음을 알리기 위해 새로운 메시지를 발행해야 한다. 송신자 클라이언트는 해당 응답을 받기 위해 새로운 토픽을 구독하고 있어야만 한다.

- MQTT는 총 14개의 패킷으로 구성, 14개의 패킷을 표시하기 위해 할당하는 바이트 수는 4바이트, 0과 15는 사용하지 않는 번호

## Topic

- MQTT에서 송신자가 특정한 토픽(topic)으로 메시지를 발행하면 해당 토픽을 구독중인 수신자들은 같은 토픽으로 발행된 모든 메시지를 받아볼 수 있다. 송신자들은 같은 방식으로 토픽에 메시지를 발행할 수 있다(5). 클라이언트들은 서버를 사이에 두고 통신한다. 서버는 토픽들의 리스트와 각 토픽을 구독 중인 수신자들의 리스트를 가지고 있다. 송신자가 토픽을 설정해 둔 메시지를 발행하면 서버는 발행된 메시지의 토픽을 구독중인 수신자들에게 전달함으로써 메시지를 중계해준다.

- 토픽과 서버를 이용해 메시지를 중계하는 방식을 사용하기 때문에 송신자와 수신자는 IP주소와 같이 직접적으로 통신하기 위해 필요한 정보가 없어도 통신이 가능하다. 대신 송신자와 수신자는 서로의 정보 및 상태를 알 수 없다. 이 탓에 송신자는 자신이 보낸 메시지가 수신자에게 확실하게 전송되었는지 알 수 없다

- Topic Filter : Subscription 요청 시 포함되는 일종의 표현식. 하나 또는 여러 개의 Topic을 구독할 수 있습니다. Topic Filter는 Wildcard 문자가 포함될 수 있습니다.

## MQTT Control Packet 

- 네트워크를 통해 전달되는 정보 Packet, MQTT 규격에서는 총 14개의 타입으로 Control Packet을 생성하여 전달할 수 있음

- 2~5byte의 Fixed Header를 시작으로 가변 헤더(Variable header)와 Payload가 붙어서 구성

![image](https://user-images.githubusercontent.com/52392004/167318811-2cebd3c0-674c-4e92-830d-189077e654cc.png)

- Control Packet Type: Byte 1의 7~4bit의 내용으로 Packet의 타입을 표현, 총 4bit이므로 2^4총 16개의 타입이 정의

![image](https://user-images.githubusercontent.com/52392004/167318918-358bc8ed-9a44-4f0c-bb68-c8bf4e916a95.png)

- Flags: byte 1의 0~3bit 부분으로 Control Packet Type에 따른 추가 Flags가 정의됩니다. 다만 현재 규격에서는 PUBLISH Packet을 제외하고는 예약된 고정 값을 사용합니다. PUBLISH Packet은 아래와 같이 Flags field를 사용합니다

  . DUP(Duplicate flag, bit 3) : 0일 경우, 첫번째 전달 시도임을 의미하며, 1일 경우에는 이전에 전달된 Packet이 재전송 Packet이라는 것을 의미합니다. (QoS > 0)

  . QoS(bit 1~2) : QoS(Quality of Service) 레벨입니다. 

  . RETAIN(bit 0) : 1일 경우, 서버는 해당 Topic의 최종 내용을 저장하고 있다가, 향후 구독하는 Client에게 이를 전달합니다.





## QoS

- 클라이언트는 구독중인 토픽에 QoS 레벨을 각각 설정할 수 있다. 클라이언트가 서버와 통신하며 발행된 메시지를 받아볼 때 설정된 QoS 레벨이 적용된다. MQTT에서는 클라이언트와 서버 간의 통신에서만 QoS 레벨을 보장하며, 클라이언트와 클라이언트 간의 간접 통신에서는 QoS 레벨에 따른 신뢰도 보장을 지원하지 않는다.


- 3개지 래벨 

- QoS 레벨 0에서는 메시지가 최대 한 번 전송된다. 송신자는 PUBLISH 패킷을 전송한 후 메시지가 제대로 도착했는지 확인하지 않는다. 메시지는 중간에서 손실될 수 있다

- QoS 레벨 1과 2에서, 송신자가 PUBLISH 패킷 발행자(publisher)일 경우 수신자는 서버가 된다. 송신자가 서버일 경우엔 수신자가 토픽 구독자(subscriber)가 된다.

- QoS 레벨 1은 메시지가 적어도 한 번 전송되는 것을 보장한다. 송신자는 자신이 보낼 PUBLISH 패킷을 저장한 후 수신자에게 전송한다. 수신자는 PUBLISH 패킷을 받았을 경우 PUBACK 패킷을 보내 메시지가 제대로 전송되었음을 알린다. PUBLISH, 혹은 PUBACK 패킷이 중간에서 손실되어 송신자가 응답 패킷인 PUBACK 패킷을 받지 못했을 경우 수신자에게 다시 PUBLISH 패킷을 전송한다. 송신자가 성공적으로 PUBACK 패킷을 받으면 저장했던 PUBLISH 패킷을 삭제한다.


- QoS 레벨 2는 4-way handshaking을 통해 메시지가 정확하게 한 번 전송되는 것을 보장한다. 송신자는 자신이 보낼 PUBLISH 패킷을 저장한 후 전송하고, 수신자는 받은 PUBLISH 패킷을 저장한 후 PUBREC 패킷으로 응답한다. PUBREC 패킷을 받은 송신자는 저장해 두었던 PUBLISH 패킷을 삭제한 후 PUBREL 패킷으로 응답한다. 수신자가 PUBREL 패킷을 받으면 저장해 두었던 PUBLISH 패킷을 삭제한 후 PUBCOMP 패킷으로 응답한다. 중간에 패킷이 손실되어 수신자가 PUBREC 패킷을 받지 못했을 경우 PUBLISH 패킷을 다시 한 번 보낸다. 수신자가 PUBCOMP 패킷을 받지 못했을 경우 PUBREL 패킷을 다시 한 번 보낸다.


## 신뢰성 

- [MQTT가 CoAP보다 딜레이가 적다. 메시지 크기가 작고, 패킷 손실률이 25% 이하일 때, CoAP는 MQTT보다 적은 추가 트래픽으로 신뢰성 있는 전송을 보장한다](http://journal.auric.kr/kiee/XmlViewer/f404797#bib12)


## Reference 

[QoS Level 3: A Synchronous Communication Mechanism in MQTT Protocol for IoT](http://journal.auric.kr/kiee/XmlViewer/f404797)

[MQTT 프로토콜 분석 (1) – 개요 및 패킷 구조 분석](https://blog.naver.com/mds_datasecurity/221989800838)
