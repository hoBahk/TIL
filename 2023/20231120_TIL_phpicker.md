# PHPicker

QA 이슈에 사진을 선택하고 등록 날짜를 오늘로 할건지 사진이 찍힌 날짜로 할건지 정하는 기능이 있는데 오늘로 하면 권한을 묻지 않고 사진을 찍은 날짜로 하면 권한을 물어본다는 것이다.

언뜻 전에 알아보았을 떄 PHPicker는 권한을 요청하지 않아도 이미지를 가져올 수 있다고 했던것이 기억났는데 확실치 않고 그럼 왜 권한을 물어보는가 알기 위해 PHPicker에 대해서 알아보았다. 

PHPicker는 이미지를 가져올 때 사용자에게 권한을 요청하지 않는다고 한다. 
PHPickerViewController는 별개의 프로세스에서 동작한다고 한다. 즉 앱에서 Photo List를 보여주는 것이 아니라 별개의 프로세스에서 보여주기 때문에 앱에서는 권한 요청을 하지 않아도 된다고 한다.

그래서 권한을 요청하지 않는것이 맞지만 이미지를 가져오는 것만 권한을 요청하지 않을 뿐,
`PHAsset.fetchAssets`을 이용해 이미지의 정보를 가져와 찍힌 날짜를 가져오려면 PHAsset라이브러리를 사용하기 때문에 이 라이브러리는 권한 요청을 해야한다.

그래서 사진첩을 열 때 권한 요청을 받는 것이 아닌 사진을 고르고 나서 이미 이미지를 가져온 상태에서 이미지의 날짜를 읽어오려고 하니 권한 팝업이 뜨는 것이다.

오늘 또 하나 알게 되었다!

Good