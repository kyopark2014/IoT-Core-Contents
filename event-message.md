# IoT Core Event Message

이벤트 메시지를 사용하려면 AWS IoT 콘솔의 설정(Settings) 탭으로 이동한 다음 이벤트 기반 메시지(Event-based messages) 섹션에서 이벤트 관리(Manage events)를 선택합니다. 관리할 이벤트를 지정할 수 있습니다.

<img width="732" alt="image" src="https://user-images.githubusercontent.com/52392004/192221816-ace6a1e0-7aca-45aa-8f0a-e676bcd61a94.png">


API 또는 CLI를 사용하여 게시할 이벤트 유형을 제어하려면 UpdateEventConfigurations API를 호출하거나 update-event-configurations CLI 명령을 사용합니다. 예:

```c
aws iot update-event-configurations --event-configurations "{\"THING\":{\"Enabled\": true}}"
```

## Reference 

[Event Message](https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/iot-events.html#iot-events-settings-table)
