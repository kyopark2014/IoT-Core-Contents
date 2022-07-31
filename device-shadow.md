# Device Shadow

Device Shadow를 사용하면 Device가 네트워크에 연결되지 않은 경우에도 Device의 상태를 사용 할 수 있습ㄴ디ㅏ. 

```c
$aws/things/+/shadow/update/delta
```

```c
$aws/things/+/shadow/update/accepted


모든 shadow는 예약된 MQTT Topic과 HTTP URL을 이용하여 Get, Update, Delete와 같은 action을 할 수 있습니다. 

- desired: 사용자가 원하는 상태 (Apps specify the desired states of device properties by updating the desired object)

- reported: Thing 보고한 현재 상태 (Devices report their current state in the reported object)

- delta: desired와 reported가 서로 다른 상태 (AWS IoT reports differences between the desired and the reported state in the delta object

```java
{
  "state": {
    "desired": {
      "color": "RED",
      "state": "STOP"
    },
    "reported": {
      "color": "GREEN",
      "engine": "ON"
    },
    "delta": {
      "color": "RED",
      "state": "STOP"
    }
  },
  "metadata": {
    "desired": {
      "color": {
        "timestamp": 12345
      },
      "state": {
        "timestamp": 12345
      }
      },
      "reported": {
        "color": {
          "timestamp": 12345
        },
        "engine": {
          "timestamp": 12345
        }
      },
      "delta": {
        "color": {
          "timestamp": 12345
        },
        "state": {
          "timestamp": 12345
        }
      }
    },
    "version": 17,
    "timestamp": 123456789
  }
}
```

## Shadow example

### Duplicated state change

아래와 같이 BLUE에서 RED, GREEN으로 state를 변경하려는 요청이 순차적으로 내려갈 경우에 device에서는 마지막 state를 version으로 받아 들여서 사용할 수 있습니다. 

#### Initial state

```java
{
  "state": {
    "reported": {
      "color": "blue"
    }
  },
  "version": 9,
  "timestamp": 123456776
}
````
#### Update1

```java
{
  "state": {
    "desired": {
      "color": "RED"
    }
  },
  "version": 10,
  "timestamp": 123456777
}
````

#### Update2

```java
{
  "state": {
    "desired": {
      "color": "GREEN"
    }
  },
  "version": 11,
  "timestamp": 123456778
}
```

Device가 offline에서 online으로 변경되었을때 Update2가 먼저 도달하고, 이후에 Update1이 도달하면, version을 보고 이전 값인 경우에 discard 할 수 있습니다. 




## Reference 

[AWS IoT Device Shadow service](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html)

[How to Get Started with Device Shadows for AWS IoT Core](https://www.youtube.com/watch?v=XsKGRA5FhiE)

[AWS Tutorials - Understanding Device Shadow Service in AWS IoT Core](https://www.youtube.com/watch?v=mPcvcsSL664)
