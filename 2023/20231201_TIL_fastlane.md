# Fastlane

## fastfile 작성

```
platform :ios do
  desc "Push a new beta build to TestFlight"
  lane :beta do
    get_certificates
    get_provisioning_profile

    increment_build_number(
        build_number: latest_testflight_build_number + 1
    )

    build_app(scheme: "")

    upload_to_testflight

    slack(
      message: "Testflight에 배포되었습니다!",
      slack_url: "슬랙 웹흑 주소"
    )
  end

  error do |lane, exception, options|
    slack(
      message: "에러 발생 : #{exception}",
      success: false,
      slack_url: "슬랙 웹흑 주소",
    )
  end
end
```

### get_certificates, get_provisioning_profile
인증하는데 필요한 Certicicate와 provisioning_profile을 가져온다.

### increment_build_number
빌드넘버를 올린다.    
`build_number: latest_testflight_build_number + 1`를 추가해 testflight에 올라간 빌드버전보다 +1 된 빌드버전이 되도록 설정한다. 

### build_app
빌드

### upload_to_testflight
testflight 업로드

### slack 메시지 
배포 완료 되면 설정한 슬랙채널에 메시지가 가도록 한다.   
```
    slack(
      message: "Testflight에 배포되었습니다!",
      slack_url: "슬랙 웹흑 주소"
    )
```

위의 작업을 하던 도중 에러가 발생하면 발생한 에러에 대해 슬랙에 메시지를 보내준다.   
```
  error do |lane, exception, options|
    slack(
      message: "에러 발생 : #{exception}",
      success: false,
      slack_url: "슬랙 웹흑 주소",
    )
  end
```

