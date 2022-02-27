# SwiftUI

## Text 설정

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Padding")
                .padding() // 전체
                .padding(.all, 10) // 전체
                .padding(.bottom, 10) // top, bottom, leading, trailing 있음

            Text("Font 속성1")
                .font(.title2) // 지정된 폰트 사용
                .font(.system(size: 50)) // 글자 크기 (지정된 폰트 사용 시 반영되지 않음)
                .strikethrough(color: .red) // 취소선
            
            Text("Font 속성2")
                .font(.system(size: 30, weight: .bold, design: .rounded)) // 사이즈, 굵기, 디자인을 설정
                .foregroundColor(.green) // 글자색 지정
            
            
            Text("TextView 설정")
                .multilineTextAlignment(.center) // 텍스트뷰 정렬
                .lineLimit(nil) // 줄 수 제한 (UIKit과 다르게 옵셔널로 되어 있어 nil일 때 제한 없음
                .frame(width: 100, height: 100) // 텍스트뷰 자체 크기 설정
                .background(Color.yellow) // 텍스트뷰 배경색 지정
                .border(.purple, width: 3) // border 설정 (Rectangle일 때)
                .overlay( // 뷰를 겹치게 하여 border 설정, 라운드 처리를 할 경우 overlay를 통해 border 처리를 해주어야 한다.
                    RoundedRectangle(cornerRadius: 20)
                        .stroke(Color.purple, lineWidth: 5)
                )
                .cornerRadius(20) // 텍스트뷰 라운드 설정

            Text("색상 지정")
                .background(Color.yellow) // 텍스트뷰 배경색 지정
                .foregroundColor(.green) // 글자색 지정
            
            Text("터치 이벤트")
                .onTapGesture {
                    print("touch")
                }
        }
    }
```


![](SwiftUI/SwiftUI_Text_img.png)


## Stack

```swift
// VStack, HStack
struct ContentView: View {
    var body: some View {
        VStack(alignment: .center, spacing: 20) { // alignment, spacing 설정
            HStack {
                CustomRectangleView()
                CustomRectangleView()
                CustomRectangleView()
            }
            HStack {
                CustomRectangleView()
                CustomRectangleView()
                CustomRectangleView()
            }
            HStack {
                CustomRectangleView()
                CustomRectangleView()
                CustomRectangleView()
            }
        }
        .padding() // padding 설정
        .background(Color.green) // 스택뷰의 배경색 설정
        .cornerRadius(20) // 스택뷰 라운드 설정
    }
}


// ZStack
struct CustomRectangleView: View {
    var body: some View {
        ZStack { // ZStack은 겹치는 스택 XYZ축 중에 Z축 방향으로 스택을 쌓아 나간다.
            Rectangle() // 사각형을 만듦
                .frame(width: 90, height: 90) // frame 설정
                .foregroundColor(.yellow) // 도형색 설정
            Rectangle()
                .frame(width: 70, height: 70)
                .foregroundColor(.purple)
            Rectangle()
                .frame(width: 50, height: 50)
                .foregroundColor(.orange)
        }
    }
}

```

![](SwiftUI/SwiftUI_Stack_img.png)

