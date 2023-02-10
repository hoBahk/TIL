# Progress View


ProgressView 등 SwiftUI에서 지원하는 것들이 있지만 실제 기획에서 요구하는 방향에서는 지원하는 것들로 구현하지 못할 때가 많다.

그래서 ProgressView도 gradient, 배경색 등 많은 것들은 변경해야 할 때 직접 커스텀뷰를 만들어서 사용했다.

```swift
struct GradientProgressBar: View {
    @Binding var progress: Double
    let backgroundColor: Color
    let gradientColor: [Color]

    var body: some View {
        GeometryReader { geometry in
            let width = geometry.size.width
            let height = geometry.size.height

            ZStack(alignment: .topLeading) {
                Rectangle()
                    .frame(width: width, height: height)
                    .cornerRadius(50)
                    .foregroundColor(self.backgroundColor)

                LinearGradient(colors: self.gradientColor, startPoint: .leading, endPoint: .trailing)
                    .frame(width: self.progress < 1 ? width * self.progress : width, height: height)
                    .cornerRadius(50)
            }
        }
    }
}
```

