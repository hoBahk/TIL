
# 학습내용

## StackView

### StackView 추가

view를 추가하는 메서드는 여러가지가 있다.

먼저 StackView에는 두 개의 배열을 가지고 있다.
- UIView class로부터 상속한 subviews 배열
- UIStackView class에 선언되어 있는 arrangedSubView 배열

subView 밑에 arrangedView가 있다.   
arrangedView에 포함 되면 subView에도 포함된다. 반대는 성립하지 않는다.

arrangedView에 추가하면 해당 StackView가 가진 배치 속성에 영향을 받지만 subView에 속한 속성은 영향을 받지 않는다.

1. `func addArrangedSubview(_ view: UIView)`
	- arrangedSubView, SubView에 모두 추가 된다. 

2. `func addSubview(_ view: UIView)`.  
	- SubView에만 추가 된다. 

3. `func insertArrangedSubview(_ view: UIView, at stackIndex: Int)`
	- `addArrangedSubview()`와 같이 arrangedSubView에 추가 하지만, 이 메서드는 배열의 인덱스를 지정하여 넣을 수 있다.

### StackView 삭제

1. `removearrangedSubView()`
	- arrangedSubView에서만 삭제

2. `func removeFromSuperview()`
	- child view를 삭제할때 사용  
	- arrangedSubView, SubView에 모두 삭제 된다. 


## ScrollView

### 스크롤 위치 설정
1. scrollView에서 setContentOffset 메서드를 이용하여 offset을 지정해준다.
	- scrollView.contentSize.height: 스크롤뷰로 볼 수 있는 전체 컨텐츠의 높이
	- scrollView.bounds.height: 내가 볼 수 있는 스크롤 뷰 화면의 높이

```swift 
CGPoint(x: 0, y: scrollView.contentSize.height - scrollView.bounds.height)
scrollView.setContentOffset(bottomOffset, animated: true)
```

# 고민한 점
계산기 프로젝트 중 입력할 때 NumberFormatter로 변환하여 보여주었다. 그랬더니 12. 이나 12.00 수를 입력하면 유효한 숫자만 나와서 12만 출력되었다. 그래서 고민을 많이 했다. 그래서 고민 끝에 "."을 기준으로 split으로 나누어서 [0]은 NumberFormatter를 따르도록 하고 [1]은 따르지 않도록 하였다.


