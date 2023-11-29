# Fastlane

iOS에 특화된 CI/CD 자동화 툴.   
사이트 들어가 보면 안드도 있는거 같은데 iOS에서 많이 쓰는듯하다.

아래 4가지의 기능이 제공된다.  
- 스크린샷 자동화 ( AUTOMATE SCREENSHOTS )
- 배포 자동화 ( BETA DEVELOPMENT) 
- 앱스토어 배포 자동화 (APPSTORE DEVELOPMENT)
- 앱 서명 / 인증서 관리  (CODE SIGNING)

나는 테스트, 배포 자동화, 인증서관리를 사용하기 위해  `fastlane`을 구축했다.


## 인증서 / 앱 서명 관리
개발하다보면 인증서 다루는게 갱신하고 하려면 귀찮은 적이 많다.  
특히 여려명의 개발자가 같이 협업을 할 때....    

앱 개발과 배포를 위해 `certificate`와 `provisioning profile` 필요하다.   
- certificate(인증서): 개발자가 Apple 대신 앱을 실행할 수 있는 권리를 허용
- provisioning profile: 디바이스가 앱을 신뢰할 수 있는지 확인

### cert and sigh
- 기존 로컬에 있는 인증서를 취소하지 않고 사용하려면 cert and sigh 방법이 좋다.   
- cert는 유효한 certificate와 private key가 로컬에 설치되어 있는지 확인하고, sigh는 설치된. certificate와 일치하는 provisioning profile 로컬에 설치되어 있는지 확인한다.   
- 개발자가 추가 된다면 인증서를 로컬에 수동으로 설치해야 한다.

여러명의 개발자가 이 인증서를 공유하면서 사용해야 하는데 직접 전달한다면 관리가 되지않을 것이다.    
개발자 수가 적다면 각 인증서를 발급 받아 사용할 수도 있지만 인증서 발급에도 제한이 있기 때문에 개발자가 많아지면 인증서를 공유해야한다.    
그래서 Fastlane에서는 `match`라는 기능이 제공된다.

### match
`match`를 사용하여 개인키와 인증서를 git private repo나 AWS, GoogleCloud에 저장하여 팀원들이 해당 저장소를 동기화 하여 인증을 할 수 있다.    
해당 기능을 사용하면 새로운 개발자가 와도 쉽게 인증서를 공유할 수 있으며, 인증서를 하나로 관리 하기 때문에 갱신을 할 때에도 한 번만 하면 되기 때문에 간편하고 관리하기도 쉽다.   


## 배포자동화
배포를 하려면 빌드하여 아카이브를 하고, 그것을 테스트 플라이트에 올려야하는데 fastlane의 배포자동화 기능을 통하여 명령어 한줄로 빌드 넘버를 올리고 빌드, 테스트를 거친 후 테스트플라이트에 업로드 할 수 있다.  물론 AppStore에도 배포할 수 있다.   


## 참고
[FERNANDO 기술 블로그](https://fernando.kr/ios/2019-05-26-introduce-fastlane/)

