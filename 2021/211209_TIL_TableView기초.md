## TableView

### 사용한 TableView 메서드

1. 섹션의 개수를 return하는 함수
```swift
func numberOfSections(in tableView: UITableView) -> Int
```

2. 한 섹션에 몇 개의 row가 들어가는지 return하는 메서드
```swift 
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int 
```

3. cell에 세팅?하는 메서드
```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
```

4. 셀의 재사용 대기중에 수행하는 작업 -> UITableViewCell
```swift
override func prepareForReuse() {
}
```


## AutoLayout

오토레이아웃은 크기, 위치만 정해주면 된다.    

### Constraints
View가 계산을 적게 하도록 제약을 걸어주는 것이 좋다.   
제약은 식으로 나타낼 수 있다.   
```
// y = mx + c
RedView.Leading = 1.0 x BlueView.Trailing + 8.0
```
1.0은 multyplier를 나타 내고 8.0은 constans를 나타낸다.

### priority
우선순위는 높을수록 더 큰 힘을 가진다. 제약이 같은 곳에 걸렸을 때 우선순위가 큰 제약이 걸린다.   
보통 250, 750, 1000을 사용한다.

### 테이블 뷰 AutoLayout
테이블 뷰의 커스텀 셀을 테이블 뷰의 contentView와의 관계를 제약으로 설정해야한다.   

