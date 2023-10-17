# Cocoapods

## pod install vs pod update
### pod install
맨처음 팟을 설정할 때 사용하기도 한다.  
그 후에는 Podfile의 추가, 삭제, 수정을 반영할 때도 사용한다.   
`pod install`을 실행하면 새로운 팟을 다운받고 설치한다.   
그리고 각 팟의 설치된 버전을 Podfile.lock에 기록한다.   
`pod install`을 실행하면 Podfile.lock에 써있는 버전들만 다운 받게 된다.  
새로운 버전이 있는지 체크하지 않는다.   


### pod update
`pod update {팟 이름}` 을 실행하면 해당 팟의 새로운 버전이 있는지 확인하고 새로운 버전이 있다면 새로운 버전으로 업데이트 해준다.    
`pod update`만 실행시키면 모든 팟들을 새로운 버전으로 업데이트 시켜준다.   


### pod repo update
` /Users/{사용자이름}/.cocoapods/repos`에 있는 모든 podspec 파일을 업데이트 한다.   


## 느낀점
코코아팟은 귀찮고 복잡해!! 에러가 나도 골치 아프고..    
SPM을 적극 활용하자 ㅎ
참고로 팀원과 협엽할 때는 Podfile.lock을 공유해서 같은 버전을 쓰도록 하는 것이 좋다.   
만약 동료들과 같은 checksum을 얻는데 실패했다면 `rm -rf Pods && pod install`을 실행시켜 다시 인스톨 해준다!