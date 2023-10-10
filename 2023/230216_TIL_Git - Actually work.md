# Git - Actually work


## 공부한 것
Git은 해시를 통해 모든 커밋을 참조한다. 이 때 SHA-1를 사용한다.   
Git은 데이터와 해당 메타 데이터를 가능한한 빠르고 효율적으로 저장하고 검색하기 위해 트리와 같은 구조로 참조한다.

세가지 중요한 객체
- Commits: 커밋에 대한 메타데이터와 사위 커밋 및 그 아래 파일엥 대한 포인터를 포함하는 구조
- Trees: 커밋에 포함된 모든 파일의 트리 구조
- Blobs: 트리에 있는 압축된 파일 모음

Git은 `.git` 폴더에 안에 objects라는 폴더에서 커밋들을 관리한다.   
두글자로 되어있는 폴더들이 많은데 커밋 해시들의 앞에 두글자만 따서 폴더들을 만들고 해당하는 두글자로 시작하는 커밋들을 해당 폴더 안에 넣어서 관리한다.

> 예를 들어, d7c33fdd7d35372cba78386dfe5928f1ba8dfb70 해시가 있으면
d7 디렉터리 안에 c33fdd7d35372cba78386dfe5928f1ba8dfb70 파일이 존재하게 된다.

해당 파일을 그냥 열면 압축되어 있기 때문에 우리가 알 수 없는 문자로 나오므로, 아래 명령어를 사용한다.   
해당 명령어는 객체의 유형을 파악하고 적절한 형식의 출력을 제공하도록 한다.
>git cat-file -p 

출력된 것을 보면 tree 해시가 나온다.   
해당 tree 해시를 또 `git cat-file -p ` 명령어로 확인하면 tree와 blob이 나오는데 tree는 그 내부의 트리 구조에 대한 참조 해시이고 blob은 압축된 파일을 뜻한다.  
우리가 Finder에서 보는 디렉터리는 tree로 관리되고 파일은 blob으로 압축되어 관리 된다.


## 느낀점
평소에 그냥 당현하게 사용하던 Git에 대해서 어떻게 동작하는지 얇게나마 보게 되어 재밌고 새롭다.
앞으로도 Git에 대해서 더 공부하고, 다른 것들도 살펴볼 생각이다.