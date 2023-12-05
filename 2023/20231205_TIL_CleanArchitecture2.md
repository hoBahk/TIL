# Clean Architecture + MVVM

## 의존관계

- 잘 변하는 것에서 변하지 않는 것으로 의존관계가 되는 것이 이상적인 형태이다.   
- 잘 변하지 않는 계층인 Domain 계층으로 Presentation과 Data 계층이 의존하는 형태.   

### Actor가 Entity를 확인하는 flow
- View가 ViewModel의 메서드 호출
- ViewModel은 usecase 실행 -> usecase는 Repository에 데이터 요청
- Repository로 부터 받은 데이터를 받아와 보여줄 프로퍼티에 저장
- 프로퍼티를 Observe 하고 있다가 프로퍼티 값이 변경 되면 View에 반영


## 계층
 
### Presentation Layer
- Flow, View, ViewModel 존재
- ViewModel은 하나 이상의 usecase를 실행하기 때문에 Domain Layer에 의존한다.  


### Domain Layer
- Entity, usecase, Interface 존재

### Data Layer
- DB, Network 존재
 
 
 


