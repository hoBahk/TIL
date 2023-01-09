# XCFramework

- 시뮬레이터 빌드를 포함하여 여러 플랫폼에서 사용할 수 있도록 Xcode에서 만든 배포 가능한 바이너리 패키지

## 생성 방법

해당 레포지토리 디렉터리로 가서 아래 명령어 입력
디바이스와 시뮬레이터 프레임워크를 만들고 두개를 합친 XCFramework를 만든다.

```
$ xcodebuild archive -workspace EVReflection.xcworkspace -scheme "EVReflection_iOS" -archivePath "./build/ios.xcarchive" -sdk iphoneos SKIP_INSTALL=NO BUILD_LIBRARY_FOR_DISTRIBUTION=YES
$ xcodebuild archive -workspace EVReflection.xcworkspace -scheme "EVReflection_iOS" -archivePath "./build/ios_sim.xcarchive" -sdk iphonesimulator SKIP_INSTALL=NO BUILD_LIBRARY_FOR_DISTRIBUTION=YES
$ xcodebuild -create-xcframework -framework ./build/ios.xcarchive/Products/Library/Frameworks/EVReflection.framework -framework ./build/ios_sim.xcarchive/Products/Library/Frameworks/EVReflection.framework -output ./build/EVReflection.xcframework
```

## 사용 방법
1. 해당 레포지토리 디렉터리 경로에 Frameworks 디렉토리 만들어서 거기에 XCFramework를 넣어서 관리한다.
2. Xcode에도 드래그앤드랍
3. `Target - General`에서 `Framework, Libraries, and Embeded Content`에 등록하고 `Embeded&sign` 선택