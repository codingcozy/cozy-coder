---
title: "Kotlin Flows  기초 개념 완벽 정리"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-07 03:11
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Kotlin Flows — Fundamentals"
link: "https://medium.com/@anitaa_1990/kotlin-flows-fundamentals-7d9b984f12bb"
---


지난 주에는 Kotlin 코루틴에 대해 더 알아보았어요. 이전 글에서는 CoroutineContext, CoroutineScope, Coroutine Builder 등과 같은 코루틴의 기본 사항에 초점을 맞췄었죠. 약속했던 대로, 오늘의 주제는 Flows입니다.

# Flows란 무엇인가요?

비동기적으로 계산될 수 있는 데이터의 연속을 'Flow(플로우)'라고합니다. LiveData나 RxJava 스트림처럼 Flow를 사용하면 옵저버 패턴(observer pattern)을 구현할 수 있습니다. 옵저버 패턴은 소스(source) 오브젝트가 의존자들을 유지하는 객체(옵저버 = 수집자) 목록을 유지하고 언제든지 상태 변경을 알리는 소프트웨어 디자인 패턴입니다. Flow는 값들을 비동기적으로 생산하고 소비하기 위해 suspend 함수를 사용합니다.

Flow를 생성하려면 먼저 프로듀서(생산자)를 만들어야 합니다.

<div class="content-ad"></div>

랜덤 플로우 값 수집을 위해 먼저 코루틴을 실행해야 합니다. 플로우는 백그라운드에서 코루틴을 활용하므로 collect 연산자를 사용하여 이틀리는 값들을 수집합니다.

```kotlin
lifecycleScope.launch {
    viewModel.uiStateFlow.collect { it ->
        binding.uiText.text = it.toString()
    }
}
```

플로우에는 두 가지 다른 유형이 있습니다.

<div class="content-ad"></div>

**Cold Flow** - 값이 생성되기 시작하려면 수집이 시작되어야 합니다. 하나의 구독자만 가질 수 있으며 데이터를 저장하지 않습니다.

```kotlin
// 일반 Flow 예시
val coldFlow = flow {
    emit(0)
    emit(1)
    emit(2)
}

launch { // 첫 번째 호출
    coldFlow.collect { value ->
        println("cold flow collector 1이 수신함: $value")
    }

    delay(2500)

    // 두 번째 호출
    coldFlow.collect { value ->
        println("cold flow collector 2가 수신함: $value")
    }
}

// 결과
// 두 수신자는 처음부터 모든 값을 받습니다.
// 두 수신자에 대해 해당 Flow는 처음부터 시작됩니다.
cold flow collector 1이 수신함: [0, 1, 2]
cold flow collector 2가 수신함: [0, 1, 2]
```

**Hot Flow** - 수집자가 없어도 값을 생성합니다. 여러 구독자를 가질 수 있으며 데이터를 저장할 수 있습니다.

```kotlin
// SharedFlow 예시
val sharedFlow = MutableSharedFlow<Int>()

sharedFlow.emit(0)
sharedFlow.emit(1)
sharedFlow.emit(2)
sharedFlow.emit(3)
sharedFlow.emit(4)

launch {
    sharedFlow.collect { value ->
        println("SharedFlow collector 1이 수신함: $value")
    }

    delay(2500)

    // 두 번째 호출
    sharedFlow.collect { value ->
        println("SharedFlow collector 2가 수신함: $value")
    }
}

// 결과
// 수신자는 수집을 시작한 곳부터 값을 받습니다.
// 첫 번째 수신자는 모든 값을 받습니다. 그러나 두 번째 수신자는 2500밀리초 후에 수집을 시작했기 때문에 2500밀리초 이후에 발행된 값을만 받습니다.
SharedFlow collector 1이 수신함: [0, 1, 2, 3, 4]
SharedFlow collector 2가 수신함: [2, 3, 4]
```

<div class="content-ad"></div>

우리는 stateIn() 및 shareIn() 연산자를 사용하여 어떤 차가 흐르던 것을 각각 뜨거운 차로 변환할 수 있어요.

## SharedFlow와 StateFlow

- StateFlow — StateFlow는 한 번에 하나의 값만을 보유하는 상태를 나타내는 뜨거운 플로우입니다. 또한 conflated flow입니다. 이는 새 값이 발행될 때 가장 최근의 값이 유지되어 즉시 새로운 수집기에 전달된다는 것을 의미해요. 상태에 대한 단일 진실의 원천을 유지하고 모든 수집기를 자동으로 최신 상태로 업데이트해야 할 때 유용해요. 항상 초기 값이 있으며 가장 최근에 발행된 값만 저장해요.

```kotlin
class HomeViewModel : ViewModel() {

    private val _textStateFlow = MutableStateFlow("Hello World")
    val stateFlow =_textStateFlow.asStateFlow()

    fun triggerStateFlow(){
        _textStateFlow.value="State flow"
    }
}

// Activity/Fragment에서 StateFlow 수집하기
class HomeFragment : Fragment() {
    private val viewModel: HomeViewModel by viewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

lifecycleScope.launchWhenStarted {

  // 플로우를 트리거하고 값 수집 시작

  // collectLatest()는 Kotlin의 Flow API에서 고차 함수로,
  // Flow에서 발행된 값을 수집하고 최근 값에 대해서만 변환을 수행하는 함수에요.
  // collect()와 유사하지만 collectLatest는 최신 값만 처리하며,
  // 아직 처리되지 않은 이전 값들은 무시돼요.
    viewModel.stateFlow.collectLatest {
          binding.stateFlowButton.text = it
    }
  }
}

// Compose에서 StateFlow 수집하기
@Compose
fun HomeScreen() {
 // Compose에서는 collectAsStateWithLifecycle 함수를 제공해요.
 // 이 함수는 플로우에서 값을 수집하고 필요할 때 가장 최신 값을 제공합니다.
 // 새로운 플로우 값이 발행되면 업데이트된 값을 얻고,
 // 다시 구성이 일어나서 값의 상태가 업데이트돼요.
 // 기본적으로 LifeCycle.State.Started를 사용하여
 // 지정된 상태에 있는 생명주기에서 값을 수집 시작하고
 // 해당 상태보다 아래일 때 중지해요.
  val someFlow by viewModel.flow.collectAsStateWithLifecycle()
  
}
```

<div class="content-ad"></div>

-  **SharedFlow(공유 플로우)** - SharedFlow는 여러 수집기를 가질 수 있는 HotFlow로, 수집기와 독립적으로 값을 발생할 수 있습니다. 여러 수집기가 동일한 값들을 플로우로부터 수집할 수 있습니다. 여러 수집기에 값을 브로드캐스트하거나 동일한 데이터 스트림에 여러 구독자를 가질 때 유용합니다. 초기값이 없으며, 새로운 수집기를 위해 이전에 발생한 값을 일정 숫자만큼 재생성 캐시에 저장할 수 있습니다.

```kotlin
class HomeViewModel : ViewModel() {
    private val _events = MutableSharedFlow<Event>() // 비공개 mutable shared flow
    val events = _events.asSharedFlow() // 읽기 전용 shared flow로 공개

    suspend fun produceEvent(event: Event) {
        _events.emit(event) // 모든 구독자가 수신할 때까지 일시 중지
    }
}

// Activity/Fragment에서 StateFlow 수집
class HomeFragment : Fragment() {
    private val viewModel: HomeViewModel by viewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        lifecycleScope.launchWhenStarted {
            // 플로우를 트리거하고 값 수신을 시작함

            // collectLatest()는 Kotlin의 Flow API에서 사용되는 고차 함수로, 
            // Flow에서 발생한 값을 수집하고 최신 값에 대한 변환을 수행하는 함수입니다. 
            // collect()와 유사하지만, collectLatest는 최신으로 발생한 값만 처리하며 
            // 아직 처리되지 않은 이전 값은 무시합니다.
            viewModel.events.collectLatest {
                binding.eventFlowButton.text = it
            }
        }
    }
}

// Compose에서 StateFlow 수집
@Compose
fun HomeScreen() {
    // Compose는 collectAsStateWithLifecycle 함수를 제공하며, 
    // 플로우에서 값을 수집하고 필요한 곳에 최신 값을 제공합니다. 
    // 새로운 플로우 값이 발생하면 업데이트된 값을 얻고 상태 값을 업데이트하기 위해 재 구성이 수행됩니다. 
    // 기본적으로 LifeCycle.State.Started를 사용하여 지정된 상태에서 값 수집을 시작하고 
    // 그것보다 아래로 떨어질 때 중지합니다.
    val someFlow by viewModel.events.collectAsStateWithLifecycle()
}
```

# 플로우에서의 예외 처리

Kotlin Flow는 예외와 오류를 처리하기 위한 여러 메커니즘을 제공합니다.

<div class="content-ad"></div>

**- try-catch 블록 -** 예외를 처리하는 기본적인 방법 중 하나는 플로우 내에서 try-catch 블록을 사용하는 것입니다.

```js
 flow {
     try {
         emit(productsService.fetchProducts())
     } catch (e: Exception) {
         emitError(e)
     }
 }
```

**- catch 연산자 -** Flow의 catch 연산자를 사용하면 예외를 처리할 때 오류 처리 로직을 한 곳에 캡슐화할 수 있습니다.

```js
flow {
    emit(productsService.fetchProducts())
}.catch { e ->
    emitError(e)
}
```

<div class="content-ad"></div>

- **onCompletion Operator** - 플로우가 성공적으로 완료되거나 예외와 함께 완료될 때 실행 코드를 실행하는 데 사용됩니다.

```kotlin
flow {
    emit(productsService.fetchProducts())
}.onCompletion { cause ->
    if (cause != null) {
        emitError(cause)
    }
}
```

- **사용자 지정 오류 처리** - 안드로이드의 복잡한 시나리오에서는 애플리케이션에 적합한 방식으로 오류를 처리하기 위해 사용자 정의 연산자나 확장 기능을 만들 수 있습니다.

```kotlin
fun <T> Flow<T>.sampleErrorHandler(): Flow<Result<T>> = transform { value ->
    try {
        emit(Result.Success(value))
    } catch (e: Exception) {
        emit(Result.Error(e))
    }
}
```

<div class="content-ad"></div>

# Flows vs LiveData

- LiveData는 라이프사이클을 감지하여 옵저버의 활성 상태일 때에만 업데이트를 전달하는 반면, Flow는 기본적으로 라이프사이클을 감지하지 않습니다. Compose에서 collectLatest() 또는 collectAsStateWithLifecycle() 함수를 사용하여 Flow의 결과를 수집할 수 있습니다.
- Flow는 더 많은 유연성을 제공하며 더 복잡한 비동기 데이터 작업에 적합하며, LiveData는 일반적으로 더 단순한 UI 업데이트에 사용됩니다.
- Flow는 백프레셔(backpressure)에 대한 내장 지원을 제공하여 데이터 발행 및 처리 속도를 제어할 수 있지만, LiveData는 백프레셔 처리를 지원하지 않습니다.
- Flow는 순차적 및 구조화된 처리를 위한 다양한 연산자를 제공하며, LiveData는 옵저버에게 최신 데이터를 전달하는 데 초점을 맞춥니다.

읽어 주셔서 감사합니다! 이 기사가 유용하고 즐거우셨기를 바랍니다. 의견을 댓글로 남겨 주세요.

즐거운 코딩 되세요!