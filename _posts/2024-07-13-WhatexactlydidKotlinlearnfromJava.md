---
title: "코틀린이 자바에서 배운 것들은 무엇일까"
description: ""
coverImage: "/assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_0.png"
date: 2024-07-13 00:36
ogImage: 
  url: /assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_0.png
tag: Tech
originalTitle: "What exactly did Kotlin learn from Java?"
link: "https://medium.com/@PurpleGreenLemon/what-exactly-did-kotlin-learn-from-java-55f566659b8d"
isUpdated: true
---





지금 이 순간을 상상해 보세요 - 그 해는 2011년입니다.

우리에게 IntelliJ IDEA를 선사해준 놀라운 팀, JetBrains가 Kotlin이라는 새롭고 열정 넘치는 프로그래밍 언어에 대해 발표합니다.

10년이 흘러, 이 신참은 코딩의 큰 리그에서 자리를 잡아 TIOBE 지수 상위 20위 정도에서도 자리를 굳혔으며 구글로부터 안드로이드 앱 개발의 필수 언어로 인정받기도 하였습니다.

하지만 그 반짝거리는 겉모습에 속지 마세요.

<div class="content-ad"></div>

카페인이 가득한 자바의 변형 버전인 코틀린은 아닙니다. 사실, 코틀린은 자바와 교환 카드처럼 잘 어울릴 수 있지만, 그게 전부는 아닙니다. 코틀린은 자기 위에 나있는 형님이 약간의 비결을 가르쳐 줬지만, 스스로의 특징을 들뜨게 펼쳐가는 데 후퇴하지 않습니다.

코틀린의 엔진 아래에서는 자바와 흡사한 리듬으로 징징거립니다.

이건 우연이 아닙니다.

코틀린은 자바와 잘 어울려지기 위해 설계된 것뿐만 아니라, 자바의 성공과 실패로부터 배웠습니다. 서로간의 존중을 바탕으로 한 이 형제 관계는 경쟁 의식 없이는 없지만, 코틀린은 자바의 지혜를 받아들이고 독특한 맛을 더해 재해석한 언어를 낳았습니다.

<div class="content-ad"></div>


![What exactly did Kotlin inherit?](/assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_0.png)

# Kotlin이 정확히 무엇을 상속했을까요?

Java의 정글에서 시간을 보냈다면, Java가 독자성을 갖고 있는 것을 알 것입니다. 견고한 원칙에 기반을 둔 Java는 변화의 폭풍을 겪으며 진화하고 적응하면서도 핵심 정체성을 유지해 왔습니다.

하지만 그것은 독특함과 '빈티지한 매력'이 없는 것은 아닙니다. 여기서 Kotlin의 제작자들이 유지하기로 결정한 Java 지혜의 보물을 해석하여, Kotlin의 정체성의 기반을 형성했습니다.


<div class="content-ad"></div>

## 문법 유사성 및 색다른 맛

프로그래밍 언어는 사람들이 서로 대화하는 방식을 규정하는 독특한 문법을 갖고 있습니다. 코틀린은 많은 면에서 자바의 변형을 사용합니다. 자바의 문법을 차용하고 다듬어 더욱 스무스하게 컴퓨터와 대화할 수 있도록 했죠.

코틀린의 문법은 자바의 것과 뚜렷한 유사성을 띄고 있습니다. 그렇지만 더 간결하고 단순하게 더 많은 내용을 전달합니다. 더 간결한 문법은 더 적은 보일러플레이트 코드로 전환됩니다. 'Hello, World!' 예제를 살펴볼까요?

자바:

```java
```

<div class="content-ad"></div>

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Kotlin:

```kotlin
fun main() {
    println("Hello, World!")
}
```

The Kotlin version is shorter, but if you're familiar with Java, you can still see the similarities. However, Kotlin code is easier to read, showcasing Kotlin's dedication to practicality and efficiency.

<div class="content-ad"></div>

## 객체 지향 프로그래밍(OOP)과 고전의 유산

OOP 세계에서 Java와 Kotlin은 한 묶음입니다. Kotlin은 Java의 OOP 원칙을 그대로 이어받지만, 몇 가지 관행적인 고통을 해소하여 개선했습니다.

Java는 OOP에 크게 의존하지만, Kotlin은 더 많은 유연성을 제공합니다. Kotlin은 OOP를 함수형 프로그래밍 요소와 조화롭게 결합하여 더 현대적이고 혼합된 방식을 만들어냅니다. 그러나 OOP의 본질은 유지됩니다. 이는 Kotlin으로 넘어가는 Java 개발자들에게 다리 역할을 하며, Kotlin이 다양한 Java 프레임워크와 라이브러리를 활용할 수 있게 합니다.

## 예외 처리와 조심스러운 교훈

<div class="content-ad"></div>

자바에서 예외 처리는 의견이 분분한 주제이며, 체크된 예외는 상당히 논란적인 기능입니다. 이들은 개발자가 오류 상황을 적절하게 처리하도록 보장하기 위해 설계되었습니다. 그러나 이들이 부가하는 오버헤드는 실용성에 대한 논쟁을 일으켰습니다.

실용성을 위해, Kotlin은 다른 방향을 취했습니다. 체크된 예외를 없애고 런타임 예외 모델로 이동했습니다. 다음은 예시입니다:

Java:

```java
void readFile(String file) throws IOException {
    // IOException을 던질 수 있는 코드
}
```

<div class="content-ad"></div>

코틀린:

```kotlin
fun readFile(file: String) {
    // IOException을 발생시킬 수 있는 코드
}
```

코틀린에서는 모든 예외가 확인되지 않는(unchecked) 예외이므로 명시적인 예외 선언이 필요하지 않아 더 깔끔하고 간결한 에러 관리 방식을 제공합니다.

## 자바와의 상호 운용성 및 플레이그라운드 공유하기

<div class="content-ad"></div>

카프카가 물으면 대답하리라. Kotlin의 주요 특징 중 하나는 Java와의 상호 운용성이다. Kotlin과 Java 파일은 동일한 프로젝트 내에서 원활하게 상호 작용하여 공존할 수 있다. 이는 Kotlin이 Java 개발자들에게 부드러운 학습 곡선을 제공하기 위한 노력의 증거이며, 마치 새로운 세계로 들어가면서도 기존의 즐겨 사용하던 것들을 그대로 가져갈 수 있는 것과 같다.

Kotlin이 Java와의 상호 운용성을 가능하게 하는 마법은 JVM 호환성에 있다. Kotlin은 JVM과 완벽하게 호환되어 Java 라이브러리와 프레임워크를 네이티브로 사용할 수 있기 때문에 Kotlin으로 진출하는 개발자들에게 안전망을 제공한다. Kotlin으로 전환한다고 해서 Java 도구와 자원의 풍부함을 뒤로 놓아야 하는 것은 아니다. 오히려, 이는 옛 좋아하는 장난감을 그대로 가져가면서 새로운 장난감을 얻는 느낌과 같은 것이다.

![image](https://blog.example.com/assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_1.png)

# Kotlin이 Java를 개선한 방법

<div class="content-ad"></div>

현명한 사람은 자신의 실수로 배우지만, 그보다 더 현명한 사람은 다른 사람의 실수로 배운다고 하죠. Kotlin은 이 속담을 심각하게 받아들여 Java가 개척한 길에 새로운 시각을 제공했습니다. 이 섹션에서는 Kotlin이 Java의 악명 높은 고통점 중 일부를 해결하고 보다 효과적인 해결책을 도출하는 방법을 탐구해 보겠습니다.

## Null 안전성: NullPointerException 소멸

NullPointerException. 이는 많은 Java 개발자들의 삶의 적이죠. 창조자인 Tony Hoare 박사가 천억 달러의 실수로 부르기도 했던 이 널 참조는 예기치 못한 충돌의 형태로 비참한 결과를 가져올 수 있습니다.

여기에 Kotlin의 null 안전성이 등장합니다. Kotlin은 nullable(널 허용)과 non-nullable(널 비허용) 유형을 구분함으로써 언어 수준에서 널(Null) 여부를 관리합니다. 간단한 비교를 하자면:

<div class="content-ad"></div>

자바:

```js
String str = null; // 허용됨
```

코틀린:

```js
var str: String = null // 컴파일 오류
var str: String? = null // 허용됨
```

<div class="content-ad"></div>

코틀린에서는 Nullable이 아닌 변수에 null을 할당하려고 시도하면 컴파일 오류가 발생합니다. 이로써 잠재적인 NullPointerException을 방지하고 코드를 null 관련 문제로부터 안전하게 보호할 수 있습니다.

## 확장 함수: 상속 없이 기능 확장하기

자바에서 클래스의 기능을 확장하려면 일반적으로 원본 클래스를 상속받는 새 클래스를 생성해야 합니다. 그런데 만약 상속이 필요없이 클래스에 함수를 추가할 수 있다면 어떨까요?

코틀린은 확장 함수를 소개하여 개발자들이 클래스에 "메소드"를 추가할 수 있게 합니다. 이 기능은 캡슐화를 훼손하지 않으면서 코드베이스를 간소화하여 유연성을 더해줍니다. 아래는 간단한 예시입니다:

<div class="content-ad"></div>

```kotlin
fun String.shout(): String {
    return this.toUpperCase() + "!!!"
}

fun main() {
    println("hello".shout()) // prints "HELLO!!!"
}
```

위 예제에서 shout 함수는 String 객체에서 호출할 수 있는 확장 함수입니다. String 클래스 자체의 일부가 아닌데도 호출할 수 있는 것이죠.

## 동시성을 위한 코루틴: 부드러운 조작자

자바의 동시성 및 비동기 프로그래밍 접근 방식은 스레드를 활용합니다. 그러나 스레드는 무겁고 관리하기 어려울 수 있어, 경합 조건 및 데드락과 같은 문제가 발생할 수 있습니다.

<div class="content-ad"></div>

코틀린 해결책? '코루틴'입니다. 쓰레드보다 가벼우며 비동기 프로그래밍을 간단하게 만들어주어 가독성을 높이고 실수를 줄여줍니다. 코루틴은 대규모 병렬성을 최소한의 부담으로 가능하게 하여 작업의 효율성을 새로운 차원으로 끌어올립니다. 코틀린의 공식 가이드를 통해 이 기능에 대해 깊이 파헤쳐보세요.

## 스크립팅 기능: 응용 프로그램 이상의 코틀린

자바는 컴파일된 응용 프로그램을 대상으로 설계되었기 때문에 스크립트 작업에는 적합하지 않습니다. 반면 코틀린은 스크립팅 잠재력을 활용합니다.

.kts (Kotlin Script) 형식을 통해 코틀린은 스크립트를 실행할 수 있어 응용프로그램 개발 뿐만 아니라 빌드 스크립트, 테스트 등에서도 다재다능한 선택지가 됩니다. 코틀린은 완전한 프로그래밍 언어와 스크립트 언어 사이의 간극을 메워주며 양쪽의 장점을 결합합니다.

<div class="content-ad"></div>

## 유추형과 기본 인수: 작은 것들

가끔, 작은 것들이 모든 차이를 만들어 냅니다. Kotlin은 개발자 경험을 향상시키기 위해 몇 가지 멋진 기능을 소개합니다: 유추형과 기본 인수.

유추형은 Kotlin이 변수의 유형을 초기화값으로부터 결정할 수 있게 해줍니다. 이로 인해 가독성이 향상됩니다:

```kotlin
val name = "Kotlin" // Kotlin은 name이 String 유형임을 유추합니다
```

<div class="content-ad"></div>

디폴트 인수는 오버로드된 함수가 필요하지 않게 하고 함수 디자인을 간단하게 만듭니다.  

카틀린은 그림과 같이 좋은 예로, 이러한 작은 기능들이 모여 코딩 프로세스를 좀 더 원활하게 해줍니다. 코드의 가독성과 유지 보수성을 향상시켜줍니다.

![2024-07-13 WhatexactlydidKotlinlearnfromJava_2](/assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_2.png)

<div class="content-ad"></div>

# 코틀린이 자바에서 남겨둔 것

혁신은 종종 어느 정도의 놓아주기를 요구합니다. 코틀린에게는 자바의 일부를 떠나야 했습니다. 이는 "좋은" 것들을 선택하고 "나쁜" 것들을 버리는 과정이 아니었습니다. 그 대신에, 자바의 임무를 수행하는 데 무엇이 작동하는지를 분석하고, 코틀린만의 비전을 이루기 위해 무엇이 발전하거나 제외될 수 있는지를 고민한 것이었습니다. 이 섹션에서는 이러한 신중하게 계산된 교환이 몇 가지에 대해 논의하겠습니다.

## 검사 예외: 더 유연한 입장

자바의 검사 예외는 뜨거운 논쟁거리였습니다. 예외를 메소드의 계약의 일부로 만들어 안전한 오류 처리를 강제하기 위한 것이지만, 비판자들은 코드를 혼란스럽게 만들고 빈 catch 블록과 예외 무시로 이어진다고 주장합니다.

<div class="content-ad"></div>

Kotlin은 다른 입장을 취합니다. 예외의 개념을 유지하면서도 체크된 예외와 비체크된 예외를 구분하지 않습니다. Kotlin에서 모든 예외는 비체크된 상태이며, 이는 선언하거나 잡을 필요가 없음을 의미합니다. 이는 오류 처리에 더 유연하고 격식을 갖추지 않는 접근 방식을 도입합니다.

## Static 키워드: Kotlin의 동반 객체

Java에서 static 키워드는 메소드가 클래스에 속하게 하여 클래스의 인스턴스가 아니라는 것을 허용합니다. 그러나 Kotlin은 static 키워드를 아예 버리기로 결정했습니다. 그 이유는 무엇일까요? Kotlin은 객체지향 프로그래밍에 중점을 두고 있기 때문입니다.

유사한 기능을 제공하기 위해 Kotlin은 동반 객체를 소개했습니다. 동반 객체는 클래스 내부에 선언된 싱글톤 객체로, 그 멤버들은 클래스 이름을 통해 접근할 수 있습니다. 이는 Java의 static 메소드와 비슷한 기능을 제공합니다.

<div class="content-ad"></div>

요즘 Kotlin에서 어떻게 하는지 예시를 들어볼게요:

```kotlin
class MyClass {
    companion object {
        fun printHello() {
            println("Hello")
        }
    }
}

fun main() {
    MyClass.printHello() // "Hello"이 출력됩니다.
}
```

그 static 키워드의 부재는 Kotlin의 더 넓은 디자인 철학 중 하나에 속해요.

## 기본 유형: 유형 시스템 통일하기

<div class="content-ad"></div>

자바에서는 int, boolean 등과 같은 기본 유형이 참조 유형(객체)과 다르게 처리됩니다. 이들은 다른 구문을 갖고 있고, 메모리에 다르게 저장됩니다. 이 이중성은 때때로 개발자들을 혼동시킬 수 있습니다, 특히 초보자들에게는.

반면에 코틀린은 모든 것을 객체로 취급하여 유형 시스템을 통합합니다. 코틀린에서는 소위 기본 유형인 것들조차 실제로는 내부적으로 객체로 취급됩니다. 그럼에도 불구하고 성능에 절충하지 않습니다. 코틀린 컴파일러는 바이트 코드로 컴파일될 때 원시 유형을 사용하는 똑똑함을 가지고 있어서 가장 좋은 두 가지 세계를 제공합니다: 간단하고 통합된 유형 시스템과 효율적인 메모리 사용.

```kotlin
val i: Int = 10 // i는 Int 형식의 객체입니다. 그러나 원시 int로 컴파일될 것입니다.
```

# 현역에서 활약하는 Kotlin

<div class="content-ad"></div>

Kotlin의 인기 상승은 단순히 구문적 설탕이나 Java에 대한 개선만으로 만들어진 것이 아닙니다. 그 실제 성과는 전 세계의 개발자들에 의해 수용되고 현실 세계 프로젝트에서 성공적으로 적용되었다는 데 있습니다. 이러한 성공 사례들 중 몇 가지를 살펴보겠습니다.

## Trello: 개발 속도와 효율 향상

유명한 프로젝트 관리 애플리케이션인 Trello는 Kotlin을 초기부터 안드로이드 앱 개발에 도입했습니다. 개발 팀은 Kotlin의 표현적인 구문과 견고한 널 안전성으로 인해 개발 속도와 효율성이 상당히 향상되었다고 보고했습니다.

그들은 Kotlin의 확장 함수를 통해 코드 중복을 줄일 수 있다는 점을 감사히 여겼습니다. 또한 Kotlin의 코루틴 사용으로 이전에 콜백으로 가득 찼던 비동기 코드를 간단하게 만들 수 있었습니다.

<div class="content-ad"></div>

## Coursera: 향상된 가독성과 유지보수성

교육 플랫폼 Coursera는 안드로이드 애플리케이션에 Kotlin을 통합했습니다. 팀은 Kotlin의 가독성을 칭찬하며 이로 인해 코드 기반을 이해하기 쉽고 유지보수하기 쉬워졌다고 언급했습니다. 그들에 따르면, Kotlin의 간결함과 타입 추론은 코드 검토 프로세스를 향상시키고 버그를 줄이는 데 중요한 역할을 했다고 합니다.

## Pinterest: Java와의 원활한 상호운용성 

Pinterest는 최초로 Kotlin을 도입한 주요 기업 중 하나였습니다. 그들을 가장 놀라게 한 것은 Kotlin이 Java와의 원활한 상호운용성이었습니다. 기존의 Java 라이브러리와 프레임워크를 매끄럽게 사용할 수 있는 능력 덕분에 기능 개발을 멈추지 않고 코드 기반을 점진적으로 이전할 수 있었습니다. 개발자들은 또한 Kotlin의 null 안전 기능을 선호했는데, 이것이 NullPointerException을 상당히 줄여준다고 말했습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-WhatexactlydidKotlinlearnfromJava_3.png)

# 그 다음에는?

코틀린은 자바에게 빚을 지고 있음을 인정하면서도 자신만의 정체성을 버리지 않습니다. 자바의 OOP 원칙의 견고함, 문법의 익숙함 및 상호 호환성을 전달함으로써 코틀린은 자바 개발자들이 안전하게 전환할 수 있는 안전한 보금자리를 제공합니다. 그러나 여기서 끝나는 것이 아닙니다. 널 안전성, 코루틴 및 확장 함수와 같은 개선 사항으로 코틀린은 자바의 오랜 고통을 개선합니다.

이 전통과 혁신의 융합은 종이 위에서만 존재하는 것이 아닙니다. 세계에서 가장 인기 있는 애플리케이션인 Trello, Coursera 및 Pinterest를 실행하여 개발자들이 더 나은, 더 신뢰할 수 있는 코드를 작성할 수 있도록 합니다.

<div class="content-ad"></div>

코틀린의 이야기는 코딩 여행을 떠나거나 새 프로그래밍 언어를 만드는 누구에게나 귀중한 교훈을 전합니다. 거인의 어깨에 서도록 하지만 독자적인 길을 찾는 데 겁먹지 마세요. 결국 진화는 이전 것 중 가장 좋은 부분을 취하고 독특한 본질을 더하는 것이라고 할 수 있죠. 🌟