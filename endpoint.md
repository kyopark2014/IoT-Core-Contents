# IoT Core Endpoint

## Endpoint 주소 확인하는 방법

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


## Endpoint types

AWS IoT Core supports two different data endpoint types, iot:Data and iot:Data-ATS. 

#### iot:Data endpoints
iot:Data endpoints present a certificate signed by the [VeriSign Class 3 Public Primary G5 root CA certificate](https://www.websecurity.digicert.com/content/dam/websitesecurity/digitalassets/desktop/pdfs/roots/VeriSign-Class%203-Public-Primary-Certification-Authority-G5.pem). 


#### iot:Data-ATS endpoints 확인 방법

iot:Data-ATS endpoints present a server certificate signed by an [Amazon Trust Services CA](https://www.amazontrust.com/repository/).

Certificates presented by ATS endpoints are cross signed by Starfield. Some TLS client implementations require validation of the root of trust and require that the Starfield CA certificates are installed in the client's trust stores.

Use iot:Data-ATS endpoints unless your device requires Symantec or Verisign CA certificates. Symantec and Verisign certificates have been deprecated and are no longer supported by most web browsers.
