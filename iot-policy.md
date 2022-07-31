# 디바이스 속성 관리 체계

## Repository

### Thing

- ThingType이 없을 경우 Max 3개까지 Attribute 지정 가능 (Indexing 대상)
- ThingType을 지정할 경우, 추가 Attribute 지정 가능. 단 ThingType에서 사전에 정의한 Attributes은 기본적으로 추가됨 (Indexing 대상)

### Thing Type

Three attributes

- Indexing 대상이 됨
- Attributes 명칭만 정해지고, 실제 값은 Thing 등록 시점에 정해짐

### Thing Group

- Group Attributes들이 있으며, 초기에 설정된 값임
- Thing 단위로 관리되는 값이 아닌, Group단위로 관리되는 값
- Thing Attributes로 상속되지 않음

## Shadow

- Thing의 현재 상태를 표시하는 JSON Document
- Shadow 변경 값에 대한 다양한 추가 정보를 제공하여 상태 추적 및 변경 관리를 용이하게 해줌 



## IoT Policy

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

