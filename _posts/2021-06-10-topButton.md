---
title:  "[iOS] 탑버튼 만들기"
excerpt: "마켓컬리 클론코딩"

categories:
  - iOS
tags:
  - iOS
last_modified_at: 2021-06-10T04:10:00-05:00
---

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91be7f4f-c11c-4130-b0c7-fbce2b08170f/buttonFloatingbtn2x.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91be7f4f-c11c-4130-b0c7-fbce2b08170f/buttonFloatingbtn2x.png)

일단 UIButton 만들고.. 슈퍼뷰 기준으로 레이아웃을 잡아 준다.

그런 다음에 Button 이벤트를 만들어줘야되는데, 맨날 IBoutlet 연결해서 해주다가 코드로 버튼 이벤트 만드는 건 처음이어서 구글링을 해봤는데,

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4139029-dcf0-436f-b59a-92bf68d8d53b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4139029-dcf0-436f-b59a-92bf68d8d53b/Untitled.png)

요런식으로 하면 된다.

저 topButtonClicked는 objc 키워드를 붙여서 함수를 따로 만들어주면 된당..

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/714bc33b-8ce1-495e-8e90-5b274fcd1ce9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/714bc33b-8ce1-495e-8e90-5b274fcd1ce9/Untitled.png)

scrollToRow 속성을 이용해서 0번째 로우, 0번째 섹션으로 이동한다. 생각보다 간단하다 !

긍데 마켓컬리 뷰를 보면.. topButton이 항상 둥둥 떠있는게 아니라 메인 테이블뷰를 어느정도 내렸을 때만 쉭~ 나타난다.

처음에 펜제스처를 만지작 거리다가 펜제스처가 아닌 스크롤이 됐을 때 현재 좌표값을 받아서 계산해주는 방식으로 해야되겠다고 깨달았다..!

그래서 어떻게 스크롤이 될 때마다 현재 좌표값을 가져오느냐? UIScrollViewDelegate의 scrollViewDidScroll을 통해 받아올 수 있다!

나는 다음과 같이 구현해주었다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b27b53d4-fdce-4fd0-b62d-c8c3377ded87/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b27b53d4-fdce-4fd0-b62d-c8c3377ded87/Untitled.png)

좌표가 어느 정도 아래로 내려갔을 때.. 대충 100정도 이상 내려갔을 때 약간의 애니메이션을 첨가해서 topButton을 불러온다. (topButton은 생성 될 때 isHidden = true를 통해 숨겨주었기 때문에, false로 바꿔주어야 함!)

그런 다음에, 반대로 위로 올라가다가 탑 쪽에 붙었을 때는 topButton의 alpha값을 0으로 변경해서 요 놈을 서서히 숨겨준다..

그러면 요로코롬 멋진 애니메이션과 함께 탑버튼 구현 성공 ~

[https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b74cc390-34b8-46fa-b600-d3be4c44de24/__2021-05-31__11.15.06.mov](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b74cc390-34b8-46fa-b600-d3be4c44de24/__2021-05-31__11.15.06.mov)
