---
title: "안드로이드 개발자를 위한 고급 코틀린 코루틴 치트 시트"
description: ""
coverImage: "/assets/img/2024-08-03-AdvancedKotlinCoroutineCheatsheetforAndroidEngineer_0.png"
date: 2024-08-03 18:25
ogImage: 
  url: /assets/img/2024-08-03-AdvancedKotlinCoroutineCheatsheetforAndroidEngineer_0.png
tag: Tech
originalTitle: "Advanced Kotlin Coroutine Cheat sheet for Android Engineer"
link: "https://medium.com/@galou.minisini/advanced-kotlin-coroutine-cheat-sheet-for-android-engineer-15e0d180fc1f"
---


<img src="/assets/img/2024-08-03-AdvancedKotlinCoroutineCheatsheetforAndroidEngineer_0.png" />

코틀린 코루틴을 일정 기간동안 사용해오셨고 이미 suspend 함수와 launch 빌더 같은 기본 개념에 익숙하실 것입니다. 그러나 프로젝트가 더 복잡해지면 더 나은 해결책을 찾고 구글이나 마음에 드는 인공지능에 도움을 요청하는 일이 늘어날 수 있습니다.

본 치트 시트는 제가 그 동안 습득한 주요 통찰을 종합한 것입니다. 보다 복잡한 코루틴 시나리오에 대처하기 위한 편리한 참조 자료로 디자인되었습니다.

# 코루틴 용어집

<div class="content-ad"></div>

Coroutine Context(코틀린 문서에서): "여러 요소의 집합. 주된 요소는 코루틴의 작업(Job) 및 해당 dispather"입니다. 더 자세히 알아보려면:

Job(코틀린 문서에서): "완료로 극대화되는 수명 주기를 가진 취소 가능한 것(cancellable thing)". 각 코루틴은 자체 Job을 생성합니다(상위 코루틴에서 상속되지 않는 유일한 코루틴 컨텍스트입니다). 더 자세히 알아보려면:

Dispatcher: 코루틴이 실행되는 스레드(또는 스레드 풀)를 결정할 수 있게 합니다. (시작 및 계속). 더 자세히 알아보려면:

Coroutine scope: 코루틴의 수명 및 컨텍스트를 정의합니다. 코루틴의 취소 및 오류 처리를 포함한 코루틴의 수명주기를 관리하는 데 책임이 있습니다.

<div class="content-ad"></div>

Coroutine 빌더: CoroutineScope에서 시작할 수 있는 비동기 coroutine을 시작하는 확장 함수들(launch, async, ...)입니다.

# 코루틴 주요 규칙

- Coroutine을 시작하려면 CoroutineScope가 필요합니다(launch 또는 async와 같은). Android에서는 주로 viewModelScope이 사용되지만 자체적으로 만들 수도 있습니다.
- 자식 coroutine(다른 coroutine에서 시작된 coroutine)은 부모의 코루틴 컨텍스트를 상속 받습니다(Job은 예외).
- 부모 coroutine의 Job은 새 coroutine의 Job의 부모로 사용됩니다.
- 부모 coroutine은 모든 자식이 완료될 때까지 일시 중단됩니다.
- 부모 coroutine이 취소되면 모든 자식도 취소됩니다.
- 예외로 인해 자식 coroutine이 중단되면 부모도 취소됩니다(SupervisorJob을 사용하지 않는 한).
- GlobalScope는 절대 사용해서는 안 됩니다. 메모리 누수를 발생시키고 활동이나 프래그먼트가 종료된 후에도 coroutine이 계속 실행될 수 있습니다.
- 코루틴 스코프를 인수로 전달해서는 안 되며 대신 coroutineScope 함수를 사용해야 합니다(아래 예제 참조).

# 코루틴 스코프 함수

<div class="content-ad"></div>

coroutineScope : 인수 함수가 생성한 값이 반환되며 범위를 시작하는 보류 중인 함수입니다.

supervisorScope : coroutineScope와 유사하지만 컨텍스트의 Job을 재정의하기 때문에 자식이 예외를 throw할 때 함수가 취소되지 않습니다.

withContext : coroutineScope와 유사하지만 범위 내에서 일부 변경을 허용해줍니다 (일반적으로 Dispatcher를 설정하는 데 사용됩니다).

withTimeout : coroutineScope와 유사하지만 본문 실행에 시간 제한을 설정해주며 너무 오래 걸리는 경우 취소됩니다. TimeoutCancellationException을 throw합니다.

<div class="content-ad"></div>

```kotlin
withTimeoutOrNull: withTimeout와 동일하지만 제한 시간이 경과될 때 예외를 throw하는 대신 null을 반환합니다.

# 디스패처

코루틴을 디스패치하는 데에는 비용이 발생합니다. withContext를 호출하면 코루틴을 중단해야 하며, 재디스패칭을 피하려면 대기열에서 다시 시작하기 전에 대기해야 할 수도 있습니다.

## Dispatchers.Default
```

<div class="content-ad"></div>

- 디스패처가 설정되지 않았을 때 기본으로 사용되는 것입니다.
- CPU 집약 작업을 실행하도록 설계되었습니다.
- 스레드 풀의 크기는 기기의 코어 수와 동일합니다.
- 우리는 연산이 사용할 수 있는 스레드 수를 Dispatchers.Default.limitedParallelism(3)으로 제한할 수 있습니다.

## Dispatchers.Main

- Android에서 UI 스레드에서 실행됩니다.
- 이 스레드를 블록하지 않도록 주의해야 합니다.
- 유닛 테스트에는 존재하지 않습니다(필요할 때 직접 생성해야 합니다).

## Dispatchers.IO

<div class="content-ad"></div>

- 블로킹 작업 (I/O 작업, 파일 읽고 쓰기, 공유 프리퍼런스 등)을 실행하도록 설계되었습니다.
- 스레드 풀의 크기는 64입니다 (코어 수가 64보다 높으면 코어 수).
- Dispatchers.Default와 동일한 스레드 풀을 공유하지만 한도는 독립적입니다.
- 블로킹 함수에 사용됩니다.
- 이 디스패처로 코루틴을 시작하려면: withContext(Dispatchers.IO) ' // 일부 블로킹 함수 '.
- 작업이 사용할 수 있는 스레드 수를 제한할 수 있습니다: Dispatchers.Default.limitedParallelism(3).
- Dispatchers.IO를 사용한 limitedParallelism은 특별한 동작을 합니다. 독립적인 스레드 풀을 만들기 때문에 제한을 64보다 높게 설정할 수 있습니다.

## Dispatchers.Unconfined

- 시작된 스레드와 동일한 스레드에서 실행되며 다른 스레드로 변경되지 않습니다.
- 단위 테스트에 유용합니다.
- 성능 면에서 가장 저렴한 디스패처입니다 (스레드 전환 없음).
- 프로덕션 코드에서 사용하는 것이 위험합니다 (메인 스레드에서 블로킹 호출을 실수로 실행할 수 있음).

## 성능 관찰

<div class="content-ad"></div>

- 쓰레드 수를 일시 중지할 때는 그렇게 중요하지 않아요.
- 쓰레드 수를 막을수록 모든 코루틴이 더 빨리 끝날 거예요.
- CPU 집약적인 작업을 할 때 Dispatchers.Default가 최선의 옵션입니다.
- 메모리 집약적인 작업을 할 때 더 많은 쓰레드가 더 나은 결과를 줄 수도 있어요 (하지만 그 정도는 큰 차이가 없어요).

# 병렬로 호출 실행

두 가지 작업을 동시에 실행하고 두 작업이 모두 완료된 후에 결과를 반환하려고 할 때:

## 범위에 액세스할 수 있는 경우(ViewModel에서 예를 들어)

<div class="content-ad"></div>

```kotlin
suspend fun getConfigFromAPI(): UserConfig {
  // API 호출이나 다른 지연 함수 호출을 수행합니다
}

suspend fun getSongsFromAPI(): List<Song> {
  // API 호출이나 다른 지연 함수 호출을 수행합니다
}

fun getConfigAndSongs() {
  // scope는 viewModelScope 등 원하는 스코프일 수 있습니다
  scope.launch {
    val userConfig = async { getConfigFromAPI() }
    val songs = async { getSongsFromAPI()}
    return Pair(userConfig.await(), songs.await())
  }
}
```

예를 들어 페이징된 API가 있다고 가정하고 사용자에게 보여주기 전에 모든 페이지를 다운로드하고 싶지만 병렬로 모든 페이지를 다운로드하고 싶은 경우:

```kotlin
suspend fun getSongsFromAPI(page: Int): List<Song> {
  // API 호출 수행
}
const val totalNumberOfPages = 10

fun getAllSongs() {
  // scope는 viewModelScope 등 원하는 스코프일 수 있습니다
  scope.launch {
    val allSongs = (0 until totalNumberOfPages)
                  .map { page -> async { getSongsFromAPI(page) } }
                  .flatMap { it.await() }
  }
}
```

async / await에 대한 참고 사항. 코루틴은 호출되는 즉시 시작됩니다. async는 Deferred`T` 타입의 객체를 반환합니다 (예: Deferred`List`Song`입니다). Deferred에는 값을 반환하는 지연 함수 await가 있으며 준비되면 값을 반환합니다.


<div class="content-ad"></div>

## 접근 권한이 없는 경우 (예: 저장소에서)

저장소나 사용 사례에서 동시에 2개 이상의 호출을 시작하는 코루틴을 정의하려고 합니다. 문제는 async를 사용하기 위해 scope가 필요하지만 viewModel이나 presenter가 아니기 때문에 여기에서 scope에 액세스할 수 없다는 것입니다 (scope를 인수로 전달하는 것은 좋은 해결책이 아님을 기억해 주세요)

위 예제에서:

```js
suspend fun getConfigAndSongs(): Pair<UserConfig, List<Song> = coroutineScope {
   val userConfig = async { getConfigFromAPI() }
   val songs = async { getSongsFromAPI()}
   Pair(userConfig.await(), songs.await())
}
```

<div class="content-ad"></div>

# 코루틴이 취소된 경우의 정리

만약 코루틴이 취소된다면, 취소 상태를 가지게 되고 그 후에 취소된 상태로 전환됩니다. 코루틴이 취소 중일 때는 필요에 따라 일부 정리 작업을 할 수 있습니다 (예를 들어 작업이 성공적으로 실행되지 않았기 때문에 로컬 DB를 정리하거나 작업이 성공하지 못했음을 서버에 알리기 위한 API 호출 실행).

`finally` 블록을 사용하여 작업을 실행할 수 있습니다.

```js
viewModelScope.launch {
  try {
    // 여기에서 일부 중단 함수 호출
  } finally {
    // 여기에서 정리 작업 실행
  }
}
```  

<div class="content-ad"></div>

하지만 정리하는 동안 일시 중지 작업은 허용되지 않습니다. Suspend 기능을 실행해야 하는 경우 다음과 같이 수행해야 합니다.

```js
viewModelScope.launch {
  try {
    // 여기서 일부 중지 함수 호출
  } finally {
    withContext(NonCancellable) {
      // 여기서 정리용 일시 중지 함수 실행
    }
  }
}
```

참고: 취소는 첫 번째 일시 중지 지점에서 발생합니다. 취소는 함수에 일시 중지 지점이 없는 경우 발생하지 않습니다.

# Coroutine이 완료될 때 정리하기

<div class="content-ad"></div>

코루틴이 취소될 때 정리를 하는 것과 마찬가지로 코루틴이 완료 또는 취소된 상태에 도달했을 때 작업을 실행하고 싶을 수 있습니다.

```kotlin
suspend fun myFunction() = coroutineScope {
  val job = launch { /* 여기에 중단 호출 */ }
  job.invokeOnCompletion { exception: Throwable -> 
    // 여기에서 작업 수행
  }
}
```

# 자식 코루틴 중 하나가 실패해도 코루틴을 취소하지 않는 방법

SupervisorJob을 사용하면 자식 코루틴에서 발생하는 모든 예외를 무시할 수 있습니다.

<div class="content-ad"></div>

## 코루틴 스코프 만들기

```js
val scope = CoroutineScope(SupervisorJob())
// 하나가 오류를 던지더라도 다른 코루틴은 취소되지 않습니다
scope.launch { myFirstCoroutine() }
scope.launch { mySecondCoroutine() }
```

## 스코프 함수 사용하기

```js
suspend fun myFunction() = supervisorScope {
  // 하나가 오류를 던지더라도 다른 코루틴은 취소되지 않습니다
  launch { myFirstCoroutine() }
  launch { mySecondCoroutine() }
}
```

<div class="content-ad"></div>

## 예외 처리하기

```kotlin
suspend fun myFunction() {
  try {
    coroutineScope {
      launch { myFirstCoroutine() }
    }
  } catch (e: Exception) {
    // 에러 처리
  }
  try {
    coroutineScope {
      launch { mySecondCoroutine() }
    }
  } catch (e: Exception) {
    // 에러 처리
  }
}
```

CancellationException은 부모로 전파되지 않고 현재 코루틴만 취소됩니다. CancellationException을 확장하여 부모로 전파되지 않는 자체 예외 유형을 만들 수 있습니다.

# 예외 발생 시 기본 동작 정의

<div class="content-ad"></div>

CoroutineExceptionHandler을 사용하면 예를 들어 서버가 401로 응답할 때 자동으로 사용자를 로그아웃할 수 있습니다.

```js
val handler = CoroutineExceptionHandler { context, exception ->
  // 다이얼로그 표시 또는 오류 메시지와 같은 기본 동작 정의
}
val scope = CoroutineScope(SupervisorJob() + handler)
scope.launch { /* 여기서 중단 호출 */ }
scope.launch { /* 여기서 중단 호출 */ }
```

# 비필수 작업 실행

<div class="content-ad"></div>

만약 에러가 발생하더라도 (예를 들어 한 가지가 취소되어도 부모 코루틴은 유지되어야 하는) 다른 함수에 영향을 미치지 않는 중단 함수를 실행하려면 어떻게 해야 할까요?

예시: 분석 호출

```kotlin
val nonEssentialOperationScope = CoroutineScope(SupervisorJob())

suspend fun getConfigAndSongs(): Pair<UserConfig, List<Song> = coroutineScope {
   val userConfig = async { getConfigFromAPI() }
   val songs = async { getSongsFromAPI()}
   nonEssentialOperationScope.launch { /* 중요하지 않은 작업 여기서 수행 */ }
   Pair(userConfig.await(), songs.await())
}
```

이상적으로는 nonEssentialOperationScope를 클래스에 주입하는 것이 좋습니다 (테스트하기 쉽기 때문입니다).

<div class="content-ad"></div>

# 동기화 문제를 피하기 위해 단일 스레드에서 작업 실행 중

```kotlin
suspend fun myFunction() = withContext(Dispatchers.Default.limiteParallelism(1)) {
  // 여기에 일시 중단 호출
}
// Dispatchers.IO도 사용할 수 있음
```

## 멀티스레딩에서 동기화 문제를 피하는 다른 접근 방법

Java의 AtomicReference를 사용할 수 있습니다.

<div class="content-ad"></div>

```kotlin
private val myList = AtomicReference(listOf(/* 여기에 객체 추가 */))

suspend fun fetchNewElement() {
  val myNewElement = // 새 요소 가져오는 과정
  myList.getAndSet { it + myNewElement }
}
```

또는 Mutex를 사용하는 경우

```kotlin
val mutex = Mutex()
private var myList = listOf(/* 여기에 객체 추가 */)

suspend fun fetchNewElement() {
  mutex.withLock {
    val myNewElement = // 새 요소 가져오는 과정
    myList = myList += myNewElement
  }
}
```

# 동일한 디스패처로 코루틴을 다시 디스패치하는 것을 피하십시오


<div class="content-ad"></div>

현재 기존 디스패처인 경우 디스패처를 전환하는 비용을 피하세요:

```js
// 필요한 경우에만 디스패치됩니다
suspend fun myFunction() = withContext(Dispatcher.Main.immediate) {
  // 여기서 중단 호출
}
```

현재 Dispatchers.Main만 즉시 디스패치를 지원합니다.

읽어주셔서 감사합니다! 코틀린 코루틴에 대한 인사이트와 프로팁을 공유해주세요!

<div class="content-ad"></div>

모든 코루틴에 대해 깊이 파고들고 그들이 어떻게 작동하는지 더 잘 이해하려면 Maricin Moskała의 이 책을 강력히 추천합니다: