---
title: "안드로이드 성능 기본 원리"
description: ""
coverImage: "/assets/img/2024-07-26-AndroidPerformancebasics_0.png"
date: 2024-07-26 11:51
ogImage: 
  url: /assets/img/2024-07-26-AndroidPerformancebasics_0.png
tag: Tech
originalTitle: "Android Performance basics"
link: "https://medium.com/@choijunseok1997/android-performance-basics-98c2a099b5e5"
---


에뮬레이터는 실제 기기를 대변하지 않습니다. 호스트 OS와 동일한 리소스를 공유합니다. 디버그 빌드는 다시 설치하지 않고 변경 사항을 적용하며 디버거와 함께 작동합니다. 성능 오버헤드로 앱이 불안정해지며 때로는 잘 실행되기도 합니다. 따라서 물리적 기기의 릴리스 버전과 테스트를 반드시 진행해야 합니다.

성능을 확인하는 방법

- 수동 디버그 및 프로파일러
- 시스템 추적
- 자동화

# 📌 수동 검사를 할 수 있어요!

<div class="content-ad"></div>

로그캣을 확인해 보세요. 끊김이 있을 때.

- ` package:mine tag:Choreographer

위의 내용을 간단히 분석해보면 1초에 90프레임을 렌더하는 기기가 있다고 가정해봅시다. 49프레임을 건너뛰면 49/90 x 1초(0.54초)의 끊김이 발생합니다. 그리고 0.7초보다 크면 "동결 프레임"이 발생합니다.

- ` package:mine tag:OpenGLRenderer

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-26-AndroidPerformancebasics_0.png)

렌더링하는 데 걸린 시간이 영상 프레임 시간 대비 대폭 증가했습니다 (초당 60프레임 기준으로 약 16밀리초).

5초가 넘어가면, 유명한 ANR 대화상자가 나타납니다.

왜 그럴까요, Davey?


<div class="content-ad"></div>

안드로이드 시스템의 "Davey!" 알림이 Dave Burke 부사장을 찬양하는 것이라는 소문이 있어요!