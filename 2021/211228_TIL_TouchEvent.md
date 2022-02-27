# # 커리어스타터 12주차(월)
# 12/27


* Q1 : iOS 환경에서 사용자의 터치 이벤트를 알아채거나 제어할 수 있는 방법의 종류를 알아봅시다
	- gesture recognizer를 활용해 이벤트 핸들링(target-action)
	- gesture recognizer delegate 메소드를 활용해 이벤트 핸들링
	- UIResponder의 각각의 이벤트 처리에 대한 메소드 활용해 이벤트 핸들링



* Q2 : iOS 환경에서 사용자가 일으킬 수 있는 이벤트의 종류는?
	- Touch: 화면의 터치
	- Motion: 사용자가 기기를 흔들 때와 같이 기기의 움직임과 관련
	- remoteControl: 원격제어 이벤트, 멀티미디어를 제어하기 위한 목적으로 헤드셋 또는 외부 액세서리에서 수신된 명령으로 발생
	- Press: 물리버튼 누름
* Standard Gestures
	* Tap, Drag, Flick, Swipe, Double tap, Pinch, Three-finger pinch, Three-finger swipe, Touch and hold, Rotate, Shake


* Q3 : 뷰 위에 텍스트 필드가 있고 텍스트 필드 위에 탭 제스쳐 인식기가 있는 상황에서
각 상황에서 사용자가 텍스트 필드 위를 탭 했을 때 어떤어떤 객체가 어떤 메서드를 통해 반응하나요?
	- USERINTERACTIVE, CONTROL STATE 모두 enabled 되었을 떄 TextField.touchesBegan 이 호출되고 나머지는 myView.touchesBegan이 호출된다.


* Q4 : Responder Chain과 Gesture Recognizer는 이벤트 제어에서 상호간 상관관계일까요? 별개관계일까요? 그렇게 생각한 이유는 무엇인가요?
	- 기본적인 것들은 Gesture Recognizer로 가능하지만 커스텀 제스쳐를 트래킹 하거나 뷰에 드로잉을 할 때 등 제스처로 해결이 안되는 특수한 경우들에 쓴다. 



touchesBegan메서드가 호출된다고 제스처에 대해 respond한다고 생각하면 안된다.
respond에 대한 메서드는 becomeFirstResponder, resignFirstResponder가 있다.


Responder를 찾았을 때와 찾지 못했을 때 터치 이벤트 전달되는 순서가 다르다?
* 리스폰더를 찾았을 때는 하위뷰에서 상위뷰로 간다.
* 리스폰더를 찾지 못했을 때는 상위뷰에서 하위뷰로 간다.
Responder Chain을 통해 responder를 찾는 과정과 터치의 이벤트를 전달하는 순서는 별개일 수 있다.