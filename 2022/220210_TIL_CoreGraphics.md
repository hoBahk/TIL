
## Core Graphics
Core Graphics는 아이폰과 아이패드에서 2차원 그래픽을 그릴 수 있도록 제공하는 그래픽 라이브러리이다. 
코어 Core Graphics는 Core Animation과 같이 애플이 제공하는 '쿼츠(Quartz)'라는 그래픽 라이브러리 안에 포함되어 있다.

## 예제
```swift
import UIKit

@IBDesignable // 버튼에 그려진 그래픽을 시뮬레이터를 켜지 않고 스토리보드에서 확인할 수 있다.
class PlusButton: UIButton {
    
    @IBInspectable private var lineWidth: CGFloat = 20
    @IBInspectable private var strokeColor: UIColor = .systemGreen
    @IBInspectable private var fillColor: UIColor = .systemBackground
    
    override func draw(_ rect: CGRect) {
        guard let context = UIGraphicsGetCurrentContext() else {
            return
        }
        
        let height = bounds.height
        let width = bounds.width
        
        // 원그리기
        let circleRect = bounds.insetBy(dx: width * 0.05, dy: height * 0.05)
        
        context.beginPath() // 그리기 시작
        context.setLineWidth(lineWidth) // 선 굵기 지정
        context.setFillColor(fillColor.cgColor) // 선으로 채워진 도형 컬러 지정
        context.setStrokeColor(strokeColor.cgColor) // 선 컬러 지정
        context.addEllipse(in: circleRect) // 지정된 사각형에 맞는 타원을 그린다.
        context.drawPath(using: .fillStroke) // 제공된 경로를 그린다. (어떤 모드로 그릴지 정한다.)
        context.closePath() // 그리기 종료
        
        context.beginPath()
        context.move(to: CGPoint(x: width * 0.2, y: height * 0.5)) // 그리기 시작점
        context.addLine(to: CGPoint(x: width * 0.8, y: height * 0.5)) // 선 그리기
        context.move(to: CGPoint(x: width * 0.5, y: height * 0.2)) // 그리기 시작점
        context.addLine(to: CGPoint(x: width * 0.5, y: height * 0.8)) // 선 그리기
        context.setLineCap(.round) // 선의 끝점을 어떻게 할 것인지
        context.drawPath(using: .stroke) // 제공된 경로를 그린다. (어떤 모드로 그릴지 정한다.)
        context.closePath() // 그리기 종료
        
        if isSelected { // 선택된다면
            let transform2 = CGAffineTransform(rotationAngle: CGFloat(Double.pi / 4)) // 180 / 4 = 45도 만큼 회전
            UIView.animate(withDuration: 0.3) {
                self.transform = transform2
            }
        } else {
            let transform2 = CGAffineTransform(rotationAngle: CGFloat(Double.pi * 4))
            UIView.animate(withDuration: 0.3) {
                self.transform = transform2
            }
        }
    }
}
```