# Realm 사용 (CRUD)

## 모델 설계
```swift
class RealmEntityTask: Object {
    enum ProgressStatus: String {
        case todo
        case doing
        case done
    }
    
    @objc dynamic var id: String = ""
    @objc dynamic var title: String = ""
    @objc dynamic var desc: String = ""
    @objc dynamic var deadline: TimeInterval = 0
    @objc dynamic var progressStatus = ProgressStatus.todo.rawValue
    var progressStatusEnum: ProgressStatus {
        get {
            return ProgressStatus(rawValue: progressStatus) ?? .todo
        }
        set {
            progressStatus = newValue.rawValue
        }
    }
}
 ```
 
### @objc dynamic 을 붙이는 이유
 Realm에 모델 속성에는 @objc dynamic를 붙여야 한다고 한다.
 왜냐하면 기본 데이터베이스 테이터에 대한 속성 접근자를 만들기 위해서이다.
 접근자를 만들어야 데이터베이스의 데이터에 접근이 가능하다.

## Create
```swift
do {
    let realm = try Realm()
    try realm.write {
        realm.add(task)
    }
} catch let error {
    print(error)
}
 ```

## Read
```swift
do {
    let realm = try Realm()
    let fetchDataList = realm.objects(RealmEntityTask.self)
    fetchDataList
        .forEach {
            realmEntityTaskList.append($0)
        }
} catch let error {
    print(error)
}
 ```

## Update
```swift
do {
    let realm = try Realm()
    let task = realm.objects(RealmEntityTask.self)
        .filter { $0.id == id }
    try realm.write {
        task.first?.title = title
        task.first?.desc = description
        task.first?.deadline = deadline.timeIntervalSince1970
    }
} catch let error {
    print(error)
}
 ```
 
## Delete
```swift
do {
    let realm = try Realm()
    let task = realm.objects(RealmEntityTask.self)
        .filter { $0.id == id }
    try realm.write {
        realm.delete(task.first ?? task[0])
    }
} catch let error {
    print(error)
}
 ```
