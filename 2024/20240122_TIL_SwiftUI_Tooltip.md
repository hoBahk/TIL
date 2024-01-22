# SwiftUI_Tooltip

오늘은 툴팁 모디파이어를 만들었다.   


## 예제 코드

사용하기 편하게 모디파이어로 만들어서 사용하도록 했다.   
관건은 사용하는 콘텐츠의 크기를 파악해야 했다. 그리고 메시지와 말꼬리의 위치를 지정해야 했다.   


```swift
struct ToolTip: ViewModifier {
    let message: String
    @Binding var isShowing: Bool

    @State private var commonSize: CGSize = .zero
    @State private var contentWidth: CGFloat = 10
    @State private var contentHeight: CGFloat = 10

    func body(content: Content) -> some View {
        ZStack {
            content
                .readSize(onChange: { size in
                    self.commonSize = size
                })
        }.overlay(
            self.tooltipView
        )
    }

    @ViewBuilder
    var tooltipView: some View {
        if self.commonSize != .zero {
            VStack(spacing: -3) {
                if isShowing {
                    self.messageView
                        .offset(x: self.alignXManually())
                    
                    Image("polygon", bundle: .module)
                        .resizable()
                        .frame(width: 18.0, height: 13.0)
                }
            }
            .offset(y: self.alignYManually() - 6.0)
            .frame(maxWidth: .infinity)
            .onTapGesture {
                self.isShowing.toggle()
            }
        } else {
            EmptyView()
        }
    }
    
    var messageView: some View {
        Text(self.message)
            .multilineTextAlignment(.leading)
            .fontAndColor(.regular_13, .white)
            .padding(.vertical, 8.0)
            .padding(.horizontal, 12)
            .background(Color.green_800)
            .cornerRadius(6.0)
            .background(self.sizeMeasurer)
            .fixedSize()
    }

    // MARK: - TooltipModifier Body Properties

    private var sizeMeasurer: some View {
        GeometryReader { geometry in
            Text("")
                .onAppear {
                    self.contentWidth = geometry.size.width
                    self.contentHeight = geometry.size.height
                }
        }
    }

    private func alignXManually() -> CGFloat {
        if self.contentWidth > self.commonSize.width {
            return -abs(self.contentWidth - self.commonSize.width) / 2 + 13
        } else {
            return abs(self.contentWidth - self.commonSize.width) / 2
        }
    }
    
    private func alignYManually() -> CGFloat {
        -(self.commonSize.height + self.contentHeight) / 2
    }
}

private struct SizePreferenceKey: PreferenceKey {
    static var defaultValue: CGSize = .zero
    static func reduce(value: inout CGSize, nextValue: () -> CGSize) {}
}

extension View {

    func tooltip(message: String,
                 isShowing: Binding<Bool>) -> some View {
        self.modifier(ToolTip(message: message,
                              isShowing: isShowing))
    }

    func readSize(onChange: @escaping (CGSize) -> Void) -> some View {
        background(
            GeometryReader { geometryProxy in

                Color.clear
                    .preference(key: SizePreferenceKey.self, value: geometryProxy.size)
            }
        ).onPreferenceChange(SizePreferenceKey.self, perform: onChange)
    }
}
```