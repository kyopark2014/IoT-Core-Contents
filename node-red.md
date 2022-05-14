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

4) [Linux/OSX]를 선택하여 파일을 적절한 폴더에 다운로드 합니다. "connect_device_package.zip" 파일이 다운로드 됩니다. 이후 [Next step]을 선택합니다. 해당 파일에는 아래와 같이 pem과 public/private key가 있습니다.

<img width="459" alt="image" src="https://user-images.githubusercontent.com/52392004/168450946-51a04436-c36f-436d-8b5e-5e7449c1ea7a.png">


5) [AWS-IoT] - [Manage] - [Things]로 이동하여, "node-red"로 생성한 thing을 선택합니다. 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/thinghub

![noname](https://user-images.githubusercontent.com/52392004/168451002-7617c020-5f04-4c60-add3-e59f4169e945.png)

4) [Certificaties]에서 "Certificate ID"을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/168451252-3d4c7a1a-7950-4e88-9d5b-654fd0807a2b.png)

5) [Actions] - [Download]를 선택하여 인증서파일(.crt)을 적절한 폴더에 다운로드 합니다. 



## Local에서 실행 

node 설치 

```c
$ brew install node
```

node-red 설치 

```c
$ sudo npm install -g --unsafe-perm node-red
```

## Docker에서 실행

```c
$ docker run -it -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red
```

## 실행화면 

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

## Reference

[Node-RED](https://nodered.org/)

[AWS IoT Core 설치 : 정책설정, 사물 Things 만들기, mqtt 테스트, 인증서만들기](https://www.youtube.com/watch?v=GRSWxdj2qA4)

[Node-RED를 해봅시다!](https://wikidocs.net/book/1815)

[Node Red 강의](https://youtu.be/bOUHYA7u3PU)
