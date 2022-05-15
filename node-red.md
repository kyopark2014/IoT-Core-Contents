# Node Red

Node-RED is a programming tool for wiring together hardware devices, APIs and online services in new and interesting ways.

It provides a browser-based editor that makes it easy to wire together flows using the wide range of nodes in the palette that can be deployed to its runtime in a single-click.

![image](https://user-images.githubusercontent.com/52392004/168412802-c3255a0c-bfc5-4462-b3d3-9bc889fc7065.png)


## Node-Red용으로 인증서 받기

1) IoT Console에 접속 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/connIntro

아래와 같이 Onboard a device를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168450786-ae2023be-e8c7-4ace-bd0b-e4d51398e1da.png)

이후, [Get started]를 선택합니다. 

2) 아래처럼 선택후 [Next]를 선택합니다.

![noname](https://user-images.githubusercontent.com/52392004/168450819-3bcfab74-3c4c-47b1-a988-4e5b4e965340.png)


3) thing의 이름으로 편의상 "node-red"로 입력후 [Next step]을 선택합니다.

![noname](https://user-images.githubusercontent.com/52392004/168450896-224e1e26-6521-497a-a53f-510490274d69.png)

4) [Linux/OSX]를 선택하여 파일을 적절한 폴더에 다운로드 합니다. "connect_device_package.zip" 파일이 다운로드 됩니다. 이후 [Next step]을 선택합니다. 해당 파일에는 아래와 같이 client 인증서인 .pem과 public/private key가 있습니다.

<img width="459" alt="image" src="https://user-images.githubusercontent.com/52392004/168450946-51a04436-c36f-436d-8b5e-5e7449c1ea7a.png">

5) [Authentification](https://github.com/kyopark2014/IoT-Core-Contents/blob/main/Authentification.md)에 따라 CA 인증서를 다운로드 합니다. 

## Node-RED client를 허용하도록 Policy에 추가 

1) [AWS IoT] - [Manage] - {Things]로 이동합니다. 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/thinghub

아래와 같이 "node-red"를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168475788-b98bd854-1f37-4019-b6e7-3709a9673bcc.png)


2) [Certificates] - [Certificate ID]를 선택하여 아래처럼 들어갑니다. 


![noname](https://user-images.githubusercontent.com/52392004/168475890-3f445497-df2c-4047-b70b-77366b9b6f83.png)


3) 아래처럼 "node-red-Policy"를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168475933-469725e0-2de1-410a-a102-64df92f8ccb2.png)

4) [Edit active version]을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168476057-504dd99d-0356-4aeb-99e0-d12aa89a7194.png)

5) [Policy statements]에서 [JSON]을 선택한 후, 아래와 같이 "iot:Connect"에 "arn:aws:iot:ap-northeast-2:[account number]:client/nodered_* "를 입력 합니다. 이렇게 함으로써, Node-RED client도 접속이 가능해집니다. 아후, 아래의 "Active policy"를 enable하고 [Save as new version]을 선택합니다. 



## Node-RED를 설치

#### Local에서 실행 

node 설치 

```c
$ brew install node
```

node-red 설치 

```c
$ sudo npm install -g --unsafe-perm node-red
```

#### Docker에서 실행

```c
$ docker run -it -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red
```

#### 실행화면 

1) node-red 실행

```c
$ node-red
```

실행화면 

<img width="567" alt="image" src="https://user-images.githubusercontent.com/52392004/168412536-ce05f6e2-d107-40ce-b86a-54d24ec40d12.png">


2) 크롭에서 실행

http://127.0.0.1:1880/


실행화면 

<img width="518" alt="image" src="https://user-images.githubusercontent.com/52392004/168412478-1e94dd07-b2cc-4b18-aa05-3d0e0425bdd1.png">


## Node-RED로 IoT Core로 메시지 보내기 

1) Chrome에서 Node-RED에 접속후, 좌측메뉴에서 "Inject"를 선택합여 플로우로 옯깁니다. 옮긴후에 이름이 "타임스템프"로 아래처럼 변경됩니다. 

2) 좌측 메뉴에서 아래로 이동하여 [network]에서 "mqtt out"을 선택하고, 그 둘을 아래처럼 연결합니다. 

![image](https://user-images.githubusercontent.com/52392004/168451422-b1c3faa2-7264-4059-a5b0-2320cc4dfe02.png)

3) "mqtt out"을 더불 클릭하여 아래처럼 [토픽]에 "topic_1"을 입력하고, "QoS"에 "0"을 선택합니다. 서버 설정을 변경하기 위하여 아래처럼 수정 아이콘을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168451503-abb6336e-6a19-4941-aa3d-794145c22549.png)


4) 아래와 같이 서버의 주소로 IoT Core의 Endpoint를 입력하고, 포트는 "8883"으로 설정합니다. "사용 TLS"을 enable 한 후에 수정 버튼을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168451565-56b72c8d-7b8c-4d66-9a67-c6110700afe2.png)

5) 인증서 옆의 파일버튼을 클릭하여 ".pem"파일을 선택합니다. 비밀키로는 ".private.key"를 선택하고, CA인증서로는 "AmazonRooTCA1.cer"을 선택합니다. 이후 [변경]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168474813-5e28979a-2cf7-404f-81ac-100389b69b01.png)

6) 노드 수정화면으로 가면 [변경]을 선택하고, 이후 [완료]를 선택합니다. 

7) 상단의 [배포하기] 선택시 아래처럼 "접속됨"으로 표시되면 정상적으로 연결된것을 확인 할 수 있습니다. 

![image](https://user-images.githubusercontent.com/52392004/168474893-c6648fea-5d6d-40d5-9b2f-c6868107a819.png)

8) [AWS IoT] - [MQTT test clinet]에서 Node-RED로 부터 들어온 메시지를 확인 할 수 있습니다. 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/test

아래와 같이 [Test] - [Subscribe to a topic]에서 [Topic filter]를 "topic_1"으로 설정합니다. 이후 [Subscribe]를 선택하고, Node-RED에서 메시지 전송시 아래와 같이 "topic_1"으로 메시지가 들어오는 것을 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/168475158-0e9d061b-d27a-4b32-93a0-31fc19023ee5.png)


## Reference

[Node-RED](https://nodered.org/)

[AWS IoT Core 설치 : 정책설정, 사물 Things 만들기, mqtt 테스트, 인증서만들기](https://www.youtube.com/watch?v=GRSWxdj2qA4)

[Node-RED를 해봅시다!](https://wikidocs.net/book/1815)

[Node Red 강의](https://youtu.be/bOUHYA7u3PU)
