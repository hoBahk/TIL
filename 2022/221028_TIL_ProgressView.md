# ProgressView SwiftUI, iOS 13에서 개발하기

## 고민한 점
ProgressView를 iOS13 SwiftUI에서 구현해야 하는데 SwiftUI에서 ProgressView는 iOS 14부터 사용할 수 있다고 한다.   
또 SwiftUI의 ProgressView는 trackingTintColor가 변경이 되지 않는다.. 내가 몰라서 그런 것일 수도 있다.. ㅎ.  
그래서 UIViewRepresentable을 사용할까 하다가 UIViewRepresentable을 사용하면 프레임 맞추기도 어렵고 SwiftUI를 쓴이유가 무색해지는 것 같아서 직접 커스텀해서 만들기로 했다.   
다행히 Bar 형태의 복잡하지 않은 구성이라 쉽게 만들었다.

```swift
struct CustomProgressView: View {
    let progress: CGFloat
    
    var body: some View {
        GeometryReader { geometry in
            ZStack(alignment: .leading) {
                RoundedRectangle(cornerRadius: geometry.size.height / 2)
                    .foregroundColor(Color.gray_0)
                    .frame(width: geometry.size.width, height: geometry.size.height)
                
                RoundedRectangle(cornerRadius: geometry.size.height / 2)
                    .foregroundColor(Color.blue_8)
                    .frame(width: geometry.size.width * progress, height: geometry.size.height)
            }
        }
    }
}
```


