# SwiftUI Ring

링그래플르 만들어 보았다.   


```swift
struct RingView: View {
    let innerRadius: Double
    let lineWidth: Double
    let backgroundColor: Color
    let color: Color

    var progress: Double
    
    var body: some View {
        let outerRadius = self.innerRadius + self.lineWidth
        
        ZStack {
            Circle()
                .stroke(self.backgroundColor, lineWidth: lineWidth)
                .padding(self.lineWidth/2)
                .frame(width: outerRadius * 2.0)
            
            Circle()
                .trim(from: 0, to: self.progress)
                .stroke(style: StrokeStyle(lineWidth: lineWidth, lineCap: .round))
                .fill(self.color)
                .rotationEffect(Angle(degrees: -90))
                .padding(self.lineWidth/2)
                .frame(width: outerRadius * 2.0)

            RingCap(progress: self.progress, ringRadius: innerRadius + lineWidth / 2)
                .fill(effectiveTipColor)
                .frame(width: lineWidth, height: lineWidth)
                .opacity(self.progress < 0.98 ? 0 : 1 )
                .shadow(
                    color: Color.black.opacity(0.6),
                    radius: 0.0,
                    x: self.getEndCircleShadowOffset().0,
                    y: self.getEndCircleShadowOffset().1
                )
        }
        .clipShape(RingShape(radius: innerRadius, lineWidth: lineWidth))
        .animation(.easeInOut(duration: 1.5), value: self.progress)
    }
    
    let startAngle: Double = 0
    let shadowOffsetMultiplier: Double = 1.0

    private func getEndCircleShadowOffset() -> (CGFloat, CGFloat) {
        let angleForOffset = absolutePercentageAngle + (self.startAngle)
        let angleForOffsetInRadians = angleForOffset.toRadians()
        let relativeXOffset = cos(angleForOffsetInRadians)
        let relativeYOffset = sin(angleForOffsetInRadians)
        let xOffset = relativeXOffset.toCGFloat() * self.shadowOffsetMultiplier
        let yOffset = relativeYOffset.toCGFloat() * self.shadowOffsetMultiplier
        return (xOffset, yOffset)
    }

    private var gradientStartAngle: Double {
        self.progress >= 1.5 ? relativePercentageAngle - 360 : startAngle
    }

    private var relativePercentageAngle: Double {
        // Take into account the startAngle
        absolutePercentageAngle + startAngle
    }

    private var absolutePercentageAngle: Double {
        self.percentToAngle(percent: self.progress, startAngle: 0)
    }

    private func percentToAngle(percent: Double, startAngle: Double) -> Double {
        (percent * 360) + startAngle
    }

    private var effectiveTipColor: Color {
        self.color
    }
}

struct RingShape: Shape {
    let radius: Double
    let lineWidth: Double

    func path(in rect: CGRect) -> Path {
        let outerRadius = radius + lineWidth
        let innerRadius = radius
        let center = CGPoint(x: rect.minX + rect.width / 2, y: rect.minY + rect.height / 2)
        var path = Path()

        path.addArc(center: center, radius: outerRadius, startAngle: .degrees(0), endAngle: .degrees(360), clockwise: true)
        path.addArc(center: center, radius: innerRadius, startAngle: .degrees(0), endAngle: .degrees(360), clockwise: false)

        return path
    }
}

struct RingCap: Shape {
    var progress: Double
    let ringRadius: Double

    var animatableData: Double {
        get { progress }
        set { progress = newValue }
    }

    func path(in rect: CGRect) -> Path {
        var path = Path()
        let progressAngle = Angle(degrees: (360.0 * progress) - 90.0)
        let tipRadius = rect.width / 2
        let center = CGPoint(
            x: ringRadius * cos(progressAngle.radians) + tipRadius,
            y: ringRadius * sin(progressAngle.radians) + tipRadius
        )

        let startAngle = progressAngle + .degrees(180)
        let endAngle = startAngle - .degrees(180)
        path.addArc(center: center, radius: tipRadius, startAngle: startAngle, endAngle: endAngle, clockwise: true)
        path.addArc(center: center, radius: tipRadius, startAngle: endAngle, endAngle: startAngle, clockwise: true)
        path.closeSubpath()

        return path
    }
}

fileprivate extension Double {
    func toRadians() -> Double {
        return self * Double.pi / 180.0
    }

    func toCGFloat() -> CGFloat {
        return CGFloat(self)
    }
}

```