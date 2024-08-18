---
title: "이제 forEach 대신 for of 루프를 사용해야하는 이유"
description: ""
coverImage: "/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_0.png"
date: 2024-07-30 17:20
ogImage:
  url: /assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_0.png
tag: Tech
originalTitle: "Stop Using forEach Transform Your Code with forof Loop  Advanced JavaScript"
link: "https://medium.com/gitconnected/stop-using-foreach-transform-your-code-with-for-of-loop-advanced-javascript-5d9d61a95740"
isUpdated: true
---

JavaScript 및 TypeScript의 세계에서 배열을 반복하는 것은 일반적인 작업입니다. 많은 개발자들이 간편함과 익숙함을 이유로 .forEach를 기본으로 선택합니다.

그러나 더 강력하고 다재다능한 대안이 있습니다: for...of 루프. 🔮

이 글에서는 for...of로 전환을 고려해야 하는 이유와 어떻게 하면 코딩 루틴을 더 효율적이고 표현력있게 만들 수 있는지 살펴보겠습니다.

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

# 기초 이해: .forEach vs. for...of 🔧

# .forEach 메소드

.forEach 메소드는 배열을 순회하는 간단한 방법입니다. 다음은 기본 예제입니다:

![이미지](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_1.png)

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

# for...of 루프

ES6에서 소개된 for...of 루프는 배열을 포함한 반복 가능한 객체를 보다 유연하게 순회하는 방법을 제공합니다:

![image](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_2.png)

# for...of를 선호하는 이유? 🌼

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

.forEach는 간결하고 사용하기 쉽지만, for...of는 코딩 실력을 향상시킬 수 있는 여러 가지 장점을 제공합니다.

## 1. 비동기 처리가 더 나은

비동기 작업을 다룰 때, for...of는 빛을 발합니다. .forEach 메서드는 약속을 네이티브로 처리하지 않기 때문에 async/await와 잘 작동하지 않습니다. 다음 예시를 살펴보세요:

비동기 코드와 함께 .forEach 사용하기

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

<img src="/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_3.png" />

이 코드는 각 fetch 작업이 완료될 때까지 기다리지 않고 다음 작업을 시작하므로 경합 조건과 예상치 못한 결과가 발생할 수 있습니다.

forEach 루프를 사용하여 배열을 반복하는 주요 하향 방향은 이전 호출이 이행될 때까지 연기할 수 없다는 것입니다.

비동기 코드와 함께 for...of 사용하기

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

아래는 예제입니다. 각 fetch 작업은 이전 작업이 완료될 때까지 기다리므로 순차적 실행과 더 예측 가능한 동작이 보장됩니다.

## 2. 반복문의 중단과 계속

forEach 메서드는 break 및 continue 문을 지원하지 않으므로 특정 시나리오에서 그 유연성이 제한될 수 있습니다.

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

for...of를 사용하여 루프 중단하기

![이미지](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_5.png)

for...of를 사용하여 루프 계속하기

![이미지](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_6.png)

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

이러한 기능들은 for...of 루프를 더욱 강력하고 복잡한 반복 논리에 유연하게 사용할 수 있도록 만듭니다.

## 3. 가독성과 유지보수성 향상

for...of 루프는 중첩된 구조물이나 복잡한 작업을 처리할 때 코드의 가독성을 향상시킬 수 있습니다. 👈

이 다차원 배열을 반복하는 예시를 살펴보세요:

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

중첩 반복에 .forEach 사용하기

![이미지](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_7.png)

중첩 반복에 for...of 사용하기

![이미지](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_8.png)

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

for...of 버전은 더 깔끔하고 이해하기 쉬워서 유지 보수가 쉬워집니다.

# for...of 사용에 대한 고급 팁

## Object.entries를 사용하여 객체를 반복

보통 배열과 함께 사용되는 for...of로 Object.entries를 사용하여 객체를 반복할 수도 있습니다:

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

![그림](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_9.png)

## 제너레이터와 함께 for...of 사용하기

제너레이터는 사용자 정의 이터러블을 만드는 강력한 방법을 제공합니다. 제너레이터 함수와 함께 for...of를 사용하는 방법은 다음과 같습니다:

![그림](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_10.png)

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

## for...of과 구조 분해 결합하기

구조 분해는 반복 논리를 더욱 간단하게 만들어줄 수 있습니다:

![image](/assets/img/2024-07-30-StopUsingforEachTransformYourCodewithforofLoopAdvancedJavaScript_11.png)

# 결론

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

JavaScript/TypeScript 프로젝트에서 .forEach에서 for...of로 전환하는 것은 비동기 코드를 더 잘 다루고 루프 실행을 더 잘 제어하며 가독성을 향상시키는 여러 이점을 제공할 수 있습니다.

그러니 다음에 .forEach를 사용하려 할 때, for...of를 시도해보는 것을 고려해보세요. 배열 반복을 위한 새로운 좋아하는 도구가 될 수도 있습니다.

즐거운 코딩 되세요!
