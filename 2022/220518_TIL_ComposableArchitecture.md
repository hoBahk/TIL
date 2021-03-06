# ComposableArchitecture

## 주요 구성

### State
- 기능을 수행하고 UI를 렌더링 하는데 필요한 데이터 타입


### Action
- 사용자 작업, 알림, 이벤트 소스 등 기능에서 발생할 수 있는 모든 작업을 나타내는 타입

### Environment
- API 클라이언트, 분석 클라이언트 등과 같이 기능에 필요한 종속성을 보유하는 타입

### Reducer
- 앱의 현재 상태를 액션이 주어진 다음 상태로 진화시키는 방법을 설명하는 기능.
- Effect 타입으로 반환하며, 실행해야하는 모든 효과를 반환한다. (ex API 요청)

### Store
- 실제로 기능을 구동하는 런타임, 스토어가 Reducer와 이펙트를 실행할 수 있도록 모든 유저 액션을 스토어에 전송하고, 스토어의 상태 변화를 관찰해 UI를 업데이트 할 수 있다.

