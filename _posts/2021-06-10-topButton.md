---
title:  "[iOS] 탑버튼 만들기"
excerpt: "마켓컬리 클론코딩"

categories:
  - iOS
tags:
  - iOS
last_modified_at: 2021-06-10T04:10:00-05:00
---
![image](https://user-images.githubusercontent.com/69361613/121549058-7b4c8300-ca48-11eb-8f2a-e423e1d25ad6.png)

일단 UIButton 만들고.. 슈퍼뷰 기준으로 레이아웃을 잡아 준다.

그런 다음에 Button 이벤트를 만들어줘야되는데, 맨날 IBoutlet 연결해서 해주다가 코드로 버튼 이벤트 만드는 건 처음이어서 구글링을 해봤는데,

![image](https://user-images.githubusercontent.com/69361613/121549143-8f908000-ca48-11eb-91a5-272d20bd26c9.png)

요런식으로 하면 된다.

저 topButtonClicked는 objc 키워드를 붙여서 함수를 따로 만들어주면 된당..

![image](https://user-images.githubusercontent.com/69361613/121549177-96b78e00-ca48-11eb-9a64-92dbfc6111a5.png)

scrollToRow 속성을 이용해서 0번째 로우, 0번째 섹션으로 이동한다. 생각보다 간단하다 !

그런데데 마켓컬리 뷰를 보면.. topButton이 항상 둥둥 떠있는게 아니라 메인 테이블뷰를 어느정도 내렸을 때만 쉭~ 나타난다.

처음에 펜제스처를 만지작 거리다가 펜제스처가 아닌 스크롤이 됐을 때 현재 좌표값을 받아서 계산해주는 방식으로 해야되겠다고 깨달았다..!

그래서 어떻게 스크롤이 될 때마다 현재 좌표값을 가져오느냐? UIScrollViewDelegate의 scrollViewDidScroll을 통해 받아올 수 있다!

나는 다음과 같이 구현해주었다.

![image](https://user-images.githubusercontent.com/69361613/121549268-adf67b80-ca48-11eb-87d1-c90d8e84d4b4.png)

좌표가 어느 정도 아래로 내려갔을 때.. 대충 100정도 이상 내려갔을 때 약간의 애니메이션을 첨가해서 topButton을 불러온다. (topButton은 생성 될 때 isHidden = true를 통해 숨겨주었기 때문에, false로 바꿔주어야 함!)

그런 다음에, 반대로 위로 올라가다가 탑 쪽에 붙었을 때는 topButton의 alpha값을 0으로 변경해서 요 놈을 서서히 숨겨준다..

그러면 요로코롬 멋진 애니메이션과 함께 탑버튼 구현 성공 ~


![화면 기록 2021-05-31 오후 11 15 06 mov](https://user-images.githubusercontent.com/69361613/121550190-7b00b780-ca49-11eb-91ff-022011c559a4.gif)
