# UserDefault


UserDefault는 간단한 타입? 정보?를 로컬에 저장하고 싶을 때 사용한다.  
iOS앱이 설치되면 앱이 실행되는 시점에 데이터를 저장할 수 있는 기본 데이터베이스가 생성된다.

기본 데이터베이스는 Property List를 기반으로 .plist 확장자 파일에 xml 형식으로 저장되며 Sandbox 내부에 저장된다.  

샌드박스에 저장되기 때문에 앱이 종료 되어도 계속 저장된다.   
앱을 삭제할 때 같이 삭제 된다.  


## 예제코드

```swift

struct UserDefaultsManager {
    static var userDefaults: UserDefaults = .standard

    static func setLocalData<T: Codable>(_ value: T, forKey: LocalDataType) {
        if let encoded = try? JSONEncoder().encode(value) {
            userDefaults.set(encoded, forKey: forKey.rawValue)
        }
    }

    static func getLocalData<T>(forKey: LocalDataType) -> T? where T: Decodable {
        guard let data = userDefaults.value(forKey: forKey.rawValue) as? Data,
            let decodedData = try? JSONDecoder().decode(T.self, from: data)
            else { return nil }
        return decodedData
    }

    static func resetLocalData() {
        LocalDataType.allCases.forEach {
            userDefaults.removeObject(forKey: $0.rawValue)
        }
    }
}


enum LocalDataType: String, CaseIterable {
    case flashOption
}

```

키를 열거형으로 관리하면 깔끔하고 실수를 줄일 수 있지 않을까 생각하여 사용했다.

