# Rule for DynamoDB

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




## Reference 

[Workshop - Rule for DynamoDB](https://catalog.us-east-1.prod.workshops.aws/workshops/f87a7c7a-0af8-416a-80ee-7c25c5789307/ko-KR/3/1)
