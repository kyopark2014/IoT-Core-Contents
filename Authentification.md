# Authentification

## Server authentification

When your device or other client attempts to connect to AWS IoT Core, the AWS IoT Core server will send an X.509 certificate that your device uses to authenticate the server.

When your devices or other clients establish a TLS connection to an AWS IoT Core endpoint, AWS IoT Core presents a certificate chain that the devices use to verify that they're communicating with AWS IoT Core and not another server impersonating AWS IoT Core. The chain that is presented depends on a combination of the type of endpoint the device is connecting to and the cipher suite that the client and AWS IoT Core negotiated during the TLS handshake.


## Enpoint types

AWS IoT Core supports two different data endpoint types, iot:Data and iot:Data-ATS. 

### iot:Data endpoints
iot:Data endpoints present a certificate signed by the [VeriSign Class 3 Public Primary G5 root CA certificate](https://www.websecurity.digicert.com/content/dam/websitesecurity/digitalassets/desktop/pdfs/roots/VeriSign-Class%203-Public-Primary-Certification-Authority-G5.pem). 


### iot:Data-ATS endpoints 확인 방법

iot:Data-ATS endpoints present a server certificate signed by an [Amazon Trust Services CA](https://www.amazontrust.com/repository/).

Certificates presented by ATS endpoints are cross signed by Starfield. Some TLS client implementations require validation of the root of trust and require that the Starfield CA certificates are installed in the client's trust stores.

Use iot:Data-ATS endpoints unless your device requires Symantec or Verisign CA certificates. Symantec and Verisign certificates have been deprecated and are no longer supported by most web browsers.

아래 명령어 또는 Console에서 확인 가능합니다. 

```c
$ aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

상기 명령어로 확인된 Endpoint 정보입니다.
```java
{
    "endpointAddress": "a**********-ats.iot.ap-northeast-2.amazonaws.com"
}
```

AWS IoT Console에서도 확인이 가능합니다. 

https://ap-northeast-2.console.aws.amazon.com/iot/home?region=ap-northeast-2#/settings

![noname](https://user-images.githubusercontent.com/52392004/168472427-9773d6f2-c09f-4f6a-bcea-0c11478b976b.png)



### Amazon Trust Services Endpoints (preferred)


RSA 2048 bit key: [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem).

RSA 4096 bit key: Amazon Root CA 2. Reserved for future use.

ECC 256 bit key: Amazon Root CA 3.

ECC 384 bit key: Amazon Root CA 4. Reserved for future use.

These certificates are all cross-signed by the Starfield Root CA Certificate. All new AWS IoT Core regions, beginning with the May 9, 2018 launch of AWS IoT Core in the Asia Pacific (Mumbai) Region, serve only ATS certificates.

IoT device의 인증을 위하여 상기의 [Amazon Root CA 1](https://www.amazontrust.com/repository/AmazonRootCA1.pem)를 선택한후, 저장하기를 선택하여 "AmazonRootCA1.cer"로 적당한 폴더에 다운로드 합니다. 


## Reference

[Server authentification](https://docs.aws.amazon.com/iot/latest/developerguide/server-authentication.html)
