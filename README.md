# IoT Core Contents

여기서는 IoT 관련된 중요 내용들을 요약 정리하고자 합니다. 

## MQTT: Message Queue Telemetry Transport

[MQTT](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/mqtt.md)에서는 MQTT Protocal에 대해 설명합니다. 

## AWS IoT Core limits and quotas

[AWS IoT Core rules engine limits and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)

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


## Macbook을 MQTT client로 사용하기

[Macbook을 MQTT client로 사용하기](https://github.com/kyopark2014/IoT-Core-Contents/tree/main/MQTT-client-using-mac)을 이용하여 MQTT를 테스트 할 수 있도록 IoT core에 Macbook을 등록하여 테스트 메시지를 발송합니다. 


## CloudWatch Logging

[Cloudwatch Logging](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/cloudwatch.md)을 참조하여 로그를 Enable 합니다. 


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
