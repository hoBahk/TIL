### View의 Drawing과 관련된 메서드에 대해 알아보고, 각각의 호출 시점과 전체적인 호출 순서에 대해 알아봅시다.

* setNeedsDisplay(): 비동기 디스플레이 업데이트 요청(feat. draw())
* setNeedsLayout(): 비동기 레이아웃 업데이트 요청(feat. layoutSubviews())
* displayIfNeeded(): 동기 디스플레이 업데이트 요청
* layoutIfNeeded(): 동기 레이아웃 업데이트 요청

#### setNeedsDisplay()
: 명시적으로 호출할 수도 있고 1,2,3에 의해서 호출될수도 있다.
1. View를 부분적으로 가리고 있던 다른 View이동 또는 제거
2. hidden 프로퍼티를 No로 설정하여, 이전에 숨겨진 View를 다시 보는 경우
3. View를 화면밖으로 스크롤한다음, 화면으로 다시 이동

### 순서
아래의 이미지를 참고, 네 개 메서드의 처리(?)순서는 아래와 같다.
* drawing cycle과는 별도로 호출 즉시 처리
`layoutIfNeeded()`, `displayIfNeeded()`
* drawing cycle에 기반하여
`setNeedsLayout()` -> `setNeedsDisplay()`

![https://s3.ap-northeast-2.amazonaws.com/media.yagom-academy.kr/resources/usr/6152fa89ccd9ef11a51aee7d/20220207/62008189a2e1977979c39263.png](https://s3.ap-northeast-2.amazonaws.com/media.yagom-academy.kr/resources/usr/6152fa89ccd9ef11a51aee7d/20220207/62008189a2e1977979c39263.png)