# Device Provisioning

1) [Cloud9](https://ap-northeast-2.console.aws.amazon.com/cloud9/home?region=ap-northeast-2#) 또는 라즈베리파이로 디바이스를 준비합니다. 

2) [Connect - one device Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/connectdevice)로 진입하여, Next를 선택합니다. 

3) [Thing name]을 넣고 Next를 선택합니다. 이후 [Platfrom and SDK]에서 "Linux / maxOS"와 "Python"을 연속으로 선택하고 [Next]를 입력합니다. 

4) [Download connection kit]를 선택하여 zip 파일을 다운로드 한후에 1)에서 준비한 Cloud9 또는 라즈베리파이로 옮겨서 압축을 풉니다.

5) 아래와 같이 "start.sh"을 실행합니다. 

```c
unzip connect_device_package.zip
chmod +x start.sh
./start.sh
```

"start.sh"의 마지막 명령어는 아래와 같습니다. 

```sh
python3 aws-iot-device-sdk-python-v2/samples/pubsub.py --endpoint anr3wll34rul5-ats.iot.ap-northeast-2.amazonaws.com --ca_file root-CA.crt --cert cloud9.cert.pem --key cloud9.private.key --client_id basicPubSub --topic sdk/test/Python --count 0
```

"start.sh"를 실행하면 상기 명령어에 의해 아래처럼 메시지를 iot core로 전송합니다. 마지막 count에 숫자를 넣으면 숫자만큼만 전송합니다. 

```java
Sending messages until program killed
Publishing message to topic 'sdk/test/Python': Hello World! [1]
Received message from topic 'sdk/test/Python': b'"Hello World! [1]"'
Publishing message to topic 'sdk/test/Python': Hello World! [2]
Received message from topic 'sdk/test/Python': b'"Hello World! [2]"'
Publishing message to topic 'sdk/test/Python': Hello World! [3]
Received message from topic 'sdk/test/Python': b'"Hello World! [3]"'
```

6) [MQTT Test Client](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test)로 이동하여 아래처럼 "sdk/test/Python" topic으로 메시지가 들어옮을 확인 할 수 있습니다. 

<img src="https://user-images.githubusercontent.com/52392004/192094869-a8f723b3-86ab-48c3-8357-1eacae3932d1.png" width="800">

