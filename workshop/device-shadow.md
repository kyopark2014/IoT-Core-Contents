# Device Shadow

여기서는 [Workshop: Devicd Shadow](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR/4/1)에 대해 설명합니다. [Device Shadow](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/device-shadow.md)를 활용하여 IoT 디바이스의 상태를 확인하고, 원하는 상태로 변경할 수 있습니다. 


1) shadow을 위하여 policy의 topic에 '$aws/things/${iot:Connection.Thing.ThingName}/shadow/\*'을 추가합니다. 

[Policy Hub Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/policyhub)에 접속합니다. 현재 Workshop에서 생성한 thing의 이름이 "Mything"이므로, "MyThing-Policy"를 선택하여 아래와 같이 업데이트 합니다. 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topic/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "arn:aws:iot:ap-northeast-2:677146750822:client/${iot:ClientId}"
    }
  ]
}
```

2) SDK의 sample/shadow.py를 수정합니다. 

"aws-iot-device-sdk-python-v2/samples/shadow.py"을 파일을 열어서, "desired"로 시작하는 부분을 주석처리합니다.

![noname](https://user-images.githubusercontent.com/52392004/192134627-314acfd4-614d-45d1-a1ac-aad654988e84.png)



아래와 같이 "Enter desired value"를 "Enter reported value"로 변경합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192134572-d29a5299-5463-4e6f-bb4e-faf82ac9fae4.png)

마찬가지로 아래 2개의 부분도 "Enter desired value"를 "Enter reported value"로 변경합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192134689-53b909d5-3367-450b-9c93-017be1fadfc1.png)

 


## Reference

[Workshop - AWS IoT Core workshop for beginners](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR)
 
