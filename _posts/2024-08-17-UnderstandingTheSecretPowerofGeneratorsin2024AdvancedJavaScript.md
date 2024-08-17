---
title: "자바스크립트 고수가 되고 싶다면 알아야할 제네레이터 정리"
description: ""
coverImage: "/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_0.png"
date: 2024-08-17 00:42
ogImage: 
  url: /assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_0.png
tag: Tech
originalTitle: "Understanding The Secret Power of Generators in 2024  Advanced JavaScript"
link: "https://medium.com/javascript-in-plain-english/understanding-the-secret-power-of-generators-in-2024-advanced-javascript-6d191d3961f2"
isUpdated: true
updatedAt: 1723863848135
---


# 소개

자바스크립트는 프로그래밍 언어의 카멜레온처럼 항상 놀라움을 줍니다.

넓은 영역을 다루고 있다고 생각했을 때, 자바스크립트는 또 다른 강력한 기능을 드러내며 깊이 탐험하도록 유도합니다.

오늘은 여러분을 하나의 기능인 Generators를 통해 여행시켜보고 싶습니다.

<div class="content-ad"></div>

JavaScript의 생성기는 처음에는 특정 도구처럼 보일 수 있지만, 한번 그들의 진정한 잠재력을 이해하면, 2024년이라 해도 왜 그들이 여전히 중요하고 강력한지 알게 될 겁니다.

# 생성기란? 👮

우선 기초부터 시작해 봅시다. 생성기는 JavaScript에서 특별한 종류의 함수로, 필요할 때 실행을 일시 중지하고 다시 시작할 수 있게 해줍니다.

한 번에 위에서 아래로 실행되는 일반 함수와 달리, 생성기는 호출 컨텍스트로 제어를 양도하며, 실행 흐름을 더 세밀하게 제어할 수 있게 합니다.

<div class="content-ad"></div>

여기 간단한 예제가 있어요:

![UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_0](/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_0.png)

이 예제에서 simpleGenerator는 한꺼번에 실행되지 않아요. 대신에 각 yield 문에서 일시 중지되며, next()를 명시적으로 호출할 때만 다시 실행됩니다. 이 동작이 제너레이터에 독특한 장점을 제공해요.

# 제너레이터의 마법: 언제 그리고 왜 사용할까요? 🐌

<div class="content-ad"></div>

이제 궁금해하고 있는 것이 있을 수 있습니다. "프라미스 대신 제너레이터를 언제 사용해야 할까요?" 좋은 질문이네요! 제너레이터는 실행 흐름을 세밀하게 제어해야 하는 경우에 특히 유용합니다. 여기 몇 가지 구체적인 사용 사례가 있습니다:

## 1. 게으른 이터레이션(Lazy Iteration)

제너레이터는 실시간으로 생성되는 값의 시퀀스인 게으른 이터레이터를 만들 때 빛을 발합니다. 이는 잠재적으로 무한한 데이터 스트림이나 메모리에 한 번에 모든 것을 로드하는 것이 비현실적인 대규모 데이터 세트를 다룰 때 특히 유용합니다.

![이미지](/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_1.png)

<div class="content-ad"></div>

이 예시에서, 발생자(generator)는 무한한 숫자 시퀀스를 생성하지만, 필요할 때만 다음 값을 계산합니다. 이 접근 방식은 메모리를 절약하고 성능을 향상시킵니다.

## 2. 사용자 정의 제어 흐름

발생자(generator)는 복잡한 제어 흐름을 효과적으로 관리하는 독특한 방법을 제공합니다, 특히 비동기적인 흐름에 유용합니다. 발생자를 프라미스(promises)와 결합하여, 거의 동기식으로 읽히지만 비동기 작업을 내부적으로 처리하는 코드를 작성할 수 있습니다.

![이미지](/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_2.png)

<div class="content-ad"></div>

이 시나리오에서 제너레이터는 fetchDataFromAPI 프라미스가 해결될 때까지 "일시 중지"하여 실행을 수행한 후 검색된 데이터로 계속합니다. 이 방법은 async/await이 표준이 되기 전에 특히 인기가 있었으며, async/await이 제공하는 것보다 더 많은 제어가 필요할 때 유용합니다.

## 3. 상태 기계

상태 기계는 복잡한 애플리케이션에서 흔히 볼 수 있는 패턴이며, 제너레이터를 사용하여 깔끔하고 간결하게 구현할 수 있습니다. 특정 상태에서 양보할 수 있는 능력으로 제너레이터는 상태 간 전환을 관리하기에 뛰어난 적합성을 보여줍니다.

<img src="/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_3.png" />

<div class="content-ad"></div>

이 방식은 응용 프로그램 로직의 다양한 상태를 처리하는 명확하고 유지 보수가능한 방법을 제공합니다.

# 제너레이터 대 프로미스: 결전 🌏

지금쯤이면, 제너레이터가 멋지다고 생각하고 있을 것이며, 그렇습니다! 하지만 언제 프로미스 대신에 제너레이터를 사용해야 할까요?

## 제너레이터의 장점

<div class="content-ad"></div>

- 섬세한 제어: 제너레이터를 사용하면 함수 실행을 멈추거나 다시 시작할 수 있어요. 이것은 게으른 평가, 사용자 정의 반복 논리 또는 복잡한 제어 흐름과 같은 시나리오에서 게임 체인저가 될 수 있어요.
- 메모리 효율성: 제너레이터는 필요한 값만 생성하기 때문에 대용량 데이터나 스트림 처리와 같은 작업에 특히 유용한 메모리 효율적인 기능을 제공해요.
- 가독성 있는 비동기 코드: async/await가 대부분의 비동기 작업을 처리하는 데 사용되고 있지만, 제너레이터는 여전히 가독성과 제어의 고유한 조합을 제공하여 복잡한 비동기 흐름을 더 효과적으로 관리할 수 있어요.

## Promises: 장점

- 간결함과 보편성: Promises는 지금 JavaScript의 표준이 되어 널리 이해되는 부분이에요. 대부분의 비동기 작업에 대해 사용하기가 더 간단하고 라이브러리 및 도구 지원 측면에서 더 나은 지원을 제공하고 있어요.
- 동시성: Promises는 여러 비동기 작업을 동시에 처리해야 하는 경우, 한꺼번에 여러 API 호출을 하는 경우와 같은 상황에서 빛을 발휘해요.
- 에러 처리: async/await를 사용하면 try/catch 블록을 통해 에러 처리가 보다 간단해져 비동기 코드에서 예외를 더 쉽게 관리할 수 있어요.

## 언제 어떤 것을 선택해야 할까요?

<div class="content-ad"></div>

### 생성기를 사용하는 경우:

- 복잡한 제어 흐름을 정밀하게 관리해야 할 때.
- 메모리 효율성이 중요하고 필요 시 값들을 생성하려고 할 때.
- 상태 머신이나 게으른 반복과 같은 패턴을 구현할 때.

### 프로미스를 사용하는 경우:

- 간단한 비동기 작업을 처리할 때.
- 동시 작업을 처리해야 할 때.
- 더 간단하고 직관적인 오류 처리가 필요할 때.

<div class="content-ad"></div>

# 2024년 제너레이터 사용의 최상의 방법 ⛅

이제 '왜'와 '언제'를 다뤘으니, 2024년에 효과적으로 제너레이터를 사용하는 몇 가지 최상의 방법을 살펴보겠습니다.

## 1. 간단하게 유지하기

제너레이터는 코드를 복잡하게 만들 수 있으므로 신중하게 사용하세요. 작업을 프로미스나 async/await로 쉽게 처리할 수 있다면, 제너레이터를 사용할 필요가 없습니다.

<div class="content-ad"></div>

## 2. 약속과 함께 최대 효과를 뽑아보세요

제너레이터와 프로미스는 상호 배타적인 것이 아닙니다. 사실, 두 가지를 아름답게 조합하여 사용할 수 있습니다. 예를 들어, 제너레이터를 사용하여 비동기 흐름을 구조화하고 실제 비동기 작업을 처리하기 위해 프로미스를 사용할 수 있습니다.

## 3. 반복에 유의하세요

반복을 위해 제너레이터를 사용할 때는 항상 멈춰야 할 때를 주의 깊게 살펴보세요. 제너레이터에서의 무한 루프는 제대로 관리되지 않으면 실제로 머리 아플 수 있습니다. 제너레이터가 무한히 실행될 수 있는 가능성이 있다면 명확한 종료 조건이 있는지 확인해주세요.

<div class="content-ad"></div>

## 4. 철저히 테스트하세요

제너레이터의 독특한 실행 흐름 때문에 철저한 테스트가 중요합니다. 발생할 수 있는 모든 실행 경로를 포함하여 테스트를 수행하고, 제너레이터가 예상치 못하게 yield하거나 일찍 종료될 수 있는 예외 상황을 확인하세요.

## 5. 타입 안전성을 위해 TypeScript 활용하기

TypeScript로 코드를 작성하고 있다면, 제너레이터 함수에 대한 타입을 정의해야 합니다. 이는 컴파일 시점에서 잠재적인 문제를 감지하는 추가적인 안전성 계층을 제공해줍니다.

<div class="content-ad"></div>


![Generators](/assets/img/2024-08-17-UnderstandingTheSecretPowerofGeneratorsin2024AdvancedJavaScript_4.png)

# 결론

2024년에는 발표된 지 얼마 되지 않았지만, 제너레이터는 그 매력을 아직도 잃지 않았습니다.

정확한 실행 흐름 제어 능력과 메모리 효율성이 결합된 제너레이터는 개발자들에게 소중한 도구로 작용합니다.


<div class="content-ad"></div>

약속과 async/await도 중요하지만, 제너레이터는 복잡한 작업을 단순화하고 성능을 최적화하는 독특한 관점을 제공합니다.

어떤 도구든 마찬가지로, 중요한 것은 언제 어떻게 사용해야 하는지를 알아야 합니다.

그래서 다음에 직면한 문제가 순수한 비동기적인 해결책 이상을 필요로 한다면, 제너레이터가 필요한 비밀병기일 수 있는지 고려해보세요.

건배!

<div class="content-ad"></div>

# 쉽게 말해 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 저희 글쓴이에게 박수를 보내고 팔로우해주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요