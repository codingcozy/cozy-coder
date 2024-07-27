---
title: "2024년 안드로이드 인터뷰 질문 모음 Part I"
description: ""
coverImage: "/assets/img/2024-07-10-AndroidInterviewQuestions2024Part-I_0.png"
date: 2024-07-10 01:41
ogImage: 
  url: /assets/img/2024-07-10-AndroidInterviewQuestions2024Part-I_0.png
tag: Tech
originalTitle: "Android Interview Questions: 2024 Part-I"
link: "https://medium.com/@vikasacsoni9211/quest-for-android-excellence-interview-edition-2024-part-i-11d5c517e9f6"
---


![Android Interview Questions 2024 Part-I](/assets/img/2024-07-10-AndroidInterviewQuestions2024Part-I_0.png)

한국어 번역:

안녕하세요! 오늘은 2024년 안드로이드 면접 질문 시리즈의 첫 번째 파트를 살펴볼 거예요. 함께 볼 주제는 Java와 Kotlin 그리고 안드로이드 컴포넌트에 대해 다뤄요. 사람들과 함께 새로운 지식들을 공유하며 즐거운 시간 보내시길 바라요! 👋🔮✨

표의 목차:  
1. Java와 Kotlin  
2. 안드로이드 컴포넌트

<div class="content-ad"></div>

3. 사용자 인터페이스 (UI) 개발

4. 안드로이드 앱 아키텍처

5. 네트워킹 및 데이터 지속성

자, 이제 핵심으로 넘어가 봅시다-

<div class="content-ad"></div>

자바와 코틀린

- 자바에서 추상 클래스와 인터페이스의 차이는 무엇인가요?

답변: 추상 클래스는 인스턴스화할 수 없는 클래스로 추상 메서드와 비추상 메서드를 포함할 수 있습니다. 반면에 인터페이스는 추상 메서드와 상수들의 모음으로 어떤 클래스에서라도 구현할 수 있습니다. 두 가지 중요한 차이점 중 하나는 클래스가 여러 인터페이스를 구현할 수 있지만 추상 클래스는 하나만 확장할 수 있다는 점입니다.

2. 자바에서 “==” 연산자와 “.equals()” 메서드의 차이는 무엇인가요?

<div class="content-ad"></div>

답변: "==" 연산자는 객체 참조 동등성을 확인하고, ".equals()" 메서드는 객체 값 동등성을 확인합니다. 예를 들어, 동일한 값을 가진 두 개의 다른 문자열 객체는 "equals()"를 사용하여 비교할 때 true를 반환하지만, "=="을 사용하여 비교할 때 false를 반환합니다.

3. 코틀린에서 람다 표현식이란 무엇인가요?

답변: 람다 표현식은 별도의 이름이 지정된 함수를 작성하지 않고 코틀린에서 함수를 정의하는 방법입니다. 이를 통해 더 간결한 구문을 사용하여 인라인으로 함수를 정의할 수 있습니다. 예를 들어, 다음 코드는 두 정수를 가져와 그들의 합계를 반환하는 람다 표현식을 정의합니다: "(x: Int, y: Int) -> x + y".

4. 코틀린에서 lateinit 속성과 초기화된 속성의 차이는 무엇인가요?

<div class="content-ad"></div>

답변: lateinit 속성은 초기값 없이 선언되지만 사용 전에 초기화될 것이 보장된 속성입니다. 이는 생성자에서 초기화할 수 없지만 사용 전에 초기화해야 하는 속성에 유용합니다. 반면 초기화된 속성은 초기값이 지정된 속성으로 즉시 사용할 수 있습니다.

5. 자바에서 HashSet과 TreeSet의 차이는 무엇인가요?

답변: HashSet은 해시 테이블을 사용하여 구현된 순서가 없는 고유 요소의 집합입니다. TreeSet은 반면 레드-블랙 트리를 사용하여 구현된 고유 요소의 순서가 있는 집합입니다. TreeSet의 요소는 정렬된 순서로 저장됩니다.

6. 코틀린에서 companion object란 무엇인가요?

<div class="content-ad"></div>

답변: 동반 객체는 클래스와 관련된 객체로, 클래스의 인스턴스가 아닙니다. 클래스에 대한 정적 메서드와 속성을 정의하는 데 사용될 수 있습니다. 예를 들어, 다음 코드는 MyClass 클래스에 대한 동반 객체를 정의하며, 정적 메서드 "myStaticMethod"를 포함합니다:

```js
class MyClass {
    companion object {
        fun myStaticMethod() {
            /* 코드 */
        }
    }
}
```

7. 코틀린에서 클래스와 객체의 차이점은 무엇인가요?

답변: 클래스는 객체를 생성하기 위한 청사진이며, 객체는 클래스의 단일 인스턴스입니다. 객체는 Kotlin에서 싱글톤을 구현하는 데 자주 사용됩니다.

<div class="content-ad"></div>

8. 자바에서의 다형성은 무엇인가요?

답변: 다형성은 객체가 여러 형태를 취할 수 있는 능력을 말합니다. 자바에서는 상속과 메소드 재정의를 통해 이를 달성할 수 있습니다. 예를 들어, 하위 클래스는 상위 클래스의 메소드를 재정의하여 다른 구현을 제공할 수 있습니다.

9. 자바에서의 함수형 인터페이스는 무엇인가요?

답변: 함수형 인터페이스는 하나의 추상 메소드만을 가지는 인터페이스를 말합니다. 이는 자바의 람다 표현식과 메소드 참조와 함께 자주 사용됩니다. 자바는 인터페이스가 함수형 인터페이스임을 나타내기 위해 @FunctionalInterface 주석을 제공합니다.

<div class="content-ad"></div>

안녕하세요, 여러분! 오늘은 자바에서 private와 protected 메소드의 차이에 대해 이야기해 보려고 합니다. 

우선, private 메소드는 동일한 클래스 내에서만 액세스할 수 있는 메소드입니다. 반면에 protected 메소드는 동일한 클래스와 하위 클래스 내에서도 액세스할 수 있습니다. 

그리고 안드로이드 앱 구성 요소 중 핵심인 것은 무엇일까요? 여러분은 이미 아시다시피 안드로이드 앱의 핵심 구성 요소는 무엇인지 궁금해 하고 계시군요!

<div class="content-ad"></div>

키 안드로이드 앱 구성 요소는 활동(Activities), 프래그먼트(Fragments), 서비스(Services), 브로드캐스트 수신기(Broadcast Receivers), 및 콘텐츠 제공자(Content Providers)입니다.

2. 안드로이드에서 활동(Activity)이란 무엇인가요?

답변: 활동은 사용자 인터페이스가 있는 단일 화면을 나타냅니다. 앱과 상호 작용하는 데 사용되며 UI 구성 요소를 관리하고 사용자 입력을 수신하고 처리합니다.

3. 안드로이드에서 프래그먼트(Fragment)란 무엇인가요?

<div class="content-ad"></div>

답변: 프래그먼트는 활동의 일부를 나타내는 재사용 가능한 UI 구성 요소입니다. 멀티 패널 UI를 구축하는 데 사용할 수 있으며 런타임 중에 동적으로 추가 또는 제거할 수 있습니다.

4. 안드로이드에서 서비스란 무엇인가요?

답변: 서비스는 사용자 인터페이스 없이 오랜 시간 동안 실행되는 백그라운드 프로세스입니다. 앱이 포그라운드에 있지 않아도 백그라운드에서 실행될 수 있습니다.

5. 안드로이드에서 방송 수신기란 무엇인가요?

<div class="content-ad"></div>

**답변:** 방송 수신기는 시스템 또는 앱 이벤트를 청취하고 그 이벤트에 기반하여 작업을 수행하는 구성 요소입니다. 다른 구성 요소나 시스템 이벤트로부터 방송 메시지를 수신하고 응답하는 데 사용됩니다.

**6. 안드로이드에서 컨텐트 프로바이더(Content Provider)란 무엇인가요?**

**답변:** 컨텐트 프로바이더는 다른 앱이나 구성 요소에서 액세스할 수 있는 공유 데이터 집합을 관리하는 구성 요소입니다. 데이터에 액세스하고 조작하기 위한 표준화된 인터페이스를 제공합니다.

**7. 안드로이드에서 액티비티(Activity)의 라이프사이클은 무엇인가요?**

<div class="content-ad"></div>

답변: 안드로이드에서 활동(Activity)의 생명주기에는 생성된 상태(Created), 시작된 상태(Started), 재개된 상태(Resumed), 일시정지된 상태(Paused), 중단된 상태(Stopped) 및 소멸된 상태(Destroyed)와 같은 여러 상태가 포함됩니다. 각 상태에는 해당 상태에서 작업을 수행하기 위해 재정의할 수 있는 특정 메소드들이 있습니다.

8. 안드로이드에서 활동 간에 데이터를 어떻게 전달하나요?

답변: 안드로이드에서는 Intent extras를 사용하거나 startActivityForResult 메소드를 사용하여 활동 간에 데이터를 전달할 수 있습니다.

9. 안드로이드에서 Bundle의 목적은 무엇인가요?

<div class="content-ad"></div>

답변: 번들(Bundle)은 안드로이드의 구성 요소인 액티비티(Activity), 프래그먼트(Fragment) 또는 서비스(Service) 간에 전달할 수 있는 데이터를 저장하는 컨테이너입니다. 주로 인스턴스 상태 데이터를 저장하고 복원하는 데 사용됩니다.

10. 안드로이드에서 서비스(Service)와 인텐트 서비스(IntentService)의 차이는 무엇인가요?

답변: 서비스(Service)는 중지될 때까지 계속 백그라운드에서 실행되는 프로세스입니다. 반면 인텐트 서비스(IntentService)는 한 가지 작업을 수행한 후 자동으로 종료되는 Service의 하위 클래스입니다. 백그라운드에서 별도의 스레드에서 수행해야 하는 작업에는 주로 인텐트 서비스가 사용될 수 있습니다.

사용자 인터페이스(UI) 개발

<div class="content-ad"></div>

- Android UI 개발에서 View와 ViewGroup의 차이점은 무엇인가요?

답변: Android UI 개발에서 View는 버튼이나 텍스트 필드와 같은 UI 요소를 나타내고, ViewGroup는 View와 다른 ViewGroup를 포함하는 컨테이너입니다. ViewGroup는 다른 ViewGroup와 View를 보유하고 다양한 방식으로 배열할 수 있습니다. 예를 들어, LinearLayout은 View를 수평 또는 수직으로 배열하는 ViewGroup입니다.

2. Android UI 개발에서 XML 기반 레이아웃의 목적은 무엇인가요?

답변: XML 기반 레이아웃은 Android 앱의 사용자 인터페이스의 구조와 외관을 정의하는 데 사용됩니다. 이러한 레이아웃은 버튼, 텍스트 필드, 이미지 등과 같은 UI 요소의 위치, 크기 및 스타일을 정의합니다. XML에서 레이아웃을 정의함으로써, 개발자들은 UI 디자인을 앱 로직에서 분리하여 코드를 유지보수하고 수정하기 쉽게 만들 수 있습니다.

<div class="content-ad"></div>

3. 안드로이드에서 사용자 지정 뷰를 만드는 방법은 무엇인가요?

답변: 안드로이드에서 사용자 지정 뷰를 만들려면 View 클래스를 확장하고 해당 사용자 지정 뷰의 그리기 동작을 정의하기 위해 onDraw() 메소드를 재정의해야 합니다. 그런 다음 앱의 XML 레이아웃에서 사용자 지정 뷰의 완전히 지정된 이름을 XML 태그로 지정함으로써 이 사용자 지정 뷰를 사용할 수 있습니다.

4. 안드로이드 앱을 개발할 때 따라야 할 일반적인 UI 디자인 원칙은 무엇인가요?

답변: 안드로이드 앱을 개발할 때 따라야 할 일반적인 UI 디자인 원칙에는 간결성, 일관성, 가시성, 피드백 및 사용성이 포함됩니다. UI는 사용하기 쉽고 이해하기 쉬워야 하며 명확한 라벨과 직관적인 탐색을 갖추어야 합니다. 사용자가 작업을 수행할 때 피드백을 제공해야 하며 앱의 UI 전반에 걸쳐 일관성을 유지해야 합니다.

<div class="content-ad"></div>

5. 안드로이드 UI 개발에서 반응형 디자인을 적용하는 방법은 무엇인가요?

답변: 안드로이드 UI 개발에서 반응형 디자인을 적용하려면 RelativeLayout, ConstraintLayout 또는 새로운 GridLayout과 같은 기술을 사용할 수 있습니다. 이러한 레이아웃 매니저를 사용하면 UI 요소를 서로 관련시켜 다양한 화면 크기와 방향에 맞게 UI가 올바르게 확장되도록 할 수 있습니다.

6. 안드로이드 UI 개발에서 서로 다른 화면 밀도를 처리하는 방법은 무엇인가요?

답변: 안드로이드 UI 개발에서는 -hdpi, -xhdpi 및 -xxhdpi와 같은 리소스 한정자를 사용하여 서로 다른 화면 밀도용 UI 요소의 다른 버전을 지정할 수 있습니다. 시스템은 장치의 화면 밀도에 따라 자동으로 리소스의 적절한 버전을 로드합니다.

<div class="content-ad"></div>

7. 안드로이드 UI 개발에서 RecyclerView란 무엇인가요?

답변: RecyclerView는 안드로이드 UI 개발에서 ListView와 GridView에 대한 더 유연하고 효율적인 대체재입니다. 개발자들이 맞춤형 항목 보기와 더 효율적인 데이터 로딩 및 재활용 메커니즘을 통해 스크롤 가능한 목록이나 그리드에 대량의 데이터를 표시할 수 있습니다.

8. 안드로이드 UI 개발에서 애니메이션을 어떻게 구현하나요?

답변: 안드로이드 UI 개발에서 Animation 및 Animator 클래스를 사용하여 페이드인, 페이드아웃, 슬라이드인과 같은 애니메이션을 구현할 수 있습니다. 또한 여러 UI 요소를 포함하는 더 복잡한 애니메이션을 만들기 위해 새로운 Transition 프레임워크를 사용할 수도 있습니다.

<div class="content-ad"></div>

9. 안드로이드에서 일반적으로 사용되는 UI 구성 요소에는 TextView, ImageView, Button, EditText, ProgressBar 및 RecyclerView 등이 있습니다. 이러한 구성 요소는 텍스트, 이미지, 사용자 입력 필드 및 진행 표시기를 표시하는 데 사용됩니다.

10. 안드로이드 UI 개발에서의 Material Design 가이드라인은 무엇인가요?

Material Design은 구글이 만든 안드로이드 UI 디자인을 위한 가이드라인 세트입니다. 안드로이드 앱을 위한 일관된 직관적인 디자인 언어를 제공하며, 서식, 색상, 레이아웃 및 애니메이션에 대한 가이드라인을 포함합니다. Material Design 가이드라인을 준수하면 개발자가 시각적으로 매력적이고 사용자 친화적인 앱을 더 쉽게 만들 수 있습니다.

<div class="content-ad"></div>

11. 안드로이드 XML 레이아웃에서 "match_parent" 속성은 무엇을 위한 것인가요?

답변: "match_parent" 속성은 View에게 자신의 부모 컨테이너 내에서 가능한 많은 공간을 차지하도록 지시하는 데 사용됩니다. 종종 전체 화면이나 동적으로 크기 조절된 UI 구성 요소를 만드는 데 사용됩니다.

```json
TextView {
  "layout_width": "match_parent",
  "layout_height": "wrap_content",
  "text": "이 텍스트는 부모의 전체 너비를 차지합니다"
}
```

12. 모바일 앱을 위한 일반적인 UI 디자인 원칙은 무엇인가요? 답변: 모바일 앱을 위한 일반적인 UI 디자인 원칙에는 간결함, 일관성, 피드백, 부적절, 그리고 찾을 수 있음이 포함됩니다. 이러한 원칙은 앱이 사용하기 쉽고 직관적이며 시각적으로 매력적인지를 보장하는 데 도움을 줍니다.

<div class="content-ad"></div>

Here is the Markdown format with the Korean translation:

단순함: 앱은 사용하기 쉽고 네비게이션하기 쉬운 간단하고 직관적인 UI를 가져야 합니다.

일관성: 앱은 다양한 화면과 구성 요소 전체에서 일관된 디자인 언어를 가져야 합니다.

피드백: 앱은 사용자가 UI 구성 요소를 상호 작용할 때 명확한 피드백을 제공해야 합니다. 버튼이나 텍스트 입력란과 같은 것들과 상호작용할 때 사용자에게 명확한 피드백이 제공되어야 합니다.

<div class="content-ad"></div>

**마법의 순간**

지친 일상에 잠시 멈춰 힐링을 위한 타로 시간을 가져보세요.

친애하는 타로 친구여, Affordance와 Discoverability는 모두 타로 세계에서도 중요한 원칙이에요. UI 구성 요소가 사용자에게 익숙한 기능을 시각적으로 표현해야 한다는 의미죠. 그래서, 누를 수 있는 것처럼 보이는 버튼은 어떤 모습이어야 한다고 생각하시나요?

또한, UI 구성 요소는 모두 쉽게 찾을 수 있고 사용하기 쉬워야 해요. 사용자들에게 친절하고 접근하기 쉬운 인터페이스를 만드는 것이 중요하답니다.

"13. 안드로이드 앱의 UI 성능을 어떻게 개선할 수 있나요?"라는 질문에 대한 답변으로, 여러분은 Android 앱의 UI 성능을 향상시킬 수 있어요. 레이아웃 계층의 수를 줄이고, 고가의 그래픽 사용을 최소화하며, 긴 목록에는 ListView 대신 RecyclerView를 사용하고, 애니메이션 및 전환을 최적화할 수 있어요.

**예시:**

실제적인 예시가 없는 것 같아 아쉽네요. 하지만, 타로 친구와 함께하는 시간만큼 소중한 게 없죠. 지금 이 순간을 함께 감상해요.


<div class="content-ad"></div>

- RelativeLayout을 사용하여 중첩된 LinearLayout 계층 구조의 레이아웃 패스를 줄입니다.
- 비트맵 이미지의 메모리 풋프린트를 줄이기 위해 벡터 드로어블을 사용하세요.
- 스크롤 성능을 향상시키기 위해 RecyclerView 위젯을 ListView 위젯 대신 사용하세요.
- UI 성능 문제를 감지하고 수정하기 위해 Lint 도구를 사용하세요.

14. 안드로이드에서 "dp" (밀도 독립적 픽셀) 단위의 목적은 무엇인가요?

답변: "dp" 단위는 기기의 화면 밀도와 독립적인 방식으로 치수를 지정하는 데 사용됩니다. 이를 통해 UI 구성 요소가 다른 화면 밀도를 가진 기기 사이에서 동일하게 표시될 수 있습니다.

```java
<Button 
    android:layout_width="100dp" 
    android:layout_height="50dp" 
    android:text="Click me!" />
```

<div class="content-ad"></div>

ViewStub은 안드로이드에서 UI 구성 요소를 당김하여 필요할 때까지 인플레이션을 지연시킬 수 있는 가벼운 UI 구성 요소입니다. 앱 시작 시간을 개선하고 메모리 사용량을 줄일 수 있습니다.

Markdown 형식의 코드:
```js
<ViewStub 
    android:id="@+id/stub" 
    android:inflatedId="@+id/my_view" 
    android:layout="@layout/my_layout" 
    android:layout_width="match_parent" 
    android:layout_height="wrap_content" />
```

이 예에서는 ViewStub이 "my_layout" 레이아웃 파일을 필요할 때 인플레이트하도록 설정되어 있습니다. 인플레이트된 레이아웃은 "my_view" ID를 갖게 될 것입니다.

<div class="content-ad"></div>

16. 안드로이드에서 다양한 화면 크기를 어떻게 처리하나요?
답변: 안드로이드에서 다양한 화면 크기를 다루는 방법은 "layout-small", "layout-large", "layout-xlarge"와 같은 레이아웃 자격 증명을 사용하여 서로 다른 화면 크기를 위해 레이아웃 파일의 다른 버전을 제공하는 것입니다. 또한 "dp" 단위를 사용하여 디바이스의 화면 밀도와 독립적인 방식으로 치수를 지정할 수 있습니다.

```js
res/ layout/ main.xml 
res/ layout-small/ main.xml 
res/ layout-large/ main.xml 
res/ layout-xlarge/ main.xml
```

안드로이드 앱 아키텍처

- Model-View-ViewModel (MVVM) 아키텍처란 무엇이며 다른 아키텍처와 어떻게 다를까요?

<div class="content-ad"></div>

답변: MVVM은 Android 앱을 세 가지 구성 요소로 나누어 인기 있는 아키텍처 패턴입니다: Model, View 및 ViewModel입니다. Model 구성 요소는 데이터와 비즈니스 로직을 나타내고, View 구성 요소는 UI를 나타내며, ViewModel은 Model과 View 구성 요소 사이에서 중재자 역할을 합니다. MVVM의 주요 장점은 각 구성 요소를 별도로 테스트하기 쉽게 만들고 데이터 바인딩을 활성화한다는 것입니다. 반면에 Model-View-Presenter(MVP)는 View와 Presenter 구성 요소를 분리하는 반면, Clean Architecture는 비즈니스 로직을 표현층과 분리하기 위해 계층을 사용합니다.

2. Jetpack을 사용하여 Android 앱에서 MVVM 아키텍처를 어떻게 구현합니까?

답변: Jetpack을 사용하여 MVVM 아키텍처를 구현하려면 다음 구성 요소를 사용할 수 있습니다:

- LiveData: ViewModel과 View 구성 요소 간의 변경 사항을 통신할 수 있는 라이프사이클을 인식하는 observable 데이터 홀더입니다.
- ViewModel: UI 관련 데이터를 저장하고 관리하며, Model 구성 요소와 통신하며, 구성 변경을 살려냅니다.
- DataBinding: UI 구성 요소가 ViewModel의 데이터 소스에 바인딩하도록 하는 라이브러리로서, findViewById() 호출을 제거합니다. 또한 데이터베이스 작업을 위해 Room, 화면 간 탐색을 위해 Navigation, 백그라운드 처리를 위해 WorkManager 등 다른 Jetpack 구성 요소를 사용할 수도 있습니다.

<div class="content-ad"></div>

3. 의존성 주입은 무엇이고 앱 아키텍처를 어떻게 개선하나요?

답변: 의존성 주입은 앱 내 객체 간 의존성을 관리하는 기술입니다. 직접 객체를 생성하는 대신 Dagger와 같은 의존성 주입 프레임워크를 사용하여 필요한 객체를 제공합니다. 이렇게 하면 각 구성 요소가 쉽게 대체 또는 수정될 수 있어 앱이 더 모듈식이고 테스트 가능하며 유지보수가 쉽습니다. 또한 코드 중복을 줄이고 코드 가독성을 높입니다.

4. Dagger의 역할은 무엇이며 어떻게 앱 아키텍처를 개선하나요?

답변: Dagger는 안드로이드용 인기있는 의존성 주입 프레임워크로 의존성 관리 프로세스를 간소화합니다. 주석을 사용하여 다른 클래스에 의존성을 제공하는 코드를 생성하므로 수동 의존성 주입이 필요 없습니다. Dagger의 주요 장점은 모듈식 앱 디자인을 가능하게 하며 구성 요소를 독립적으로 테스트하기 쉽게 만듭니다. 이는 컴파일 시간에 의존성 그래프를 만들고 그래프를 사용하여 런타임에 앱에 의존성을 제공하는 방식으로 작동합니다.

5. Jetpack의 ViewModel 구성 요소는 어떻게 앱 아키텍처를 개선하나요?

<div class="content-ad"></div>

답변: Jetpack의 ViewModel 구성 요소는 UI와 사용자 입력과 같은 UI 관련 데이터를 저장하고 관리하는 데 사용됩니다. 화면 회전과 같은 구성 변경을 감지하여 상태를 유지하는 생명주기 인식 메커니즘을 사용하여 작동합니다. 이는 상태를 번들에 저장하는 방식과 같은 해결책이 필요 없게 하며 표현층과 데이터 층을 쉽게 분리할 수 있도록 합니다. 또한 Fragment와 같은 다중 UI 구성 요소 간 데이터 공유와 모듈식 앱 아키텍처를 촉진합니다.

6. Jetpack의 LiveData의 역할은 무엇이며 반응형 프로그래밍을 어떻게 가능하게하는가요?

답변: LiveData는 Android 앱의 ViewModel 및 View 구성 요소 간의 변경 사항 통신에 사용할 수 있는 생명주기 인식 가능한 관찰 가능한 데이터 홀더입니다. 반응형 프로그래밍을 통해 ViewModel의 데이터 변경을 View가 관찰하고 UI를 업데이트할 수 있게 합니다. 이를 통해 수동 UI 업데이트 작업이 불필요하며, 보일러플레이트 코드를 줄이고 성능을 향상시킵니다. LiveData는 또한 Fragment와 Activity와 같은 앱 구성 요소의 수명주기를 존중하며, 더 이상 필요하지 않은 경우 관찰자를 자동으로 제거하여 메모리 누수를 방지합니다.

7. Android 앱 개발에서 Model-View-Controller(MVC)와 Model-View-Presenter(MVP) 패턴의 차이점을 설명할 수 있나요?

<div class="content-ad"></div>

답변: MVC 패턴은 앱을 모델(데이터 및 비즈니스 로직), 뷰(사용자 인터페이스), 그리고 컨트롤러(모델과 뷰 사이의 중재자) 세 가지 구성 요소로 분리합니다. MVP 패턴은 MVC 패턴을 기반으로 하며, 뷰와 모델 사이를 중개하는 프리젠터를 추가하여 사용자 입력을 처리하고 뷰를 업데이트합니다. MVP에서는 뷰와 모델이 독립적이므로 앱을 테스트하기 쉽습니다.

8. 안드로이드 개발에서 MVVM 패턴과 MVP 패턴의 차이점은 무엇인가요?

답변: MVVM에서는 뷰가 ViewModel에 바인딩되며, 이는 프레젠테이션 로직 및 상태 관리를 처리합니다. ViewModel은 뷰가 표시할 데이터를 검색하고 준비하는 역할을 합니다. 이 패턴은 관심사의 분리를 더 잘 해 주며 코드를 테스트하기 쉽게 만듭니다. MVP와 달리, 뷰와 ViewModel은 직접적으로 연결되어 있지 않습니다.

9. 안드로이드 앱 개발에서 Clean Architecture 패턴을 사용하는 이점을 설명해주시겠어요?

<div class="content-ad"></div>

답변: LiveData의 사용은 안드로이드 앱 개발에서 어떻게 앱 아키텍처를 향상시키는가?

LiveData는 Jetpack 구성 요소 중 하나로, UI 계층에 변화 가능한 데이터를 제공합니다. 데이터가 변경될 때 뷰를 자동으로 업데이트하여 UI에 대한 효율적이고 반응적인 업데이트를 가능하게 합니다. LiveData는 뷰와 비즈니스 로직을 분리하여 앱을 유지보수하기 쉽게 해줍니다.

ViewModel의 목적을 설명하고, 안드로이드 개발에서 UI 계층과의 관계에 대해 설명해 주실 수 있나요?

<div class="content-ad"></div>

답변: 뷰모델은 Jetpack 컴포넌트로, UI와 관련된 데이터를 관리하기 위한 라이프사이클을 인식하는 컨테이너를 제공합니다. 뷰모델은 UI를 위한 데이터를 보유하며 구성 변경을 견뎌냅니다. UI와 데이터를 분리하여 UI와 데이터 계층 사이에 관심사를 깔끔하게 분리합니다.

12. Android 앱 개발에서 Dagger 사용이 앱 아키텍처를 어떻게 개선하는지 설명해주세요.

답변: Dagger는 의존성 주입 프레임워크로, 앱에서 의존성을 관리하는 것을 단순화합니다. 앱의 구성 요소를 분리하고 테스트와 유지관리를 더 쉽게 할 수 있도록 도와줍니다. 또한, 일반적인 코드를 피하고 코드 재사용성을 높이는 데 도움을 줍니다.

13. Android 앱 개발에서 Repository 패턴이 어떻게 작동하는지 설명해주실 수 있나요?

<div class="content-ad"></div>

답변: 저장소 패턴은 로컬 데이터베이스나 원격 API와 같은 다양한 소스에서 데이터를 관리하는 데 사용됩니다. 저장소는 데이터 소스와 앱 간의 중재자 역할을 하며, 추상화 계층을 제공하여 데이터 소스를 교체하는 것을 쉽게 만들어 나머지 앱에 영향을 미치지 않도록 합니다.

14. 안드로이드 앱 개발에서 의존성 역전 원칙(Dependency Inversion Principle, DIP)이 어떻게 구현되는지 설명해주세요.

답변: 의존성 역전 원칙은 고수준 모듈이 저수준 모듈에 의존하지 않아야 한다는 것을 말합니다. 대신 두 모듈 모두 추상화에 의존해야 합니다. 안드로이드 앱 개발에서는 비즈니스 로직이 프레임워크나 인프라의 구현 세부사항에 의존하지 않도록 코드를 작성해야 한다는 것을 의미합니다.

네트워킹 및 데이터 영속성

<div class="content-ad"></div>

- '레트로핏(Retrofit)'이란 무엇이고 어떻게 작동합니까?

레트로핏은 안드로이드 및 자바용 타입 안전한 HTTP 클라이언트로, 네트워크 요청을 쉽고 효율적으로 처리할 수 있도록 도와줍니다. 개발자들은 HTTP 요청 방식, URL, 요청 매개변수, 헤더 및 응답 유형을 지정하는 주석이 달린 메소드로 인터페이스를 정의할 수 있습니다. 이러한 인터페이스는 HTTP 요청을 생성하고 보내며, 응답을 처리하는 데 사용됩니다. 레트로핏은 또한 Gson, Jackson, Moshi를 포함한 여러 변환 라이브러리를 지원하여 응답 데이터를 구문 분석할 수 있습니다.

```java
@GET("posts") Call<List<Post>> getPosts(@Query("userId") int userId);
```

위의 예시에서, "https://example.com/posts" URL로 GET 요청이 전송되며, 쿼리 매개변수 "userId"가 포함되어 있습니다. 응답은 포스트 객체들의 리스트로 예상됩니다.

<div class="content-ad"></div>

Gson과 Jackson은 안드로이드에서 JSON 데이터를 파싱하는 데 사용되는 두 가지 인기 있는 자바 라이브러리입니다. Gson은 Google에서 개발되었으며 간단함과 사용 편의성으로 유명합니다. 한편 Jackson은 더 강력하고 기능이 풍부하지만 사용법이 더 복잡합니다. Gson은 Java 객체를 JSON으로 변환하거나 그 반대로 변환하기 위한 API 세트를 제공하며, Jackson은 JSON 처리를 위한 스트리밍 및 데이터 바인딩 API를 모두 제공합니다.

```java
Gson gson = new Gson(); 
String json = "{\"id\": 1, \"title\": \"Post Title\", \"body\": \"Post Body\"}"; 
Post post = gson.fromJson(json, Post.class);
```

위 예시에서는 Gson 라이브러리를 사용하여 JSON 문자열에서 Post 객체가 생성되었습니다.

<div class="content-ad"></div>

룸(Room)은 안드로이드 앱의 로컬 데이터 저장을 위해 사용되는 안드로이드 Jetpack 라이브러리의 일부입니다. SQLite 데이터베이스 작업에 대한 추상화 계층을 제공하여 개발자가 쿼리, 삽입, 업데이트 및 삭제와 같은 일반적인 데이터베이스 작업을 수행하기 쉽게 해줍니다. 룸은 엔티티를 정의하는 데 어노테이션을 사용하며, 엔티티는 데이터베이스의 테이블을 표현하는 객체이며 DAO(Data Access Objects)는 데이터베이스에 액세스하기 위한 메서드를 제공하는 인터페이스입니다.

위 예시에서 Post 엔티티는 기본 키와 테이블 이름을 지정하는 어노테이션을 사용하여 정의됩니다. 또한 PostDao 인터페이스는 Post 객체를 검색하고 삽입하는 메서드를 정의합니다.

**참고:**
```java
@Entity(tableName = "posts") 
public class Post { 
     @PrimaryKey public int id; 
     public String title; 
     public String body; 
} 

@Dao 
public interface PostDao { 
     @Query("SELECT * FROM posts") 
     List<Post> getAllPosts(); 
     
     @Insert 
     void insertPost(Post post); 
}
```

위의 예시에서 Post 엔티티는 기본 키와 테이블 이름을 지정하는 어노테이션을 사용하여 정의됩니다. 또한 PostDao 인터페이스는 Post 객체를 검색하고 삽입하는 메서드를 정의합니다.

다음 문제로 이어지는데, 안드로이드에서 동기식과 비동기식 네트워크 요청의 차이점에 대해 설명하겠습니다.

<div class="content-ad"></div>

답변: 안드로이드에서 동기 네트워크 요청은 주 스레드에서 실행되어 응용 프로그램이 멈추고 민첩성이 떨어질 수 있습니다. 반면에 비동기 네트워크 요청은 백그라운드에서 별도의 스레드에서 실행되어 주 스레드가 계속하여 사용자 인터페이스 이벤트를 처리할 수 있습니다. 비동기 요청은 콜백(callbacks), 인터페이스(interfaces) 또는 Kotlin 코루틴을 사용하여 일반적으로 구현됩니다.

```java
OkHttpClient client = new OkHttpClient(); 
Request request = new Request.Builder() 
          .url("https://example.com/posts") 
          .build(); client.newCall(request)
          .enqueue(new Callback() { 
                  
                @Override public void onResponse(Call call, Response response) throws IOException { 
                String responseBody = response.body().string(); 
                Log.d(TAG, responseBody); 
           } 
           
           @Override public void onFailure(Call call, IOException e) { 
                 Log.e(TAG, e.getMessage()); 
           } 
});
```

위의 예시에서 OkHttp 라이브러리를 사용하여 비동기 네트워크 요청을 수행했습니다. 요청은 별도의 스레드에서 실행되며 응답은 콜백을 사용하여 처리됩니다.

5. 안드로이드에서 Retrofit은 무엇이며 어떻게 작동하나요?

<div class="content-ad"></div>

[정답] Retrofit은 안드로이드 애플리케이션에서 API 호출을하는 데 사용되는 인기있는 타입 안전 HTTP 클라이언트 라이브러리입니다. 웹 서비스에서 JSON 또는 XML 데이터를 검색하고 보내는 프로세스를 간소화합니다. API의 HTTP 엔드 포인트를 설명하는 인터페이스를 정의함으로써 작동하며 Retrofit은 런타임 중에 이 인터페이스의 구현을 생성하는 데 주의합니다. 예를 들어, 다음 코드 조각은 Retrofit을 사용하여 인터페이스를 정의하는 방법을 보여줍니다:

```java
public interface MyApi {
    @GET("users") Call<List<User>> getUsers();
}
```

6. 안드로이드에서 Room은 무엇인가요? SQLite와 어떻게 다른가요?

[답변] Room은 안드로이드에서 SQLite 위에 추상화 레이어를 제공하는 데이터베이스 라이브러리로, 안드로이드 애플리케이션에서 로컬 데이터 저장소를 다루는 것을 더 쉽게 만듭니다. 데이터베이스 스키마 정의, 데이터 쿼리, 데이터베이스 트랜잭션 관리 과정을 간소화합니다. Room은 SQL 쿼리의 컴파일 시간 확인을 제공하며 데이터베이스 스키마를 마이그레이션하기 쉽게 합니다. SQLite과 다른 점은 데이터베이스 작업에 대해 더 객체 지향적인 접근 방식을 제공하며 LiveData와 같은 다른 안드로이드 라이브러리와 통합되어 데이터베이스 변경 사항을 쉽게 관찰할 수 있습니다.

<div class="content-ad"></div>

7. REST과 SOAP API의 차이는 무엇인가요?

Answer: REST (Representational State Transfer)와 SOAP (Simple Object Access Protocol)은 웹 서비스에 사용되는 두 가지 다른 유형의 API입니다. REST API는 가벼우며 데이터를 생성, 읽기, 업데이트, 삭제하는 작업을 수행하기 위해 HTTP 요청에 의존합니다. REST API는 서버의 리소스를 조작하기 위해 GET, POST, PUT, DELETE와 같은 HTTP 메소드를 사용합니다. 반면, SOAP API는 좀 더 무겁고 XML 메시징에 의존하여 작업을 수행합니다. SOAP API는 클라이언트와 서버 간의 형식적인 계약을 정의하며 교환될 수 있는 데이터의 유형을 포함하며 더 많은 보안 기능을 제공합니다.

8. 안드로이드에서 네트워크 요청을 어떻게 처리하나요?

Answer: 안드로이드에서는 Retrofit, OkHttp, 또는 Volley와 같은 라이브러리를 사용하여 네트워크 요청을 처리할 수 있습니다. 이러한 라이브러리는 API 요청을 쉽게 만들고 응답을 비동기적으로 처리하는 HTTP 클라이언트를 제공합니다. 요청 및 응답 데이터는 JSON 또는 XML 파서인 GSON 또는 Jackson을 사용하여 직렬화 및 역직렬화할 수 있습니다. 네트워크 요청은 별도의 스레드에서 수행되거나 메인 스레드를 차단하지 않도록 콜백이나 코루틴과 같은 비동기 메커니즘을 사용해야 합니다.

<div class="content-ad"></div>

9. Okhttp이란?

답변: OkHttp는 Android 앱 개발에서 사용되는 인기 있는 네트워킹 라이브러리로, HTTP 요청을 만드는 데 활용됩니다. Java의 HttpURLConnection API를 기반으로 구축되었으며 HTTP 요청과 응답을 보내고 받기 위한 간단한 API를 제공합니다. OkHttp에는 캐싱, 압축, 타임아웃 및 인증과 같은 기능을 지원합니다. 또한 동기 및 비동기 요청을 지원합니다.

예시: 아래는 OkHttp를 이용하여 API에서 데이터를 가져오는 GET 요청을 수행하는 예시입니다:

```kotlin
val client = OkHttpClient() 
val request = Request.Builder() 
   .url("https://api.example.com/users") 
   .build() 
val response = client.newCall(request).execute() 
val responseBody = response.body?.string() // 여기서 응답 본문을 처리합니다
```

<div class="content-ad"></div>

10. HTTP 요청에서 Cache-Control 헤더의 목적은 무엇인가요? 대답: HTTP 요청에서의 Cache-Control 헤더는 응답이 클라이언트나 프록시와 같은 중간 캐시에 의해 어떻게 캐싱되어야 하는지를 지정합니다. 이 헤더 값에는 응답이 캐싱될 수 있는 최대 시간을 나타내는 max-age와 같은 지시자, 캐시 없이는 응답을 제공할 수 없다는 no-cache, 그리고 전혀 캐시할 수 없다는 no-store 등이 포함될 수 있습니다. Cache-Control 헤더는 응답의 캐싱 동작을 제어하는 데 유용하며, 네트워크 요청의 수를 줄이고 앱 성능을 향상시키는 데 도움이 됩니다.

11. JSON은 무엇이며, Android 앱 개발에서 어떻게 사용되나요?

대답: JSON (JavaScript Object Notation)은 웹 서비스와 클라이언트 간의 데이터 교환에 사용되는 가벼운 데이터 교환 형식입니다. 이는 중첩되어 복잡한 데이터 구조를 형성할 수 있는 키-값 쌍의 컬렉션으로 구성되어 있습니다. Android 앱 개발에서는 JSON이 주로 웹 서비스에서 데이터를 구문 분석하고 GSON이나 Jackson과 같은 라이브러리를 사용하여 Java나 Kotlin 객체로 변환하는 데 사용됩니다. JSON 데이터는 또한 HTTP 요청의 일부로 웹 서비스에 보내져 리소스를 생성하거나 업데이트 하는 데 사용될 수 있습니다.

이 Android 인터뷰 여정에 동행해 주셔서 감사합니다! 저희 안내서가 여러분의 자신감과 통찰력을 높여 주었다면 👏 박수를 보내 주세요. 다가오는 인터뷰를 위한 당신의 신뢰할 수 있는 동반자로 이 문서를 즐겨찾기에 추가하고, 아래 댓글에 여러분의 제안과 생각을 꼭 남겨 주세요. 다가오는 제2부에 기대해 주세요. 거기에서는 Android 인터뷰 영역에 더 깊이 파고들어 당신의 경력에서 새로운 높이를 정복할 수 있는 지식과 전략을 제공할 것입니다. 여러분의 성공 스토리는 여기에서 시작됩니다!

<div class="content-ad"></div>

## Quest for Android Excellence: 인터뷰 에디션 2024 Part-II

## Quest for Android Excellence: 인터뷰 에디션 2024 Part-III