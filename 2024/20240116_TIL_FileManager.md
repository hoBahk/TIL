
# FileManager

5개 정도의 뷰에서 사용하는 이미지를 뷰 옮겨 다닐 때마다 가지도 다니기 번거로우니 공용으로 사용할 수 있도록 하기 위해서 여러가지 방법을 생각해봤다.   
싱글톤... 유저디폴트... 등 생각해봤는데  이미지가 10개가 넘을 수도 있고 용량이 많을수도 있어 메모리로 싱글톤을 사용하여 옮겨 다니는건 조금 부담스러워서 파일매니저를 사용하였다.
이게 최선인지는 모르겠다.   
근데 다른 좋은 대안도 크게 없어서 일단 전에 쓰던대로 가긴 하는데 잘 모르겠다. 


## 예제 코드

```swift 
struct FileHelper {
    let documentURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first
    
    func save(data: Data, path: String? = nil, fileName: String) throws {
        var dictionaryPath = documentURL
        if let path = path {
            dictionaryPath = dictionaryPath?.appendingPathComponent(path)
        }
        guard let path = dictionaryPath?.appendingPathComponent(fileName) else {
            return
        }

        try FileManager.default.createDirectory(
            at: dictionaryPath!,
            withIntermediateDirectories: true,
            attributes: nil
        )
        try data.write(to: path, options: .atomic)
    }
    
    func read(path: String?, fileName: String) -> Data? {
        var dictionaryPath = documentURL
        if let path = path {
            dictionaryPath = dictionaryPath?.appendingPathComponent(path)
        }
        
        guard let finalPath = dictionaryPath?.appendingPathComponent(fileName),
              let data = try? Data(contentsOf: finalPath) else {
            return nil
        }
        
        return data
    }
    
    func delete(path: String?, fileName: String?) throws {
        var dictionaryPath = documentURL
        if let path = path {
            dictionaryPath = dictionaryPath?.appendingPathComponent(path)
        }
        if let fileName = fileName {
            dictionaryPath = dictionaryPath?.appendingPathComponent(fileName)
        }
        
        guard let path = dictionaryPath else {
            return
        }
        
        do {
            try FileManager.default.removeItem(at: path)
        } catch {
            print(error)
        }
    }
}

```