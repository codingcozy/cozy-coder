---
title: "Java Enum 활용 예시 상태 패턴과의 조합 방법"
description: ""
coverImage: "/assets/img/2024-08-18-ImaginingJavaEnums_0.png"
date: 2024-08-18 11:21
ogImage: 
  url: /assets/img/2024-08-18-ImaginingJavaEnums_0.png
tag: Tech
originalTitle: "Imagining Java Enums"
link: "https://medium.com/@viraj_63415/java-enum-in-a-nutshell-192100524776"
isUpdated: true
updatedAt: 1723951513464
---

자바 Enum을 상상하고 프로젝트에서 효과적으로 활용하는 방법을 배워보세요

![Enumerated Type](/assets/img/2024-08-18-ImaginingJavaEnums_0.png)

열거형은 고정된 값 집합을 가질 수 있는 유형을 나타냅니다. 예를 들어, 일주일에는 7일, 1년에는 12개월만 있을 수 있습니다. 데이터베이스에서는 일반적으로 참조 유형 테이블이 있고 거기에는 상당히 고정된 행 집합이 있을 수 있습니다.

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

전통적으로 자바에서 참조 타입은 public static final 변수로 모델링되었습니다. 예를 들어, 다음은 시간 단위를 상수로 선언하는 Java 인터페이스와 해당 단위를 두 번째 매개변수로 받는 handleTime(..) 메서드를 보여줍니다.

```js
public interface TimerUnit {

  public static final int HOUR        = 1;
  public static final int MINUTE      = 2;
  public static final int SECOND      = 3;
  public static final int MILLISECOND = 4;

}

void handleTime(int time, int unit) {
  // 시간 처리
}
```

그러나 이는 안전하지 않고 표현력이 좋지 않은 해결책입니다. 예를 들어, 개발자가 handleTime(..) 메서드에 단위로 100과 같은 임의의 정수 값을 전달하는 것을 막는 방법이 없습니다. 컴파일러는 이것이 허용되는지 여부를 알 방법이 없으며 이미 알고 있는 것처럼, 컴파일 시간에 발견된 문제가 이상적입니다.

표현력이 좋지 않은 이유는 handleTime(..) 메서드를 보고서 unit 매개변수에 무엇을 전달해야 하는지 명확하지 않기 때문입니다.

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

여기서 Java Enum이 도움이 됩니다.

# Java Enum

참조 유형은 다음과 같이 enum 키워드를 사용하여 Java Enum으로 모델링할 수 있습니다.

```js
public enum TimerUnit {
    HOUR,
    MINUTE,
    SECOND,
    MILLISECOND
}

void handleTime(int time, TimerUnit unit) {
    // 시간을 처리
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

TimerUnit라는 상수를 가진 Enum을 생성합니다 - HOUR, MINUTE, SECOND 및 MILLISECOND. Enum은 기본적으로 Java 클래스이며 컴파일 중에 Enum인 X는 X.class라는 클래스로 컴파일됩니다. Java Enum 내의 각 상수는 TimerUnit 유형의 객체로 대체됩니다(나중에 확장될 것으로 보입니다). 이러한 객체는 TimerUnit 클래스 내에서 public static final로 선언되며 TimerUnit의 정적 초기화 중에 초기화됩니다. 자세한 내용을 위해 나중에 기사에서 생성된 클래스를 살펴볼 것입니다.

또한 각 상수 객체의 인스턴스는 하나만 존재하며 이는 동등성 확인 중에 유용할 수 있습니다. 즉, Java Enum은 싱글턴 패턴의 일반화입니다 - 해당 유형의 고정된 수의 객체가 존재합니다. 이는 참조 유형에 필요한 바로 그것입니다.

위의 예제에서 handleTime(..) 메서드는 두 번째 매개변수로 TimerUnit Enum을 사용합니다. 이는 타입 안전하며 개발자는 허용 가능한 Enum 상수 중 하나만 전달할 수 있습니다.

# Java Enum 사용하기

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

이제 Enum을 정의했으니, Enum을 사용해 봅시다.

```js
TimerUnit unit = TimerUnit.HOUR;

// HOUR 출력
System.out.println(unit.name());
```

위에서 볼 수 있듯이 Enum에는 점 표기법을 사용하여 특정 상수를 참조할 수 있습니다. name()이라는 메서드는 상수의 이름을 반환합니다.

```js
// 텍스트에서 TimerUnit로 변환
TimerUnit hour = TimerUnit.valueOf("HOUR");
System.out.println(hour);

// 각 TimerUnit 상수에는 단일 인스턴스만 존재합니다
// 따라서 == 연산자가 잘 동작해야 합니다
assert hour == TimerUnit.HOUR;
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

"valueOf(..)"이라는 정적 메서드는 HOUR과 같은 상수 문자열을 TimerUnit 객체로 변환할 수 있습니다. valueOf(..) 메서드는 해당 상수를 새 객체로 만들지 않고 이미 사용 가능한 상수 인스턴스를 반환합니다.

# values() 메서드

```js
for (TimerUnit value : TimerUnit.values()) {
  System.out.printf("%d : %s\n", value.ordinal(), value);
}
```

모든 Enum 클래스와 마찬가지로 TimerUnit에는 모든 TimerUnit 열거 상수의 배열을 반환하는 values()라는 정적 메서드가 포함되어 있습니다. 위 코드에서 볼 수 있듯이 각 Enum 상수에 대해 코드가 상수 이름 및 ordinal(각 상수와 내부적으로 연관된 정수 값)을 출력합니다. 실제 용도로는 ordinal 값은 사용되지 말아야 합니다. 왜냐하면 Enum 클래스 내 상수의 순서에 따라 변경되기 때문입니다.

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

# Enum에 연결된 Class

```js
// unit이 TimerUnit을 확장합니다
assert unit instanceof TimerUnit;

// TimerUnit을 출력합니다
System.out.println(unit.getClass());
```

TimerUnit Enum은 컴파일 중에 TimerUnit.class로 컴파일되므로 연관된 클래스는 TimerUnit 클래스일 것입니다. 그러나 나중에 어떤 상황에서 TimerUnit에서 상속된 클래스일 수 있고 간단히 TimerUnit 클래스가 아닐 수 있습니다.

# switch 문에서 Enum 사용하기

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

Enum 사용의 큰 장점 중 하나는 switch 문에서 사용할 때, 컴파일러가 모든 경우를 다루지 않으면 완전성을 확인한다는 점입니다. 미래에 enum 클래스에 더 많은 상수가 추가될 때 이 기능은 매우 유용합니다. 컴파일러가 변경해야 하는 기존 switch 문을 모두 표시해 줍니다. 이는 컴파일러가 우리 대신에 오류를 잡는 또 다른 예다.

아래 예시를 보면, MILLISECOND가 switch 표현식에 포함되지 않아 컴파일되지 않습니다. 그러나 else 케이스가 있다면 이러한 컴파일 문제가 발생하지 않았을 것입니다.

```js
// 이 코드는 컴파일되지 않습니다
String text = switch(unit) {

  case HOUR ->  "hours";
  case MINUTE -> "minutes";
  case SECOND -> "seconds";

  // MILLISECOND가 빠져 있습니다

};

System.out.println(text);
```

# Enum 내부 간략 소개

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

지금쯤 궁금해하고 있을지도 모르겠지만, TimerUnit Enum이 실제로 무엇으로 컴파일되는지 정확히 알고 싶을 거에요. 이 정보를 통해 Java Enum 개념을 더 명확하게 상상할 수 있을 거예요. 어떤 모습의 클래스가 생성되는지 생각해 보세요.

이를 알아내기 위해 javap라는 편리한 명령어가 있어요. TimerUnit.class 파일에 javap 명령어를 실행하면, 생성된 클래스의 개요가 나와요.

c:\dir` javap ./TimerUnit.class

```java
public final class TimerUnit extends java.lang.Enum<TimerUnit> {

  public static final TimerUnit HOUR;
  public static final TimerUnit MINUTE;
  public static final TimerUnit SECOND;
  public static final TimerUnit MILLISECOND;

  public static TimerUnit[] values();
  public static TimerUnit valueOf(java.lang.String);

  static {};
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

위에서 다음을 명확히 볼 수 있습니다.

- Enum은 기본적으로 Enum이라는 클래스에서 확장된 클래스입니다.
- Enum 내의 상수는 public static final TimerUnit 객체로 변환됩니다.
- values() 및 valueOf(...)라는 두 개의 정적 메서드가 생성됩니다(위 섹션에서 설명함).

위의 구조에 표시되지는 않았지만, Enum의 상수는 각 상수에 대해 TimerUnit 객체의 새 인스턴스를 생성하여 정적 블록(비어 있는 상태로 표시됨) 내에 설정됩니다.

# Enum 속성

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

지금까지는 데이터가 연관되어 있지 않은 상수 선언만 있는 Enum을 보았습니다. 그러나 Enum에 속성(데이터)을 연결할 수 있으며 이는 매우 유용할 수 있습니다.

예를 들어, TimerUnit 열거형에 코드와 설명 두 가지 속성을 추가할 것입니다. Enum은 단순히 강화된 클래스이므로 멤버 변수를 추가하고 상수 정의의 일부로 데이터를 포함시킬 수 있습니다. 또한 속성에 대한 접근자 메서드를 추가합니다.

예를 들어, 각 TimerUnit Enum 상수 (HOUR, MINUTE 등)는 이제 코드와 설명이 연결될 것입니다.

```java
public enum TimerUnit {

    HOUR("H", "hours"),
    MINUTE("M", "minutes"),
    SECOND("S", "seconds"),
    MILLISECOND("M", "milliseconds");

    private final String code;
    private final String descr;

    TimerUnit(String code, String descr) {
        this.code = code;
        this.descr = descr;
    }

    public String getCode() {
        return code;
    }

    public String getDescr() {
        return descr;
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

이 변경으로 인해 속성에 액세스하는 방법을 살펴보겠습니다. 아래 코드는 그 작업을 어떻게 하는지 보여주는 것이며, 클래스에 대한 것과 다를 바 없습니다.

```js
TimerUnit unit = TimerUnit.HOUR;

// Enum에 연결된 데이터에 액세스
String code = unit.getCode();
String descr = unit.getDescr();
System.out.printf("%s은(는) %s를 나타냅니다%n", code, descr);
```

Enum에 데이터를 직접 연결함으로써, 우리는 설명을 얻기 위해 switch 문을 사용할 필요가 없습니다(이전 예제에서 한 바와 같음); 단순히 열거형 상수에서 액세서 메서드 getDescr()를 호출하여 설명을 얻습니다.

# Enum 메서드

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

Enum 클래스에 임의의 메서드를 추가할 수 있습니다. Enum 클래스에 추가하는 메서드는 모든 열거 상수에서 사용할 수 있으며 재사용하기에 좋습니다.

예를 들어, 우리는 속성을 표시하는 사용자 정의 toString() 메서드를 추가할 수 있으며 이는 모든 열거 상수에 적용될 것입니다.

여기서 할 수 있는 또 다른 중요한 작업은 Enum에 추상 메서드를 생성하고 각 상수가 해당 추상 메서드의 버전을 구현할 수 있도록 하는 것입니다.

예를 들어, 특정 기간 동안 간단히 슬립하는 snooze(..) 메서드를 만들고 싶다고 가정해 봅시다. 지정된 기간의 단위는 메서드가 호출된 열거 상수에 의해 결정될 것입니다. 따라서 아래에서 볼 수 있듯이 snooze(..)라는 추상 메서드를 만들고 각 상수에 대한 구체적인 구현을 갖게 됩니다.

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

```js
public enum TimerUnit {

    HOUR("H", "시간") {
        @Override
        public void snooze(long time) throws Exception {
            Thread.sleep(time * 60 * 60 * 1000);
        }
    },
    MINUTE("M", "분") {
        @Override
        public void snooze(long time) throws Exception {
            Thread.sleep(time * 60 * 1000);
        }
    },
    SECOND("S", "초") {
        @Override
        public void snooze(long time) throws Exception {
            Thread.sleep(time * 1000);
        }
    },
    MILLISECOND("MS", "밀리초") {
        @Override
        public void snooze(long time) throws Exception {
            Thread.sleep(time);
        }
    };

    private final String code;
    private final String descr;

    TimerUnit(String code, String descr) {
        this.code = code;
        this.descr = descr;
    }

    public String getCode() {
        return code;
    }

    public String getDescr() {
        return descr;
    }

    // Constants에 의해 구현된 추상 메서드
    public abstract void snooze(long time) throws Exception;

}
```

snooze 메서드를 어떻게 사용해야합니까?

TimerUnit 상수에 해당 메서드를 호출하면 됩니다. 예를 들어 SECOND에 이를 호출하면 5를 전달하면 5초 동안 대기합니다.

다음은 snooze(..) 메서드 호출의 몇 가지 예시입니다.

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
System.out.println("5초를 기다리는 중");
TimerUnit.SECOND.snooze(5);

System.out.println("2000밀리초를 기다리는 중");
TimerUnit.MILLISECOND.snooze(2000);
```

# Enum 상수 클래스

이전에 TimerUnit 상수와 연관된 클래스가 TimerUnit(패키지 이름은 무시한다)라고 언급했습니다. 이는 상수와 관련된 메서드가 없는 경우에만 해당합니다.

그러나 snooze와 같이 상수에 대해 구현이 다른 메서드가 있는 경우 어떻게 될까요? 이러한 메서드를 TimerUnit과 동일한 클래스로 표현할 수는 없습니다. 이 경우 컴파일러는 필요한 상수에 대해 이러한 메서드를 구현하기 위해 TimerUnit의 하위 클래스를 생성합니다.

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

그래도 괜찮을까요?

```js
// TimerUnit$1을 출력합니다
System.out.println(TimerUnit.HOUR.getClass());

// TimerUnit$3을 출력합니다
System.out.println(TimerUnit.SECOND.getClass());
```

자바 Enum이 무엇인지, 언제 사용해야 하는지, 내부 구현은 무엇이며 왜 강력한지에 대해 이해하는 데 도움이 되었으면 좋겠어요.

이 게시물이 도움이 되었다면 아래의 박수 버튼을 여러 번 클릭하여 지원을 표시해주세요. 읽어주셔서 감사합니다!
