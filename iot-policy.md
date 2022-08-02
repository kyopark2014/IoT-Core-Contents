# IoT Policy


IoT Policy를 이용하여 유연하고 세분화된 접근제어(Access Control)를 할 수 있습니다. 

- Device Identities와 registry와 연결할 수 있습니다. 

- MQTT Topic 수준에서 접근제어할 수 있습니다.

### 그룹 정책 적용 및 계층화

![image](https://user-images.githubusercontent.com/52392004/182020963-23b03441-be3e-40af-a390-29a7a835a21b.png)


Light bulb – Effective policy

```java
{
    "Version": "2012-10-17",
     "Statement": {
        "Effect": "Allow",
        "Action":["iot:Subscribe"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/lightbulb/admin"]
    },
    {
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/lights/${iot:ClientId}/status"]
    },
    {
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/lights/roomN/status"]
    }]
} 
```


## Reference

[대규모 디바이스 구성 및 운영을 위한 AWS IoT 서비스](https://www.youtube.com/watch?v=HTm2nORrkSw)

