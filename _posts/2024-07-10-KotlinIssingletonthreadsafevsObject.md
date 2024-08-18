---
title: "Kotlin 싱글톤과 Object, 어느 쪽이 더 안전한가"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-10 01:47
ogImage:
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Kotlin: Is singleton thread safe? vs Object"
link: "https://medium.com/@hiteshdhamshaniya-wvmagic/kotlin-is-singleton-tread-safe-vs-object-3414d8e79259"
isUpdated: true
---

# Kotlin에서의 싱글톤

안녕하세요! Kotlin에서는 object 키워드를 사용하여 싱글톤을 생성합니다. 이를 통해 해당 object의 인스턴스가 애플리케이션 내에서 한 번만 생성되고 사용됨이 보장됩니다.

```kotlin
object Singleton {
    fun doSomething() {
        // 구현 내용
    }
}
```

# Kotlin 싱글톤의 스레드 안전성

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

카드 하나 뽑기에 오신 여러분, 안녕하세요! 코틀린 싱글톤은 본질적으로 스레드 안전합니다. 코틀린 컴파일러는 싱글톤 객체 인스턴스가 처음 액세스될 때 스레드 안전하게 생성되도록 보장합니다. 이는 싱글톤 인스턴스의 스레드 안전성을 보장하기 위해 추가 동기화 메커니즘을 구현할 필요가 없음을 의미합니다.

\`\`\`js
object Singleton {
init {
// 초기화 로직
}
fun doSomething() {
// 구현
}
}
\`\`\`

위 예에서 Singleton 객체는 스레드 안전하게 초기화됩니다. init 블록은 Singleton 객체에 액세스하려는 스레드가 몇 개든 간에 한 번만 실행됩니다.

# 코틀린 오브젝트와 스레드 안전성

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

코틀린에서 객체는 본질적으로 싱글턴입니다. 싱글턴에 대해 설명된 스레드 안전성 속성들은 코틀린에서 object 키워드를 사용하여 선언된 모든 객체에 적용됩니다.

# 싱글턴과 객체의 차이점

코틀린에서는 명시적인 "싱글턴" 키워드가 없으며 대신 object 키워드가 사용되어 싱글 인스턴스 클래스를 생성합니다. 이는 사실상 싱글턴이기 때문에 코틀린에서는 객체가 본질적으로 싱글턴입니다. 따라서 코틀린에서는 두 용어를 서로 바꿔 사용할 수 있습니다.

## 객체의 예시:

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

**NetworkManager 클래스 업데이트**

```kotlin
object NetworkManager {
    fun sendRequest() {
        // 네트워크 요청 보내기
    }
}
```

**코틀린 객체의 쓰레드 안전성**

이전에 언급한대로, 코틀린은 NetworkManager 객체가 처음 액세스될 때 쓰레드 안전을 보장합니다. 여러 쓰레드에서 안전하게 sendRequest()를 호출할 수 있으며 여러 인스턴스의 생성이나 동시 초기화 문제를 걱정하지 않아도 됩니다.

**레이지 초기화를 통한 쓰레드 안전성**

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

만약 싱글톤의 초기화에 대해 더 많은 제어를 원한다면, Kotlin의 lazy delegation을 사용할 수 있습니다. 이를 통해 커스텀 초기화 로직을 정의하고 요구사항에 맞게 스레드 안전성을 보장할 수 있습니다.

```javascript
class MySingleton private constructor() {
    companion object {
        val instance: MySingleton by lazy(LazyThreadSafetyMode.SYNCHRONIZED) {
            MySingleton()
        }
    }
}
```

위 예시에서는 LazyThreadSafetyMode.SYNCHRONIZED를 사용하여 MySingleton 인스턴스가 지연 초기화되고 스레드 안전하게 초기화됩니다.

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

**Kotlin 싱글톤 (object 키워드를 사용하여 생성된)은 본질적으로 스레드 안전합니다.**

**Kotlin 객체(object)는 사실상 싱글톤이며 동일한 스레드 안전성 속성을 공유합니다.**

**더 많은 제어를 위해 느린 초기화를 사용할 수 있으며 생성 및 초기화 프로세스를 적절히 동기화하여 필요한 스레드 안전성을 보장합니다.**

**Kotlin의 디자인을 통해 스레드 안전한 싱글톤과 객체를 손쉽게 생성할 수 있어 수동 동기화로 인한 복잡성과 잠재적 오류를 줄일 수 있습니다.**
