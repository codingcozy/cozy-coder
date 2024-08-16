---
title: "안드로이드 구성 변경과 라이프사이클에 미치는 영향 효과 및 해결 방법"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-07 20:04
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Android Configuration Changes and their Impact on Lifecycle: Effects and Solutions"
link: "https://medium.com/@cugo15/android-configuration-changes-and-their-impact-on-lifecycle-effects-and-solutions-190a2359c6ec"
isUpdated: true
---





앱이 세로 모드에서 가로 모드로 전환되거나 누군가가 전화의 시계 설정이나 다른 전화 설정을 변경할 때, 앱은 마치 "뭐야, 잠깐! 설정 변경 알림!" 하는 것과 같아요.

예를 들어서요:

결제 중에 신용카드 정보를 입력하고 있다고 상상해봐요. 갑자기 전화를 90도 돌리거나 전화 설정을 바꾸기로 결정하면, 해당 과정은 onDestroy() 메소드를 호출하고 onCreate()가 다시 호출되어 활동을 다시 생성하게 될 거예요. 결과적으로, 입력한 신용카드 정보는 마술사 토끼보다 빨리 마법사처럼 사라질 수도 있어요.

이런 순간들에서, 전화가 방향을 바꾸거나 설정이 조정될 때 데이터가 손상되지 않도록 하려면 onSaveInstanceState() 및 onRestoreInstanceState() 메소드를 사용하는 것이 매우 중요해요. 이러한 메소드들은 변화가 발생하기 전에 앱이 상태를 저장하고, 변경된 내용을 복원할 수 있도록 도와줘요. 이를 통해 입력한 정보를 다시 입력하지 않고도 순조롭게 정보를 복원할 수 있게 됩니다.

<div class="content-ad"></div>

이런 상황에서는 다음과 같은 라이프사이클 메서드가 순차적으로 호출됩니다:

- 애플리케이션이 처음으로 시작될 때:
  - onCreate()
  - onStart()
  - onResume()
- 화면 방향이 변경될 때 (세로 -> 가로):
  - onPause()
  - onStop()
  - onSaveInstanceState()
  - onDestroy()
  - onCreate()
  - onStart()
  - onRestoreInstanceState()
  - onResume()

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    // onSaveInstanceState는 활동이 일시 중지되거나 다시 생성될 때 사용할 데이터를 저장하는 데 사용됩니다.
    
    // txtCardNumber TextView 구성 요소에서 카드 번호를 검색하여 문자열로 저장합니다.
    val text = binding.txtCardNumber.text.toString()
    
    // "CardNumber" 키와 함께 카드 번호를 outState Bundle 객체에 저장합니다.
    outState.putString("CardNumber", text)
}

override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)
    // onRestoreInstanceState는 onSaveInstanceState로 저장한 데이터를 복원하는 데 사용됩니다.
    
    // savedInstanceState에서 "CardNumber" 키로 데이터를 검색합니다.
    val text = savedInstanceState.getString("CardNumber")
    
    // 복원된 카드 번호를 txtCardNumber TextView 구성 요소에 설정합니다.
    binding.txtCardNumber.text = text
}
```