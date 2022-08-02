# IoT Core의 Limits and Quotas

주요한 Limit에 대해서만 설명합니다.


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


## AWS IoT Core message broker and protocol limits and quotas

- Client ID (UTF-8) 기본값: 128 Bytes
- MQTT CONNECT 초당 요청수: 100개 (region)

- AccountId와 ㅊ
- AccountId




[Limits and Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)
