# 아키텍처

## TCA
Point-free에서 제안한 아키텍처로 Store의 state 변화에 따라 View를 업데이트 해주는 상태 기반의 단방향 아키텍처

참고  
- [TCA - Github](https://github.com/pointfreeco/swift-composable-architecture)  
- [blog](https://0urtrees.tistory.com/326)

### Composable Architecture
1) 상태관리  (State management)
단순 값 타입을 사용하여 개발할 어플리케이션의 상태를 관리하거나 많은 화면간의 상태를 공유하는 방법을 제공
이는 특정 화면의 변화를 다른 화면에서 즉시 관찰할 수 있도록 도와줍니다.

2) 구성 (Composition)
큰 기능들을 작은 컴포넌트로 분해할 수 있도록 해준다.
분해된 작은 컴포넌트들은 격리된 모듈로 추출될 수 있으며, 기느으이 형태를 구성하기 위해 쉽게 붙을 수 있다.

3) 사이드 이펙트 (Side Effect)
어플리케이션의 특정 부분이 최대한 테스트 가능하고, 이해가능한 방식으로 외부 세계와 소통할 수 있도록 도와준다.

4) 테스팅(Testing)
아키텍처 상에 구축된 기능들을 테스트할 뿐 아니라, 많은 파트로 구성되어진 기능들의 통합 테스트틀 작성할 수 있다. 또한 종단테스트(end - to - end tests)를 통해 사이드 이펙트가 어플리케이션에 어떤 영향을 주는지 테스트 할 수 있다. 이를 통해 개발자가 예상한 방식대로 비즈니스 로직이 돌아가고 있는지를 강력하게 보장할 수 있도록 해준다.

5) 인체공학(Ergonomics)
가능한 적은 수의 개념과 동작 파트만을 가진 단순한 API(Application Programming Interface)만으로 모든것을 수행할 수 있습니다.

### Flow

각 View는 저마다 Store를 갖고 있다. State에는 View에 대한 비즈니스로직 및 설명에 필요한 데이터가 정의되고, Action에는 이벤트가 enum 타입으로 정의 된다. Store는 사용자가 특정 Action을 보낼 때마다 내부의 reducer에서 state 값을 변경하고 다음 실행할 이벤트를 Effect를 반환할 수 있다. 그리고 변경된 State에 맞게 View를 업데이트 할 수 있다.

Reducer를 결합 또는 분해할 수 있다.
pullback을 통해 다양한 기능 화면의 reducer들이 local에서 global하게 사용될 수 있도록 변경하고, combine으로 하나의 reducer로 결합할 수 있다.
그렇게 되면 상위 Reducer에서 하위 Reducer를 관리 할 수 있다.


```swift 
//  Reducer를 가변 매개변수로 받는다.
  public static func combine(_ reducers: Self...) -> Self {
    .combine(reducers)
  }

```

### Elm이나 Redux와 다른점
TCA는 The Elm Architecture(TEA)dhk Redux가 대중화한 아이디에어 기반하고 있지만 애플의 플랫폼에서 Swiift 언어에 맞게 만들어졌다.   

TCA가 좀 더 고집있는 부분.  
Redux는 사이드 이펙트를 발생하는 것에 대한 규칙이 없는 반면, TCA는 모든 사이드 이펙트를 effect 타입으로 모델링하고 리듀서가 반환해야하는 것이 필수이다.

TCA가 다른 라이브러리가 신경쓰지 않는 부분에 높은 우선 순위를 주기도 한다.   
거대한 기능을 작은 단위로 쪼개고 다시 결합할 수 있게 만들어주는 합성(Composition)은 TCA에서 아주 중요한 측면 중 하나이다.   
합성은 리듀서의 `pullback`과 `combine`연산자 덕분에 완성할 수 있었고, 결과적으로 복잡한 기능을 모듈화해서 더 독립된 코드로 만들고 더나은 컴파일 시간을 제공할 수 있게 되었다.


### Store가 thread-safe 한 이유
action이 `Store`로 보내지면 리듀서는 지금 상태에서 실행되고, 이 작업 자체는 여러 스레드에서 실행될 수 없다. `send`구현부에서 큐를 사용하는 방법도 있겠지만, 이는 새로운 문제를 만든다.   
- 간편하게 `DispatchQueue.main.async`를 사용한다면 메인스레드에서 스레드를 뛰어넘으려는 일이 일어날 것입니다. 떄로는 애니메이션 블락처럼 동기적으로 일어나야 하는 작업이 있을텐데, 이럴 경우 UIKit과 SwiftUI의 예상치 못한 문제가 발생할 것이다
-  `DispatchQueue.main.async`를 사용하고 쌓인 작업을 바로 실행하는 스케줄러를 만들 수도 있을것입니다. (ex: ReactiveSwift의 UIScheduler) 이는 오히려 상황을 더 복잡하게 만들기 때문에 엄청 괜찮은 이유가 없다면 채택되지 않을것이다.

그래서 TCA는 메인스레드에서 Action을 보내는 것은 사용자에게 맡긴다. 만약 출력을 메인스레드가 아닌곳으로 전달하는 이펙트를 사용한다면 `.recieve(on: )`을 이용하여 메인스레드로 넘기도록 만들어야 한다.

이 접근법은 이펙트가 생성되고 변환된느 방법에 대한 가설의 수를 최소화 해줬으면, 불필요한 스레드 뛰어넘기 같은 문제를 막을 수 있다. 이펙트에 스케줄링에 대한 책임이 없다면 이펙트에 대한 테스트가 동기적으로 진행될 것이다. 그러면 실행되고 있는 이펙트가 어떻게 다음으로 넘어가는지 혹은 어플리케이션의 상태에 어떻게 영향을 끼치는지에 대한 상황을 전혀 알 수 없을 것이다. 하지만 원한다면 `Store`에서 이펙트의 이러한 측면을 체스트하거나 무시할 수 있는 유연성은 남겨두었다.

아래와 같이 모든 이펙트에 대해서 결과를 메인 스레드에 전달하게 만드는 고계 리듀서를 만들어서 주입하면 스레드에 대한 책임을 걱정하지 않아도 된다.

```swift
extension Reducer {
  func receive<S: Scheduler>(on scheduler: S) -> Self {
    Self { state, action, environment in
      self(&state, action, environment)
        .receive(on: scheduler)
        .eraseToEffect()
    }
  }
}
```

그래도 여전히 불필요한 스레드 건너뛰기가 생기지 않도록 해주는 UIScheduler는 니즈가 있을것이다.


### Async 사용하기
참고 - [pointfree](https://www.pointfree.co/episodes/ep195-async-composable-architecture-the-problem)

Async를 사용한 통신의 경우

Client를 아래와 같이 작성하고

```swift
struct FactClient {
  var fetch: (Int) -> Effect<String, Failure>
  var fetchAsync: @Sendable (Int) async throws -> String

  struct Failure: Error, Equatable {}
  
  static let live = Self(
  fetch: { number in
    …
  },
  fetchAsync: { number in
    try await Task.sleep(nanoseconds: NSEC_PER_SEC)
    let (data, _) = try await URLSession.shared
      .data(from: URL(string: "http://numbersapi.com/\(number)/trivia")!)
    return String(decoding: data, as: UTF8.self)
  }
)
}
```

테스트용

```swift
extension FactClient {
  static let unimplemented = Self(
    fetch: { _ in … },
    fetchAsync: XCTUnimplemented("\(Self.self).fetchAsync")
  )
}
```



Reducer에서 이렇게 사용한다.

```swift
return .task { [count = state.count] in
  do {
    return .numberFactResponse(
      .success(try await environment.fact.fetchAsync(environment.random(0...count)))
    )
  } catch {
    return .numberFactResponse(.failure(FactClient.Failure()))
  }
}
```

async let을 사용하여 여러개의 비동기 로직을 처리할 수 있다.

```swift
return .task { [count = state.count] in
  do {
    async let fact1 = environment.fact.fetchAsync(count)
    async let fact2 = environment.fact.fetchAsync(count)
    return try await .numberFactResponse(
      .success(fact1 + "\n" + fact2 + "!!!")
    )
  } catch {
    return .numberFactResponse(.failure(FactClient.Failure()))
  }
}
```

TaskGroup도 사용 가능하다.

```swift
return Effect.task { [count = state.count] in
  do {
    let facts = try await withThrowingTaskGroup(of: String.self, returning: String.self) { group in
      for _ in 1...count {
        group.addTask {
          try await environment.fact.fetchAsync(count)
        }
      }
      return try await group
        .reduce(into: []) { $0.append("• " + $1) }
        .joined(separator: "\n")
    }
    return .numberFactResponse(.success(facts))
  } catch {
    return .numberFactResponse(.failure(FactClient.Failure()))
  }
}
```
