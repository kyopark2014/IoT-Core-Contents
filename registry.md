# Registry

AWS 서비스에서 쉽게 사용할 수 있도록 디바이스 정의 및 카탈로그화 합니다. 

## 디바이스 Onboarding

Device가 onboard되면 아래 순서로 적용합니다.

1) 디바이스를 등록(Thing)합니다.

2) Device 인증서, private key를 생성합니다. 

3) 디바이스에 인증서를 적용합니다. 

4) 디바이스에 IoT Policy를 적용합니다. (Thing Group으로 한꺼번에 적용 할 수도 있습니다.) 이때, Provisioning template이 필요합니다. 


## Provisining Template


#### Parameters section

Resource에 대한 parameter를 아래와 같이 정의 할 수 있습니다.

```java
{
    "Parameters" : {
        "ThingName" : {
            "Type" : "String"
        },
        "SerialNumber" : {
            "Type" : "String"
        },
        "Location" : {
            "Type" : "String",
            "Default" : "WA"
        },
        "CSR" : {
            "Type" : "String"    
        }
    }
}
```

Resource example

```java
{ 
    "Resources" : {
        "thing" : {
            "Type" : "AWS::IoT::Thing",
            "Properties" : {
                "ThingName" : {"Ref" : "ThingName"},
                "AttributePayload" : { "version" : "v1", "serialNumber" :  {"Ref" : "SerialNumber"}}, 
                "ThingTypeName" :  "lightBulb-versionA",
                "ThingGroups" : ["v1-lightbulbs", {"Ref" : "Location"}]
            },
            "OverrideSettings" : {
                "AttributePayload" : "MERGE",
                "ThingTypeName" : "REPLACE",
                "ThingGroups" : "DO_NOTHING"
            }
        },  
        "certificate" : {
            "Type" : "AWS::IoT::Certificate",
            "Properties" : {
                "CertificateSigningRequest": {"Ref" : "CSR"},
                "Status" : "ACTIVE"      
            }
        },
        "policy" : {
            "Type" : "AWS::IoT::Policy",
            "Properties" : {
                "PolicyDocument" : "{ \"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\":[\"iot:Publish\"], \"Resource\": [\"arn:aws:iot:us-east-1:123456789012:topic/foo/bar\"] }] }"
            }
        }
    }
}
```


### Just-in-time provisioning




### Fleet Provisioning

![image](https://user-images.githubusercontent.com/52392004/182393672-7d794600-2f74-4b40-92b4-d3a91b5a165b.png)




## Multi-account registration

![image](https://user-images.githubusercontent.com/52392004/182389844-cff6751e-897e-4df5-8626-cfce345b0390.png)




