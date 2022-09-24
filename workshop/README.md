# IoT Core Workshop

여기서는 [IoT Core Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR)에 대해 설명합니다. 이 워크샵의 기본적인 architecture는 아래와 같습니다.

<img src="https://user-images.githubusercontent.com/52392004/192094151-88a49f14-3c6a-42bd-ac79-3a74de0ef55d.png" width="800">

## IoT Device Provisioning

[Device Provisioning](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/workshop/device-provisioning.md)에서는 IoT Device를 위한 인증서를 발급받고, MQTT로 메시지를 IoT Core로 전송하고 [MQTT Test client](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test)를 이용해 메시지를 확인합니다. 

## Rule Engine

[Rule Engine](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/rule.md)을 이용해 [Basic Ingent](https://docs.aws.amazon.com/iot/latest/developerguide/iot-basic-ingest.html)을 구현하면, MQTT 메시지 비용없이 rule action에서 지원하는 AWS service로 데이터를 보낼 수 있습니다. 



## Reference

[Workshop - AWS IoT Core workshop for beginners](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR)
