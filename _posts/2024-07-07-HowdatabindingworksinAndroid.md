---
title: "안드로이드에서 데이터 바인딩이 작동하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowdatabindingworksinAndroid_0.png"
date: 2024-07-07 23:15
ogImage: 
  url: /assets/img/2024-07-07-HowdatabindingworksinAndroid_0.png
tag: Tech
originalTitle: "How data binding works in Android"
link: "https://medium.com/@sandeepkella23/how-data-binding-works-in-android-7e9bbdb47a0d"
---


![HowdatabindingworksinAndroid_0.png](/assets/img/2024-07-07-HowdatabindingworksinAndroid_0.png)

당신은 마스터 퍼피터라고 상상해봐요. 손으로 조작하는 대신에 스크립트(데이터)에 기반하여 퍼피터(당신의 UI 요소들)들이 행동하는 여러 퍼피터들을 가지고 있다고 상상해봐요. findViewById와 같은 손 끝으로 유아이를 직접 제어하는 대신에, 당신은 퍼피터들이 스크립트의 변경에 자동적으로 반응하는 마법 시스템을 만들었다고 생각해봐요. 이 마법 시스템을 데이터 바인딩이라고 해요.

# 단계 1: 무대 설정하기

먼저, 안드로이드 프로젝트에서 데이터 바인딩을 활성화해야 해요. 이건 마치 퍼피트 극장을 준비하는 것과 비슷해요. 이걸 build.gradle 파일에서 하게 돼요:

<div class="content-ad"></div>

안녕하세요! 타로 전문가인 저는 여러분을 위해 한국어로 번역해드리겠습니다.

# 단계 2: 스크립트와 퍼펫 준비하기

이제 XML 레이아웃을 `layout` 태그로 감싸야 합니다. 이렇게 하면 Android에게 이 레이아웃이 데이터 바인딩에 준비되었음을 알리게 됩니다. 이 태그 안에서 UI 구성 요소(퍼펫)가 바인딩될 데이터 변수(스크립트)를 정의합니다:

```js
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="user"
            type="com.example.User"/>
    </data>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.name}"/>
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.age}"/>
    </LinearLayout>
</layout>
```

<div class="content-ad"></div>

# 스텝 3: 스크립트 작성

지금, 여러분이 스크립트를 나타내는 데이터 클래스가 필요합니다. 이 클래스는 귀하의 퍼펫들이 반응할 데이터를 보유합니다. 간단한 User 클래스를 만들어봅시다:

```kotlin
data class User(val name: String, val age: Int)
```

# 스텝 4: 퍼펫을 생동감 있게 만들기

<div class="content-ad"></div>

당신의 활동이나 조각에서는 데이터를 뷰에 바인딩합니다. 여기서 마법이 일어납니다 — 스크립트를 인형에 연결하여 공연을 시작할 수 있도록 만드는 것이죠:

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        val user = User("John Doe", 25)
        binding.user = user
    }
}
```

# 단계 5: 스크립트 변경에 반응하기

데이터 바인딩의 가장 멋진 점 중 하나는 스크립트가 변경될 때 자동으로 업데이트된다는 것입니다. 이것을 달성하기 위해 데이터 클래스를 observable로 만들 수 있습니다. Observable을 사용하여 간단히 이렇게 할 수 있어요:

<div class="content-ad"></div>

```js
class User : BaseObservable {
    @get:Bindable
    var name: String by Delegates.observable("John Doe") { _, _, _ ->
        notifyPropertyChanged(BR.name)
    }

    @get:Bindable
    var age: Int by Delegates.observable(25) { _, _, _ ->
        notifyPropertyChanged(BR.age)
    }
}
```

위 예시에서, 이름이나 나이가 변경될 때마다 notifyPropertyChanged 메서드가 데이터 바인딩 시스템에게 이러한 속성에 바인딩된 UI 구성 요소를 업데이트하도록 알려줍니다.

# 모두 묶어서

마무리로, 데이터 바인딩이 퍼펫티어 비유와 어떻게 작동하는지 요약해 봅시다:


<div class="content-ad"></div>

"무대 설정: build.gradle에서 데이터 바인딩 활성화하기.
대본과 인형 준비: XML 레이아웃을 `layout` 태그로 감싸고 데이터 변수를 정의합니다.
대본 작성: 데이터를 보유하는 데이터 클래스를 생성합니다.
인형들을 생동시키기: 데이터를 액티비티나 프래그먼트의 뷰에 바인딩합니다.
대본 변경에 반응하기: UI를 자동으로 업데이트하도록 데이터 클래스를 Observable로 만듭니다.

데이터 바인딩은 UI를 데이터와 동기화시키는 지루한 부분을 자동화하여 놀라운 앱 경험을 만드는 데 집중할 수 있도록 도와줍니다.

# 추가 자료

- 공식 데이터 바인딩 가이드
- 데이터 바인딩 라이브러리 소개
- 양방향 데이터 바인딩"

<div class="content-ad"></div>

이제 당신의 데이터 스크립트의 조종으로 편안하게 무대에 오르도록 UI를 춤꾼들로 만들어 보세요! 🎭📜