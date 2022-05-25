# IoT Core Contents

AWS IoT Architecutre에는 아래와 같은 서비스들이 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/167635145-b465c2a5-481b-4589-b07f-818b9a8c79d4.png)

여기서는 IoT 관련된 중요 내용들을 요약 정리하고자 합니다. 


## AWS IoT Core limits and quotas

[AWS IoT Core rules engine limits and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)에 따라 아래와 같이 특성이 있으니 참고 바랍니다.

- Device Shadow API requests/second per account: 4000

- Maximum number of in-flight, unacknowledged messages per thing: 10

- Maximum size of a JSON state document: 8k

- Inbound publish requests per second per account: 2000 (Region)

- Maximum concurrent client connections per account: 1,000,000 (Region)

- Outbound publish requests per second per account: 2000

- Connection inactivity (keep-alive interval): 1200s

- Persistent session expiry period: 3600s

- Publish requests per second per connection: 100

- Queued messages per second per account: 500

- Subscriptions per account: 100,000

- WebSocket connection duration: 86,400s

## MQTT: Message Queue Telemetry Transport

[MQTT](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/mqtt.md)에서는 MQTT Protocal에 대해 설명합니다. 

## Macbook을 MQTT client로 사용하기

[Macbook을 MQTT client로 사용하기](https://github.com/kyopark2014/IoT-Core-Contents/tree/main/MQTT-client-using-mac)을 이용하여 MQTT를 테스트 할 수 있도록 IoT core에 Macbook을 등록하여 테스트 메시지를 발송합니다. 


## CloudWatch Logging

[Cloudwatch Logging](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/cloudwatch.md)을 참조하여 로그를 Enable 합니다. 

## Node-Red로 시험하기

[Node-Red](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/node-red.md)로 MQTT를 시험할 수 있습니다. 

## AWS IoT Edukit Workshop

[AWS IoT Edukit Workshop](https://edukit.workshop.aws/en/)에는 M5stack을 위한 여러가지 예제를 제공하고 있습니다. 해당 예제를 통해 M5stack을 제어하고 AWS IoT Core와 연결 할 수 있습니다. 



### 1) Visual Studio Code 환경 구성

M5Stack에 펌웨어를 업그레이드 하거나 삭제하는 동작은 Visual Studio Code를 이용해 편리하게 할 수 있습니다. 

[Visual Studio Code에 PlatformIO IDE Extension 설치 및 활용](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/edukit-platformio.md)에서는 M5Stack을 Visual Studio Code에 연결 후 Build, Upload, Monitoring 하는 방법을 설명하고 있습니다.



### 2) CLOUD CONNECTED BLINKY

Workshop sample중 하나인 [CLOUD CONNECTED BLINKY](https://edukit.workshop.aws/en/blinky-hello-world.html)에서는 M5stack을 IoT Core에 등록하는 script를 실행 할 수 있고, 이를 통해 연결이 되면 MQTT로 IoT Core에 메시지를 보낼 수 있습니다. IoT Core에서 blinky 명령을 실행하면 M5Stack의 LED를 제어 할 수 있습니다. 

[AWS Edukit(M5Stack) - Blinky](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/edukit-blinky.md)에서는 M5Stack을 IoT Core에 등록하는 script를 실행하고, M5Stack에 펌웨어 업그레이드를 진행하고, IoT Core에서 전송한 명령어로 Blinky 동작을 수행하는 과정을 설명합니다.

### 3) SMART THERMOSTAT

SMART THERMOSTAT에서는 IoT core에 등록된 




## Others

AWS IoT Device SDKs simplify using AWS IoT Core with your devices and
applications with an API tailored to your programming language or platform.

Amazon FreeRTOS is a real time operating system for microcontrollers that lets you
program small, low-power, edge devices while leveraging memory-efficient, secure,
embedded libraries.

AWS IoT Greengrass is a software component that extends the Linux Operations
System of your IoT devices. AWS IoT Greengrass allows you to run MQTT local routing
between devices, data caching, AWS IoT shadow sync, local AWS Lambda functions,
and machine learning algorithms. 
