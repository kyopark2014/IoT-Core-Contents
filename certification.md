# Device 인증서 생성

1) Device 인증서를 받기 위하여 [AWS IoT] - [Connect to AWS IoT] - [Get started]로 진입합니다. 이후 단계를 확인 한 후, [Get started]를 선택합니다.

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/connIntro


![noname](https://user-images.githubusercontent.com/52392004/170201603-eb22fa6d-3386-4d81-8eed-eff63c8a4c08.png)

2) 아래와 같이 [Linux/OSX]와 [Python]을 선택하고 [Next]를 선택합니다. 선호에 따라 [Node.js]나 [Java]를 선택할 수 있습니다. 

<img src="https://user-images.githubusercontent.com/52392004/170202244-bcf39e71-381d-4a54-a823-2a45b471cb7f.png" width=“800">
                       

3) [Register a thing] - [Name]에 적절한 디바이스 이름을 넣습니다. 여기서는 [M5Stack]이라고 입력했습니다. 이후 [Next step]을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170202636-5c424bfb-c654-4c19-9728-81e863ad4e8a.png)

4) 아래와 같이 [Download connection kit for]를 선택해서 인증서(certification)을 다운로드 합니다. 

![noname](https://user-images.githubusercontent.com/52392004/170203424-e5b21e02-fd03-4c37-82c0-1cec595e2ec3.png)

다운로드 한 파일에는 아래와 같은 파일들이 있습니다.

![image](https://user-images.githubusercontent.com/52392004/170203597-c54507c4-d346-4088-80d7-4b652132b267.png)

여기서 "cert.pem"은 해당 device의 인증서이며, "private.key"는 비밀키, "public.key"은 공개키입니다. "start.sh"는 MQTT에 메시지를 보내는 script 입니다.

이 파일 이외에 서버의 CA인증서는 아래와 같이 다운로드 합니다. 

```c
$ curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > AmazonRootCA1.cer
```

이후 [Next step]을 누르면 아래와 같이 현재 Configure와 실행방법을 안내 합니다. 

![image](https://user-images.githubusercontent.com/52392004/170204656-ca1e6199-2deb-49fc-9317-6e6eae774f8e.png)


