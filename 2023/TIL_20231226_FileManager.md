# FileManager

Foundation 프레임워크에서 제공하는 FileMnager 클래스를 통해서 파일을 관리할 수 있다.   
FileManager는 싱글톤을 사용하여 파일과 관련된 작업을 수행한다.   

## 사전 작업

1. 파일 인스턴스를 생성
2. 경로 설정(폴더 생성 가능)


## CRUD

Update는 Create를 사용하여 덮어쓴다.   

```swift 
struct FileHelper {
    let fileName: String = "customfoodlists.dat"
    let documentURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first

    
    func save<T: Codable>(data: T) {
        guard let path = documentURL?.appendingPathComponent(fileName) else {
            return
        }
        
        do {
            let jsonData = try JSONEncoder().encode(data)
            let jsonString = String(data: jsonData, encoding: .utf8)
            try jsonString?.write(to: path, atomically: true, encoding: .utf8)
        } catch {
            print(error)
        }
    }
    
    func read() -> Data? {
        guard let path = documentURL?.appendingPathComponent(fileName),
              let jsonString = try? String(contentsOf: path),
              let jsonData = jsonString.data(using: .utf8) else {
            return nil
        }
        return jsonData
    }
    
    func delete() {
        guard let path = documentURL?.appendingPathComponent(fileName) else {
            return
        }
        
        do {
            try FileManager.default.removeItem(at: path)
        } catch let e {
            print(e.localizedDescription)
        }
    }
}

```