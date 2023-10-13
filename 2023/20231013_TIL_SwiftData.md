# SwiftData

## 특징
- iOS17 +
- Swift macro가 제동하는 표현식 사용
- @model 키워드로 사용
- 선언형 코드 사용
- 기본적인 value type 프로퍼티를 기본적으로 포함
	- 기본 타입(String, Int, Double)
	- Collection 타입(Array, Set, Dictionary)
	- 기타(Struct, Codable)
- SwiftData 모델은 타입을 관계로 참조함
	- 모델 유형 간에 link를 만들 수 있음


## metadata
- @Model: SwiftData 모델임을 선언
- @Attribute: 고유성 제약 조건을 추가 가능
- @Relationship을 사용하여 모델간의 링크 동작을 변경 할 수 있음

ex) @Attribute(.unique)를 통해 unique로 지정,   
 @Relationship(.cascade)를 통해 변수가 삭제될 때마다 SwiftData에 관련 항목을 모두 삭제하도록 지시

```swift
@Model
class Trip {
    @Attribute(.unique) var name: String // Unique 지정
    var destination: String
    var endDate: Date
    var startDate: Date
 
    @Relationship(.cascade) var bucketList: [BucketListItem]? = [] // Trip 객체 삭제시 같이 삭제되도록 설정
    var livingAccommodation: LivingAccommodation?
}
```

## ModelContainer

- model 타입에 관한 영구적인 backend를 제공 (스키마를 지정하기만 하면, config와 옵션들을 커스텀마이징할수 있음) 
- 지정하려는 모델 유형 목록을 지정하기만 하면 모델 컨테이너 생성이 가능
- 컨테이너 설정이 되면 ModelContext로 데이터를 가져오고 저장할 준비 완료

ex) 

```swift 
// Initialize with only a schema
let container = try ModelContainer([Trip.self, LivingAccommodation.self])

// Initialize with configurations
let container = try ModelContainer(
    for: [Trip.self, LivingAccommodation.self],
    configurations: ModelConfiguration(url: URL("path"))
)
```

ex) SwiftUI

```swift
import SwiftUI

@main
struct TripsApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .modelContainer(
            for: [Trip.self, LivingAccommodation.self]
        )
    }
}
```

## Fetching your Data

### predicate
쿼리 작성하는 방법

```swift 
let today = Date()
let tripPredicate = #Predicate<Trip> { 
    $0.destination == "New York" &&
    $0.name.contains("birthday") &&
    $0.startDate > today
}
```

### FetchDescriptor
필터를 사용한 Fetch

```swift
let descriptor = FetchDescriptor<Trip>(predicate: tripPredicate)

let trips = try context.fetch(descriptor)
```

정렬 사용

```swift
let descriptor = FetchDescriptor<Trip>(
    sortBy: SortDescriptor(\Trip.name),
    predicate: tripPredicate
)

let trips = try context.fetch(descriptor)
```


## Using SwiftData with SwiftUI
- SwiftUI에서 사용하기 쉽게 되어 있다.
- view modifier `modelContainer`를 사용해서 쉽게 저장소를 구성한다.
- environment로 등록하여 사용

ex)

```swift
import SwiftUI

struct ContentView: View  {
    @Query(sort: \.startDate, order: .reverse) var trips: [Trip]
    @Environment(\.modelContext) var modelContext
    
    var body: some View {
       NavigationStack() {
          List {
             ForEach(trips) { trip in 
                 // ...
             }
          }
       }
    }
}
```

## 느낀점
SwiftUI와 같이 사용했을 때 아주 간단하게 저장소를 사용할 수 있어서 간편하다.
애플에서 SwiftUI를 계속해서 밀어주는 것으로 보인다.
코어데이터보다 효율적인 lite 버전인 것 같다.
코어데이터를 SwiftData로 변경할 수 있다고 하니 시간이 난다면 변경하는 작업도 해봐야겠다.