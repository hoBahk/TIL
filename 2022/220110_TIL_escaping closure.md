## escaping closure

### 설명
- 함수의 파라미터로  전달된 일반적인 클로저는 해당 함수 내부에서만 사용이가능하다.
- escaping cloure는 해당 클로저를 외부 변수/상수에 저장이 가능하다. 
- 해당 함수가 실행이 끝나도 클로저 실행이 가능하다.
- escaping cloure는 해당 함수가 실행된 후에 클로저가 실행되는 것을 보장할 수 있다.
- 주로 통신할 때 많이 쓰일 수 있다. 통신이 완료 되고 response가 오고 나서 실행할 수 있도록 보장해준다.

### 예시

```swift
let dataTask = urlSession.dataTask(request) { response in
        switch response {
        case .success(let data):
          let result = JSONParser.shared.decode(data: data, type: ProductList.self)
         switch result {
          case .success(let data):
            complition(.success(data))
          case .failure(let error):
            print(error)
          }
        case .failure(_):
          complition(.failure(.responseFailed))
        }
      }
      dataTask.resume()
```


# 프로젝트 하면서 배운 것들
## Result 타입
- result타입은 <success, failure>를 사용한다.
- 성공하면 첫번째에 있는 값을 반환하고 실패하면 두번째에 있는 값을 반환한다.
- 주로 성공하면 얻은 값을 반환하고, 실패하면 에러를 반환할 때 사용한다.
```swift 
func decode<Element: Codable>(data: Data, type: Element.Type) -> Result<Element, JSONParserError> {
    guard let decodedData = try? decoder.decode(Element.self, from: data) else {
      return .failure(.decodeFailed)
    }
    
    return .success(decodedData)
  }

let result = JSONParser.shared.decode(data: data, type: Product.self)
          switch result {
          case .success(let data):
            complition(.success(data))
          case .failure(let error):
            print(error)
          }
```