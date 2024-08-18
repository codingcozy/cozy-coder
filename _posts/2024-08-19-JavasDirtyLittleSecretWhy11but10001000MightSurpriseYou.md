---
title: "자바의 오류 메시지 1  1이지만 1000  1000은 놀라울 수도 있어"
description: ""
coverImage: "/assets/img/2024-08-19-JavasDirtyLittleSecretWhy11but10001000MightSurpriseYou_0.png"
date: 2024-08-19 02:39
ogImage: 
  url: /assets/img/2024-08-19-JavasDirtyLittleSecretWhy11but10001000MightSurpriseYou_0.png
tag: Tech
originalTitle: "Javas Dirty Little Secret Why 1  1 but 1000  1000 Might Surprise You"
link: "https://medium.com/@patidar05priya/javas-dirty-little-secret-why-1-1-but-1000-1000-might-surprise-you-1e44acfe2c6a"
isUpdated: false
---



![JavasDirtyLittleSecretWhy11but10001000MightSurpriseYou_0.png](/assets/img/2024-08-19-JavasDirtyLittleSecretWhy11but10001000MightSurpriseYou_0.png)

자바는 직관적이라고 알려져 있지만 가끔씩은 예기치 못한 상황이 발생할 때가 있습니다. 그 중 하나는 Integer 객체를 비교할 때 발생하는 문제입니다. 예를 들어, 1000 == 1000은 참이라고 생각할 수 있지만, 자바에서는 그렇지 않을 수도 있습니다. 그 이유를 알아보겠습니다.

# 기본 자료형 vs. 객체 비교

자바에서 두 개의 기본 자료형인 int a = 1; int b = 1;을 a == b로 비교하면 결과는 참이 됩니다. 이는 기본 자료형은 값으로 비교되기 때문에 간단하게 처리됩니다.

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

이제 Integer 객체(정수의 래퍼 클래스)를 사용할 때 흥미로운 부분이 시작됩니다. ==를 사용하여 두 Integer 객체를 비교할 때, 그들의 값이 아닌 참조를 비교합니다. 이것이 혼란의 가능성이 들어오는 곳입니다.

# 특이점: Integer 캐싱

Java는 -128에서 127 범위 내에 있는 Integer 객체를 캐싱하여 메모리 사용량을 최적화합니다. 이 범위 내의 값을 가진 Integer를 생성하면, Java는 해당 값으로 이미 캐시에 존재하는 객체가 있는지 확인합니다. 만약 있다면, Java는 해당 객체를 재사용합니다.

아래는 설명을 위한 예시입니다:

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

```java
Integer x = 1;
Integer y = 1;
System.out.println(x == y); // Output: true
```

이 경우 x와 y는 동일한 캐시된 객체를 가리키기 때문에 x == y는 true를 반환합니다.

그러나 캐시된 범위를 벗어나면 어떻게 될까요?

```java
Integer a = 1000;
Integer b = 1000;
System.out.println(a == b); // Output: false
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

이제 a와 b는 동일한 객체를 가리키지 않습니다. 1000은 -128에서 127 범위 밖에 있기 때문에 Java는 매번 새 Integer 객체를 생성하며, a == b는 두 개의 다른 참조를 비교하여 false를 반환합니다.

비트 변환 설명:

더 깊이 파헤쳐서 1000이 이 캐시된 범위에 속하지 않는 이유를 이해해 봅시다:

- 이진수로 표현하면 1000은 1111101000으로 나타납니다.
- 이 이진 표현에는 10비트가 필요합니다.

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

하지만, -128부터 127까지의 캐싱 범위는 8비트 내에서 표현할 수 있는 값에 해당합니다. 예를 들어:

- 127을 이진수로 나타내면 01111111이며, 이는 8비트 내에 들어갑니다.
- -128을 이진수로 나타내면 (2의 보수를 사용하여) 10000000이며, 이 또한 8비트 내에 들어갑니다.

그러나 1000은 8비트보다 많은 비트가 필요하므로 캐시 범위를 벗어나며, Java는 각 발생마다 캐시된 인스턴스를 사용하는 대신 새 Integer 객체를 생성합니다.

# 값 비교를 위해 .equals()를 사용하세요

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

위험 요소를 피하기 위해 Integer 객체를 비교할 때 항상 .equals()를 사용하세요:

```js
System.out.println(a.equals(b)); // 결과: true
```

이 방법은 참조가 아닌 값들을 비교하기 때문에 직관적으로 예상하고 있는 결과를 제공합니다.

# Java는 왜 -128에서 127까지만 캐시하는 걸까요?

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

자바가 -128부터 127까지의 값을 캐시하는 이유가 궁금할 수도 있겠죠? 그 이유는 효율성에 있습니다. 이 값들은 프로그래밍에서 흔히 사용되며, 이를 캐시함으로써 자주 사용되는 숫자에 대해 새 객체를 만드는 오버헤드를 줄일 수 있습니다. 이는 작지만 효과적인 최적화로 시간과 메모리를 절약할 수 있습니다.

# 요약

- 기본 데이터 타입(int)은 값으로 비교됩니다.
- 객체(Integer)는 ==를 사용할 때 참조로 비교됩니다.
- 정수 캐싱은 -128부터 127 범위에 적용됩니다.
- Integer 객체를 비교할 때는 항상 .equals()를 사용하여 값을 비교하세요.

자바에서 정수를 사용할 때 다음부터는 이를 생각해보세요. 가끔 1000 == 1000이 당신을 놀라게 할지도 몰라요!