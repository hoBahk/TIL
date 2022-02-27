## Accessbility

시각, 청각 등의 어려움을 겪고 있는 사람들을 위한 접근성기능이다.

### Voice Over

#### isAccessibilityElement: Bool
- `Accessbility`를 활성화 하려면 `true` 반대는 `false`
- `label`이나 `button`은 default가 `true`이지만 `image`는 `false`이다.


#### accessibilityLabel: String?
- accessibilityLabel을 설정할 수 있는 프로퍼티
- VoiceOver에서 해당 처음에 읽어주는 속성이다.
- label의 경우 설정해주지 않으면 label에 설정된 텍스트를 읽어준다.
- button의 경우는 image가 있다면 해당 image이름으로 label이 설정되기 떄문에 따로 설정이 필요할 수 있다.


#### accessibilityHint: String?
- accessibilityHint를 설정할 수 있는 프로퍼티
- 해당 인터페이스에 대해 부가 설명을 한다.
- 인터페이스를 어떻게 사용하는게 좋은지 설명, 인터페이스를 사용하면 어떻게 되는지에 대한 설명을 해줄 때 사용


#### accessibilityTraits: UIAccessibilityTraits
- accessibilityTraits를 설정할 수 있는 프로퍼티
- 해당 element의 특성을 나타낸다.
- [accessibilityLabel + accessibilityTraits]형식으로 Voice Over 출력

<br><br>

### Dynamic Type

#### adjustsFontForContentSizeCategory
- Dynamic Type 설정 여부