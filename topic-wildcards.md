# Topic과 Wildcard

Topic의 명명 단위는 어플리케이션을 반영하여 계층 구조(hierarchy)로 설계합니다. 

예) sensor/temperature/room1

## Topic 이름 생성 규칙

- Topic의 이름과 필터는 UTF-8 Encoding을 따릅니다.
- case senstitive이므로 대소문자 사용시 주의합니다.
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
