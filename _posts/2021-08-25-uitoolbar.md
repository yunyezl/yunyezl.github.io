<img src="https://images.velog.io/images/yunyezl/post/eade4e07-2f5a-4b57-8c6b-a4043999420b/image.png" width="300" height="100">

애플 기본 날씨앱을 클론 코딩 하면서, UIToolbar를 사용해 볼 기회가 생겼는데요.
이미지에서 하단 pageControl를 담고 있는 부분을 UIToolbar를 이용해서 구현하였습니다. UIToolbar 안에 들어갈 버튼들은 CustomView 로 만들어 준 상태였습니다.
정상적으로 구현된 것처럼 보였지만, 콘솔창에

![](https://images.velog.io/images/yunyezl/post/d7dab1b3-8c50-4af5-9c0b-bb50c7cb53f3/image.png)

이런 식으로 layout에 대한 warning message가 잔뜩 찍혔습니다. [wtf autolayout](https://www.wtfautolayout.com/) 사이트에 워닝 메시지를 넣고 에러를 파악해보려 했지만, 툴바 하나 넣었다고 무수히 쏟아지는 워닝 메시지들을 이해할 수 없었습니다 🥲 
돌아가긴 하니까, 흐린 눈을 할까 했지만 아무래도 찝찝하니까..

기존에 저는 아래와 같이 toolbar를 선언해주었고, 기본 백그라운드를 없애기 위해 빈 UIImage()를 셋팅해주었습니다.

~~~swift
private let pageToolbar = UIToolbar().then {
        $0.setBackgroundImage(UIImage(), forToolbarPosition: .any, barMetrics: .default)
}
 ~~~

그러던 중 [스택 오버 플로우](https://stackoverflow.com/questions/54284029/uitoolbar-with-uibarbuttonitem-layoutconstraint-issue)에서 한 가지 해결책을 찾을 수 있었는데요. 
CGRect값을 별도로 선언해주지 않았기 때문에 나타난 에러였습니다. 아래와 같이 frame값을 지정해주니 해결되었습니다.
~~~swift
private let pageToolbar = UIToolbar(frame: CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: 60)).then {
        $0.setBackgroundImage(UIImage(), forToolbarPosition: .any, barMetrics: .default)
}
~~~

이해가 안가는 점은 빈 toolbar를 선언한 뒤 leading, trailing, bottom 제약조건과 height값을 모두 설정해주었는데 왜 에러가 났는지를 모르겠습니다. 
여러 자료를 찾아보니, 이러한 이슈가 저한테만 일어난 게 아닌 듯 했습니다. 다음과 같은 답변이 있더라고요 ..

![](https://images.velog.io/images/yunyezl/post/9020fb03-8358-4018-9b92-d615c18751dc/image.png)

.. 🤣

[애플 개발자 포럼에 올라온 관련 스레드](https://developer.apple.com/forums/thread/121474)를 남기며 글 마칩니다.
