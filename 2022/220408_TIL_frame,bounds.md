

## Frame

먼저 Frame에 대해서 알아보도록하겠습니다.

Frame에서 중요한 것은 SuperView 입니다.
SuperView를 기준으로 정해지기 때문입니다!

<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/97d1a43a-a0b2-485a-32df-eb401589d600/public" width=400></p>


### origin
FirstView의 frame의 origin을 알아보겠습니다.
frame은 SuperView의 (0,0)에서 얼마나 떨어졌는가 입니다!

FirstView의 SuperView는 DefaultView겠죠!
그러면 FirstView의 frame의 origin은 DefaultView의 (0,0)에서 떨어진 정도입니다.

그렇다면 FirstView의 frame의 origin은 (50,100)이 되겠습니다.

<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/35cb58de-7bb4-4bf7-2513-165188702100/public" width=400></p>

똑같이 ScondView의 frame의 origin은 FirstView의 (0,0)에서 떨어진 정도겠죠?

ScondView의 frame의 origin은 (41,47)이 되겠습니다.

<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/1defc0b9-824e-42a0-f397-aae481e4de00/public" width=400></p>


### size(width, height)
이번에는 width와 height에 대해서 알아보겠습니다.

아래는 frame의 width와 height입니다.
이거는 우리가 일반적으로 생각하는 것과 다르지 않습니다!
<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/eeb774d9-6a2a-4ec3-b7f8-455c139c2600/public" width=400></p>

지금은 반듯한 직사각형으로 되어있어서 문제가 없는데...
문제는 이 직사각형이 기울어져 있으면 조금 달라집니다.
아래와 같이 기울어져 있다면 외부에 가상의 사각형을 씌워서 width와 height를 정하게 됩니다.

<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/a0e87656-4eec-41c4-49f1-201775412e00/public" width=400></p>

흠.. 그럼 기울어진 사각형의 실제 width, height는 어떻게 구할까요..?

이럴때 사용하는 것이 bounds라고 합니다..!


## Bounds

### origin
bounds는 자신의 뷰가 기준이 됩니다.
그래서 SuperView와는 상관 없이 (0,0)이 됩니다.
<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/f6ae31d6-a481-4807-d79a-cecabfe7ea00/public" width=400></p>


## size(width, height)

bounds의 size는 자신의 뷰가 기준이 되기 때문에 뷰 자체의 영역을 말합니다.

origin은 변하지 않습니다!
원래의 origin을 가지고 있습니다!

<p align=center><img src="https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/293f1e05-9503-47ba-e6e7-f20f694d5400/public" width=400></p>



