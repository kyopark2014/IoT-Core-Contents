# Rule for DynamoDB

## Rule 생성 

1) [DynamoDB Tables Console](https://ap-northeast-2.console.aws.amazon.com/dynamodbv2/home?region=ap-northeast-2#tables)로 접속하여 [Create table]을 선택합니다. 

2) 아래와 같이 [Table name]으로 "IoTDB"를 입력하고, [Partition key]로 "SensorId"를 Number로 등록합니다. 또한 [Sort key]로 "TimeStamp"을 Number로 등록합니다. 이후 아래로 이동하여 [Create table]을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192097482-911c0a69-6650-44c0-933f-045250abfd3e.png)

아래와 같이 "IoTDB"가 생성됩니다. 

![image](https://user-images.githubusercontent.com/52392004/192097553-0bf6262a-41a9-4dcc-94dd-01c71ea3d314.png)

3) [IoT Core - Manage - Message Routing - Rules Console]로 진입하여 [Create rule]을 선택합니다. 이후 [Rule name]으로 아래처럼 "iotdb"로 입력 후에 [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192098125-24a1885a-1524-4e6c-b7ec-b0475a44d8f0.png)

4) 이후 [SQL statement]에서 아래와 같이 "SELECT * FROM 'iot/sensor'"라고 입력합니다. 이후 [Next]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192098241-fd8d6c4d-0d5b-46bb-b13f-074194acbde4.png)

5) [Rule actions]를 위해, 아래와 같이 [Action 1]으로 "DynamoDB"를 선택합니다. [Partition key]로 "SensorId"를 입력하고, [Partition key type]로 "NUMBER"를 선택하고, [Partition key value]로 "${SensorId}"를 입력합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192098474-4b6860d0-dae9-4f99-8886-9c974e4200c6.png)

6) 이후 아래와 같이 [Sort key]로 "TimeStamp"을 입력하고, [Sort key type]는 "NUMBER"를 [Sort key value]는 "${TimeStamp}"을 입력합니다. 편의상 [Write message data to this column]에 "Payload"라고 입력합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192098602-d34228db-157b-4612-be9e-a7c045ccf772.png)


7) 아래처럼 [Create new role]을 선택하여, "iotdb_role"을 입력합니다. 이후 [Next]를 선택한후, [Create]를 눌러서 Rule을 생성합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192098809-b864e869-1b36-4466-8e0e-fcc04f2e41a9.png)


## IoT Core로 메시지를 보내서 DynamoDB로 저장하기 

1) "run_basicpubsub.sh" 생성합니다. 

[Device Provisioning](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/workshop/device-provisioning.md)에서 다운로드한 start.sh의 마지막은 아래와 같습니다. 이것은 thing의 이름을 MyThing으로 했을때의 예제입니다. 

```c
python3 aws-iot-device-sdk-python-v2/samples/pubsub.py \
--endpoint samplel34rul5-ats.iot.ap-northeast-2.amazonaws.com \
--ca_file root-CA.crt \
--cert MyThing.cert.pem \
--key MyThing.private.key \
--client_id basicPubSub \
--topic sdk/test/Python \
--count 0
```

상기의 IoT Core Endpoint는 [IoT Core - Settings Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/settings)에서 확인하거나, 아래와 같이 AWS CLI 명령어로 확인 할 수 있습니다. 

```c
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

"run_basicpubsub.sh"을 상기의 명령어를 복사하거나, 아래처럼 "start.sh"에서 추출합니다. 

```c
tail -1 start.sh > run_basicpubsub.sh
chmod +x run_basicpubsub.sh
```

run_basicpubsub.sh파일에서 client_id로 "MyThing"을 입력하고, topic으로 '$aws/rules/iotddb/iot/sensor'을 입력합니다. 

즉, 아래와 같이 구성될 수 있습니다. 

```c
python3 aws-iot-device-sdk-python-v2/samples/pubsub.py \
--endpoint samplel34rul5-ats.iot.ap-northeast-2.amazonaws.com \
--ca_file root-CA.crt \
--cert MyThing.cert.pem \
--key MyThing.private.key \
--client_id MyThing \
--topic '$aws/rules/iotddb/iot/sensor' \
--count 0
```


2) 아래와 같이 "pubsub.py"에서 ingest를 위한 topic을 처리할수 있도록 변경하고, message로 JSON 형태의 데이터를 전송할 수 있도록 수정합니다. 

"aws-iot-device-sdk-python-v2/samples/pubsub.py" 파일을 열어서 아래와 같이 2개의 package를 import 합니다. 

```python
import random
from datetime import datetime
```

"pubsub.py"의 subscribe 부분을 모두 주석처리 합니다. 이것은 Ingest시에 사용할 Topic은 '$aws/rules/iotddb/iot/sensor'은 AWS가 reserved 한 Topic으로 subscribe를 할수 없기 때문입니다. 

![noname](https://user-images.githubusercontent.com/52392004/192132024-7fad0622-efec-4572-9855-1b95e685ebaa.png)

client에서 JSON 형태의 센서 데이터를 전송하는것처럼 시뮬레이션을 하기 위하여, 아래와 같이 message에 SensorId, SensorName, TimeStamp, Value을 JSON 포맷으로 넣을 수 있습니다.

```python
            t = datetime.utcnow() 
            timeInSeconds = int((t-datetime(1970,1,1)).total_seconds())
            message = {}
            message['SensorId'] = 0
            message['SensorName'] = 'UltraSonic'
            message['TimeStamp'] = timeInSeconds
            message['Value'] = round(random.uniform(0.1, 9.9),2)
```

아래와 같이 기존 WHILE문에 상기의 JSON 메시지를 넣고, 원래있던 message 생성부분은 주석처리를 합니다. 

![noname](https://user-images.githubusercontent.com/52392004/192132101-475461d9-7954-472c-af34-f79500ccd4e9.png)


3) Policy를 수정합니다. 

[IoT Policy Console](https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/policyhub)로 진입하여, thing 이름으로 policy를 찾습니다. 여기서는 thing 이름으로 "MyThing"을 사용하였으므로, "MyThing-Policy"라는 policy를 선택합니다. 

수정전의 MyThing-Policy"의 JSON 포맷은 아래와 같습니다. 

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
        "arn:aws:iot:ap-northeast-2:677146750822:topic/topic_1",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/topic_2"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/topic_1",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/topic_2"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect"
      ],
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:client/sdk-java",
        "arn:aws:iot:ap-northeast-2:677146750822:client/basicPubSub",
        "arn:aws:iot:ap-northeast-2:677146750822:client/sdk-nodejs-*"
      ]
    }
  ]
}
```

Ingest에서 사용할 topic은 "$aws/rules/iotddb/\*"이고, thing의 이름을 resouce로 사용할수 있도록 "${iot:ClientId}"으로 아래와 같이 변경합니다. 

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
        "arn:aws:iot:ap-northeast-2:677146750822:$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topic/topic_2"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/java",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/sdk/test/Python",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/$aws/rules/iotddb/*",
        "arn:aws:iot:ap-northeast-2:677146750822:topicfilter/topic_2"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": [
        "arn:aws:iot:ap-northeast-2:677146750822:client/${iot:ClientId}"
      ]
    }
  ]
}
```

## Reference 

[Workshop - Rule for DynamoDB](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR/3/1)
