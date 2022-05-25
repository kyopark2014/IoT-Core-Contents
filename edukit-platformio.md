# Visual Studio Code에 PlatformIO IDE Extension 설치 및 활용

## Visual Studio Code 환경 구성 

아래와 같이 Visual Studio Code의 Extentions에서 "PlatformIO IDE"를 설치합니다. 


![noname](https://user-images.githubusercontent.com/52392004/169784302-dbc12af6-301b-4e72-8c6a-217ead799a3b.png)


이후, 왼쪽의 메뉴에서 "PlatformIO"를 선택하면 [PLATFORM QUICK ACCESS]로 진입 할 수 있습니다.

![noname](https://user-images.githubusercontent.com/52392004/169671783-b558e864-78ee-40f9-957a-50490050ad31.png)



## AWS Sample Download

아래와 같이 m5stack에 대한 sample을 git으로 다운로드 합니다.

```c
$ git clone https://github.com/m5stack/Core2-for-AWS-IoT-EduKit.git
```


## Device 연결

1) M5Stack을 USB-C 케이블로 컴퓨터와 연결합니다. 이때 자동으로 전원이 들어오지만, 안들어오는 경우에 Power on 버튼을 선택하여 Device를 켭니다. 

<img width="485" alt="image" src="https://user-images.githubusercontent.com/52392004/169696896-6e9d7c75-bd78-48de-8cd2-d7c9356afeb1.png">

2) Visual Studio Code의 왼쪽 메뉴에서 [PlatformIO]를 선택 후, [Devices]를 선택후, 오른쪽에서 [Refresh]를 선택하면 아래와 같이 Device 정보를 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169672768-978ad794-a39b-4030-a42d-921e8465acf9.png)

여기서 "CP2104 USB to UART Bridge Controller"의 USB port name이 "/dev/cu.SLAB_USBtoUART" 임을 알 수 있습니다.


## 프로젝트 시작하기 

1) 아래와 같이 [PlatformIO] - [Open] - [Open Project]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673061-f2593512-d7da-4c4b-98da-3bd058fdc2f0.png)


2) 다운로드 받은 "Core2-for-AWS-IoT-Edukit" 폴더로 이동하여, "Blinky-Hello-World"와 같은 Sample Project를 선택하고, [Open "Blinky-Hello-World"]을 이용해 프로젝트를 열수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169726454-31675dde-fcb1-4ccb-8c84-7eab9af85086.png)


3) 아래와 같이 



![noname](https://user-images.githubusercontent.com/52392004/170182678-c6272162-6762-4dee-b887-db4aa334c3ad.png)



