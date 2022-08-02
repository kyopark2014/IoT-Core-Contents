# AWS IoT Core

## Identify Service

- 자체 인증서 또는 IoT Core에서 생성된 인증서(X.509)로 인증서 기반으로 접속을 허용할 수 있습니다. 
- AWS 인증 메소드 (SigV4)를 기반으로 AWS Access keys로 인증할 수 있습니다.
- Token 기반 인증 지원합니다.
- IoT Policy를 통한 유연하고 세분화 된 엑세스 컨트롤을 할 수 있습니다.

## Device Gateway

- Device를 위한 클라우드 진입점입니다.
- 장시간(Long-Lived Connection) 양방향 통신 기능을 제공합니다.
- MQTT, Websockets, HTTP를 포함한 다양한 프로토콜 지원합니다.
- TLS 1.2를 통한 보안 통신을 지원합니다.

## Message Broker

- MQTT 프로토콜 기반의 메시지 라우팅 (PUB/SUB)제공합니다.
- 디바이스와 어플리케이션간 양방향 메시지 스트리밍을 지원합니다.
- QoS0와 QoS1 메시지 지원합니다. (QoS2 미지원)
- 와일드 카드 토픽 필터를 포함한 커스텀 토픽을 지원합니다. 

## Rule Engine (데이터 변환 및 Actions)

- SQL과 유사한 변환 언어 사용할 수 있습니다.
- 변환시 수학, 문자열, 날짜변환 내장 함수들 제공합니다.
- Where 구분 사용하여 필터링을 할 수 있습니다.
- 쉐도우, 머신러닝 등 외부소스에서 데이터 가져오기와서 가공할 수 있습니다.
- AWS 서비스 및 3rd Party에 연결할 수 있습니다. 

## Device Shadow

- 클라우드 내 디바이스의 상태를 가지는 이미지 (쌍둥이모델)입니다.
- 쉐도우 업데이트를 통한 디바이스 컨트롤이 가능합니다.
- Dedicate된 MQTT 토픽들에 실시간의로 변경상태를 공지합니다.
- 오프라인 디바이스의 최종상태 추적할 수 있습니다.
- 어플리케이션을 위한 REST APIs를 제공합니다.

## Registry

- 고정된 디바이스 메타데이터의 클라우드 카탈로그입니다.
- 자격증명이나 시리얼, 기능 성명 등을 설정할 수 있습니다.
- 공통 속성의 Things 그룹화 합니다.
- 그룹을 계층구조화 관리 기능이 있습니다.
- 상위 그룹에 적용된 Policy를 하위그룹에 상속 가능합니다. 


## Reference 

[AWS IoT 톺아보기 - 디바이스에서 분석까지](https://www.youtube.com/watch?v=zZBXYSVSWI8)
