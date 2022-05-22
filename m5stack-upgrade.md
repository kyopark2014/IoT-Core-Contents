# AWS Edukit(M5Stack) Upgrade

## Visual Studio Code 환경 구성 

아래와 같이 Visual Studio Code의 Extentions에서 "PlatformIO IDE"를 설치합니다. 

![image](https://user-images.githubusercontent.com/52392004/169671706-b232090b-0c1a-4f8c-b31c-5f732729c48d.png)

이후, 왼쪽의 메뉴에서 "PlatformIO"를 선택하면 [PLATFORM QUICK ACCESS]로 진입 할 수 있습니다.

![noname](https://user-images.githubusercontent.com/52392004/169671783-b558e864-78ee-40f9-957a-50490050ad31.png)


## AWS Sample Download

아래와 같이 m5stack에 대한 sample을 git으로 다운로드 합니다.

```c
$ git clone https://github.com/m5stack/Core2-for-AWS-IoT-EduKit.git
```


## Device 연결

1) M5Stack을 컴퓨터와 연결합니다. 이때 자동으로 전원이 들어오지만, 안들어오는 경우에 Power on 버튼을 선택하여 Device를 켭니다. 

2) Visual Studio Code의 왼쪽 메뉴에서 [PlatformIO]를 선택 후, [Devices]를 선택후, 오른쪽에서 [Refresh]를 선택하면 아래와 같이 Device 정보를 확인 할 수 있습니다. 

![noname](https://user-images.githubusercontent.com/52392004/169672768-978ad794-a39b-4030-a42d-921e8465acf9.png)

여기서 "CP2104 USB to UART Bridge Controller"의 USB port name을 복사해 놓습니다. 상기의 경우에는 "/dev/cu.SLAB_USBtoUART" 입니다. 


## 첫 프로젝트 다운로드 하기 

1) 아래와 같이 [PlatformIO] - [Open] - [Open Project]를 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673061-f2593512-d7da-4c4b-98da-3bd058fdc2f0.png)

2) 다운로드 받은 "Core2-for-AWS-IoT-Edukit"으로 이동하여, "Getting-Started"을 선택후에 [Open "Getting-Started"을 선택합니다. 

![noname](https://user-images.githubusercontent.com/52392004/169673127-a84f49e9-ca12-4261-8670-d9c14ff80022.png)

3) 아래와 같이 [PlatformIO] - [Miscellaneous] - [New Terminal]을 연 후에, "pio run --environment core2foraws"을 입력하고 [Enter]를 누릅니다.








