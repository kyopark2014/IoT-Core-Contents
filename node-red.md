# Node Red

Node-RED is a programming tool for wiring together hardware devices, APIs and online services in new and interesting ways.

It provides a browser-based editor that makes it easy to wire together flows using the wide range of nodes in the palette that can be deployed to its runtime in a single-click.

![image](https://user-images.githubusercontent.com/52392004/168412802-c3255a0c-bfc5-4462-b3d3-9bc889fc7065.png)



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
