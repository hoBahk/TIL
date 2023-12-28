# SPM_Localization

SPM으로 SDK를 배포하는데 Localization도 해야했다.  
그래서 알아봤는데 생각보다 간단했다.   
일반적으로 Localization 햘 때와 같이 구조와 이름을 똑같이 해주면 됐다.   
먼저 다국어 처리할 언어를 en.lproj, ko.lproj와 같이 폴더를 만들어준다.   
그리고 나서 폴더 안에 Localizable.String 파일을 만들어준다.   

그리고 나서 아래와 같은 형식으로 써주면 된다.  

```
"key" = "value";  
```
  
  메인 프로젝트에서 다국어 처리한 것이 적용되게 하려면 먼저 프로젝트에서 Localizations에서 다국어 처리할 언어를 추가해준다.   
  그리고 나서 Info.plist에 아래와 같이 추가해준다.

  ```
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ko</string>
</array>
  ```
  
  굳굳
  