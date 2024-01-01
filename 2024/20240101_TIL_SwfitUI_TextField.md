# SwiftUI TextField

SwiftUI로 TextField 구현하기

```swift
struct ContentView: View {
    @State var name: String = ""
    @State var password: String = ""
    @State var text: String = ""
    
    var body: some View {
        VStack {
          TextField("Enter your name", text: $name)
            .padding()
            .background(Color(uiColor: .secondarySystemBackground))
            .textFieldStyle(.roundedBorder)
          
          Text("name: \(name)")
            
            SecureField("Enter your Password", text: $password)
              .padding()
              .background(Color(uiColor: .secondarySystemBackground))
              .textFieldStyle(.roundedBorder)
            
            Text("password: \(password)")
            
            TextEditor(text: $text)
              .cornerRadius(15)
              .padding()
              .background(.yellow)
              .frame(height: 300)
        }
    }
}
```

`Placeholder` 도 지정할 수 있고 스타일도 두가지로 지정할 수 있다. (plain, roundedBorder)
`backgroundcolor` 도 지정할 수 있다. 

`TextField`는 크게 커스텀이 필요하지 않으면 UIKit을 사용하지 않고 SwiftUI로 가능할 것으로 보인다.   
하지만 텍스트필드가 포커싱이 되었는지 확인하고 작업을 처리하는 것은 iOS15 부터 사용할 수 있는 @FocusState와 SubmitLabel을 사용해야 가능하다.   
하지만 현업에서 iOS15를 사용하기에 아직은 이르니까..
근데 포커싱을 트래킹 하는건 보통 구현이 되어야할 거 같아서 iOS14 까지는 UIKit을 사용하는게 좋을듯..

근데 TextEditor는 iOS 16부터는 사용할만 하나 이전 버전에서는 placeholder를 overlay로 제공해줘야 하고, 텍스트 입력칸의 Inset을 조정하는 것도 커스텀하게 구현해야한다.   
이런것들을 모두 커스텀으로 구현하기에는 공수가 오히려 많이 들기 때문에 그냥 UIKit을 사용하는 것이 좋을 것 같다.   