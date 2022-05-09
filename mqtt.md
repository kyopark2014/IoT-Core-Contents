# Message Queuing Telemetry Transport(MQTT)

## Summary 

- publish-subscribe 통신 모델 기반의 메시지 전송 프로토콜

- ISO/IEC PRF 20922

- OASIS 에서 Spec에 대한 유지 보수를 담당. (2020년 기준) 최신 버전은 5.0, 가장 널리 쓰이는 버전은 3.1.1 (ISO 표준으로 등록), IoT Core 지원 버전은 3.1.1


- 비동기식: 송신자는 메시지를 전송한 이후 그에 대한 메시지 수신자의 응답을 기다리지 않고 즉시 다음 작업을 수행

- 주로 Machine to Machine (M2M) 과 Internet of Things (IoT) 컨텍스트와 같이 리소스가 제한된 환경을 포함해 다양한 상황에서 사용

- 실시간으로 데이터 처리시 문제점 : 
. 지연: 환자 측이 보낸 변화한 상태 정보를 의사 측이 아직 전송받지 못한 상황에서 의사 측이 보낸 이전 상태에 대한 처방을 환자 측이 적용한다면 문제
. 순서보장


- MQTT에서 메시지를 발행한 송신자 클라이언트는 수신자 클라이언트가 자신이 보낸 메시지를 제대로 받았는지 확인할 수 있는 별도의 방법이 존재하지 않는다. 따라서, 송신자 클라이언트가 발행한 메시지를 해당 토픽을 구독 중인 수신자 클라이언트들이 전송받은 이후, 수신자 클라이언트는 자신이 메시지를 받았음을 알리기 위해 새로운 메시지를 발행해야 한다. 송신자 클라이언트는 해당 응답을 받기 위해 새로운 토픽을 구독하고 있어야만 한다.

- MQTT는 총 14개의 패킷으로 구성, 14개의 패킷을 표시하기 위해 할당하는 바이트 수는 4바이트, 0과 15는 사용하지 않는 번호

## Topic

- MQTT에서 송신자가 특정한 토픽(topic)으로 메시지를 발행하면 해당 토픽을 구독중인 수신자들은 같은 토픽으로 발행된 모든 메시지를 받아볼 수 있다. 송신자들은 같은 방식으로 토픽에 메시지를 발행할 수 있다. 클라이언트들은 서버를 사이에 두고 통신한다. 서버는 토픽들의 리스트와 각 토픽을 구독 중인 수신자들의 리스트를 가지고 있다. 송신자가 토픽을 설정해 둔 메시지를 발행하면 서버는 발행된 메시지의 토픽을 구독중인 수신자들에게 전달함으로써 메시지를 중계해준다.

- 토픽과 서버를 이용해 메시지를 중계하는 방식을 사용하기 때문에 송신자와 수신자는 IP주소와 같이 직접적으로 통신하기 위해 필요한 정보가 없어도 통신이 가능하다. 대신 송신자와 수신자는 서로의 정보 및 상태를 알 수 없다. 이 탓에 송신자는 자신이 보낸 메시지가 수신자에게 확실하게 전송되었는지 알 수 없다

#### Topic Name과 Filter 작성 규칙

- Topic Name 과 Topic Filter는 최소한 1글자 보다 길어야 합니다.

- Topic Name과 Topic Filter는 대소문자를 구분합니다.

- 공백 문자의 사용은 가능합니다.

- Null (Unicode U+0000) 문자는 포함할 수 없습니다.

- Maximum topic 길이는 65535 bytes 입니다.

- 앞, 뒤에 "/"가 붙으면 전혀 다른 Topic Name or Topic Filter가 됩니다.


#### Topic Names and Topic Filters

- Topic은 Level Separator (‘/’)를 이용하여 아래와 같은 형태로 계층적으로 구성 가능합니다.

```c
“sport/tennis/player1”
“sport/tennis/player1/ranking”
“sport/tennis/player1/score/wimbledon”
```

#### Topic Filter에서의 wildcard 사용

- 구독자는 Topic Filter를 사용하여 Topic를 구독합니다. Topic Filter에는 wildcard 문자가 포함될 수 있으며 구독자는 이 wildcard 문자를 이용하여 다수의 Topic을 한번에 Subscribe할 수 있습니다. 사용 가능한 wildcard 문자와 의미는 아래와 같습니다. 

- “#”: Multi level wildcard. 

여러 단계의 Topic Level을 대체할 수 있습니다. 

Multi level wildcard는 단독으로 쓰이거나 Topic Filter의 마지막 Level에 위치 가능합니다. 

```c
"#": Valid. 모든 메시지 수신. 단 시스템 예약 Topic은 수신 불가
"sport/tennis/#": Valid. sport/tennis 밑의 모든 메시지 수신
“sport/tennis#”: Not valid, Level Separator 뒤에 있어야 유효
“sport/tennis/#/ranking”: Not valid. 중간 Level에 사용 불가
```

- “+”: Single level wildcard.

한단계의 토픽 레벨을 대체할 수 있습니다. 

단독으로 쓰이거나 Topic Filter내의 특정 Topic Level에 위치 가능합니다.

```c
“+”: Valid. 한 단계로 구성된 Topic 구독 가능
“sport/+/player1”: Valid. 중간 Level에 사용 가능
“+/tennis/#”: Valid. “#”과 같이 사용 가능
“sport+”: Not Valid, Level Separator 뒤에 있어야 유효
```

- 시스템 예약 Topic(‘$’)

Topic중에는 '$'로 시작하는 특수 목적의 Topic이 있습니다. 이 Topic은 읽기 전용 Topic이며 시스템에 의해 예약된 특수한 목적의 Topic이기에 Client에서는 사용할 수 없습니다. 기본적으로 제공되는 시스템 예약 토픽으로는 ‘$SYS/’ Topic이 있습니다. ‘$SYS/’ Topic은 Server의 고유 정보를 모니터링 하기 위해 사용됩니다. $SYS/의 내용은 규격상 정해져 있지 않습니다만 기본적인 지침은 있으며 아래와 같습니다.

![image](https://user-images.githubusercontent.com/52392004/167319599-d2f26b26-0d6d-4ba0-81cb-081c397b546f.png)




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


## Keep Alive

- 'CONNECT' 타입의 MQTT 제어 패킷에 포함되어 전달되는 정보이며 16bit를 사용합니다. MQTT 프로토콜은 이 Keep Alive 필드를 이용하여 Keep Alive Interval을 설정합니다. 기본적으로 Client는 Keep Alive Interval이 지나기 전의 메시지 전송을 보장해야 합니다. 전송할 메시지가 없는 경우에도 Client는 Connection 연장을 위하여 메시지를 전달해야 하며 이때 사용되는 제어 패킷이 PINGREQ, PINGRESP 입니다.

- Server는 Keep Alive 시간의 1.5배 내에 아무런 패킷이 들어오지 않으면 Client와의 연결을 끊겼다고 판단하고 Will 메시지 전송 등의 절차를 수행할 수 있습니다.

- Keep Alive 설정의 최대값은 65535초, 즉 18시간 12분 15초입니다.

- MQTT keep alive 주기는 30 ~ 1200 초

[Connection inactivity (keep-alive interval)](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)

![image](https://user-images.githubusercontent.com/52392004/167326641-7db3f13c-ac0a-41b2-adfb-69d5386c82f4.png)



## QoS

- 클라이언트는 구독중인 토픽에 QoS 레벨을 각각 설정할 수 있다. 클라이언트가 서버와 통신하며 발행된 메시지를 받아볼 때 설정된 QoS 레벨이 적용된다. MQTT에서는 클라이언트와 서버 간의 통신에서만 QoS 레벨을 보장하며, 클라이언트와 클라이언트 간의 간접 통신에서는 QoS 레벨에 따른 신뢰도 보장을 지원하지 않는다.


- 3개지 래벨 

### QoS 0: At most once delivery

- 최대 1번 전달을 보장하는 구조입니다. 수신자(Broker)가 응답을 보낼 필요가 없기에 발신자(Publisher)는 수신자가 메시지를 제대로 받았는지 확인하지 않습니다. 응답을 받지 않기에 발신자는 재전송도 하지 않습니다. 수신자는 받은 메시지를 저장하지 않고 바로 구독자에게 발송합니다. 메시지가 누락될 가능성이 있기에 메시지는 0 or 1번 전달됩니다.

![image](https://user-images.githubusercontent.com/52392004/167319083-cea51867-4bbe-469b-80a9-df6ae75e990d.png)

### QoS 1: At least once delivery

- 최소 1번 이상의 메시지 전송을 보장하는 방식입니다. 메시지 전송 시 발신자(Publisher)은 Packet ID를 포함하여 보내고 수신자(Broker)는 메시지를 받으면, 메시지를 저장한 이후 구독자에게 메시지를 보내고 메시지를 삭제합니다. 이후 메시지 수신 시 받은 Packet ID를 사용하여 PUBACK 메시지를 발신자에게 전송하면서 메시지 전달이 완료됩니다. 송신 측은 PUBACK 메시지로 수신 측이 메시지를 받았는지 확인할 수 있습니다.

- PUBACK 단계에서 Network 이슈 등으로 지연이 발생되었을 때, 발신자(Publisher)는 응답이 없기에 메시지를 재전송 할 수 있습니다. 이때 수신자가 메시지를 구독자에게 이미 발생하고 삭제한 상태라면 수신자는 메시지 중복여부를 판별할 수 없기에 구독자에게 동일한 메시지를 다시 보낼 수 있습니다. 이렇기에 QoS 1은 메시지가 미 전송되는 상황은 막을 수 있으나 메시지가 중복으로 전달될 가능성이 존재합니다.

![image](https://user-images.githubusercontent.com/52392004/167319122-1ad062ae-cb6e-4f8f-9ce5-023aba3472de.png)

### QoS 2: Exactly once delivery

- 가장 높은 QoS 레벨입니다. QoS 1에서와 같이 발신자(Publisher)는 메시지 전송 후 응답을 받아 메시지 전송성공 여부를 판단합니다. 다만 QoS 2에서는 4-way handshaking 를 사용하여 정확히 한번의 메시지 전송을 보장합니다.

- QoS 2에서 수신자는(Broker)는 자신이 가지고 있는 메시지를 삭제하기 전에 구독자에게 메시지를 전달했음을 발신자에게 알려줍니다. 이후 발신자에게 확인 메시지를 다시 받고 나서야 수신자는 자신이 저장하고 있는 메시지를 삭제합니다. PUBREC 단계에게 지연이 발생되어 발신자가 메시지를 재전송한다 하더라도 수신자는 이전에 받은 메시지를 가지고 있기에 메시지를 구독자에게 재전송하지 않습니다. 또한 PUBCOMP 단계에서 지연이 발생되었다 하더라도 이미 이전에 PUBREC를 받음으로써 메시지를 구독자에게 보냈음을 확인되었기에 구독자에게 메시지가 재전송되는 상황은 없게 됩니다.

- QoS 2는 처리에 소모되는 리소스가 크지만 이러한 방식으로 정확히 1번 구독자가 메시지를 받게 되는 것을 보장합니다. 메시지 중복 전달 시 문제가 발생되는 데이터의 경우에 사용할 수 있습니다.

![image](https://user-images.githubusercontent.com/52392004/167319139-fa8b9b01-8d29-4de1-bfd8-b71fe1aa18ea.png)





- QoS 레벨 0에서는 메시지가 최대 한 번 전송된다. 송신자는 PUBLISH 패킷을 전송한 후 메시지가 제대로 도착했는지 확인하지 않는다. 메시지는 중간에서 손실될 수 있다

- QoS 레벨 1과 2에서, 송신자가 PUBLISH 패킷 발행자(publisher)일 경우 수신자는 서버가 된다. 송신자가 서버일 경우엔 수신자가 토픽 구독자(subscriber)가 된다.

- QoS 레벨 1은 메시지가 적어도 한 번 전송되는 것을 보장한다. 송신자는 자신이 보낼 PUBLISH 패킷을 저장한 후 수신자에게 전송한다. 수신자는 PUBLISH 패킷을 받았을 경우 PUBACK 패킷을 보내 메시지가 제대로 전송되었음을 알린다. PUBLISH, 혹은 PUBACK 패킷이 중간에서 손실되어 송신자가 응답 패킷인 PUBACK 패킷을 받지 못했을 경우 수신자에게 다시 PUBLISH 패킷을 전송한다. 송신자가 성공적으로 PUBACK 패킷을 받으면 저장했던 PUBLISH 패킷을 삭제한다.


- QoS 레벨 2는 4-way handshaking을 통해 메시지가 정확하게 한 번 전송되는 것을 보장한다. 송신자는 자신이 보낼 PUBLISH 패킷을 저장한 후 전송하고, 수신자는 받은 PUBLISH 패킷을 저장한 후 PUBREC 패킷으로 응답한다. PUBREC 패킷을 받은 송신자는 저장해 두었던 PUBLISH 패킷을 삭제한 후 PUBREL 패킷으로 응답한다. 수신자가 PUBREL 패킷을 받으면 저장해 두었던 PUBLISH 패킷을 삭제한 후 PUBCOMP 패킷으로 응답한다. 중간에 패킷이 손실되어 수신자가 PUBREC 패킷을 받지 못했을 경우 PUBLISH 패킷을 다시 한 번 보낸다. 수신자가 PUBCOMP 패킷을 받지 못했을 경우 PUBREL 패킷을 다시 한 번 보낸다.


## 신뢰성 

- [MQTT가 CoAP보다 딜레이가 적다. 메시지 크기가 작고, 패킷 손실률이 25% 이하일 때, CoAP는 MQTT보다 적은 추가 트래픽으로 신뢰성 있는 전송을 보장한다](http://journal.auric.kr/kiee/XmlViewer/f404797#bib12)


## AWS IoT Core message broker and protocol limits and quotas

[AWS IoT Core message broker and protocol limits and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)

![image](https://user-images.githubusercontent.com/52392004/167324981-c9e9b524-1bf7-48c0-8680-0bc448a643e4.png)


## NLB (Network Load Balancer) Limitation

[Connection idle timeout](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancers.html)

For each TCP request that a client makes through a Network Load Balancer, the state of that connection is tracked. If no data is sent through the connection by either the client or target for longer than the idle timeout, the connection is closed. If a client or a target sends data after the idle timeout period elapses, it receives a TCP RST packet to indicate that the connection is no longer valid.

Elastic Load Balancing sets the **idle timeout value for TCP flows to 350 seconds**. You cannot modify this value. Clients or targets can use TCP keepalive packets to reset the idle timeout. Keepalive packets sent to maintain TLS connections cannot contain data or payload.

While UDP is connectionless, the load balancer maintains UDP flow state based on the source and destination IP addresses and ports, ensuring that packets that belong to the same flow are consistently sent to the same target. After the idle timeout period elapses, the load balancer considers the incoming UDP packet as a new flow and routes it to a new target. Elastic Load Balancing sets the idle timeout value for UDP flows to 120 seconds.

EC2 instances must respond to a new request within 30 seconds in order to establish a return path.


## Reference 

[QoS Level 3: A Synchronous Communication Mechanism in MQTT Protocol for IoT](http://journal.auric.kr/kiee/XmlViewer/f404797)

[MQTT 프로토콜 분석 (1) – 개요 및 패킷 구조 분석](https://blog.naver.com/mds_datasecurity/221989800838)
