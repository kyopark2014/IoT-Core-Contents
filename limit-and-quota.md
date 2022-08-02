# IoT Core의 Limits and Quotas

주요한 Limit에 대해서만 설명합니다.


[AWS IoT Core rules engine limits and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)에 따라 아래와 같이 특성이 있으니 참고 바랍니다.

- Maximum number of rules per AWS account : 1000

- Rule evaluations per second per AWS account : 20000

- Device Shadow API requests/second per account: 4000

- Maximum number of in-flight, unacknowledged messages per thing: 10

- Maximum size of a JSON state document: 8k






## AWS IoT Core message broker and protocol limits and quotas

- Client ID (UTF-8) 기본값: 128 Bytes

- MQTT CONNECT 초당 요청수: 100개 (region)

- accountId와 clientId가 같을때 초당 요청할 수 있는 MQTT CONNECT: 1

- Keep alive interval: 1200s (default)

- 1개 account가 보낼수 있는 초당 최대 PUBLISH: 2000 

- 1개 account가 생성할 수 있는 최대 동시 세션수: 1,000,000 (Region)

- Outbound publish requests per second per account: 2000

- Connection inactivity (keep-alive interval): 1200s

- Persistent session expiry period: 3600s

- Publish requests per second per connection: 100

- Queued messages per second per account: 500

- Subscriptions per account: 100,000

- WebSocket connection duration: 86,400s


## MQTT based File Delivery

- Account당 Stream의 숫자: 10,000

- 최대 파일 크기: 24 MB

- 최대 데이터 블럭: 128 KB

## AWS IoT Core rules

- Rule당 최대 action 숫자: 10

- Account당 최대 Rule의 숫자: 10000

- Account당 1초동안 가능한 최대 Rule: 20000

- Rule 최대 크기: 256 KB




[Limits and Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)
