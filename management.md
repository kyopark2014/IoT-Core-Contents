# 디바이스 속성 관리 체계

## Repository

### Thing

- ThingType이 없을 경우 Max 3개까지 Attribute 지정 가능 (Indexing 대상)
- ThingType을 지정할 경우, 추가 Attribute 지정 가능. 단 ThingType에서 사전에 정의한 Attributes은 기본적으로 추가됨 (Indexing 대상)

### Thing Type

Three attributes

- Indexing 대상이 됨
- Attributes 명칭만 정해지고, 실제 값은 Thing 등록 시점에 정해짐

### Thing Group

- Group Attributes들이 있으며, 초기에 설정된 값임
- Thing 단위로 관리되는 값이 아닌, Group단위로 관리되는 값
- Thing Attributes로 상속되지 않음

## Shadow

- Thing의 현재 상태를 표시하는 JSON Document
- Shadow 변경 값에 대한 다양한 추가 정보를 제공하여 상태 추적 및 변경 관리를 용이하게 해줌 
