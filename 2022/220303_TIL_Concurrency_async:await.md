## Concurrency ( async / await )

iOS15, Swift 5.5에 새로 나왔다.
비동기함수(async와 await)를 사용하면 비동기 코드를 장풍코드로 만들지 않고 보기 좋도록 만들 수 있다.

기존의 비동기 작업들은 문제들이 있었다.
### 기존 비동기 작업의 문제점
1. 비동기 작업에는 중첩 클로저가 상당하다.
2. 오류처리가 복잡하고 번거롭다.   
	- Swift 5.0에서 Result타입이 나오면서 에러처리가 비교적 쉬워 졌다.
3. 비동기 호출간의 제어 흐름이 복잡한 경우에 복잡해진다.
4. completionBlock을 호출하지 않고 그냥 return을 하는 실수를 범할 수 있다. 그렇게 되면 디버깅하기 어려워진다.

### async와 await 장점
그래서 async와 await가 나왔다.
async와 await를 사용하면 위 기존의 문제점을 해결할 수 있다.
비동기코드를 동기코드처럼 사용할 수 있다. (시각적으로)
또, 비동기 코드의 성능을 향상 시킨다.


### 예제 코드

```swift
func processImageData2c(completionBlock: (Result<Image, Error>) -> Void) {
    loadWebResource("dataprofile.txt") { dataResourceResult in
        switch dataResourceResult {
        case .success(let dataResource):
            loadWebResource("imagedata.dat") { imageResourceResult in
                switch imageResourceResult {
                case .success(let imageResource):
                    decodeImage(dataResource, imageResource) { imageTmpResult in
                        switch imageTmpResult {
                        case .success(let imageTmp):
                            dewarpAndCleanupImage(imageTmp) { imageResult in
                                completionBlock(imageResult)
                            }
                        case .failure(let error):
                            completionBlock(.failure(error))
                        }
                    }
                case .failure(let error):
                    completionBlock(.failure(error))
                }
            }
        case .failure(let error):
            completionBlock(.failure(error))
        }
    }
}

processImageData2c { result in
    switch result {
    case .success(let image):
        display(image)
    case .failure(let error):
        display("No image today", error)
    }
}
```

위의 코드를 아래의 코드로 아주 짧고 간단하게 해줄 수 있다.

```swift
// 아래 3개의 메서드는 따로  커스텀을 해주긴 해야한다.
func loadWebResource(_ path: String) async throws -> Resource
func decodeImage(_ r1: Resource, _ r2: Resource) async throws -> Image
func dewarpAndCleanupImage(_ i : Image) async throws -> Image

// async로 선언된 메서드만 await을 사용하여 흐름을 suspend할 수 있다.
func processImageData() async throws -> Image {
  let dataResource  = try await loadWebResource("dataprofile.txt")
  let imageResource = try await loadWebResource("imagedata.dat")
  let imageTmp      = try await decodeImage(dataResource, imageResource)
  let imageResult   = try await dewarpAndCleanupImage(imageTmp)
  return imageResult
}
```