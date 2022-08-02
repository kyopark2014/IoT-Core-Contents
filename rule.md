# Rule Engine

저렴한 비용으로 대량의 IoT 데이터를 수집하여 사전 처리하고 분석, 보고 및 시각화를 위해 20개 이상의 서비스에 제공합니다. 

![image](https://user-images.githubusercontent.com/52392004/182387069-5c3b1e69-be59-414b-98ec-9a851a534390.png)


### Transform

수학, 문자열 조작, 날짜 등을 위한 기능이 내장되어 있습니다.

### Filter

WHERE 절을 사용하여 원하는 데이터만 캡처합니다.

### Enrich

Device Shadow 및 Amazon Machine Learning에서 또는 인라인 AWS Lambda 실행 및 DynamoDB 룩업을 통해 외부 소스로부터 컨텍스트를 가져옵니다.

### Route

데이터를 20개 이상의 AWS 서비스와 Salesforce와 같은 타사 서비스에 전송합니다.


## Basic Ingest

Basic Ingest를 이용하여 메시지 비용을 절감할 수 있습니다.

- Basic Ingest를 사용하면 AWS IoT 규칙 작업에서 지원하는 AWS 서비스로 디바이스 데이터를 안전하게 전송할 수 있으며, 이때 메시징 요금이 발생하지 않습니다. 


- Basic Ingest 또는 AIA(Alexa Voice Service for AWS IoT Core)항목을 사용하여 송신하거나 수신하는 메시지에 대해서는 메시지 요금이 발생하지 않습니다. 

- Basic Ingest는 수집 경로에서 publish/subscribe를 위한 message broker를 제거해 데이터 흐름을 최적화하므로 비용을 줄 일 수 있습니다.

- Basic Ingest의 prefix는 $aws/rules/rule_name으로 $aws/rules/<rule-name>/<optional-customer-defined-segments> 형식으로 사용합니다. 여기서 rule-name은 trigger할 AWS IoT 규칙의 이름을 따릅니다. 
  

### AWS IoT SQL reference
  
```java
SELECT color AS rgb FROM 'topic/subtopic' WHERE temperature > 50
```
  
## Reference 

[Reducing messaging costs with Basic Ingest](https://github.com/awsdocs/aws-iot-docs/blob/master/developerguide/iot-basic-ingest.md)  
  
[AWS IoT SQL reference](https://github.com/awsdocs/aws-iot-docs/blob/master/developerguide/iot-sql-reference.md)
  

  
  

  


