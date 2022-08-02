# Authentification

## Server authentification

인증방식에는 X.509 인증서, SigV4, Cognito, Custom Authentification(Lambda이용) 하는 방법이 있습니다. 


![image](https://user-images.githubusercontent.com/52392004/182384968-47b660d2-43f6-4f2b-b8bb-52d789a8fc24.png)

- When your device or other client attempts to connect to AWS IoT Core, the AWS IoT Core server will send an X.509 certificate that your device uses to authenticate the server.

- When your devices or other clients establish a TLS connection to an AWS IoT Core endpoint, AWS IoT Core presents a certificate chain that the devices use to verify that they're communicating with AWS IoT Core and not another server impersonating AWS IoT Core. The chain that is presented depends on a combination of the type of endpoint the device is connecting to and the cipher suite that the client and AWS IoT Core negotiated during the TLS handshake.

### 상호 TLS 인증 

- 인증서를 이용한 서버와 클라이언트간의 양방향 인증 진행합니다.

- 디바이스에 x.509 인증서와 Private Key를 보관합니다.

- 디바이스 인증서는 디바이스의 정보인 Thing과 IoT Policy에 연결하여 어떠한 디바이스를 인증할지와 어떤 권한을 가질지를 결정합니다.


### ATS(Amazon Trust Service) 인증서 다운로드 

Amazon Trust Services Endpoints (preferred)


RSA 2048 bit key: [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem).

RSA 4096 bit key: Amazon Root CA 2. Reserved for future use.

ECC 256 bit key: Amazon Root CA 3.

ECC 384 bit key: Amazon Root CA 4. Reserved for future use.

These certificates are all cross-signed by the Starfield Root CA Certificate. All new AWS IoT Core regions, beginning with the May 9, 2018 launch of AWS IoT Core in the Asia Pacific (Mumbai) Region, serve only ATS certificates.

IoT device의 인증을 위하여 상기의 [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem)를 선택한후, 저장하기를 선택하여 "AmazonRootCA1.cer"로 적당한 폴더에 다운로드 합니다. 

아래와 같이 curl 명령어로도 다운로드 가능합니다. 

```c
$ curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > AmazonRootCA1.cer
```

## Reference

[Server authentification](https://docs.aws.amazon.com/iot/latest/developerguide/server-authentication.html)
