# Macbook을 MQTT client로 사용하기 

여기서는 사용하는 Macbook을 MQTT로 동작하는 IoT Device로 사용하고자 합니다.

1) AWS IOT의 Console에서 [Connect to AWS IoT]로 진입하여 "Get started"을 선택합니다.

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/connectdevice/

![noname](https://user-images.githubusercontent.com/52392004/167255937-f0b756c6-15a3-40b9-add4-b9721e3dcb3a.png)


2) [How are you connecting to AWS IoT?]에서 [Linux/OSX] - [Python]을 선택하고 [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167256102-23844124-206d-4c15-a873-b1c65abcaafd.png)

3) [Name]에 적당한 이름을 넣습니다. 여기서는 "mymac"이라고 입력합니다. 이후 [Next step]을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167256163-788c8f5c-350c-475f-8f2a-c03a29b04504.png)

4) 이후 아래와 같이 IoT device에 대한 connection kit이 생성됩니다. 

![image](https://user-images.githubusercontent.com/52392004/167256203-55ced53b-32ee-4ff7-b9c3-df5d3dcbf61a.png)

5) [Preview policy]를 선택하면 현재 생성된 policy를 확인 할 수 있습니다. 

```java
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
              "arn:aws:iot:ap-northeast-2:123456789012:topic/sdk/test/java",
              "arn:aws:iot:ap-northeast-2:123456789012:topic/sdk/test/Python",
              "arn:aws:iot:ap-northeast-2:123456789012:topic/topic_1",
              "arn:aws:iot:ap-northeast-2:123456789012:topic/topic_2"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Subscribe"
            ],
            "Resource": [
              "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/sdk/test/java",
              "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/sdk/test/Python",
              "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/topic_1",
              "arn:aws:iot:ap-northeast-2:123456789012:topicfilter/topic_2"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Connect"
            ],
            "Resource": [
              "arn:aws:iot:ap-northeast-2:123456789012:client/sdk-java",
              "arn:aws:iot:ap-northeast-2:123456789012:client/basicPubSub",
              "arn:aws:iot:ap-northeast-2:123456789012:client/sdk-nodejs-*"
            ]
          }
        ]
      }
```      

6) [Download connection kit]를 선택하여 "connect_device_package.zip" 파일을 적당한 폴더에 다운로드하고 압축을 풉니다. 

7) command로 아래와 같이 다운로드한 폴더로 이동하여, "chmod +x start.sh" 명령어로 "start.sh"를 실행가능하도록 퍼미션을 변경합니다. 

![image](https://user-images.githubusercontent.com/52392004/167256452-278b4b13-4daa-4483-b445-3a484138a5f7.png)

8) "./start.sh"로 실행합니다. 

정상적으로 실행되면 아래처럼 "MQTT" client가 실행됩니다.

```c
Running pub/sub sample application...
2022-05-07 22:29:31,533 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Initializing MQTT layer...
2022-05-07 22:29:31,533 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Registering internal event callbacks to MQTT layer...
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - MqttCore initialized
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Client id: basicPubSub
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Protocol version: MQTTv3.1.1
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Authentication type: TLSv1.2 certificate based Mutual Auth.
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring endpoint...
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring certificates and ciphers...
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring reconnect back off timing...
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Base quiet time: 1.000000 sec
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Max quiet time: 32.000000 sec
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Stable connection time: 20.000000 sec
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring offline requests queueing: max queue size: -1
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring offline requests queue draining interval: 0.500000 sec
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring connect/disconnect time out: 10.000000 sec
2022-05-07 22:29:31,534 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Configuring MQTT operation time out: 5.000000 sec
```

이후 아래와 같이 메시지가 IoT Core로 전송됩니다. 

```c
2022-05-07 22:29:33,899 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
2022-05-07 22:29:34,878 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Performing sync publish...
2022-05-07 22:29:34,879 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Filling in custom puback (QoS>0) event callback...
2022-05-07 22:29:34,905 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [puback] event
2022-05-07 22:29:34,905 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [puback] event
2022-05-07 22:29:34,905 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
2022-05-07 22:29:34,905 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - This custom event callback is for pub/sub/unsub, removing it after invocation...
2022-05-07 22:29:34,932 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [message] event
2022-05-07 22:29:34,932 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [message] event
Received a new message:
b'{"message": "Hello World!", "sequence": 1}'
from topic:
sdk/test/Python
--------------


2022-05-07 22:29:34,932 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
2022-05-07 22:29:35,910 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Performing sync publish...
2022-05-07 22:29:35,910 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Filling in custom puback (QoS>0) event callback...
2022-05-07 22:29:35,936 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [puback] event
2022-05-07 22:29:35,936 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [puback] event
2022-05-07 22:29:35,936 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
2022-05-07 22:29:35,936 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - This custom event callback is for pub/sub/unsub, removing it after invocation...
2022-05-07 22:29:35,962 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [message] event
2022-05-07 22:29:35,962 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [message] event
Received a new message:
b'{"message": "Hello World!", "sequence": 2}'
from topic:
sdk/test/Python
--------------
```


# Trouble Shooting

## Failed to build awscrt

"Failed to build awscrt"에러 발생시 [AWS CRT Python](https://pypi.org/project/awscrt/)에 따라 아래와 같이 설치를 진행합니다.

```c
git clone https://github.com/awslabs/aws-crt-python.git
cd aws-crt-python
git submodule update --init
python3 -m pip install .
```

## CMake executable is not found

"CMake executable is not found"에러시 [npm install failed - CMake executable is not found](https://github.com/cmusphinx/node-pocketsphinx/issues/24)에 따라서 아래처럼 설치 합니다. 

```c
brew install cmake
```

## ModuleNotFoundError: No module named 'AWSIoTPythonSDK'

[AWS IoT Device SDK for Python](https://github.com/aws/aws-iot-device-sdk-python)에 따라 아래와 같이 설치를 진행합니다. 

```c
git clone https://github.com/aws/aws-iot-device-sdk-python.git
cd aws-iot-device-sdk-python
python setup.py install
```


## Reference 

[Use your Windows or Linux PC or Mac as an AWS IoT device](https://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)