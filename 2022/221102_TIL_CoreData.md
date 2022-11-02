# CoreData

- 코어데이터(Core Data)란 macOS 및 iOS 운영 체제에서 Apple이 제공하는 객체 그래프 및 지속성 프레임워크




## Core Data Stack
Core Data Stack은 Model, Context, Store coordinator, 그리고 Persistent container로 이루어져 있다.

- Model: Entity를 설명하는 Database 스키마이다. managed objects의 struct를 정의한다.
- Store coordinator: persistent storage(영구 저장소)와 managed object model을 연결한다.
- Context: managed objects를 생성하고, 저장하고, 가져오는 작업을 제공한다.
- Persistent container: Core Data Stack을 나타내는 모든 객체를 포함한다.


## CoreData 사용 과정
1. Persistent container를 만든다.
2. Context와 Entity를 가져온다.
3. object를 만들고 값을 세팅한다.
4. Context를 저장한다.

## 고민한 점
- 코어데이터를 사용하여 비교적 많은 데이터를 로컬에 저장할 수 있도록 했다.
- `UserDefaults`보다 많은 데이터를 다루기에 좋기 떄문에 사용했다.
- 코어데이터를 사용할 때 여러 엔티티를 호환하기 위해서 제네릭하게 사용할 수 있도록 처리를 해야한다.(내일)