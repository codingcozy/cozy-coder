---
title: "코틀린 플로우로 타임아웃 작업 마스터하기"
description: ""
coverImage: "/assets/img/2024-08-18-MasteringTimeoutOperationswithKotlinFlows_0.png"
date: 2024-08-18 10:48
ogImage: 
  url: /assets/img/2024-08-18-MasteringTimeoutOperationswithKotlinFlows_0.png
tag: Tech
originalTitle: "Mastering Timeout Operations with Kotlin Flows"
link: "https://medium.com/@sandeepkella23/mastering-timeout-operations-with-kotlin-flows-cb8a45a3014e"
isUpdated: true
updatedAt: 1723951656393
---

![2024-08-18-MasteringTimeoutOperationswithKotlinFlows_0.png](/assets/img/2024-08-18-MasteringTimeoutOperationswithKotlinFlows_0.png)

친구가 항상 늦게 온다고 기다리는 일이 있었나요? 카페에 앉아 커피를 마시며 시계를 몇 분마다 한 번씩 쳐다보곤 했습니다. 마침내, "5분 안에 안 오면 나간다!" 하고 결정합니다. 이것이 바로 프로그래밍에서의 타임아웃 동작 방식입니다. 어떤 일이 일어나기를 기다리다가 일정 기간 안에 일어나지 않으면 기다리는 것을 멈추고 다음 단계로 넘어가는 것입니다. 오늘은 Kotlin Flows를 사용하여 타임아웃 동작을 구현하는 방법을 살펴보겠습니다. 이를 통해 코드에서 너무 오래 기다리지 않도록 할 수 있습니다.

# Kotlin Flows란?

타임아웃에 대해 알아보기 전에, Kotlin Flows가 무엇인지 간단히 되짚어봅시다. Kotlin Flows는 데이터의 꾸준한 흐름과 같습니다. 공장의 컨베이어 벨트를 상상해보세요. 물건(데이터)이 계속 이동하는 컨베이어 벨트가 있습니다. 컨베이어 벨트 옆에 서서 물건이 지나가면 주울 수 있고, 한 번에 하나씩 처리할 수 있습니다. Kotlin Flows도 비슷하게 작동하며 시간이 지나면서 값들을 방출하고, 이 값을 비동기적으로 수집할 수 있습니다.

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

# 왜 타임아웃 작업이 필요한가요?

가끔 데이터가 도착하는 것을 기다리는 데 시간이 너무 오래 걸릴 수 있고, 앱이 영원히 멈춰 있기를 원치 않을 것입니다. 이때 타임아웃이 유용합니다. 카페에서 너무 오래 기다린 후에 나가는 것처럼, 데이터가 일정 시간 내에 도착하지 않으면 기다리는 것을 멈추고 다른 일을 할 것을 Flow에 알릴 수 있습니다.

# Kotlin 플로우로 타임아웃 작업 구현하기

여러분이 Kotlin 플로우에서 타임아웃 작업을 구현하는 방법을 알아보겠습니다.

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

## 단계 1: 간단한 Flow 생성하기

먼저, 일정한 간격으로 값을 방출하는 간단한 Flow를 만들겠습니다. 몇 초마다 메시지를 보내야 하는 친구처럼 생각해보세요.

```js
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.flow
import kotlinx.coroutines.delay

fun simpleFlow(): Flow<Int> = flow {
    for (i in 1..5) {
        delay(1000L) // 1초마다 값을 방출
        emit(i)
    }
}
```

여기서 simpleFlow()는 매 초 메시지를 보내는 당신의 친구와 같습니다. emit(i)가 호출될 때마다 새 값이 컨베이어 벨트(Flow)를 통해 전송됩니다.

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

## 단계 2: 타임아웃 적용하기

이제, 메세지를 기다리는 시간을 2초를 넘기고 싶지 않다고 가정해 봅시다. 타임아웃 연산자를 사용하여 다음 값을 기다릴 시간 제한을 설정할 수 있습니다.

```js
import kotlinx.coroutines.flow.collect
import kotlinx.coroutines.flow.flow
import kotlinx.coroutines.flow.timeout
import kotlinx.coroutines.runBlocking
fun main() = runBlocking {
    try {
        simpleFlow()
            .timeout(2000L) // 2초의 타임아웃 설정
            .collect { value ->
                println("Received: $value")
            }
    } catch (e: Exception) {
        println("타임아웃 발생: ${e.message}")
    }
}
```

- timeout(2000L): 2초 안에 다음 값이 도착하지 않으면 타임아웃이 발생합니다.
- collect { value -> ... }: Flow에서 각 값을 수집하고 처리합니다.
- catch (e: Exception): 타임아웃이 발생하면 친구가 나타나지 않아 카페를 나가는 것처럼 예외를 우아하게 처리합니다.

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

## 단계 3: 테스트해보세요

위의 코드를 실행해보세요. Flow가 2초보다 빠르게 값을 내보내면 모든 것이 잘 작동하는 것을 알 수 있을 겁니다. 하지만 이벤트 사이에 2초 이상의 지연이 발생하면 타임아웃이 발생하고 "시간 초과가 발생했습니다"라는 메시지가 표시됩니다.

# 타임아웃을 세련되게 다루기

타임아웃은 짜증나긴 하지만 막다른 길은 아닙니다. 타임아웃을 더 세련되게 처리하기 위해 `timeoutOrNull` 연산자를 사용하여 예외가 발생하는 대신 null을 반환할 수 있습니다.

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
import kotlinx.coroutines.flow.timeoutOrNull
fun main() = runBlocking {
    simpleFlow()
        .timeoutOrNull(2000L) // 타임아웃을 2초로 설정하고 타임아웃 시 null 반환
        .collect { value ->
            if (value == null) {
                println("시간 내에 값이 수신되지 않았습니다. 계속 진행합니다!")
            } else {
                println("받은 값: $value")
            }
        }
}
```

이제 예외를 던지는 대신에 타임아웃이 발생하면 Flow가 null을 반환하므로 상황을 더 우아하게 처리할 수 있습니다.

# 마무리

Kotlin Flows의 타임아웃 작업은 얼마나 오랫동안 기다릴 의지가 있는지에 대한 시간 제한을 설정하는 것과 같습니다. 이를 통해 앱이 반응성을 유지하고 무제한으로 대기하지 않도록 보장합니다. timeout 및 timeoutOrNull 연산자를 사용하여 데이터가 지연될 수 있는 상황을 쉽게 처리하여 부드럽고 효율적인 사용자 경험을 보장할 수 있습니다.
