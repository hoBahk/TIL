# Clean Architecture

![](image/clean_architecture.png)

- 소스 코드의 의존성은 안쪽을 향하게끔 설계
	- 안쪽으로 갈수록 잘 변하지 않는 요소들이기 때문
- 안쪽의 원은 바깥쪽의 원을 모르는 상태
- 바깥쪽의 원은 어떠한 것도 안쪽의 원에 영향을 주지 않는 구조
- 아키텍처 설계의 가정: 사용자(Actor)의 요구사항은 변경이 많이 없고 내부적으로 Web이나 DB, UI가 자주 바뀐다고 가정하여 설계


## Entity
- DataSource에서 사용되는 데이터를 저장한 모델
- RestAPI의 Request/Response를 위한 JSON을 위한 Codable 모델
- 로컬 DB에 저장하기 위한 struct 모델


## Usecase
- 비즈니스 로직
- 비즈니스 로직 판단 기준: 개발팀 외부 사업 부서의 사람도 알고 있어야 하는 로직인가!
- 비즈니스로직은 앱에서 사용자와의 상호작용이 아닌 업무 요구사항을 담고 있다.

## Presenter
- Entity 데이터를 그대로 표현하는데 필요한 계층
- ex) Coordinator, View, ViewModel, Behaviors(특정 View의 event에 관해 적용되는 UI)

## External Interfaces

- 안쪽의 원과 통신할 연결 코드 이외에는 별다른 코드를 작성하지 않는 계층 
- 네트워크, DB 같은 것을 의미 
	- Network(URLSession)
	- DB(CoreData, UserDefaults)
