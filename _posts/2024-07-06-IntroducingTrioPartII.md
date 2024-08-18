---
title: "트리오 소개  2부"
description: ""
coverImage: "/assets/img/2024-07-06-IntroducingTrioPartII_0.png"
date: 2024-07-06 11:14
ogImage:
  url: /assets/img/2024-07-06-IntroducingTrioPartII_0.png
tag: Tech
originalTitle: "Introducing Trio | Part II"
link: "https://medium.com/airbnb-engineering/introducing-trio-part-ii-fe836013a798"
isUpdated: true
---

## 에어비앤비 Android 앱에서 컴포즈 기반 아키텍처를 만드는 방법, 매버릭과 함께하는 두 번째 이야기

![이미지](/assets/img/2024-07-06-IntroducingTrioPartII_0.png)

저자: Eli Hart, Ben Schwab, and Yvonne Wong

이 시리즈의 이전 게시물에서 우리는 안드로이드용 Jetpack Compose 화면 아키텍처 프레임워크인 Trio를 소개했습니다. Trio의 장점 중 일부는 다음과 같습니다:

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

- 복잡한 앱에서 모듈 간 통신 시 안전한 타입을 보장합니다.
- ViewModel의 사용 및 공유 방법과 화면 간 인터페이스에 대한 기대를 명확히 합니다.
- 안정적인 스크린샷 및 UI 테스트를 가능하게 하며 간단한 네비게이션 테스트도 수행할 수 있습니다.
- Jetpack용 에어비앤비의 오픈 소스 상태 관리 라이브러리 'Mavericks'와 호환됩니다 (Trio는 Mavericks의 기반 위에 구축되었습니다).

Trio에 대해 다시 보거나 이 프레임워크에 대해 처음 알게 되었다면, Part 1부터 시작하세요. Part 1에서는 Compose로의 전환 시 Fragment 기반 아키텍처에서 Trio를 만든 이유에 대한 개요를 제공합니다. 또한 Trio 클래스와 UI 클래스와 같은 핵심 프레임워크 개념을 설명합니다.

본 포스트에서는 지금까지 공유한 내용을 보완하고 Trio에서 네비게이션 작업이 어떻게 이루어지는지 알아봅니다. Trio를 설계할 때 네비게이션을 더 간단하고 큰 모듈화된 애플리케이션에 대해 테스트하기 쉽게 만들기 위해 노력했다는 점을 알게 될 것입니다.

## Trio로 네비게이션하기

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

우리 디자인에서의 독특한 접근 방식은 Trios가 ViewModel의 State에 저장된다는 것입니다. 이 State는 스크린이 UI에 노출하는 다른 모든 데이터와 함께 저장됩니다. 예를 들어, 일반적인 사용 사례는 화면 스택을 나타내기 위해 Trios의 목록을 저장하는 것입니다.

```js
data class ParentState(
  @PersistState val trioStack: List<Trio>
) : MavericksState
```

PersistState 주석은 Mavericks의 메커니즘으로, 프로세스의 종료마다 Parcelable State 값을 자동으로 저장하고 복원합니다. 따라서 네비게이션 상태가 유지됩니다. 컴파일 시간 유효성 검사는 State 클래스의 Trio 값이 항상 올바르게 저장되도록 아래와 같이 주석이 달려 있는지 확인합니다.

ViewModel은 이 상태를 제어하며, 새로운 화면을 추가하거나 화면을 꺼내는 함수를 노출할 수 있습니다. ViewModel은 Trios 목록을 직접 제어하기 때문에 화면의 순서를 바꾸거나 여러 화면을 삭제하거나 모든 화면을 클리어하는 등 복잡한 네비게이션 변경도 쉽게 수행할 수 있습니다. 이것은 네비게이션을 매우 유연하게 만듭니다.

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
class ParentViewModel : TrioViewModel {
  fun pushScreen(trio: Trio) = setState {
    copy(trioStack = trioStack + trio)
  }

  fun pop() = setState {
    copy(trioStack = trioStack.dropLast(1))
  }
}
```

부모 Trio의 UI는 State에서 Trio 목록에 액세스하고 Trios를 어떻게, 어디에 배치할지 선택합니다. 최신 Trio를 스택에서 보여주는 방식으로 화면 흐름을 구현할 수 있습니다.

```kotlin
@Composable
override fun TrioRenderScope.Content(state: ParentState) {
  ShowTrio(state.trioStack.last())
}
```

## Navigation을 조정하기

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

Store 클래스 Trio에 Trios를 왜 저장해야 할까요? 대안적인 방법으로는 Compose UI에서 navigator object를 사용할 수 있습니다. 그러나 State에 애플리케이션의 네비게이션 그래프를 표현하는 것은 ViewModel이 데이터와 네비게이션을 한 곳에서 업데이트할 수 있게 해줍니다. 이는 네트워크 요청과 같은 비동기 작업이 완료된 후에 네비게이션 변경을 지연해야 할 때 매우 유용할 수 있습니다. Fragments로는 이 작업을 쉽게 처리할 수 없었으며, Trio의 접근 방식을 통해 네비게이션이 더 간단하고 명시적이며 테스트하기 쉬워진다는 것을 발견했습니다.

이 예시에서는 ViewModel이 UI로부터 "저장 및 종료" 호출을 처리하는 방법을 보여줍니다. 이를 위해 코루틴에서 중단 네트워크 요청을 시작합니다. 요청이 완료되면 Trio 스택을 업데이트하여 화면을 팝업할 수 있습니다. 또한 네트워크 요청 결과에 기반하여 동시에 상태의 다른 값을 원자적으로 수정할 수도 있습니다. 이는 네비게이션과 ViewModel 상태가 동기화되도록 보장하는 데 쉽게 도움이 됩니다.

```js
class CounterViewModel : TrioViewModel {

  fun saveAndExit() = viewModelScope.launch {
    val success = performSaveRequest()

    setState {
      copy(
        trioStack = trioStack.dropLast(1),
        success = success
      )
    }
  }
}
```

![이미지](/assets/img/2024-07-06-IntroducingTrioPartII_1.png)

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

UI 계층구조가 더 복잡해지면, 애플리케이션 UI 계층구조는 ViewModel 및 그 상태의 연쇄로 모델링됩니다. 상태가 렌더링되면, 해당 Compose UI 계층구조가 생성됩니다.

Trio는 중첩된 화면 및 섹션을 포함한 임의의 크기의 UI 요소를 표현하면서, 계층구조에서 다른 Trio들과 통신하는 백업 상태와 메커니즘을 제공할 수 있습니다.

이처럼 ViewModel 상태로 계층구조를 모델링하는 두 가지 추가적인 장점이 있습니다. 하나는 테스트 설정 시 사용자 정의 네비게이션 시나리오를 지정하기가 간단해진다는 것입니다 — 테스트를 위해 원하는 네비게이션 상태를 쉽게 생성할 수 있습니다.

또 다른 장점은 네비게이션 계층구조가 Compose UI에서 분리되어 있기 때문에, 미리 필요한 Trios를 초기화함으로써 예상된 Trios를 미리로드할 수 있다는 것입니다. 이로써, 화면 사전로드를 통해 성능 최적화가 훨씬 간단해졌습니다.

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

매버릭스 상태는 일반적으로 Trio와 같은 라이프사이클이 있는 복잡한 객체가 아닌 간단한 데이터 클래스를 보유합니다. 그러나 이러한 접근 방식이 가져다주는 혜택은 추가 복잡성을 충분히 상쇄한다는 것을 발견할 수 있습니다.

## 활동 관리

이상적으로 Trio를 사용하는 응용 프로그램은 Google의 표준 응용 프로그램 아키텍처 권장 사항을 따라 하나의 활동만 사용할 것입니다. 그러나 상호 운용성을 위해 종종 Trios가 새 활동 인텐트를 시작해야 할 때가 있습니다. 일반적으로 이는 ViewModel에서 수행하지 않습니다. ViewModel은 활동 수명 주기를 초과하므로 활동 참조를 포함해서는 안되기 때문입니다. 그러나 ViewModel에서 모든 탐색을 수행하는 패러다임을 유지하기 위해 Trio는 예외를 만들었습니다.

초기화 중에 Trio ViewModel은 초기화 프로세스를 통해 활동의 Flow를 받게 됩니다. 이 Flow는 ViewModel이 연결된 현재 활동을 제공하며, 활동이 다시 생성되는 동안과 같이 분리될 때 null을 제공합니다. Trio 내부에서 Flow를 관리하여 항상 최신 상태로 유지하고 활동이 누출되지 않도록 보장합니다.

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

필요할 때 ViewModel은 awaitActivity suspend 함수를 통해 다음으로 null이 아닌 액티비티 값을 접근할 수 있습니다. 예를 들어, 네트워크 요청이 완료된 후 새로운 액티비티를 시작하는 데 사용할 수 있습니다.

```js
class ViewModelInitializer<S : MavericksState>(
  val initialState: S,
  internal val activityFlow: Flow<Activity?>,
  ...
)

class CounterViewModel(
  initializer: ViewModelInitializer
) : TrioViewModel {

  fun saveAndOpenNextPage() = viewModelScope.launch {
    performSaveRequest()
    awaitActivity().startActivity()
  }
}
```

awaitActivity 함수는 TrioViewModel에서 제공되며, 액티비티 플로우에서 다음 값을 가져오는 편리한 방법으로 제공됩니다.

```js
suspend fun awaitActivity(): ComponentActivity {
  return initializer.activityFlow.filterNotNull().first()
}
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

조금 독특하지만, 이 패턴을 사용하면 활동 기반 탐색을 ViewModel의 다른 비즈니스 로직과 함께 놓을 수 있습니다.

## 모듈화 구조

대규모 코드 기반을 적절하게 모듈화하는 것은 많은 애플리케이션이 직면한 문제입니다. 에어비앤비에서는 더 빠른 빌드 속도와 명시적인 소유권 경계를 설정하기 위해 코드베이스를 2000개 이상의 모듈로 분할했습니다. 이를 지원하기 위해 기능 모듈을 분리하는 내부 네비게이션 시스템을 구축했습니다. 이 시스템은 원래 Fragment와 Activity를 지원하도록 만들어졌으며, 나중에 Trio와 통합되어 대규모 애플리케이션에서 규모별 탐색 일반 문제를 해결하는 데 도움이 되었습니다.

우리의 프로젝트 구조에서 각 모듈은 접두사와 접미사로 표시된 특정 유형을 가지며, 이를 통해 모듈의 목적을 정의하고 다른 모듈이 의존할 수 있는 규칙 집합을 강제합니다.

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

Feature 모듈은 "feat"로 접두사가 붙어 있으며 Trio 스크린을 포함하고 있습니다. 응용 프로그램의 각 화면은 자체 모듈에 살 수 있습니다. 순환 종속성을 방지하고 빌드 속도를 향상시키기 위해 기능 모듈이 서로에게 의존하지 않도록합니다.

이것은 한 기능이 다른 기능을 직접적으로 생성할 수 없음을 의미합니다. 대신, 각 기능 모듈에는 해당하는 네비게이션 모듈이 있으며 이는 "nav"로 접미사가 붙어 해당 기능에 대한 라우터를 정의 합니다. 순환 종속성을 피하기 위해 라우터와 해당 Trio는 Dagger 다중 바인딩과 연결됩니다.

이 간단한 예제에서 카운터 기능과 십진 기능이 있습니다. 카운터 기능은 십진 카운트를 수정하기 위해 십진 기능을 열수 있으므로 카운터 모듈은 십진 네비게이션 모듈에 의존해야 합니다.

/assets/img/2024-07-06-IntroducingTrioPartII_2.png

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

![Routing](https://miro.medium.com/v2/resize:fit:600/1*yTEItzOwdvBn3soU3phGzg.gif)

## 경로 설정

네비게이션 모듈은 작습니다. 각 Trio에 해당하는 중첩된 라우터 객체를 포함하는 Routers 클래스만 포함합니다.

```js
// In feat.decimal.nav
@Plugin(pluginPoint = RoutersPluginPoint::class)
class DecimalRouters : RouterDeclarations() {

  @Parcelize
  data class DecimalArgs(val count: Double) : Parcelable

  object DecimalScreen
    : TrioRouter<DecimalArgs, NavigationProps, NoResult>
}
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

A Router 객체는 Trio의 공개 인터페이스를 정의하는 유형으로 구성됩니다: 인스턴스화하는 데 사용되는 Arguments, 활성 통신에 사용되는 Props, 그리고 필요한 경우 Trio가 반환하는 Result입니다.

Arguments는 종종 화면에 대한 시작 값으로 사용되는 기본 데이터를 포함한 데이터 클래스입니다.

중요한 점은 Routers 클래스가 @Plugin으로 주석이 달려 있어 Routers PluginPoint에 추가되어야 함을 선언한다는 것입니다. 이 주석은 의존성 주입에 사용되는 내부 KSP 프로세서의 일부이지만 본질적으로 Dagger 다중 바인딩 세트를 설정하는 보일러플레이트 코드를 생성합니다. 결과적으로 각 Routers 클래스가 세트에 추가되어 Dagger 그래프에서 런타임에 액세스할 수 있게 됩니다.

기능 모듈의 해당 Trio 클래스에서는 @TrioRouter 주석을 사용하여 Trio가 어떤 Router에 매핑되는지 지정합니다. 우리의 KSP 프로세서는 컴파일 시간에 이들을 일치시키고 코드를 생성하여 런타임에서 각 Router에 대한 Trio 목적지를 찾을 수 있게 합니다.

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

// In feat.decimal

@TrioRouter(DecimalRouters.DecimalScreen::class)
class DecimalScreen(
initializer: Initializer<DecimalArgs, ...>
) : Trio<DecimalArgs, NavigationProps, ...>

컴파일 시간에 프로세서는 라우터의 Arguments와 Props가 Trio의 타입과 일치하며, 각 라우터가 하나의 대응 목적지를 갖는지를 유효성 검사합니다. 이는 우리의 내비게이션 시스템에서 런타임 타입 안전성을 보장합니다.

## 라우터 사용법

Trios를 수동으로 인스턴스화하는 대신에, 라우터에게 처리해주도록 합니다. 라우터는 적절한 Arguments의 유형이 제공되었는지 확인하고, Dagger 그래프에서 해당 Trio 클래스를 찾아서 초기화 프로세스를 묶어주는 클래스를 만들고, 마지막으로 Reflection을 사용하여 Trio의 생성자를 호출합니다.

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

라우터에서 createTrio 함수를 통해 이 기능을 사용할 수 있습니다. 이 함수는 ViewModel에서 호출할 수 있어요. 이를 통해 우리는 Trio의 새 인스턴스를 쉽게 만들어 Trio 스택에 넣을 수 있어요. 아래 예시에서 Props 인스턴스는 Trio가 부모에게 다시 호출해 이 푸시를 수행할 수 있도록 합니다. 이 Props에 대해 우리는 시리즈의 제3부에서 자세히 살펴볼 거예요.

```kotlin
class CounterViewModel : TrioViewModel {

  fun showDecimal(count: Double) {
    val trio = DecimalRouters.DecimalScreen.createTrio(DecimalArgs(count))
    props.pushScreen(trio)
  }
}
```

만약 새로운 액티비티에서 Trio를 시작하고 싶다면, Router에는 Trio 인스턴스를 래핑하는 새로운 액티비티를 위한 인텐트를 생성하는 기능도 제공해요. 그럼 앞서 설명된 Trio의 액티비티 메커니즘을 사용하여 ViewModel에서 이를 시작할 수 있어요.

```kotlin
class CounterViewModel : TrioViewModel {

  fun showDecimal(count: Double) = viewModelScope.launch {
    val activity = awaitActivity()
    val intent = DecimalRouters.DecimalScreen
                    .newIntent(activity, DecimalArgs(count))

    activity.startActivity(intent)
  }
}
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

새로운 활동에서 트리오가 시작되면, 우리는 간단히 인텐트에서 Parcelable Trio 인스턴스를 추출하고, 해당 인스턴스를 활동 콘텐츠의 루트에 표시하면 됩니다.

```kotlin
class TrioActivity : ComponentActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    val trio = intent.parseTrio()
    setContent {
      ShowTrio(trio)
    }
  }
}
```

또한 결과를 위해 활동을 시작할 수도 있는데, 이를 위해 라우터에 Result 유형을 정의할 수 있습니다.

```kotlin
class DecimalRouters : RouterDeclarations() {

  data class DecimalResult(val count: Double)

  object DecimalScreen : TrioRouter<DecimalArgs, ..., DecimalResult>
}
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

이 경우에는 ViewModel에 "launcher" 속성이 포함되어 있습니다. 이 속성은 새로운 활동을 시작하는 데 사용됩니다.

```kotlin
class CounterViewModel : TrioViewModel {

  val decimalLauncher = DecimalScreen.createResultLauncher { result ->
    setState {
      copy(count = result.count)
    }
  }

  fun showDecimal(count: Double) {
    decimalLauncher.startActivityForResult(DecimalArgs(count))
  }
}
```

예를 들어, 사용자가 Decimal 화면에서 십진수를 조정하면 새 카운트를 반환하여 상태를 업데이트할 수 있습니다. 런처에 대한 람다 인수를 사용하여 십진수 화면이 반환될 때 결과를 처리할 수 있으며, 이를 통해 상태를 업데이트하는 데 사용할 수 있습니다. 이는 모든 네비게이션을 ViewModel에 중앙화하고 동시에 타입 안전성을 보장하는 것이 목표입니다.

## Fragment 상호 운용

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

트리오 화면이 기존 프래그먼트 화면과 상호 운용 가능하도록 만드는 것이 저희에게 매우 중요했습니다. 저희는 Trio로의 이관이 수 년이 걸리는 작업이며, 트리오와 프래그먼트는 쉽게 공존할 수 있어야 합니다.

상호 운용성에 대한 저희 접근 방식은 두 가지로 나눌 수 있습니다. 첫째, 프래그먼트와 트리오가 생성될 때 동적으로 정보를 공유할 필요가 없는 경우(다시 말해, 초기 인수를 받고 결과를 반환하는 경우에 해당), 프래그먼트와 트리오 간 전환 시 새로운 활동을 시작하는 것이 가장 쉽습니다. 두 아키텍처 유형 모두 인수를 통해 쉽게 새로운 활동에서 시작할 수 있으며, 완료 시 선택적으로 결과를 반환할 수 있어서 이 방법으로 쉽게 이동할 수 있습니다.

또 다른 접근 방식은, 트리오와 프래그먼트 화면이 활성화된 상태에서 서로 데이터를 공유해야 하는 경우(다시 말해, Trio의 Props에 해당하는 경우), 또는 인수로 넘기기에 너무 큰 복잡한 데이터를 공유해야 하는 경우, 트리오가 "Interop Fragment" 내에 중첩되고 두 프래그먼트가 동일한 활동에서 표시될 수 있습니다. 프래그먼트는 일반적으로 Mavericks와 유사하게 ViewModel을 공유하는 방식으로 통신할 수 있습니다.

우리의 Router 객체는 다른 프래그먼트에서 Trio를 쉽게 만들고 표시할 수 있도록 도와주며, 단일 함수 호출만으로 가능합니다:

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

## 이전 버전 조각: MavericksFragment {

fun showTrioScreen() {  
 showFragment(
CounterRouters
.CounterScreen
.newInteropFragment(SharedCounterViewModelPropsAdapter::class)
)
}
}

루터는 셸 Fragment를 만들고 그 안에 Trio를 렌더링합니다. 위 예시에서 SharedCounterViewModelPropsAdapter와 같은 선택적인 어댑터 클래스는 Fragment에 전달될 수 있으며, Trio가 액티비티의 다른 Fragment에서 사용되는 Mavericks ViewModel과 어떻게 통신할지를 지정합니다. 이 어댑터를 사용하면 Trio가 액세스하려는 ViewModel을 지정하고 해당 ViewModel 상태를 Trio가 사용하는 Props 클래스로 변환하는 StateFlow를 생성할 수 있습니다.

## 사용된 SharedCounterViewModelPropsAdapter : LegacyViewModelPropsAdapter<SharedCounterScreenProps> {

override suspend fun createPropsStateFlow(
legacyViewModelProvider: LegacyViewModelProvider,
navController: NavController<SharedCounterScreenProps>,
scope: CoroutineScope
): StateFlow<SharedCounterScreenProps> {

// 액티비티 ViewModel 조회
val sharedCounterViewModel: SharedCounterViewModel = legacyViewModelProvider.getActivityViewModel()

// 필요한 경우 여러 ViewModel 조회 가능
val fragmentClickViewModel: SharedCounterViewModel = legacyViewModelProvider.requireExistingViewModel(viewModelKey = {
SharedCounterViewModelKeys.fragmentOnlyCounterKey
})

// 상태 업데이트를 Trio의 Props로 결합하고, StateFlow로 반환합니다.
// 이는 어떤 상태 Flow가 새 상태 객체를 가질 때마다 호출됩니다.
return combine(sharedCounterViewModel.stateFlow, fragmentClickViewModel.stateFlow) { sharedState, fragmentState ->
SharedCounterScreenProps(
navController = navController,
sharedClickCount = sharedState.count,
fragmentClickCount = fragmentState.count,
increaseSharedCount = {
sharedCounterViewModel.increaseCounter()
}
)
}.stateIn(scope)
}
}

## 결론

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

이 기사에서는 Trio에서 탐색이 작동하는 방법을 이야기했습니다. 우리는 사용자 정의 라우팅 시스템을 사용하거나 ViewModel에서 활동에 액세스할 수 있는 기능, 그리고 Trios를 ViewModel State에 저장하여 모듈화, 상호 운용성을 달성하고 탐색 논리에 대해 간단하게 이해하기 쉽도록 하는 목표를 달성합니다.

Part 3을 계속 읽어보세요. Trio의 Props가 화면 간에 동적 통신을 가능하게 하는 방법을 설명할 것입니다.

그리고 이것이 당신이 즐겨하는 도전이라면, 오픈되어 있는 채용 공고를 확인해보세요 — 우리는 채용 중입니다!
