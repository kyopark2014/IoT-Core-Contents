# Topic과 Wildcard

## Topic

- MQTT에서 송신자가 특정한 토픽(topic)으로 메시지를 발행하면 해당 토픽을 구독중인 수신자들은 같은 토픽으로 발행된 모든 메시지를 받아볼 수 있습니다. 송신자들은 같은 방식으로 토픽에 메시지를 발행할 수 있습니다. 
- 클라이언트들은 서버를 사이에 두고 통신합니다. 
- 서버는 토픽들의 리스트와 각 토픽을 구독 중인 수신자들의 리스트를 가지고 있습니다. 
- 송신자가 토픽을 설정해 둔 메시지를 발행하면 서버는 발행된 메시지의 토픽을 구독중인 수신자들에게 전달함으로써 메시지를 중계해줍니다.
- 토픽과 서버를 이용해 메시지를 중계하는 방식을 사용하기 때문에 송신자와 수신자는 IP주소와 같이 직접적으로 통신하기 위해 필요한 정보가 없어도 통신이 가능합니다. 대신 송신자와 수신자는 서로의 정보 및 상태를 알 수 없습니다. 이때, 송신자는 자신이 보낸 메시지가 수신자에게 확실하게 전송되었는지 알 수 없습니다. 



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


## Wildcard Topic Filter

사용자 지정 Topic space를 정의합니다. 

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



Topic의 명명 단위는 어플리케이션을 반영하여 계층 구조(hierarchy)로 설계합니다. 

예) sensor/temperature/room1

## Topic 이름 생성 규칙

- Topic의 이름과 필터는 UTF-8 Encoding을 따릅니다.
- case sensitive이므로 대소문자 사용시 주의합니다.
- '$'로 시작하는 [reserved topics](https://docs.aws.amazon.com/iot/latest/developerguide/reserved-topics.html)는 AWS IoT Core를 위해 사용합니다. 
  예) $aws/sitewise, $aws/things, $aws/events/, $aws/certificates
- '/'를 이용해 Topic의 계층 구조를 표현합니다. 
  
## Topic filter wildcard

Wildcard를 유용하게 사용할 수 있습니다.

![image](https://user-images.githubusercontent.com/52392004/182006975-8e251b28-956a-454d-8b0e-9e59bbfdc4f9.png)

사용예: sensor/#, sensor/+/room1




## Reference

[MQTT topics](https://docs.aws.amazon.com/iot/latest/developerguide/topics.html)

[AWS Greengrass, Lambda and ML Inference at the Edge site](https://www.youtube.com/watch?v=T9do1kWL6SE)
