---
title: "안드로이드 UI 레이어 속성 집중 분석  2부"
description: ""
coverImage: "/assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_0.png"
date: 2024-07-10 01:30
ogImage: 
  url: /assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_0.png
tag: Tech
originalTitle: "Crash Course on the Android UI Layer | Part 2"
link: "https://medium.com/bumble-tech/crash-course-on-the-android-ui-layer-part-2-2335171467e0"
---


## 상태 보유자 및 상태 저장

안드로이드 개발자 안내서를 요약하는 이 블로그 시리즈는 UI 레이어에 대해 다뤄보려 합니다. 우리는 관련된 모든 엔티티를 탐험하고, 각 부분이 하는 역할을 이해하며, 최적의 실천 방안을 논의할 것입니다.

첫 번째 파트에서는 UI와 UI 상태에 대해 다루었습니다. 이미 UI 레이어에 존재하는 다양한 엔티티를 알고 있고, UI와 UI 상태에 대해 효과적으로 생각하는 방법을 이해했을 것입니다.

이제 Part 2로 넘어가겠습니다! 상태 보유자와 안드로이드에서 UI 상태를 저장하는 방법 등 다른 UI 레이어 관련 주제를 다룰 예정입니다.

<div class="content-ad"></div>

만약 비디오 형식으로 이 콘텐츠를 섭취하고 싶다면 Droidcon London 2023에서 제가 진행한 강연을 확인해보세요:

## 상태 보유자

상태 보유자는 로직을 처리하거나 UI 상태를 노출함으로써 UI를 단순화합니다. 이 섹션에서는 상태 보유자를 구현하는 방법과 고려해야할 구현 세부 사항을 살펴보겠습니다.

구현 세부 사항을 결정하려면 먼저 안드로이드 앱에서 일반적으로 발견되는 로직 유형을 식별해야 합니다.

<div class="content-ad"></div>

## 논리의 종류

이미 제품 요구 사항을 구현하는 비즈니스 로직이 어플리케이션 데이터를 어떻게 생성, 저장 및 수정할지를 명시합니다는 것에 대해 이야기했습니다. 비즈니스 로직이 UI 레이어에 존재할 때, 그 로직은 화면 수준에서 관리하는 것이 좋습니다. 나중에 이에 대해 더 자세히 알아보겠습니다.

다른 유형의 로직은 UI 로직입니다. UI 로직은 화면에서 상태 변경을 어떻게 표시할지를 결정합니다. 비즈니스 로직이 데이터와 무엇을 할지를 지시한다면, UI 로직은 시각적으로 어떻게 표시할지를 결정합니다. UI 로직은 UI 구성에 의존합니다.

예를 들어 일반적인 앱에서는 상세 화면을 표시하는 것이 휴대폰에서 실행될 때에는 탐색을 포함할 수 있습니다. 그러나 태블릿에서 실행될 때 다른 요소를 옆에 표시하는 것을 포함할 수도 있습니다.

<div class="content-ad"></div>

![2024-07-10-CrashCourseontheAndroidUILayerPart2_0](/assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_0.png)

다른 유형의 로직은 구성 변경에 각각 다르게 반응합니다:

- UI 로직은 구성 변경에 영향을 받으면 다시 실행해야 합니다.
- 비즈니스 로직은 일반적으로 구성 변경 후에 계속되어야 합니다.

예를 들어, 하단 표시줄 또는 네비게이션 레일을 표시할지 여부를 결정하는 UI 로직은 화면 크기 구성 변경 후에 다시 실행하거나 다시 평가해야 합니다. 반면에 특정 관심사를 따르거나 그들을 새로 고침해야 하는 비즈니스 로직은 사용자가 장치를 회전하거나 펼쳤다고 해서 취소되거나 다시 시작되어서는 안 됩니다. 그러한 중단은 좋은 사용자 경험을 제공하지 못할 것입니다.

<div class="content-ad"></div>

## 로직을 처리해야 할 장소

UI 레이어에서 비즈니스 로직은 화면 레벨에 가능한 가까이 처리해야 합니다. 대부분의 비즈니스 로직은 데이터 레이어에서 처리됩니다. 따라서 화면에 가까이 유지하면 로직을 올바르게 범위 지정하기가 쉬워지며, 낮은 수준의 UI 구성 요소가 비즈니스 로직과 강하게 결합되는 것을 방지할 수 있습니다.

비즈니스 로직은 일반적으로 androidX.ViewModel에서 확장된 화면 레벨 상태 홀더에 의해 처리되어야 합니다.

UI 로직의 경우, 로직과 상태가 상대적으로 간단한 경우 UI 자체에서 처리하는 것이 적합합니다. 그러나 UI가 더 복잡해지면 UI 로직 복잡성을 평범한 클래스 상태 홀더로 위임하는 것이 좋습니다. 이 경우 상태 홀더는 androidX.ViewModel에서 확장되지 않을 것입니다.

<div class="content-ad"></div>

다음 섹션에서 더 자세히 다룰 예정이에요! 이제 각 상태와 로직 유형이 어떻게 관련되어 있는지 알아봅시다:

![Android UI Layer](/assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_1.png)

전형적인 화면에서 발생하는 일에 대한 요약으로, 데이터 레이어는 응용 프로그램 데이터를 계층구조의 나머지에 노출시킵니다. 그런 다음 ViewModel은 해당 데이터에 비즈니스 로직을 적용하여 화면 UI 상태를 생성합니다. UI 자체 또는 일반 상태 보유자 클래스가 화면 UI 상태를 관찰하여 UI 요소나 상태를 수정합니다.

# 비즈니스 로직 처리하기 — androidX.ViewModel

<div class="content-ad"></div>

우리는 androidX.ViewModel 또는 Architecture Components ViewModel 클래스를 화면 레벨 상태 보관함의 구현 세부사항으로 상세히 설명해 왔습니다.

아래 코드 스니펫에서 이 동작의 주요 기능을 관찰할 수 있습니다: 1) 화면 UI 상태 노출 및 2) 비즈니스 로직 처리.

```kotlin
@HiltViewModel
class InterestsViewModel @Inject constructor(
  private val userDataRepository: UserDataRepository,
  authorsRepository: AuthorsRepository,
  topicsRepository: TopicsRepository
) : ViewModel() {

  val uiState: StateFlow<InterestsUiState> = ...

  fun followTopic(followedTopicId: String, followed: Boolean) {
    viewModelScope.launch {
      userDataRepository.toggleFollowedTopicId(followedTopicId, followed)
    }
  }

  ...
}
```

하지만, 왜 ViewModel이 이에 적합한 위치일까요?

<div class="content-ad"></div>

## 안드로이드X.ViewModel의 장점

안드로이드X.ViewModel의 주요 장점은 ViewModel이 구성 변경을 견뎌내며 화면 자체보다 더 오랜 수명을 제공한다는 것입니다. ViewModel을 Activity, Fragment, Navigation 그래프 또는 Navigation 그래프의 대상에 제한 범위로 설정할 수 있습니다. 구성 변경이 발생할 때 시스템은 ViewModel의 동일한 인스턴스를 제공합니다.

구성 변경을 견뎌내는 능력은 androidX.ViewModel을 화면 UI 상태를 노출하고 비즈니스 로직을 처리하는 완벽한 장소로 만들어줍니다. 화면 UI 상태는 구성 변경 전후에도 캐시되어 즉시 사용할 수 있습니다. 그리고 ViewModel 범위의 CoroutineScope(예: viewModelScope로 시작된 경우)를 통해 시작된 작업은 구성 변경이 발생해도 계속 실행됩니다.

또 다른 이점은 다른 Jetpack 라이브러리와의 원활한 통합에 있습니다. 특히 Jetpack Navigation과의 통합은 매우 원활합니다. Navigation은 대상이 백 스택의 일부인 경우에 ViewModel의 동일한 인스턴스를 메모리에 유지합니다. 이를 통해 백 스택 내의 대상 간을 왕복하면서 데이터가 즉시 화면에 사용 가능하며 해당 대상으로 다시 이동할 때마다 데이터를 다시로드할 필요가 없습니다.

<div class="content-ad"></div>

Jetpack Navigation은 목적지가 더 이상 백 스택의 일부가 아닌 경우 ViewModel의 인스턴스를 자동으로 파괴합니다. 이는 화면에 이전 사용자 데이터를 표시하지 않고 이전 목적지로 이동하는 것을 안전하게 만듭니다.

다른 Jetpack 통합 사례로는 Hilt이 있습니다. @HiltViewModel 주석을 사용하여 도메인이나 데이터 레이어의 종속성을 가진 ViewModel을 쉽게 얻을 수 있습니다.

## androidX.ViewModel 최상의 규칙

ViewModel의 범위 설정은 이 유형을 화면 수준 상태 보유자의 구현 세부 정보로 적합하게 만드는 요소입니다. 그러나 이 권한을 남용해서는 안 됩니다. 이 클래스를 사용할 때 염두에 둘 가장 좋은 방법은 다음과 같습니다:

<div class="content-ad"></div>

- 화면 수준에서 사용하세요. 재사용 가능한 UI 요소의 복잡성을 처리하기 위해 ViewModel을 사용하지 마세요. 동일한 ViewModel 범위 내에서 동일한 UI 요소를 사용하면 동일한 ViewModel 인스턴스를 얻게 됩니다. 대부분의 경우, 이는 바람직하지 않습니다.
- ViewModel을 충분히 범용적으로 만들어서 모든 UI 폼 팩터를 수용할 수 있도록 하세요. ViewModel은 자신을 사용하는 UI가 어떤 것인지 인식해서는 안 됩니다. ViewModel의 API 표면(노출된 화면 UI 상태 및 노출된 기능)은 처리하는 애플리케이션 데이터를 대표하도록 유지하세요. 예를 들어, 데이터를 로딩 중임을 나타낼 때, 화면 UI 상태에는 showLoadingSpinner가 아닌 isLoading이라는 필드를 포함할 수 있습니다. UI가 사용자에게 데이터 로딩을 어떻게 전달하는지는 UI에만 관련이 있습니다.
- Lifecycle 관련 API에 대한 참조를 보유하지 마세요. ViewModel은 UI보다 더 오래 살아있고, Context나 Resources 객체에 대한 참조를 유지하면 메모리 누수로 이어질 수 있습니다.
- ViewModel을 전달하지 마세요. 언급된 모든 점을 고려하면, ViewModel 클래스를 화면 수준에 가능한 가까이 유지하세요. 그렇지 않으면, 저수준 구성 요소가 실제로 필요한 것보다 더 많은 상태와 로직에 액세스할 수 있게 될 수도 있습니다.

## androidX.ViewModel의 함정

ViewModel 영역에서 모든 것이 완벽한 것은 아닙니다. 특히 ViewModel의 viewModelScope에 대해 사용할 때 주의해야 할 사항이 있습니다:

- viewModelScope를 사용하여 시작된 작업은 ViewModel이 메모리에 있는 동안 계속 실행됩니다. 이는 좋지만 작업이 긴 시간 동안 실행될 경우 문제가 발생할 수도 있습니다. 10초 이상 소요될 수 있는 긴 작업의 경우 WorkManager와 같은 다른 대안을 고려해보세요. 백그라운드 작업에 대한 자세한 내용은 문서에서 확인하세요.
- viewModelScope에 의해 트리거된 작업을 단위 테스트하는 경우, 테스트 환경에서 추가 설정이 필요합니다. 테스트에서 MainDispatcher를 대체해야 합니다.

<div class="content-ad"></div>

## AndroidX.ViewModel를 사용하는 방법

이 섹션은 항상 ViewModel을 사용해야 한다는 것을 의미합니까? 화면 수준 상태 보유자의 구현으로는 맞지만, 앱에 적용되는 이점이 있다면 그렇습니다.

구성 변경에 관심이 있다면 (그러니까 해야 합니다!) 그리고/또는 다른 Jetpack 라이브러리를 사용하고 있다면 사용해도 될 수 있습니다. 그러나 그렇지 않기로 결정하더라도, 화면 수준에서 비즈니스 로직 복잡성을 처리하기 위한 간단한 화면 수준 상태 보유자 클래스를 도입하는 것을 고려해보세요.

# UI 로직 처리 — 간단한 상태 보유자 클래스

<div class="content-ad"></div>

컴포넌트의 복잡성이 증가할 때 상태 보유 클래스를 소개해야 합니다. 기준은 여러분과 여러분의 팀에 달려 있습니다. UI를 간소화할 필요를 느낄 때입니다.

다음 코드 스니펫에서 UI를 위한 상태 보유자를 만들 필요가 없습니다. 사용자가 UI와 상호 작용할 때 수정되는 확장된 부울이 포함되어 있습니다.

```js
@Composable
fun <T> NiaDropdownMenuButton(items: List<T>, ...) {
  var expanded by remember { mutableStateOf(false) }

  Box(modifier = modifier) {
    NiaOutlinedButton(
      onClick = { expanded = true },
      ...
    )
    NiaDropdownMenu(
      expanded = expanded,
      onDismissRequest = { expanded = false },
      ...
    )
}
```

UI에서 더 많은 상태가 필요하고 관련 로직이 더 복잡해지면 상태 보유자를 도입하세요. 이것은 바로 Compose 라이브러리가 일부 컴포넌트에 대해 수행하는 작업입니다. 다음 코드 스니펫은 다양한 Drawer 컴포저블의 상태 보유자에 속합니다:

<div class="content-ad"></div>

```kotlin
@안정적
클래스 DrawerState(
  initialValue: DrawerValue,
  confirmStateChange: (DrawerValue) -> Boolean = { true }
) {
  내부 val swipeableState = SwipeableState(...)

  val currentValue: DrawerValue
    get() { return swipeableState.currentValue }

  val isOpen: Boolean
    get() = currentValue == DrawerValue.Open

  suspend fun open() = animateTo(DrawerValue.Open, AnimationSpec)

  suspend fun animateTo(targetValue: DrawerValue, anim: AnimationSpec<Float>) {
    swipeableState.animateTo(targetValue, anim)
  }
}
```

거기 몇 가지 주목할 점이 있어요:

- 이는 Drawer의 currentValue와 같은 상태를 보유합니다.
- 상태 보유체들은 조합 가능합니다. DrawerState는 내부적으로 다른 상태 보유체인 SwipeableState에 의존합니다.
- 화면 개발 로직을 관리하며, 서랍을 열거나 특정 값으로 애니메이션하는 작업과 같은 작업을 포함합니다.

Compose가 이러한 상태 보유체를 제공하는 것처럼, 프로젝트에서 비슷한 패턴을 구현하여 UI를 단순화할 수 있습니다. 다음 코드 스니펫은 NiaApp 복합 함수의 상태 보유체 인 NiaAppState에 속합니다.


<div class="content-ad"></div>

```kotlin
@안정적
class NiaAppState(
  val navController: NavHostController,
  val windowSizeClass: WindowSizeClass
) {
  val currentDestination: NavDestination?
    @Composable get() = navController
      .currentBackStackEntryAsState().value?.destination

  val shouldShowBottomBar: Boolean
    get() = windowSizeClass.widthSizeClass == WindowWidthSizeClass.Compact ||
      windowSizeClass.heightSizeClass == WindowHeightSizeClass.Compact

  fun navigate(...) { ... }

  fun onBackClick() { ... }
}
```

비슷한 방식으로, 이 클래스는 currentDestination과 하단 바를 표시해야 하는지 여부와 같은 UI 상태를 노출하면서, UI 이동 및 뒤로 가기 클릭 이벤트 처리와 같은 UI 로직을 관리합니다.

## 일반 상태 보유 클래스의 최적 사례

재사용 가능한 UI 구성 요소에 대한 상태 보유자를 생성하는 것이 좋습니다. 이는 UI의 재사용성을 향상시키고 외부 제어를 제공합니다.


<div class="content-ad"></div>

생명주기 관련 API의 참조를 보유할 수 있는 Plain state holder 클래스입니다. 이러한 인스턴스는 UI 생명주기를 따릅니다. UI가 구성 변경을 거치면 상태 홀더의 새 인스턴스가 생성됩니다. 따라서 Context나 Resources에 대한 참조를 보유해도 메모리 누수가 발생하지 않습니다. Jetpack Compose에서 이러한 상태 홀더는 Composition에도 범위가 지정됩니다.

일반 클래스에 비즈니스 로직이 필요한 경우, 이 기능을 클래스에 주입하는 것이 좋은 관행입니다. 이 기능을 주입하는 사람은 UI 범위를 벗어나도록 보장할 수 있습니다.

## 큰 ViewModels 다루기

ViewModel이 여러 크기의 UI 요소의 비즈니스 로직 복잡성을 처리하고 있을 때, 이는 크고 관리 및 이해하기 어려워질 수 있습니다. ViewModel을 어떻게 단순화할 수 있을까요?

<div class="content-ad"></div>

- 도메인 레이어를 소개해보겠습니다. ViewModel의 비즤크 로직 복잡성을 처리하는 유즈케이스에 위임하여 다양한 저장소와 상호작용을 다룹니다. 그러나 이 접근 방식은 여전히 의존해야 할 유즈케이스 목록이 많은 ViewModel을 만들어낼 수 있습니다.
- UI의 다양한 요소들을 위한 여러 상태 보유자를 작성하고, UI의 ViewModel에서 이들을 올려놓아 모든 장점을 누릴 수 있습니다. ViewModel은 기본적으로 환경 구성 변경을 견딜 수 있는 상태 끌어올리기 메커니즘이 됩니다.
- #2 대신, 재사용할 수 없는 UI 요소들의 복잡성을 관리하기위해 여러 개의 ViewModel을 만드는 것을 고려해볼 수 있습니다. 이 접근 방식은 허용되지만, ViewModel은 메모리 사용량이 제한되지 않는데 주의해야 합니다. 그리고 다수의 ViewModel이 있는 경우, 그 크기와 메모리 풋프린트를 모니터링하기 어려워질 수 있습니다.

# 상태를 어디에 끌어올려야 하는가

상태를 읽거나 쓰는 가장 일반적인 조상에 상태를 놓아야 합니다.

간단히 요약하면: UI에서 1) 전혀 상태가 없을 수 있고, 2) UI 자체에 상태가 있을 수 있으며, 3) UI를 간단하게하기 위해 상태를 보유자에 두거나 4) 상태를 더 높은 UI 트리로 끌어올려 다른 호출자나 조상이 상태를 제어하게 할 수 있고, 5) 비즈니스 로직에서 필요한 경우 상태를 ViewModel에 끌어올릴 수 있습니다.

<div class="content-ad"></div>

만약 비즈니스 로직에 상태가 필요한 경우, 읽기 또는 쓰기를 위해서라도, 화면 수준의 상태 홀더에 위치시켜야 합니다. 필요 없다면, 적절한 UI 트리 노드에 배치되어야 합니다.

일반적인 채팅 앱의 UI 계층 구조를 살펴보고 왜 특정 상태가 해당 위치에 배치되었는지에 대해 이야기해 봅시다:

![UI Hierarchy](/assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_2.png)

- 화면 UI 상태는 비즈니스 로직을 적용하기 위해 ViewModel (#5)에 위치시켜야 합니다.
- LazyList는 MessageList가 아닌 ConversationScreen의 일부이며, 사용자가 UserInput에서 새 메시지를 보낼 때 가장 최신 메시지로 스크롤하는 등 화면에 추가 기능이 필요한 이유로 해당 상태가 필요합니다.

<div class="content-ad"></div>

이 주제에 대해 더 알고 싶다면, Alejandra Stamato로부터의 Compose 세션에서 상태 끌어올리기에 대해 살펴보세요.

# UI 상태 저장하기

이 블로그 포스트에서는 androidx.ViewModel API를 사용하여 구성 변경 간 상태를 보존하는 방법을 탐색했습니다. 그러나 안드로이드는 상태를 더 효과적으로 보호할 수 있는 추가 대안을 제공합니다.

SavedState API는 구성 변경 및 시스템에서 시작된 프로세스 종료를 통해 상태를 계속 유지할 수 있게 합니다. 시스템은 이 데이터를 묶음에 저장하며, 데이터를 저장하기 위해 묶음으로 만들어야 합니다. 일반적으로 사용자 입력이나 탐색에 의존하는 일시적인 UI 상태를 저장합니다.

<div class="content-ad"></div>

지금까지 위에서 언급된 사항 뿐만 아니라 예상치 못한 앱 종료(예: 사용자가 앱을 강제로 종료하는 경우)에도 살아남기 위해서는 영구 저장소를 활용할 수 있습니다. 이는 디스크 공간 제약 조건을 준수해야 하며 일반적으로 응용 프로그램 데이터 저장에 사용됩니다.

![Link](/assets/img/2024-07-10-CrashCourseontheAndroidUILayerPart2_3.png)

이 주제에 대해 더 알고 싶다면 Android에서 UI 상태 저장하기에 대한 토크를 확인해보세요.

# 결론

<div class="content-ad"></div>

UI 레이어에 대한 이 간편 안내서를 읽은 후에는, 이 레이어 내에서 발생하는 프로세스와 상태 및 로직을 효과적으로 관리하기 위해 필요한 도구에 대한 일반적인 이해가 있어야합니다.

안드로이드가 어떻게 설계되어 앱을 다양한 UI 구성 및 기기에 반응적으로 만드는지는 모든 API 결정 트리들이 몇몇 개발자들이 원하는 것보다 복잡하게 만듭니다. 그러나 동시에, 이러한 설계는 예상대로 앱이 작동하도록 하는 도구를 제공하여 훌륭한 사용자 경험을 제공할 수 있게 해줍니다.

이 글이 마음에 들었기를 바랍니다! 의견을 나누거나 질문을 하고 싶다면 댓글 섹션에 자유롭게 남겨주세요! 감사합니다 😊

이 내용을 동영상 형식으로 소비하는 것을 선호한다면, 2023년 드로이드콘 런던에서 진행한 제 발표 영상을 확인해보세요: