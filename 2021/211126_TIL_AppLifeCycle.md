# App Life Cycle

App Life Cycle은 앱이 실행되고 종료될 때까지의 앱의 생명주기를 말한다.

App Life Cycle은 ios13 이전과 이후가 많이 다르다.
ios13 이전은 AppDelegate만 있지만 ios13 이후에는 AppDelegate와 SceneDelegate가 존재한다.   
ios13부터 아이패드에서 멀티윈도우를 지원하기 시작하면서 App과 Scene의 두 관점에서 Life Cycle을 관리한다.   
멀티 윈도우는 하나의 앱을 두 개 이상의 UI로 보는 것을 말한다.   
Scene(UI)하나를 하나의 인스턴스로 생각하면 된다.   
앱은 하나지만 Scene인스턴스들을 생성하여 여러개의 Scene(UI)을 볼 수 있게 된다.   

## App의 상태

1. Not Runinng: 종료되어 있고 실행되지 않은 초기 상태
2. Inactive: 실행되었지만 사용자의 동작을 받을 수 없는 상태
	- 알림창을 내렸을 떄
	- 앱 스위칭 할 때
	- 시스템 알림을 받을 때
3. Active: 앱이 활성화 되어 실행되는 상태
4. Background: 백그라운드에서 실행되는 상태
5. Suspended: 백그라운드에서 정지된 상태

## AppDelegate
1. 설명
	- App이 해야 할 일을 대신 해주는 Delegate

2. 역할
	- 프로세스 Life Cycle 관리
	- 세션 Life Cycle 관리
		- Scene과 세션을 맺어 Scene 인스턴스를 관리한다.
	- 앱의 데이터 구조를 초기화
	- 앱의 EntryPoint(진입점) 제공
	- 앱 이벤트 컨트롤

3. 메서드
	- application(_ application: UIApplication, willFinishLaunchingWithOptions~)
		- 앱 실행 시 호출
	- application(_ application: UIApplication, DidFinishLaunchingWithOptions~)
		- 앱 실행 직후 호출
	- applicationWillTerminate(_ application: UIApplication)
		- 앱 종료시 호출

## SceneDelegate

1. 설명
	- Scene의 일을 대신하기 위한 Delegate로, iOS13 이후 아이패드의 멀티윈도우 기능을 지원하기 위해 새로 만들어졌다.
	- 앱은 여러개의 Scene인스턴스를 생성할 수 있다.
	- 기존의 AppDelegate의 일의 일부를 가져왔다.

2. 역할
	- Scene(UI)의 Life Cycle을 관리한다.
		- background, foreground active, foreground inactive 등 각 시점에서 수행해야할 작업을 관리한다.

3. 메서드
	- sceneDidBecomeActive
		- 앱이 Active 상태로 전환될 때 호출되는 함수
	- sceneWillResignActive
		- 앱이 Inactive 상태로 전환될 때 호출되는 함수
	- sceneWillEnterForeground
		- 앱이 Foreground로 진입할 때 호출되는 함수
	- sceneDidEnterBackground
		- 앱이 Background로 진입한 직후 호출되는 함수

		
## 멀티태스킹과 멀티 윈도우
1. 멀티태스킹은 여러개의 앱을 한번에 띄우고 사용할 수 있는 기능 (1 App 1 UI)
2. 멀티윈도우는 하나의 앱을 여러개의 Scene(UI)으로 띄우고 사용할 수 있는 기능 (1 App N UI)