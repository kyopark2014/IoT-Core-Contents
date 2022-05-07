# CloudWatch에서 로그를 확인할 수 있도록 Log를 Enable 하는 방법 

1) [AWS IoT] - [Settings] - [Logs]에서 [Manage logs]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167259406-cee805f6-2589-4d7b-9dd5-cbdeca2f5def.png)


2) 아래와 같이 [Select role]에 "cloudwatch_role"을 선택하고, [Log level]은 모든 로그를 보기 위해 "Info"로 설정합니다. 


![noname](https://user-images.githubusercontent.com/52392004/167259558-4b245aba-fec4-423b-94c3-82a744d6ddf2.png)


3) IoT client에서 "Publish"와 같은 동작을 참조합니다. 

여기서는 [Macbook을 MQTT client로 사용하기](https://github.com/kyopark2014/IoT-Core-Contents/tree/main/MQTT-client-using-mac)을 참조하여 메시지를 전송합니다. 

4) CloudWatch로 이동하여 로그를 확인합니다. 

![noname](https://user-images.githubusercontent.com/52392004/167259653-6683be97-d7bc-4f6b-972a-8e716fe7800c.png)
