
## 프로젝트 하면서 알게 된 것 

### 1. 제목과 본문의 폰트를 다르게 하여 구분하는 기능 구현
* `AttributtedString`을 사용하여 TextView의 제목과 본문의 폰트를 다르게 하여 사용자가 보기에 편하도록 구현
* textView의 delegate 메서드(`shouldChangeTextIn`)와 textView의 `typingAttributes` 프로퍼티를 사용하여 입력중에도 제목과 본문에 맞는 폰트가 적용되도록 구현

```swift
func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
        let currentText = textView.text as NSString
        let titleRange = currentText.range(of: Constant.lineBreak.description)
        if titleRange.location < range.location {
            textView.typingAttributes = Constant.bodyAttributes
        } else {
            textView.typingAttributes = Constant.headerAttributes
        }
        return true
    }
```


## 2. tableView의 Delegate 메서드를 활용한 스와이프 기능 활용
    
tableView의 Delegate 메서드를 활용하여 셀을 스와이프 했을 때 선택할 수 있는 옵션을 설정 할 수 있다.
    
```swift
// 오른쪽에서 왼쪽으로 스와이프 했을 때의 옵션 설정
override func tableView(
        _ tableView: UITableView,
        trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath
    ) -> UISwipeActionsConfiguration? {
        // ...
    }
    
// 왼쪽에서 오른쪽으로 스와이프 했을 때의 옵션 설정
override func tableView(_ tableView: UITableView, 
    leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath
    ) -> UISwipeActionsConfiguration? {
        // ...
    }
```
