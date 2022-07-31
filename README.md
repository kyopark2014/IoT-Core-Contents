# IoT Core Contents

AWS IoT에는 아래와 같은 서비스들이 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/172954318-da3283ac-991c-4cb7-934f-2a97fd49edcd.png)

여기서는 IoT 관련된 중요 내용들을 요약 정리하고자 합니다. 


## AWS IoT Core

[AWS IoT Core](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/aws-iot-core.md)에 대해 설명합니다. 


## MQTT: Message Queue Telemetry Transport

[MQTT](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/mqtt.md)에서는 MQTT Protocal에 대해 설명합니다. 

## Macbook을 MQTT client로 사용하기

[Macbook을 MQTT client로 사용하기](https://github.com/kyopark2014/IoT-Core-Contents/tree/main/MQTT-client-using-mac)을 이용하여 MQTT를 테스트 할 수 있도록 IoT core에 Macbook을 등록하여 테스트 메시지를 발송합니다. 


## IoT Policy

[Iot Policy](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/iot-policy.md)에서는 Policy에 대해 설명합니다.


## CloudWatch Logging

[Cloudwatch Logging](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/cloudwatch.md)을 참조하여 로그를 Enable 합니다. 

## Node-Red로 시험하기

[Node-Red](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/node-red.md)로 MQTT를 시험할 수 있습니다. 

## AWS IoT Edukit Workshop

[AWS IoT Edukit Workshop](https://github.com/kyopark2014/aws-iot-edukit)은 M5stack을 예제를 통해 M5stack을 제어하고 AWS IoT Core와 연결 할 수 있습니다. 


## Topic과 Wildcard

[Topic 생성 규칙 및 Wildcard](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/topic-wildcards.md)에 대해 설명합니다. 

## Device Shadow

[Device Shadwow](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/device-shadow.md)에 대해 설명합니다. 

## OPC UA

Open Platform Communications Unified Architecture (OPC UA)는 PLC(Programable Logic Controller)의 데이터를 수집하기 위한 국제 산업 표준 통신 규약 입니다. 

## AWS IoT Core limits and quotas

[AWS IoT Core rules engine limits and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)에 따라 아래와 같이 특성이 있으니 참고 바랍니다.

- Maximum number of rules per AWS account : 1000

- Rule evaluations per second per AWS account : 20000

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
