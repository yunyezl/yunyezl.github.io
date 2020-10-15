---
title:  "[Android] 안드로이드 이벤트 처리, onTouch() vs onTouchEvent()"
excerpt: "모바일 소프트웨어"

categories:
  - android
tags:
  - android
  - xml
  - study
  - event handler
last_modified_at: 2020-10-15T23:00:00-05:00
---

# 안드로이드 이벤트 처리

### 1. 콜백메서드 재정의를 이용한 이벤트 처리
* 상호 작용 주체가 View이므로 이벤트 콜백은 주로 View가 제공함.

~~~java
public class HandleEvent1 extends Activity {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		View vw = new MyView(this);
		setContentView(vw);
	}

	class MyView extends View {
		public MyView(Context context) {
			super(context);
		}

		public boolean onTouchEvent(MotionEvent event) {
			super.onTouchEvent(event);
			if (event.getAction() == MotionEvent.ACTION_DOWN) {
				Toast.makeText(HandleEvent.this,"Touch Event Received",
						Toast.LENGTH_SHORT).show();
				return true;
			}
			return false;
		}
	}
}
//*/
~~~

콜백 메서드 재정의를 위해 반드시 상속을 받아야 하고, 모든 이벤트에 대한 콜백이 정의되어 있지 않다는 단점이 있다.  

(콜백 메서드란? http://www.dreamy.pe.kr/zbxe/CodeClip/3768942 참조)

### 2. 리스너를 통한 이벤트 처리  

* 인터페이스 구현 객체를 생성하여 이벤트 처리

~~~java
public class HandleEvent extends Activity {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		View vw = new View(this);
		// 3.리스너 등록
		vw.setOnTouchListener(TouchListener);
		setContentView(vw);
	}

	// 1.리스너 구현 클래스 선언
	class TouchListenerClass implements View.OnTouchListener {
		public boolean onTouch(View v, MotionEvent event) {
			if (event.getAction() == MotionEvent.ACTION_DOWN) {
				Toast.makeText(HandleEvent.this,"Touch Event Received",
						Toast.LENGTH_SHORT).show();
				return true;
			}
			return false;
		}
	}

	// 2.리스너 객체 생성
	TouchListenerClass TouchListener = new TouchListenerClass();
}
//*/
~~~ 
인터페이스는 어떠한 클래스에서도 구현이 가능하다. 위처럼 별도의 클래스를 선언하여 인터페이스를 구현할 수도 있지만 Activity가 리스너를 구현할 수도 있고, 뷰가 리스너를 구현할 수도 있다.

* Activity가 리스너를 구현
~~~Java
public class HandleEvent extends Activity implements View.OnTouchListener {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		View vw = new View(this);
		vw.setOnTouchListener(this);
		setContentView(vw);
	}

	public boolean onTouch(View v, MotionEvent event) {
		if (event.getAction() == MotionEvent.ACTION_DOWN) {
			Toast.makeText(this,"Touch Event Received",
					Toast.LENGTH_SHORT).show();
			return true;
		}
		return false;
	}
}
~~~
위의 방식보다 코드는 짧아지지만 구조적으로 바람직하지 못하며 뷰의 독립성이 떨어진다.

* View가 리스너를 구현
~~~java
public class HandleEvent extends Activity {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		MyView vw = new MyView(this);
		vw.setOnTouchListener(vw);
		setContentView(vw);
	}

	class MyView extends View implements View.OnTouchListener {
		public MyView(Context context) {
			super(context);
		}

		public boolean onTouch(View v, MotionEvent event) {
			if (event.getAction() == MotionEvent.ACTION_DOWN) {
				Toast.makeText(HandleEvent.this,"Touch Event Received",
						Toast.LENGTH_SHORT).show();
				return true;
			}
			return false;
		}
	}
}
~~~

* 익명 내부 클래스를 이용하여 이벤트 처리
~~~java
public class HandleEvent extends Activity {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		View vw = new View(this);
		vw.setOnTouchListener(TouchListener);
		setContentView(vw);
	}

	View.OnTouchListener TouchListener = new View.OnTouchListener() {
		public boolean onTouch(View v, MotionEvent event) {
			if (event.getAction() == MotionEvent.ACTION_DOWN) {
				Toast.makeText(HandleEvent.this,"Touch Event Received",
						Toast.LENGTH_SHORT).show();
				return true;
			}
			return false;
		}
	};
}
~~~

* 익명 내부 클래스의 임시 객체 생성
~~~java
public class HandleEvent extends Activity {
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		View vw = new View(this);
		vw.setOnTouchListener(new View.OnTouchListener() {
			public boolean onTouch(View v, MotionEvent event) {
				if (event.getAction() == MotionEvent.ACTION_DOWN) {
					Toast.makeText(HandleEvent.this,"Touch Event Received",
							Toast.LENGTH_SHORT).show();
					return true;
				}
				return false;
			}
		});
		setContentView(vw);
	}
}
~~~

### 3. onTouch() vs onTouchEvent()   
공부를 하다보니 한 가지 의문점이 생겼다. onTouch는 뭐고, onTouchEvent는 뭐지? 보아하니 콜백 메서드를 재정의하여 사용할 땐 onTouchEvent를 쓰고, 리스너를 사용할 때는 onTouch를 사용하는 것 같았다. 이해가 부족하여 자료를 찾아보았다.

참고자료 : https://stackoverflow.com/questions/5002049/ontouchevent-vs-ontouch

**구현 방식에서의 차이**    
onTouch()를 이용하기 위해서는 다음과 같이 세 가지의 과정을 모두 수행해야한다. 
1. onTouchListener를 implement할 클래스 생성
2. onTouch()를 재정의 
3. 이벤트를 처리할 뷰에 setOnTouchListener를 호출

반면 onTouchEvent()를 사용할 경우에는 1번과 3번의 과정이 필요없고, 단지 onTouchEvent()를 재정의하면 된다. 이러한 특징은 위의 코드들에서 확인 가능하다.  

* onTouch()를 사용한 Listener 등록 방식의 경우 View.onTouchListener를 implement한 클래스에 onTouchEvent()를 재정의하고, 뷰를 인스턴스화한 후 해당 뷰에서 setOnTouchListener를 호출하면서 리스너 객체를 등록해주는 과정을 거친다.

* onTouchEvent()를 사용한 Callback Method 재정의 방식의 경우 setOnTouchListener를 호출하여 별도로 리스너를 등록하는 과정이 없다. 단지 onTouchEvent()를 재정의할 클래스를 생성하고 이를 인스턴스화하면 끝이다.  


**동작 방식에서의 차이**

* onTouch()는 뷰, 뷰그룹, 액티비티에서 작동한다. 해당 메서드는 두 개의 인자를 전달받는데(onTouch(View v, MotionEvent e)), 이를 통해 액티비티나 뷰그룹 내의 각기 다른 뷰들에 대한 이벤트를 구분할 수 있다. 
* onTouchEvent()의 경우 MotionEvent만을 인자로 전달받는다. 그래서 이러한 메서드는 onTouchEvent()를 재정의하고 있는 뷰 혹은 파생된 뷰 내에서만 사용할 수 있다.

결론 : onTouch()는 제네릭하지만, onTouchEvent()는 뷰에 종속적이다.
