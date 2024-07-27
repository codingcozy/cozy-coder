---
title: "Kotlin에서 인라인 함수로 함수 호출 강화하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-EnhanceYourFunctionCallswithInlineFunctionsinKotlin_0.png"
date: 2024-07-07 03:12
ogImage: 
  url: /assets/img/2024-07-07-EnhanceYourFunctionCallswithInlineFunctionsinKotlin_0.png
tag: Tech
originalTitle: "Enhance Your Function Calls with Inline Functions in Kotlin"
link: "https://medium.com/@mahbubmridha07/enhance-your-function-calls-with-inline-functions-in-kotlin-dfe1d938648d"
---


![Image](/assets/img/2024-07-07-EnhanceYourFunctionCallswithInlineFunctionsinKotlin_0.png)

인라인 함수는 코틀린에서 성능을 향상시키기 위해 고안된 강력한 기능으로, 고계 함수와 관련된 오버헤드를 줄이는 데 사용됩니다. 여기에서 모두가 이해해야 하는 내용을 소개합니다.

## 인라인 함수란?

인라인 함수는 코틀린에서 해당 함수의 바이트 코드를 직접 호출 지점에 삽입하는 방식으로, 함수 호출이 필요 없게 만드는 함수입니다. 고계 함수(다른 함수를 매개변수로 취하는 함수)가 포함된 경우 특히 성능 향상을 이끌어 낼 수 있습니다.

<div class="content-ad"></div>

인라인 함수의 장점

- 성능 향상: 함수 호출의 오버헤드를 피함으로써 인라인 함수는 코드를 더 빠르게 실행할 수 있게 도와줍니다. 특히 코드의 성능이 중요한 부분에서 이점을 뚜렷하게 볼 수 있습니다.
- 객체 생성 감소: 고차 함수는 종종 추가 객체를 생성합니다. 인라인 함수는 불필요한 객체 생성을 없애므로 이를 최소화합니다.
- 간단한 바이트 코드: 인라인 함수는 간단한 바이트 코드를 유발함으로써 Just-In-Time (JIT) 컴파일러가 코드를 최적화하는 데 도움을 줍니다.

## 인라인 함수 선언 방법

함수 정의의 시작 부분에 inline 키워드를 추가하세요.

<div class="content-ad"></div>

```kotlin
## 예제로 이해해보죠:

1부터 10까지의 숫자를 처리하는 함수가 있다고 가정해봅시다. 각 숫자마다, 2씩 증가하면서 100까지의 숫자의 합을 계산하고 그 합을 출력합니다.

fun main() {
     for (i in 1..10) {
        startProcess(i) {
            val sum = calculateSum(i)
            println("해당 프로세스는: $i 이고 합계는: $sum")
        }
    }
}


inline fun startProcess(i: Int, block: () -> Unit) {
    println("-------------------------프로세스 $i 시작--------------------------------")
    block()
    println("-------------------------프로세스 $i 끝---------------------------------\n\n")
}

fun calculateSum(from: Int): Int {
    var sum = 0
    for (i in from..100 step 2) {
        sum += i
    }
    return sum
}

<div class="content-ad"></div>

## 설명:

- main(): 1부터 10까지 반복하여 각 숫자에 대해 startProcess(i)를 호출합니다.
- startProcess(): 프로세스의 시작과 끝을 출력하고 제공된 코드 블록을 실행합니다.
- calculateSum(): from부터 100까지 2씩 증가하면서 숫자들의 합을 계산합니다.

이 코드는 각 숫자가 짝수인지 홀수인지 그리고 해당 숫자부터 100까지의 숫자 합을 출력합니다.

# 코드를 디컴파일하여 JVM에서의 차이점을 관찰하세요.

<div class="content-ad"></div>

코틀린에서 인라인 함수를 사용할 때 바이트 코드의 차이를 시연하겠습니다. 먼저, 인라인 키워드 없이 코드를 디컴파일하고 바이트 코드를 관찰합니다. 그런 다음, 함수에 인라인 키워드를 추가하고 다시 디컴파일하여 바이트 코드가 어떻게 변경되는지 확인해 봅니다.

- 인텔리J IDEA에서 코틀린 파일을 엽니다.
- 도구 ` 코틀린 ` 코틀린 바이트 코드 표시로 이동합니다.
- 디컴파일을 클릭하여 해당하는 자바 코드를 확인하세요.

인라인 키워드 없이:

![Image](/assets/img/2024-07-07-EnhanceYourFunctionCallswithInlineFunctionsinKotlin_1.png)

<div class="content-ad"></div>

각 프로세스마다 새 Function0() 객체가 생성된다는 것을 알 수 있어요. 이러한 객체들은 힙 메모리에 할당되며, 이는 메모리와 성능 측면에서 비용이 발생할 수 있어요. 이제 우리는 inline 키워드를 추가하면 어떻게 되는지 알아볼게요.

inline 키워드를 추가한 후:

![EnhanceYourFunctionCallswithInlineFunctionsinKotlin](/assets/img/2024-07-07-EnhanceYourFunctionCallswithInlineFunctionsinKotlin_2.png)

inline 키워드를 추가한 후, 함수가 호출 함수로 직접 복사되어 객체 생성이 필요 없어졌어요. 이로써 메모리 사용량과 성능이 최적화되었답니다.

<div class="content-ad"></div>

# Inline 함수의 장점

- 성능 향상: Inline 함수는 함수 호출의 오버헤드를 줄일 수 있어 작은 함수와 람다 표현에 특히 유용합니다.
- 비지역 반환: Inline 함수는 비지역 반환을 가능하게 합니다. 이는 람다 자체뿐만 아니라 호출하는 함수에서도 반환할 수 있음을 의미합니다.
- Reified 유형 매개변수: Inline 함수에는 reified 유형 매개변수를 사용할 수 있어 실행 중에 유형 매개변수를 실제 유형으로 사용할 수 있습니다.

## 비지역 반환:

Inline 함수의 주요 기능 중 하나는 람다에서 바로 반환할 수 있는 능력입니다.

<div class="content-ad"></div>

**Tarot Reading:** 🌟

inline fun inlineFunctionWithReturn(block: () -> Unit) {
    println("안녕하세요! 이 카드는 여러분에게 전달하고자 하는 메시지를 가지고 있습니다. 지금부터 해석해보겠습니다.

먼저, 'Before block'란 메시지가 보입니다. 이것은 당신의 직감이 가장 중요하다는 신호일 수 있습니다. 그 다음으로 'Inside block'란 메시지가 나타나는데, 이는 당신이 내부 자아와 대면하고 있는 중요한 시간일 수 있습니다. 그리고 '이 부분은 실행되지 않습니다' 라는 메시지가 있습니다. 이것은 현재의 결정이 중요하며, 미래를 바라보는 것이 좋겠다는 힌트를 주고 있습니다.

아래는 다음 주제인 '구체화된 유형 매개변수' 입니다. 함께 살펴보겠습니다. 😉

함수 내에서 실제 유형을 사용할 수 있게 하는 구체화된 유형 매개변수입니다. 코드 내용을 확인해보세요:

inline fun <reified T> printType() {
    println(T::class)
}

fun main() {
    printType<String>() // 출력: class kotlin.String
}

이 카드가 당신에게 주는 메시지를 잘 생각해보세요. 좋은 하루 보내세요! 🌙🔮
```

<div class="content-ad"></div>

## Noinline과 Crossinline

특정 람다 매개변수를 inline 처리하지 않으려면 noinline 키워드로 표시할 수 있습니다:

```kotlin
inline fun inlineFunction(noinline block: () -> Unit) {
    println("블록 호출 전")
    block()
    println("블록 호출 후")
}
```

만약 람다가 비로컬 반환을 사용하지 못하도록 보장하려면, crossinline 키워드로 표시할 수 있습니다:

<div class="content-ad"></div>

```kotlin
inline fun inlineFunctionWithCrossinline(crossinline block: () -> Unit) {
    println("블록을 호출하기 전")
    block()
    println("블록을 호출한 후")
}

fun main() {
    inlineFunctionWithCrossinline {
        println("블록 안에서")
        // return // 이 부분은 컴파일 오류를 발생시킵니다
    }
}
```

```kotlin
inline fun execute(crossinline action: () -> Unit, noinline callback: () -> Unit) {
    // action은 인라인화됩니다
    // callback은 인라인화되지 않습니다
}
```

## 사용 사례와 한계


<div class="content-ad"></div>

**하이어오더 함수:** 다른 함수를 인자로 받는 함수는 인라인화를 통해 가장 큰 이점을 얻을 수 있습니다.

**작은 유틸리티 함수:** 작은 작업을 수행하는 함수는 함수 호출 오버헤드를 피하기 위해 인라인화될 수 있습니다.

## 제한 사항

- 코드 크기 증가: 인라인화의 과도한 사용은 함수가 호출될 때마다 함수 본문이 복사되므로 코드가 비대해질 수 있습니다.
- 복잡한 함수: 인라인 함수는 간단하고 작아야 합니다. 복잡한 함수나 큰 본문을 갖는 함수는 인라인화하기에 적합하지 않습니다.

# 요약

<div class="content-ad"></div>

코틀린의 인라인 함수는 고차 함수를 최적화하고 비지역 반환 및 실체화된 타입 매개변수를 허용하는 강력한 기능입니다. 이들은 함수 호출의 오버헤드를 줄여 코드를 더 효율적으로 만들어 줍니다. 그러나 코드 크기를 불필요하게 늘리지 않도록 신중하게 사용해야 합니다.

더 자세한 정보는 공식 코틀린 문서를 참조해 주세요. 🔮✨