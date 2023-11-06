# CocoaPods Podfile	


오늘 회사에서 관리 안된지 몇년 된 앱을 빌드하려고 했는데 여러곳에서 에러가 많이 나왔다.   
아무래도 오래동안 관리 되지 않아 코코아팟 오류도 나고 Swfit 버전이 바뀌고 XCode 버전도 바뀌었으니 문법에러도 발생했다.   

문법은 금방 수정할 수 있었지만 코코아팟에서 발생한 오류는 조금 복잡했다.   
일단 코코아팟 버전을 업데이트 하였다. 그러고서도 오류는 상당했다.   
처음에 DEPLOYMENT_TARGET이 12.0이상만 가능하다고 하여 20개 넘은 라이브러리 버전 타겟을 수동으로 12.0으로 올렸는데 당연히 pod install 하니까 원상태로 돌아왔다.   
그래서 Podfile을 수정했다. 

먼저 DEPLOYMENT_TARGET이 12.0 아래인 라이브러리는 12.0으로 설정하도록 코드를 추가했다.   


``` ruby

post_install do |installer|
  installer.generated_projects.each do |project|
        project.targets.each do |target|
            target.build_configurations.each do |config|
              if config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'].to_f < 12.0
                config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '12.0'
              end
             end
        end
 end
end
```


 그리고 나서 또 오류가 발생했는데..
 
걍 환경병수 이름이 바뀐듯 보였다.   
라이브러리가 너무 많아 하나씩 바꾸기 힘들어서 코드를 podfile에 넣었다.   


```
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      if config.base_configuration_reference.is_a? Xcodeproj::Project::Object::PBXFileReference
          xcconfig_path = config.base_configuration_reference.real_path
          IO.write(xcconfig_path, IO.read(xcconfig_path).gsub("DT_TOOLCHAIN_DIR", "TOOLCHAIN_DIR"))
      end
    end
  end
end
```

아 SPM 쓰자고!!!

이상 끝.
 
 