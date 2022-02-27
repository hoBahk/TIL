## DataSource VS Delegate

DataSource는 데이터를 받아 뷰에 나타날 데이터를 설정해주는 것을 말한다.

```swift
func tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String? // Section개수
func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int  //Row개수
func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: IndexPath) -> UITableViewCell  // 셀에 무엇을 보여줄지
```

delegate는 동작, 이벤트를 말한다. 
```swift
func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: IndexPath) // 셀을 클릭했을 때 동작
func tableView(tableView: UITableView, willBeginEditingRowAtIndexPath indexPath: IndexPath) // 테이블 편집상태일 때 실행
```


## 고민한 점
JSON을 디코딩 하여 셀을 반환하는 과정에서 옵셔널 처리와 오류처리를 적절하고 보기 좋게 하려고 많은 고민을 했지만 아직 좋은 아이디어가 떠오르지 않아 알아보고 생각해보는 중이다.

콘의 조언을 구해보았다.   
콘의 의견:    
1. 에러가 발생하면 개발자는 그것을 트래킹 할 수 있어야 한다.   
2. 크래시를 최대한 발생시키지 않는 것이 좋다. 그렇기 때문에 강제 언래핑을 지양하는 것이다.   
3. 개발과정 중에서 충분히 잡을 수 있는 크래시의 경우 강제 언랩핑도 나쁜 옵션은 아니라고 생각한다.   
4. 빈셀을 반환하는 것도 방법이다.   
5. 혹은 DiffableDataSource을 사용하면 nil을 반환할 수 있다.


DiffableDataSource에 대해서 공부해보아야겠다.