---
title: "LiveData와 MutableLiveData 최신 비교 어떤 것을 사용할까"
description: ""
coverImage: "/assets/img/2024-07-10-LiveDatavsMutableLiveData_0.png"
date: 2024-07-10 01:28
ogImage:
  url: /assets/img/2024-07-10-LiveDatavsMutableLiveData_0.png
tag: Tech
originalTitle: "LiveData vs MutableLiveData"
link: "https://medium.com/@kaurparam.30jeet31/livedata-vs-mutablelivedata-df11d4d8da19"
isUpdated: true
---

**What is LiveData?**

LiveData is an Observable Data Holder class. It observes changes in its data and informs its observers about these changes.

LiveData is a part of Jetpack components and is aware of the lifecycle. It notifies its observers about changes only when the lifecycle owner is active. If the lifecycle owner is not in memory, LiveData stops notifying about changes.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

라이브데이터에는 두 가지 유형이 있어요:

1.) LiveData

2.) MutableLiveData

### LiveData 주요 포인트:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

위의 코드는 LiveData의 새 인스턴스를 만들어내며, 제네릭 타입으로 String을 사용하고 MutableLiveData로 초기화됩니다.

LiveData는 감시자들이 그 값을 수정하는 것을 허용하지 않습니다. 그저 값이 변했음을 알릴 뿐이며, 그에 따라 원하는 조치를 취할 수 있도록 합니다. 감시자들은 변경 사항을 관찰할 수만 있고 값을 수정할 수는 없습니다. 따라서 외부 감시자들에게는 항상 LiveData를 변경할 수 없는 형태로 사용해야 합니다.

이제 생각하고 계실 것입니다. LiveData가 알림을 보내는 변경 사항은 누가 가져올까요? 데이터의 실제 변경 사항은 다른 MutableLiveData 변수로 처리됩니다. MutableLiveData 변수는 데이터를 업데이트하는 리소스에게 노출되며, LiveData를 업데이트하는 데 사용됩니다. LiveData가 업데이트되면 감시자들에게 데이터가 변경되었음을 알리고 UI를 업데이트하거나 사용자 정의 로직을 실행할 수 있습니다.

LiveData는 스레드 안전성을 가지고 있어서, 메인 스레드에서 안전하게 액세스할 수 있습니다.

## MutableLiveData 주요 포인트:

```kotlin
private val _text = MutableLiveData<String>("초기값")
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

**위 코드는 MutableLiveData의 제네릭 타입을 String으로 설정하고 값으로 "초기값"을 초기화하는 새로운 MutableLiveData 인스턴스를 생성합니다.**

**MutableLiveData는 값을 변경할 수 있는 관찰자(observer)를 허용합니다. 이것이 외부 관찰자가 값 변경을 방지하기 위해 일반적으로 비공개로 유지되는 이유입니다.**

**MutableLiveData는 데이터의 읽기와 쓰기를 모두 허용하는 LiveData의 하위 클래스입니다. MutableLiveData는 setValue 및 postValue 메서드를 추가하여 저장된 값을 수정하는 기능을 확장합니다.**

**setValue는 UI를 업데이트하거나 실시간 변경 내용을 반영하기 위해 메인 스레드에서 호출해야 합니다. postValue는 백그라운드 스레드에서 호출할 수 있습니다. 네트워크 작업은 일반적으로 백그라운드 스레드에서 수행되므로 postValue()를 사용하여 UI 업데이트가 메인 스레드에서 수행되도록 할 수 있습니다.**

**언더스코어 접두사는 이 속성이 내부적으로만 사용되는 것을 나타내는 일반적인 네이밍 규칙입니다.**

## 추천 사용법

**ViewModel에서 데이터 소스로부터의 데이터 업데이트에는 내부적으로 MutableLiveData를 사용하고, LiveData를 관찰자(observer)에 노출시켜 직접적인 수정을 방지해야 합니다.**

## LiveData에 관찰자(observer) 추가 방법

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이것은 LiveData에 옵저버를 추가하는 샘플 구문입니다.

```kotlin
/**
 * @param this is the Lifecycle owner. If the Lifecycle owner is not in memory, then LiveData will stop notifying changes in data.
 *
 * @param observer is the lambda where you can write your custom logic that you want to get executed whenever there is any change in LiveData myViewModel.text
 *
 * @param updatedValue is the new value of LiveData.
 */

liveData.observe(LifecycleOwner, Observer { updatedValue ->
    // Custom code to be executed when LiveData changes.
})
```

## Full Code Example

MyViewModel.kt

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```kotlin
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class MyViewModel : ViewModel() {

    /**
     * apply 함수는 MutableLiveData 인스턴스가 생성된 직후에 즉시 구성하는 데 사용됩니다.
     *
     * apply 블록 내에서 MutableLiveData 인스턴스의 초기 값이 "안녕, 세상!"으로 설정됩니다.
     */
    private val _text = MutableLiveData<String>().apply {
        value = "안녕, 세상!"
    }

    val text: LiveData<String> = _text

    // LiveData를 업데이트하는 함수
    fun updateText(newText: String) {
        _text.value = newText
    }
}
```

MainActivity.kt

```kotlin
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.Observer
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    // ViewModel에 대한 참조 가져오기
    private val myViewModel: MyViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        /**
         * @param this는 라이프사이클 소유자입니다. ViewModel은 라이프사이클을 인식하므로 ViewModelProvider에게 라이프사이클 소유자가 누구인지 알려주어야 합니다.
         */
        myViewModel = ViewModelProvider(this).get(MyViewModel::class.java)

        /**
         * @param this는 라이프사이클 소유자인 MainActivity입니다. 이렇게 함으로써 LiveData에게 MainActivity가 활성 상태인 동안 변경 사항을 관찰하도록 알려줍니다. 사용자에게 더 이상 MainActivity가 표시되지 않는다면 변경 사항을 알리는 것은 의미가 없습니다.
         * @param observer는 LiveData인 myViewModel.text에 변경 사항이 있을 때 실행하고자 하는 사용자 정의 논리를 작성할 수 있는 방법입니다.
         */
        myViewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })

        // LiveData를 업데이트하도록 버튼에 클릭 리스너 설정
        button.setOnClickListener {
            myViewModel.updateText("텍스트 업데이트됨!")
        }
    }
}
```

만약 이 글이 유용하다고 느끼셨다면 팔로우해 주세요. LinkedIn과 GitHub에서도 저를 만날 수 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 건강하고 큰 꿈을 꾸며 계속 진행해보세요 :)
