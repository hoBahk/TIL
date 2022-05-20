# TCACoordinator

## 설명
- 내비게이션에 유연한 접근방식을 제공한다.
- 복잡한 내비게이션과 프레젠테이션 흐름 관리
- 화면이동 같은것을 쉽게 관리해준다?

### TCACoodinator 라이브러리를 사용하면 좋을 때: 

-  앱에서 깊이 중첩된 탐색 경로로 연결되는 딥링크 지원   
-  다양한 탐색 컨텍스트에서 화면 기능을 쉽게 재사용   
-  루트 화면 또는 탐색 스택의 특정 화면으로 쉽게 돌아갈 수 있음
-  모든 탐색 로직을 한 곳에 보관  
-  앱의 탐색을 여러 개의 재사용 가능한 코디네이터로 나누고 함께 구성

## 사용법
### 화면 reducer를 생성한다. 
- 이동할 수 있는 화면을 모두 case에 넣는다.

```swift
enum ScreenState: Equatable {
  case home(HomeState)
  case numbersList(NumbersListState)
  case numberDetail(NumberDetailState)
}

enum ScreenAction {
  case home(HomeAction)
  case numbersList(NumbersListAction)
  case numberDetail(NumberDetailAction)
}
```

- `combine` 연산자를 사용하여 reducer를 하나로 결합한다.

```swift
let screenReducer = Reducer<ScreenState, ScreenAction, Void>.combine(
  homeReducer
    .pullback(
      state: /ScreenState.home,
      action: /ScreenAction.home,
      environment: { _ in }
    ),
  numbersListReducer
    .pullback(
      state: /ScreenState.numbersList,
      action: /ScreenAction.numbersList,
      environment: { _ in }
    ),
  numberDetailReducer
    .pullback(
      state: /ScreenState.numberDetail,
      action: /ScreenAction.numberDetail,
      environment: { _ in }
    )
)
```
   
   
   
### coordinator reducer 생성
- coordinator는 내비게이션 흐름에서 여러 화면을 관리한다. 이 때 내비게이션 스택을 나타내는 Route<ScreenState>배열이 포함되어야 한다.
- 이 배열에 새 화면 상태를 추가하면 해당 화면이 푸시되거나 표시된다.
- Route는 케이스가 화면 상태를 캡처하고 화면 상태를 표시하는 방법을 포함하는 열거형이다.

```swift
struct CoordinatorState: Equatable, IndexedRouterState {
  var routes: [Route<ScreenState>]
}
```

- Coordinator의 액션은 두가지의 특별한 경우가 포함되어야한다.
- 첫째, Screen 액션이 routes 배열의 올바른 화면에 디스패치될 수 있도록하는 인덱스를 포함한다.
- 둘째, 예를 들어 사용자가 `뒤로`를 누를 때 경로 배열을 자동으로 업데이트할 수 있도록 한다.

```swift
enum CoordinatorAction: IndexedRouterAction {
  case routeAction(Int, action: ScreenAction)
  case updateRoutes([Route<ScreenState>])
}
```

- Coordinator의 reducer는 `forEachIndexedRoute`를 사용하여 routes 배열의 각 화면에 screen 리듀서를 적용하고 이를 새 screen을 푸시, 표시 또는 pop 해야하는 시기를 정의하는 두 번쨰 reducer와 결합한다.


```swift
let coordinatorReducer: Reducer<CoordinatorState, CoordinatorAction, Void> = screenReducer
  .forEachIndexedRoute(environment: { _ in })
  .withRouteReducer(
    Reducer { state, action, environment in
      switch action {
      case .routeAction(_, .home(.startTapped)):
        state.routes.presentSheet(.numbersList(.init()), embedInNavigationView: true)
        
      case .routeAction(_, .numbersList(.numberSelected(let number))):
        state.routes.push(.numberDetail(.init(number: number)))

      case .routeAction(_, .numberDetail(.goBackTapped)):
        state.routes.goBack()
        
      case .routeAction(_, .numberDetail(.goBackToNumbersList)):
        state.routes.goBackTo(/.numbersList)
        
      case .routeAction(_, .numberDetail(.goBackToRootTapped)):
        state.routes.goBackToRoot()

      default:
        break
      }
      return .none
    }
  )
```

### Coordinator View 생성

- TCARouter를 사용하여 routes 배열을 보이지 않는 NavigationLinks와 프레젠테이션 호출이 있는 화면 뷰의 중첩된 목록으로 변환하며, 모두 route 배열의 변경에 적절하게 반응하는 바인딩으로 구성된다.
- TCARouter는 내비게이션 플로우의 모든 화면에 대한 뷰를 만들 수 있는 클로저를 취한다.
- SwitchStore는 가능한 각 화면에 대해 CaseLet을 사용하여 이를 달성하는 일반적인 방법이다.

```swift
struct CoordinatorView: View {
  let store: Store<CoordinatorState, CoordinatorAction>

  var body: some View {
    TCARouter(store) { screen in
      SwitchStore(screen) {
        CaseLet(
          state: /ScreenState.home,
          action: ScreenAction.home,
          then: HomeView.init
        )
        CaseLet(
          state: /ScreenState.numbersList,
          action: ScreenAction.numbersList,
          then: NumbersListView.init
        )
        CaseLet(
          state: /ScreenState.numberDetail,
          action: ScreenAction.numberDetail,
          then: NumberDetailView.init
        )
      }
    }
  }
}
```


## route 배열 method
- push: 새로운 화면을 스택에 푸시한다.
- presentSheet: 새로운 화면을 present한다. (sheet형태)
- presentCover: 새로운 화면을 풀스크린으로 present한다.
- goBack: 한 화면 뒤로가기
- goBackToRoot: 스택의 첫번쨰 화면으로 간다.
- goBackTo: 특정 화면으로 간다.
- pop: 푸시된 현재 화면을 pop한다.
- dismiss: 최근에 present한 화면을 dismiss한다.

## Routes 배열 자동 업데이트
사용자가 뒤로가기 버튼을 누르거나, 엣지 스와이프 제스처 등을 사용하여 뒤로 이동하면 배열이 자동으로 업데이트 된다.

## Cancellation of in-flight effects on dismiss
- 기본적으로 특정 화면에서 시작된 모든 in-flight효과는 해당 화면이 팝업되거나 해제될 때 자동으로 취소된다.
- 이 로직은 보통 많은 보일러 플레이트 코드가 필요하지만 추가 작업 없이 해당 라이브러리로 처리할 수 있다.
- 이 동작을 재정의 하려면 `cancelEffectsOnDismiss: false`를 `withRouteReducer`로 전달해야한다.


## 복잡한 내비게이션 업데이트 수행
- SwiftUI는 한 번의 업데이트에서 두 개 이상의 화면을 푸시, 표시, 해제하는 것을 하지 못한다.
- 그래서 내비게이션 계층의 깊은 뷰에 직접 링크하거나, 여러 프레젠테이션 계층을 루트로 되돌리거나, 임의 탐색 상태를 복원할 때처럼 내비게이션 상태에 대한 대규모 업데이트를 수행하기가 까다롭다.
- 해당 라이브러리를 사용하면, 대규모 업데이트를 SwiftUI가 지원하는 작은 업데이트로 분해하여 필요한 지연을 발생시킬 수 있으며, 이를 coordinator reducer에서 반환할 Effect롤 사용할 수 있도록 할 수 있다.
- `Effect.routeWithDelaysIfUnsupported`호출에서 route mutations를 래핑하기만 하면 된다.

```swift
return Effect.routeWithDelaysIfUnsupported(state.routes) {
  $0.goBackToRoot()
}
```
```swift
return Effect.routeWithDelaysIfUnsupported(state.routes) {
  $0.push(...)
  $0.push(...)
  $0.presentSheet(...)
}
```

## 하위 Coordinator 구성
- Coordinator는 상태 및 액션 타입이 있는 뷰와 Reducer로 구성된 Composable Architecture의 다른 UI 장치와 동일하다.
- SwiftUI 및 TCA를 허용한다. 
- Route 배열에 Coordinator를 추가하여 Coordinator를 표시하거나, TabView에 추가할 수 있으며, 부모 Coordinator에서 하위 Coordinator를 푸시하거나 표시할 수 있다.
- 그렇게 할 때, 하위 Coordinator는 부모 route배열의 마지막 요소여야 한다. 왜냐하면 그것은 해제될 때까지 새로운 화면을 푸시하고 present하는 책임을 떠맡게 될 것이기 때문이다. 그렇지 않으면 하위 Coordinator가 이미 화면을 푸시하고 있을 때 부모 Coordinator가 화면을 푸시하려고 시도하여 충돌이 발생할 수 있다.


## 화면 식별
- 화면은 route 배열의 인덱스로 식별된다.
- 표준 내비게이션 업데이트에 대해 인덱스가 안정적이므로 안전하다.


## Flexible and resuable
- 화면흐름을 한 곳에서 쉽게 바꿀 수 있다.
- 화면 뷰 및 reducer는 더 이상 내비게이션 흐름의 다른 화면에 대한 정보가 필요하지 않다.
- 작업을 전송하고 coordinator가 새로운 뷰를 푸시할 것인지, present할 것인지를 결정할 수 있으므로 다른 context에서 쉽게 재사용할 수 있고 화면 책임과 내비게이션 책임을 구분하는데 도움이 된다.