# SwfitLint


## 배운점

### SwiftLint 정책

####identifier_name 옵션
- min_length
- max_length
- allowed_symbols: 해당 문자열은 제외 시켜준다.
- excluded: 해당 문자열과 일치하는 이름은 제외시켜준다.

#### line_length
- warning
- error
- ignores_urls: url은 무시하고 column수를 센다.


### Autocorrect

스크립트에서 `--fix` 옵션을 붙여주면 자동으로 고쳐준다.   
AutoCorrect가 지원되는 정책에 대해서만 자동으로 고쳐준다.

## 느낀점
요즘 회사에서 SwiftLInt의 정책을 정리하고 수립하고 있다.   
같이 컨벤션에 대해서도 이야기를 나눌 수 있는 시간이 되어서 나름 재미도 있었다.
이미 짜여진 코드에 대해서 린트를 적용하는 것이라 수정사항이 많긴 했지만...
린트.. 생각보다 옵션들이 많다.. 근데 생각보다 유도리가 있진 않은 것 같다..