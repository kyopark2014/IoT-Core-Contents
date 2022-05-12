# PC에서 MQTT client를 만들어서 IoT Core를 통해 DynamoDB로 데이터 전송하기 

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

6) [Download connection kit]를 선택하여 "connect_device_package.zip" 파일을 적당한 폴더에 다운로드하고 압축을 풉니다. 압축을 풀면 아래와 같은 파일들이 있음을 확인 할 수 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/167256956-31bc75e7-8368-494d-bc59-f42b57aaaa02.png)

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


## 실제적인 동작

"start.sh"파일을 열면 실제적인 실행은 아래 명령어로 이루어짐을 알 수 있습니다. 

```c
python aws-iot-device-sdk-python/samples/basicPubSub/basicPubSub.py \
        -e abcdefghijk-ats.iot.ap-northeast-2.amazonaws.com \
        -r root-CA.crt \
        -c mymac.cert.pem \
        -k mymac.private.key
```

여기에 대한 상세한 설명은 [Use your Windows or Linux PC or Mac as an AWS IoT device](https://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)을 참고합니다.


## 실행하는 코드 분석

[basicPubSub.py](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/MQTT-client-using-mac/basicPubSub/basicPubSub.py)의 주요 내용은 아래와 같습니다.

MQTT connection을 연결합니다. 

```python
# Connect and subscribe to AWS IoT
myAWSIoTMQTTClient.connect()
if args.mode == 'both' or args.mode == 'subscribe':
    myAWSIoTMQTTClient.subscribe(topic, 1, customCallback)
time.sleep(2)
```

루프를 돌면서 publish를 수행합니다.

```python
# Publish to the same topic in a loop forever
loopCount = 0
while True:
    if args.mode == 'both' or args.mode == 'publish':
        message = {}
        message['message'] = args.message
        message['sequence'] = loopCount
        messageJson = json.dumps(message)
        myAWSIoTMQTTClient.publish(topic, messageJson, 1)
        if args.mode == 'publish':
            print('Published topic %s: %s\n' % (topic, messageJson))
        loopCount += 1
    time.sleep(1)
```    

 


## IoT Endpoint

상기 명령어의 "abcdefghijk-ats.iot.ap-northeast-2.amazonaws.com"은 iot endpoint로서 [IoT Console] - [Settings] - [Device data endpoint]에서 아래와 같이 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/167257301-6457f0d3-974b-4076-9da6-24568bbb188b.png)


## 등록된 Device 확인 

아래와 같이 [AWS IoT] - [Manage] - [Things]에 접속하면 아래와 같이 "mymac"이 등록된것을 확인 할 수 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/167257509-fab07a76-a934-48c3-acda-8602d6889012.png)

아래와 같이 "mymac"의 ARN은 "thing/mymac"으로 등록되어 있음을 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/167257547-97a7d420-24b6-4341-a130-9548f1c02319.png)


## Client 보낸 메시지를 AWS IoT Core에서 확인 

[AWS IoT] - [Test]로 이동합니다.

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test

[Subscribe to a topic]에서 [Topic filter]를 "sdk/test/Python"으로 입력하고, [Subscribe]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167955833-d690d068-eab6-4d45-848b-7ba1c50e0cb6.png)

이후 아래처럼 메시지가 들어오는지 확인 합니다. 

![image](https://user-images.githubusercontent.com/52392004/167956099-1b7810d7-c520-47a7-a26f-63322da3516f.png)

메시지 수신을 종료하려면 client에서 [Ctrl-C]를 눌러서 "start.sh"를 종료합니다.

## DynamoDB에 저장하기 

1) DynamoDB console로 이동합니다. 

https://ap-northeast-2.console.aws.amazon.com/dynamodbv2/home?region=ap-northeast-2#create-table

아래와 같이 [Table name]으로 "mymac"으로 입력하고, [Partition key]로 "iot_message"를 입력한 후, [Sort key]에서 "iot_sequence"을 입력합니다. 이후, 아래로 스크롤하여 [Create table]을 선택합니다.

![noname](https://user-images.githubusercontent.com/52392004/167959345-e31751df-1539-4818-95b7-cc30c3333f8a.png)

2) [AWS IoT Core] - [Act] - [Rules]로 이동하여 [Create rule]을 선택합니다. 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/rulehub

![image](https://user-images.githubusercontent.com/52392004/167959759-7e1aa3e1-9e38-4534-8bb7-c484afc1e2b6.png)

아래와 같이 [Rule name]을 "mymac"으로 하고 [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167960369-134b162c-bf1e-43a0-aaca-d3f5c7440275.png)

3) [SQL statement]에서 "SELECT * FROM 'sdk/test/Python'"으로 아래와 같이 입력하고 [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167967643-0c45a346-6a65-45a8-801c-104926c167ef.png)

4) [Rule actions]에서 아래와 같이 [Partition key]로 "iot_message"를 입력하고, [Partition key value]로 "${message}"로 입력합니다. [Sort key]로 "iot_sequence"를 입력하고, [Sort key value]로 "${sequence}"를 입력합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167968251-ba9efa4b-8fd0-4a5b-bcc5-76f8b52d53fa.png)

5) 아래와 같이 [IAM role]에서 [Create new role]을 선택합니다. 


![noname](https://user-images.githubusercontent.com/52392004/167968862-04a5d5c0-6be4-4b02-a050-7d533b81655c.png)

[Role name]으로 "IoT_DynamoDB"로 입력하고 [Create]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167968935-6e0b0768-b776-43d6-bdfb-415a82e02db8.png)

다시 [IAM role]에서 "IoT_DynamoDB"를 선택하고, [View]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167969096-62c00f3b-a35c-4ff9-b813-af8c5eda6f81.png)

[Trusted relationships]에서 Service로 "iot.amazons.com"이 포함되어 있음을 확인합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167969239-28e62d14-b61a-4a24-9c40-53c5233ed211.png)

```java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "iot.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

[Permissions] - [Add Permissions]에서 [Create inline policy]를 아래와 같이 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167969612-c2982bb9-11f1-47e6-852b-7ffe0e14738a.png)

아래와 같이 [JSON]에 아래의 퍼미션을 추가하고, [Review policy]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167969880-6cfa482d-4b9e-4ac7-b158-6a78e40d1c6e.png)

```java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "dynamodb:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }        
    ]
}
```
이후, 아래와 같이 [policy name]으로 "IoT_DynamoDB]로 입력한 후, [Create policy]를 선택합니다.  

![noname](https://user-images.githubusercontent.com/52392004/167970188-f8859d09-5dbb-4b08-bdd9-b08487870acb.png)

6) 다시 [IoT Core] - [Attach rule actions]로 돌아가서, [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167970490-07c0400e-12d7-4bb5-9621-f8d8e0e852c9.png)


7) client 화면으로 이동해서 다시 "start.sh"을 실행하여, [IoT Core]로 메시지를 전송합니다. 이후 아래처럼 DynamoDB로 가서 조회하면 수신된 메시지가 DyanmoDB에 잘 저장됨을 확인 할 수 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/167970686-99f4450e-6d13-42a9-892d-6a1edb543d36.png)




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


# Reference 

[Use your Windows or Linux PC or Mac as an AWS IoT device](https://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)
