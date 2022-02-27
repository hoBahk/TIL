## Assets, JSON

### Assets

Assets에 있는 data파일 사용하는 법
```swift
 guard let dataAsset = NSDataAsset(name: jsonName) else {
      return nil
}
  ```
위와 같이 `NSDATAAsset`을 사용하여 매개변수에 json파일을 입력하여 사용한다.    
옵셔널이 반환된다.


### JSON decoder

json파일을 디코드 하는 방법

```swift
let decodedData = try? JSONDecoder().decode(Element.self, from: dataAsset.data)
```

`JSONDecoder`의 `decode`메서드를 사용하고 매개변수 첫번째에는 json 구성에 맞게 작성해놓은 타입의 `self`를 적는다. (왜 `self`를 하는지는 아직 찾아보지 못함..) 그리고 `from`매개변수에 json 파일을 적어주어 디코드 해준다.    
에러를 던지는 메서드이므로 `try`를 해주어야한다.

