---
title: "코틀린에서 비동기 처리를 하기 위한 방법"
description: ""
coverImage: "/assets/img/2024-08-18-KotlinCoroutinesGuide_0.png"
date: 2024-08-18 10:46
ogImage: 
  url: /assets/img/2024-08-18-KotlinCoroutinesGuide_0.png
tag: Tech
originalTitle: "Kotlin Coroutines Guide"
link: "https://medium.com/@daniel.atitienei/kotlin-coroutines-guide-543556ada927"
isUpdated: true
updatedAt: 1723951550908
---

커피 한 잔을 따르고 ☕, 코루틴과 같은 큰 관심사에 대해 이야기해보세요.

![이미지](/assets/img/2024-08-18-KotlinCoroutinesGuide_0.png)

## 코루틴이란?

코루틴은 스레드와 유사하지만 단일 스레드에 바운드되지 않고 훨씬 가벼운 코드 블록을 동시에 실행합니다.

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

## 코루틴 실행

첫 번째 코루틴을 작성해 보겠습니다. runBlocking을 호출하여 코루틴을 실행할 수 있습니다. 이렇게 하면 CoroutineScope가 제공됩니다. 안에 launch 블록을 사용하여 다른 코루틴을 생성해 보겠습니다. launch 블록 안에 delay를 추가하고 "SECOND"를 출력한 다음 launch 블록 아래에 "FIRST"를 출력해 보겠습니다.

이 동작의 설명은 다양한 작업을 비동기적으로 처리하는 여러 코루틴을 가질 수 있다는 것입니다. 이 경우에는 두 번째 코루틴을 지연시키고 주요 코루틴이 멈추지 않습니다.

```js
fun myCoroutine() {
    runBlocking {
        launch {
            delay(1.seconds)
            println("SECOND")
        }
        println("FIRST")
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

## 비동기 & 기다림

fetchProducts와 fetchStores 두 함수가 있다고 가정해 보겠습니다. 데이터를 동시에 가져오고 싶기 때문에 빠른 응답을 원합니다.

이론적으로 async은 launch와 유사하지만 이 함수들의 반환 값에서 차이가 납니다. launch는 어떤 결과 값을 가지지 않는 coroutine Job을 반환합니다. async는 나중에 값을 반환하는 Deffered를 반환합니다.

```kotlin
suspend fun fetchData() {
    val products = async { fetchProducts() }
    val stores = async { fetchStores() }

    products.await()
    stores.await()

    println("완료") // 두 함수가 모두 완료되었을 때 표시됩니다
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

## 코루틴 디스패처

코루틴 디스패처는 작업이 실행되는 스레드를 정의합니다. 우리는 여러 디스패처를 가지고 있어요:

- Dispatchers.Main — 주 스레드에서 실행됩니다. 이는 작업이 완료될 때까지 앱이 차단될 수 있습니다.
- Dispatchers.IO — 이는 백그라운드 스레드에서 실행됩니다. 이는 앱을 차단하지 않습니다.
- Dispatchers.Unconfined — 주 스레드에서 실행됩니다.
- Dispatchers.Default — 공유 스레드 풀을 사용합니다.

## 코루틴 예외 처리

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

코루틴은 예외를 발생시킬 수 있기 때문에 해당 예외를 처리하려면 CoroutineExceptionHandler를 사용해야 합니다. 먼저 CoroutineExceptionHandler를 생성하고 발생한 오류를 출력하는 방법부터 시작해봅시다.

```js
val handler = CoroutineExceptionHandler { coroutineContext, throwable ->
    Log.e("Throwable", "", throwable)
}
```

그 다음으로는 CoroutineScope를 사용하여 새로운 코루틴 스코프를 생성해야 합니다. 여기서 핸들러를 직접 전달하거나 특정 디스패처에 대해 핸들러를 실행할 수 있습니다.

```js
CoroutineScope(handler);
// 또는
CoroutineScope(Dispatchers.IO + handler);
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

그럼 coroutine을 시작하려면 launch를 사용하고, 내부에 Exception을 던져보겠습니다. 이렇게 하면 앱이 크래시되지 않도록 방지할 수 있습니다.

```kotlin
CoroutineScope(Dispatchers.IO + handler).launch {
    throw Exception("Unexpected error")
}
```

요렇게요. 최신 업데이트를 보려면 팔로우하고 뉴스레터를 구독하세요. 더 많은 콘텐츠를 보고 싶다면 X에서 팔로우하고 YouTube 채널을 구독하세요! 읽어 주셔서 감사합니다! 😊☕️

![이미지](/assets/img/2024-08-18-KotlinCoroutinesGuide_1.png)
