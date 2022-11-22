# TCA Flow

### Store

각 View는 저마다 Store를 갖고 있습니다.

Store는 state, Action, Reducer (+ Environment)들 가지고 있습니다.

State: 각 View에 대한 비즈니스로직 및 설명에 필요한 데이터가 정의. State에 변경에 따라 View를 업데이트 함.

Action: 이벤트가 enum 타입으로 정의

Reducer:  Action 이벤트에 맞게 State를 변경할 수 있고 다음 이벤트를 실행할 Effect를 반환

(Effect는 Combine Framework의 publisher 타입, Combine Operator 지원)



### WithViewStore

withViewStore를 통해 Store를 ObservableObject 프로토콜을 채택한 ViewStore로 변환

State 변경에 따른 View 업데이트를 위해 사용됨

```swift
struct ExampleView: View {
    let store: Store <ExampleState, ExampleAction>
    
    var body: View {
        WithViewStore(self.store) { viewStore in
        // ...
        }
    }
}
```


### ViewStore를 통해 특정 Action 전송

아래와 같이 특정 action을 전송하면 Reducer는 Action에 맞는 동작을 수행합니다.

```swift
Button {
    viewStore.send(.closeRenewalOpenView)
} label: {
    Image("navbar_close")
        .resizable()
        .frame(width: 20.0, height: 20.0)
        .foregroundColor(.gray_0)
        .padding(20)
        .padding(.top, 20)
}
```


### Reducer

Reducer는 전송된 Action에 따라 동작을 실행합니다.

State를 바꾸거나 추가적인 이벤트를 Effect 타입으로 반환할 수 있습니다.

concatenate 래핑 연산자를사용하여 여러개의 Effect를 순차적으로 실행할 수 있습니다.

반환할 이벤트가 없다면 return .none을 사용합니다.

```swift
case .submitChangedUnit:
    let updateUserInfo: UpdateUserInfoModel = .init(unit: Unit(rawValue: state.unitVM.selectedIndex ?? -1) ?? .none)
    return .concatenate(
        .init(value: .requestUpdateUserInfo(updateUserInfo)),
        .init(value: .moveBackward)
    )
    
case .onAppear:
    return .none
```




### Reducer 결합 or 분해

pullback을 통해 다양한 기능 화면의 reducer들이 local에서 global하게 사용될 수 있도록 변경하고, 

combine으로 하나의 reducer로 결합할 수 있습니다.

그렇게 되면 상위 Reducer에서 하위 Reducer를 관리 할 수 있습니다.

```swift
typealias OnboardingScreenReducer = Reducer<
    OnboardingScreenState,
    OnboardingScreenAction,
    Void
>

extension OnboardingScreenReducer {
    init() {
        self = Self.combine(
            OnboardingSurveyContainerReducer()
                .pullback(
                    state: /OnboardingScreenState.onboardingSurvey,
                    action: /OnboardingScreenAction.onboardingSurvey,
                    environment: { .init(mainQueue: .main)}
                ),
            SignUpReducer()
                .pullback(
                    state: /OnboardingScreenState.signUp,
                    action: /OnboardingScreenAction.signUp,
                    environment: { .init(networkClient: .live, mainQueue: .main) }
                )
        ).tcaDebug() // debug
    }
}

```

아래 코드를 보면 combine operator는 여러개의 reducer들을 가변 매개변수로 받아 처리합니다.

```swift
public static func combine(_ reducers: Self...) -> Self {
    .combine(reducers)
}
```