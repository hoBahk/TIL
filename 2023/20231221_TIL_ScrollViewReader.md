# ScrollViewReader

이거 아주 신박하다..   

이것이 뭐냐면 스크롤뷰 안에 있는 뷰에 id로 태그를 걸고 해당 id로 이동할 수 있도록 해준다.   
귀찮게 오프셋을 사용하지 않아도 된다.   너무 편함

맨 위에 있는 뷰에 id를 넣어주고 해당뷰로 이동하면 `scrollToTop`이 된다.
맨 아래 있는 뷰에 id를 넣어주면 `scrollToBottom` 이 되겠지

그리고 정말 좋은게 `proxy.scrollTo(_: id, anchor: )`가 있는데 `anchor` 파라미터에 해당뷰를 top에 놓을건지 center, bottom, leading, trailling, bottomleading 등등 모든 방향으로 볼 수 있다.  너무 편한하다...!!!


## 예제코드

```swift

import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack {
            ScrollViewReader(content: { proxy in
                ZStack(alignment: .bottomTrailing) {
                    ScrollView {
                        VStack {
                            Button("go") {
                                withAnimation {
                                    proxy.scrollTo(14, anchor: .center)
                                }
                            }
                            .id(0)
                            
                            VStack {
                                Text("asd")
                            }
                            
                            ForEach(1..<100) {
                                ItemView(id: $0)
                                    .id($0)
                            }
                        }
                        .padding()
                    }
                    
                    Button {
                        withAnimation {
                            proxy.scrollTo(0, anchor: .top)
                        }
                    } label: {
                        Text("TOP")
                            .foregroundColor(.white)
                            .padding(.horizontal, 8)
                            .padding(.vertical, 4)
                            .background(Color.blue)
                            .clipShape(.capsule)
                            .padding()
                    }
                }
                
            })
        }
    }
}

private struct ItemView: View {
    let id: Int

    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: 12.0)
                .foregroundColor(.yellow)
            
            Text("\(id)")
        }
        .frame(height: 197)
    }
}
```