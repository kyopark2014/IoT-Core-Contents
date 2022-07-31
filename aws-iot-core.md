# AWS IoT Core

## Identify Service

- 자체 인증서 또는 IoT Core에서 생성된 인증서(X.509)로 인증서 기반으로 접속을 허용할 수 있습니다. 
- AWS 인증 메소드 (SigV4)를 기반으로 AWS Access keys로 인증할 수 있습니다.
- Token 기반 인증 지원
- IoT Policy를 통한 유연하고 세분화 된 엑세스 컨트롤

## Device Gateway

- Device를 위한 클라우드 진입점
- 장시간(Long-Lived Connection) 양방향 통신 기능
- MQTT, Websockets, HTTP를 포함한 다양한 프로토콜 지원
- TLS 1.2를 통한 보안 통신

## Message Broker

- MQTT 프로토콜 기반의 메시지 라우팅 (PUB/SUB)제공
- 디바이스와 어플리케이션간 양방향 메시지 스트리밍
- QoS0와 QoS1 메시지 지원
- 와일드 카드 토픽 필터를 포함한 커스텀 토픽 지원

## Rule Engine (데이터 변환 및 Actions)

- SQL과 유사한 변환 언어 사용
- 변환: 수학, 문자열, 날짜변환 내장 함수들 제공
- 필터: Where 구분 사용
- 가공: 쉐도우, 머신러닝 등 외부소스에서 데이터 가져오기
- 라우팅: AWS 서비스 및 3rd Party에 연결됨

## Reference 

[AWS IoT 톺아보기 - 디바이스에서 분석까지](https://www.youtube.com/watch?v=zZBXYSVSWI8)
