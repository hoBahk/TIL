# TCACoordinator

## 사용법


1. 스크린 리듀서 생성

```swift
// 홈 뷰 (HomeView)

import SwiftUI
import ComposableArchitecture

public struct HomeView: View {
  var store: Store<HomeState, HomeAction>
  
  public var body: some View {
    WithViewStore(store) { viewStore in
      List {
        ForEach(viewStore.nums, id: \.self) { num in
          Button(
            action: {
              ViewStore(store).send(.itemTapped(num))
            },
            label: { Text("\(num)") }
          )
        }
      }
    }
  }
}

// 홈 코어 (HomeCore)

import ComposableArchitecture

public struct HomeState: Equatable {
  public var nums: [Int]
  
  public init(nums: [Int] = Array(1...10)) {
    self.nums = nums
  }
}

public enum HomeAction {
  case itemTapped(Int)
}

public struct HomeEnvironment {
}

public let homeReducer = Reducer<HomeState, HomeAction, HomeEnvironment> { state, action, env in
  switch action {
  case let .itemTapped(num):
    return .none
  }
}

// 디테일 뷰 (DetailView)

import SwiftUI
import ComposableArchitecture

public struct DetailView: View {
  var store: Store<DetailState, DetailAction>
  
  public var body: some View {
    WithViewStore(store) { viewStore in
      Text("\(viewStore.title)")
      Button(
        action: { ViewStore(store).send(.backButtonTapped) },
        label: { Text("Go back to list view") }
      )
    }
    .navigationBarHidden(true)
  }
}

// 디테일 코어 (DetailCore)

import ComposableArchitecture

public struct DetailState: Equatable {
  public var title: Int
  
  public init(title: Int) {
    self.title = title
  }
}

public enum DetailAction {
  case backButtonTapped
}

public struct DetailEnvironment {
}

public let detailReducer = Reducer<DetailState, DetailAction, DetailEnvironment> { state, action, env in
  switch action {
  case .backButtonTapped:
    return .none
  }
}

// ScreenCore

import ComposableArchitecture

public enum ScreenState: Equatable {
  case home(HomeState)
  case detail(DetailState)
}

public enum ScreenAction {
  case home(HomeAction)
  case detail(DetailAction)
}

public struct ScreenEnvironment {
}

public let screenReducer = Reducer<ScreenState, ScreenAction, ScreenEnvironment>.combine([
  homeReducer.pullback(
    state: /ScreenState.home,
    action: /ScreenAction.home,
    environment: { _ in HomeEnvironment() }
  ),
  detailReducer.pullback(
    state: /ScreenState.detail,
    action: /ScreenAction.detail,
    environment: { _ in DetailEnvironment() }
  )
])
```


2. 코디네이터 리듀서 생성

```swift 
// CoordinatorCore

import ComposableArchitecture
import TCACoordinators

public struct CoordinatorState: Equatable, IndexedRouterState {
  public var routes: [Route<ScreenState>]
  
  public init(routes: [Route<ScreenState>] = [.root(.home(.init()), embedInNavigationView: true)]) {
    self.routes = routes
  }
}

public enum CoordinatorAction: IndexedRouterAction {
  case routeAction(Int, action: ScreenAction)
  case updateRoutes([Route<ScreenState>])
}

public struct CoordinatorEnvironment {
}

public let coordinatorReducer: Reducer<CoordinatorState, CoordinatorAction, CoordinatorEnvironment> = screenReducer
  .forEachIndexedRoute(environment: { _ in ScreenEnvironment() })
  .withRouteReducer(
    Reducer { state, action, environment in
      switch action {
      case let .routeAction(_, action: .home(.itemTapped(num))):
        state.routes.push(.detail(.init(title: num)))
        return .none
        
      case .routeAction(_, action: .detail(.backButtonTapped)):
        state.routes.pop()
        return .none
        
      default:
        return .none
      }
    }
  )
```


3. 코디네이터뷰 생성

```swift
// CoordinatorView

import ComposableArchitecture
import SwiftUI
import TCACoordinators

public struct CoordinatorView: View {
  let store: Store<CoordinatorState, CoordinatorAction>
  
  public var body: some View {
    TCARouter(store) { screen in
      SwitchStore(screen) {
        CaseLet(
          state: /ScreenState.home,
          action: ScreenAction.home,
          then: HomeView.init
        )
        CaseLet(
          state: /ScreenState.detail,
          action: ScreenAction.detail,
          then: DetailView.init
        )
      }
    }
  }
}
```


## 화면 전환 메서드


| Method | Effect |
| --- | --- |
| push | Pushes a new screen onto the stack. |
| presentSheet | Presents a new screen as a sheet. |
| presentCover | Presents a new screen as a full-screen cover. |
| goBack | Goes back one screen in the stack. |
| goBackToRoot | Goes back to the very first screen in the stack. |
| goBackTo | Goes back to a specific screen in the stack. |
| pop | Pops the current screen if it was pushed. |
| dismiss | Dismisses the most recently presented screen. |