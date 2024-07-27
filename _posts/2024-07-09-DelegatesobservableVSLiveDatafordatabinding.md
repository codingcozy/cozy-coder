---
title: "Delegatesobservable와 LiveData 데이터를 바인딩할 때 어느 것이 더 좋을까"
description: ""
coverImage: "/assets/img/2024-07-09-DelegatesobservableVSLiveDatafordatabinding_0.png"
date: 2024-07-09 10:43
ogImage: 
  url: /assets/img/2024-07-09-DelegatesobservableVSLiveDatafordatabinding_0.png
tag: Tech
originalTitle: "Delegates.observable VS LiveData for data binding"
link: "https://medium.com/proandroiddev/delegates-observable-vs-livedata-for-data-binding-2b4777c732cb"
---


자신의 앱의 구체적인 요구사항과 맥락에 따라 안드로이드에서 데이터 바인딩에 Delegates.observable과 LiveData 중 어느 것을 선택할지가 달라집니다. 두 가지 방법에는 각각의 장점이 있으며 서로 다른 시나리오에 적합합니다. 자세히 비교해보겠습니다:

# Delegates.observable

## 장점:

<div class="content-ad"></div>

**장점:**

- 간편함: Delegates.observable은 직관적이고 사용하기 쉽습니다. 이를 사용하면 값이 변경될 때마다 콜백을 트리거하는 속성을 정의할 수 있습니다.
- Kotlin 표준 라이브러리: Kotlin 표준 라이브러리의 일부이므로 추가 종속성이 필요하지 않습니다.
- 유연성: Android 특정 구성 요소뿐만 아니라 모든 Kotlin 클래스에서 사용할 수 있습니다.

**단점:**

- 라이프사이클 인식 미포함: Android 라이프사이클 이벤트를 처리하지 않으므로 메모리 누수가 발생하거나 비활성 상태의 구성 요소로 업데이트가 전송될 수 있습니다.
- 수동 알림: 더 복잡한 데이터 바인딩 시나리오에서는 콜백을 사용하여 변경 사항을 수동으로 알려줘야 하므로 불편할 수 있습니다.

## 예시:

<div class="content-ad"></div>

```kotlin
class User {
    var name: String by Delegates.observable("John Doe") { _, oldValue, newValue ->
        println("이름이 $oldValue에서 $newValue로 변경되었습니다.")
    }
}
```

# LiveData

## 장점:

- 라이프사이클 인식: LiveData는 라이프사이클을 감지하여 활성화된 옵저버들에게만 업데이트되도록 설계되어 메모리 누수와 불필요한 업데이트를 방지합니다.
- 아키텍처 컴포넌트와 통합: LiveData는 ViewModel과 같은 Android 아키텍처 컴포넌트와 완벽하게 통합되어 UI 관련 데이터를 효과적으로 관리할 수 있는 견고한 프레임워크를 제공합니다.
- 스레드 안전성: LiveData는 스레드 안전하며 서로 다른 스레드에서 사용할 수 있도록 설계되었습니다.


<div class="content-ad"></div>

## 단점:

- 의존성: AndroidX 라이브러리를 필요로 하므로 프로젝트에 추가적인 의존성이 필요합니다.
- 더 복잡함: Delegates.observable에 비해 설정하는 과정이 조금 더 복잡할 수 있습니다.

## 예시:

```kotlin
class UserViewModel : ViewModel() {
    private val _name = MutableLiveData<String>().apply { value = "John Doe" }
    val name: LiveData<String> get() = _name

    fun updateName(newName: String) {
        _name.value = newName
    }
}

// 액티비티나 프래그먼트에서
viewModel.name.observe(this, Observer { newName ->
    textView.text = newName
})
```

<div class="content-ad"></div>

# 각각 사용 시기

## Delegates.observable를 사용해야 하는 경우:

- 간단하고 가벼운 솔루션이 필요할 때.
- Android에 특화되지 않은 컨텍스트에서 작업하거나 라이프사이클이 관련되지 않은 ViewModel 내에서 작업할 때.
- 추가적인 종속성을 추가하고 싶지 않을 때.

## LiveData를 사용해야 하는 경우:

<div class="content-ad"></div>

- 옵저버의 라이프사이클 상태에 따라 업데이트를 자동으로 관리하는 라이프사이클 인식형 솔루션이 필요합니다.
- ViewModel, Room 등 다른 Android 아키텍처 구성요소와 통합해야 합니다.
- 데이터를 안전하게 관리하고 관찰하기 위한 쓰레드 안전한 방법이 필요합니다.

## 결론

Delegates.observable과 LiveData는 각각 사용 사례가 있습니다. Android 애플리케이션에서 UI 구성요소 및 라이프사이클 관리와 관련된 경우에는 LiveData가 라이프사이클을 인식하고 다른 아키텍처 구성요소와 원활하게 통합되는 점 때문에 일반적으로 더 나은 선택입니다. 그러나 더 간단한 시나리오나 Android가 아닌 환경에서는 Delegates.observable이 적합하고 효율적인 옵션일 수 있습니다.

**추가 자료:**

<div class="content-ad"></div>

- LiveData 개요
- Android Data Binding 라이브러리