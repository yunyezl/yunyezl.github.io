---
title:  "[week6] 리눅스 환경에서 mariaDB 실습하기"
excerpt: "2020-2 SSWU Database programming 학습 일지"

categories:
  - database programming
tags:
  - database
  - programming
  - study
  - php
last_modified_at: 2020-10-05T23:49:00-05:00
---


## 새로 배운 내용
- limit 키워드를 통해 select 문의 개수를 조절할 수 있음.
- readonly 키워드를 통해 입력값의 수정을 제한할 수 있음.
- vscode에서 server에 접속하는 방법. 너무 신기하고 편했다.

### 문제가 생기거나 고민한 부분
- 새로운 기능을 추가할때, 검색하지 않을 때는 테이블이 보이지 않고 검색할 때만 보일 수 없을까를 고민하다가,
이전 수업 때 데이터베이스에 추가한 목록들을 선택하면 동적으로 title과 description의 값이 바뀌도록 구현한 것이 떠올라 해당 기능을 응용하여 구현할 수 있었다. 
- 수업을 한번에 듣고 실습한게 아니라서 중간에 vscode를 끄고 서버에 재접속할 일이 있었는데, 다시 vscode를 여니까
![image](https://user-images.githubusercontent.com/69361613/95403098-41246200-094c-11eb-9c04-794149810ecb.png)  
이렇게 오류가 나왔다. retry를 해도 반복적으로 동일한 오류가 발생해서 그냥 f1을 눌러서 호스트를 다시 새로 추가해서 접속하는 방식으로 하니까 되긴 됐다.

### 참고 사이트
vscode 단축키 https://demun.github.io/vscode-tutorial/shortcuts/

### 회고
#### +
>  이번 수업은 배웠던 걸 총망라해서 복습하는 느낌이여서 좋았다. 세팅하느라 시간이 많이 걸려서 분량도 많아 힘들었지만 중요한 주차였던 것 같다. 
금요일이 되기 전 강의 듣기를 성공했다.
#### -
> php는 여전히 낯설고 어렵다. 반복적인 코드가 많아서 리팩토링을 하고싶은데 php에 익숙하지 않아 힘들다.
#### !
> atom에서 vscode로 넘어가서 너무 좋았다. 기존에 사용하고 있던 편집기라 익숙했다. atom으로 수업을 들었을 때보다 생산성이 훨씬 나아진 것 같다.

[구동 영상](https://youtu.be/gHkvJ1xkHeg)
