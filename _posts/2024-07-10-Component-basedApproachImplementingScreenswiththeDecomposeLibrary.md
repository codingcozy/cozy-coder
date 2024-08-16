---
title: "컴포넌트 기반 접근법 Decompose 라이브러리로 화면 구현하기"
description: ""
coverImage: "/assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_0.png"
date: 2024-07-10 01:48
ogImage: 
  url: /assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_0.png
tag: Tech
originalTitle: "Component-based Approach. Implementing Screens with the Decompose Library"
link: "https://medium.com/itnext/component-based-approach-implementing-screens-with-the-decompose-library-19c41d8ed087"
isUpdated: true
---





![Decompose Library](/assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_0.png)

안녕하세요! 이 글은 구성 요소 기반 접근 방식에 관한 시리즈 중 두 번째 글입니다. 첫 번째 글 '구성 요소 기반 접근 방식. 안드로이드 애플리케이션의 복잡성 극복하기'를 아직 읽지 않으셨다면, 그 글부터 읽어보시는 것을 권장드립니다. 이전 토론에서는 구성 요소 기반 접근 방식이 어떻게 UI 요소 ➜ 기능 블록 ➜ 화면 ➜ 흐름 ➜ 애플리케이션의 계층 구조로 애플리케이션을 조직하는지 살펴보았습니다. 이 구조는 화면과 네비게이션의 복잡성을 관리하는 데 효과적입니다.

이제 이 접근 방식을 실전에 적용해보려고 합니다. 우리는 Decompose 라이브러리를 활용하여 간단하고 복잡한 화면을 만들어보겠습니다. 실제 애플리케이션에서의 예시를 통해 이 작업이 어떻게 이루어지는지 알아보겠습니다. 흥미롭고 유익한 내용이 될 것으로 기대됩니다.

# Decompose 라이브러리

<div class="content-ad"></div>

컴포넌트 기반 접근 방식은 다양한 기술 스택으로 구현할 수 있어 다재다능합니다. 이러한 접근 방식을 용이하게 하는 라이브러리 중 하나로, 구글의 개발자인 Arkadii Ivanov이 만든 Decompose 라이브러리가 눈에 띕니다.

Decompose는 기능 블록, 화면, 흐름, 그리고 애플리케이션 전반의 구조를 구축하는 데 적합합니다. 일반적으로 최소한의 논리를 포함하는 UI 요소들에 대해서는 그리 필요하지 않습니다.

Decompose는 컴포넌트를 다루는 간단하고 사용자 친화적인 메커니즘을 제공합니다. 여기서 모든 기능을 다 다루지는 않겠지만, 상세한 문서에서 그 역할을 충분히 설명하고 있습니다. Decompose를 사용하여 화면을 만들기 위해 아래 중요한 구성 요소 두 가지를 사용해야 합니다:

- ComponentContext — Decompose의 핵심 요소로, 컴포넌트의 핵심 역할을 담당합니다. 컴포넌트에 수명 주기를 제공하여 생성, 기능 수행, 그리고 최종적으로 파괴될 수 있게 합니다.
- childContext — 이 함수는 자식 컴포넌트를 생성하는 데 도움을 줍니다.

<div class="content-ad"></div>

Decompose는 선언적 UI 프레임워크와 함께 사용할 때 가장 효과적입니다. 이 시리즈에서는 Jetpack Compose를 사용할 것입니다.

만약 XML 레이아웃, Fragment 및 ViewModel과 같은 보다 전통적인 스택과 작업 중이라면 걱정하지 마세요. 컴포넌트 기반 접근 방식은 특정 라이브러리 집합에 국한되지 않는 유연한 개념입니다. Decompose를 통해 학습함으로써 해당 원칙을 어떠한 기술에도 적응할 수 있습니다.

# Decompose를 사용하여 간단한 화면 만들기

Decompose와 Jetpack Compose를 사용하여 로그인 화면을 만들어보겠습니다. 그 간단함을 고려하면, 별도의 기능적 블록으로 분리할 필요가 없습니다.

<div class="content-ad"></div>


![Component Logic](/assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_1.png)

## Component Logic

Let’s start with the logic of this screen. We’ll create a SignInComponent interface, along with its implementation, named RealSignInComponent. The rationale behind introducing the interface will be discussed a little later.

Here is the code for SignInComponent:


<div class="content-ad"></div>


신입사원 구성 요소 인터페이스:

- login: StateFlow<String>을 가져옵니다.
- password: StateFlow<String>을 가져옵니다.
- inProgress: StateFlow<Boolean>을 가져옵니다.
- onLoginChanged(login: String) 함수를 정의합니다.
- onPasswordChanged(password: String) 함수를 정의합니다.
- onSignInClick() 함수를 정의합니다.

실제 신입사원 구성 요소 코드:

```js
class RealSignInComponent(
   componentContext: ComponentContext,
   private val authorizationRepository: AuthorizationRepository
) : ComponentContext by componentContext, SignInComponent {

   override val login = MutableStateFlow("")
   override val password = MutableStateFlow("")
   override val inProgress = MutableStateFlow(false)
   
   private val coroutineScope = componentCoroutineScope()

   override fun onLoginChanged(login: String) {
       this.login.value = login
   }

   override fun onPasswordChanged(password: String) {
       this.password.value = password
   }

   override fun onSignInClick() {
       coroutineScope.launch {
           inProgress.value = true
           authorizationRepository.signIn(login.value, password.value)
           inProgress.value = false
           
           // TODO: 다음 화면으로 이동
       }
   }
}
```

주요 포인트를 살펴보겠습니다:


<div class="content-ad"></div>

인터페이스에서 사용자 작업을 처리하기 위해 구성 요소의 속성과 메소드를 정의했습니다. StateFlow를 사용하면 이러한 속성은 관찰 가능해져서 자동으로 변경 사항을 알립니다.

클래스 생성자에서 ComponentContext를 전달하고 동일한 인터페이스를 위임을 사용하여 구현했습니다 (by 키워드로 표시됨). 이 방법은 Decompose로 구성 요소를 만들 때 표준적인 방법이며 기억해야 할 중요한 것입니다.

우리는 componentCoroutineScope 메소드를 활용하여 비동기 작업 (코루틴)을 실행하는 CoroutineScope를 설정했습니다. 이 CoroutineScope는 ComponentContext의 수명주기 기능을 활용하여 구성 요소가 파괴될 때 자동으로 취소됩니다.

onSignInClick 메소드에서 사용자 이름과 비밀번호를 사용하여 로그인 프로세스를 실행했습니다. 필드 유효성 검사와 오류 처리를 제외하고 이 예제를 단순화했습니다. 성공적인 로그인 후에는 일반적으로 다음 화면으로 이동할 것입니다. 그러나 아직 네비게이션 세부 사항이 다루어지지 않았으므로, 나중에 구현 예정으로 TODO로 표시했습니다.

전체적으로 코드는 직관적입니다. MVVM에 익숙하다면 이해하기 쉬울 것입니다.

## Component UI

이제 화면 UI를 구현해 봅시다. 간결함을 위해 일부 레이아웃 설정은 생략하고 주요 내용에만 초점을 맞추었습니다.

<div class="content-ad"></div>


@Composable
fun SignInUi(component: SignInComponent) {

   val login by component.login.collectAsState(Dispatchers.Main.immediate)
   val password by component.password.collectAsState(Dispatchers.Main.immediate)
   val inProgress by component.inProgress.collectAsState()

   Column {
       TextField(
           value = login, 
           onValueChange = component::onLoginChanged
       )

       TextField(
           value = password,
           onValueChange = component::onPasswordChanged
       )

       if (inProgress) {
           CircularProgressIndicator()
       } else {
           Button(onClick = component::onSignInClick)
       }
   }
}


To link the component with its UI:

- We obtain values from StateFlow using the `collectAsState` function and incorporate these values into the UI elements. The UI will automatically update whenever there are changes in the component’s properties.
- We connect text inputs and button presses to the component’s handler methods.

## Important Information about the Terminology


<div class="content-ad"></div>

'구성 요소(component)'라는 용어는 두 가지 명확한 의미를 갖도록 발전해 왔습니다. 일반적으로 구성 요소는 특정 기능을 담당하는 모든 코드를 포괄합니다. 이에는 SignInComponent, RealSignInComponent, SignInUi 및 심지어 AuthorizationRepository와 같은 구성 요소가 포함됩니다. 그러나 Decompose 라이브러리의 맥락에서 '구성 요소'라는 용어는 주로 로직을 관리하는 클래스 또는 인터페이스를 의미하는 경우가 많습니다 — 즉, RealSignInComponent 및 SignInComponent입니다. 일반적으로 이러한 이중 사용은 혼란을 초래하지 않습니다. 의도한 의미가 대부분의 상황에서 명확하기 때문입니다.

## UI 미리보기

Android Studio에서 미리보기를 추가하려면 구성 요소의 인터페이스를 도입해야 합니다. 여기서 UI의 시각적 미리보기가 코드 옆에 표시됩니다. 이를 용이하게 하기 위해 구성 요소의 가짜 구현을 만들고 미리보기에 연결하겠습니다.

```kotlin
class FakeSignInComponent : SignInComponent {

   override val login = MutableStateFlow("login")
   override val password = MutableStateFlow("password")
   override val inProgress = MutableStateFlow(false)

   override fun onLoginChanged(login: String) = Unit
   override fun onPasswordChanged(password: String) = Unit
   override fun onSignInClick() = Unit
}

@Preview(showSystemUi = true)
@Composable
fun SignInUiPreview() {
   AppTheme {
       SignInUi(FakeSignInComponent())
   }
}
```

<div class="content-ad"></div>

## Root ComponentContext

마지막으로 고려해야 할 사항은 위임해야 하는 ComponentContext를 어디서 얻을지입니다. 이것이 RealSignInComponent에 제공해야 하는 ComponentContext입니다.

ComponentContext를 생성해야 하지만 중요한 점은 이것이 전체 애플리케이션에 대해 한 번만 수행된다는 것입니다. 특히 루트 컴포넌트에 대해 이루어집니다. 다른 컴포넌트들도 각자의 ComponentContext를 가지게 되지만, 이들은 다르게 획득됩니다. 이에 대해 나중에 알아볼 것입니다.

우리의 논의 목적을 위해 애플리케이션이 하나의 화면, 즉 로그인 화면만 있다고 가정해 봅시다. SignInComponent는 효과적으로 루트 컴포넌트가 됩니다. ComponentContext를 초기화하려면 Decompose의 defaultComponentContext 유틸리티 메서드를 사용합니다. 이 메서드는 Activity에서 호출되어야 합니다. 이렇게 함으로써 ComponentContext의 라이프사이클이 Activity의 라이프사이클과 동기화됨을 보장합니다.

<div class="content-ad"></div>

더 간단한 화면을 위한 컴포넌트가 준비되었습니다.

# 복잡한 화면을 부분으로 나누기

<div class="content-ad"></div>

복잡한 화면을 부분으로 나누는 것은 실용적인 방법입니다. 이는 부모 컴포넌트와 다양한 기능 블록을 나타내는 여러 자식 컴포넌트로 구성됩니다. 예를 들어, 운전 시험 준비를 위한 애플리케이션의 주 화면을 고려해보십시오:

![image](/assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_2.png)

이 화면에서 툴바와 진행 상황, '다음 시험' 카드, 모든 시험, 이론, 시험 및 피드백을 나타내는 구별된 블록이 명확하게 보입니다.

## 자식 컴포넌트

<div class="content-ad"></div>

한 번에 하나씩 기능 블록마다 별도의 구성 요소를 개발할 것입니다. 이러한 구성 요소의 코드는 익숙한 패턴을 따릅니다. 구성 요소의 인터페이스, 구현 및 UI로 구성됩니다.

예를 들어, 툴바 구성 요소는 다음과 같습니다:

```kotlin
interface ToolbarComponent {

   val passingPercent: StateFlow<Int>

   fun onHintClick()
}
```

```kotlin
class RealToolbarComponent(componentContext: ComponentContext) :
   ComponentContext by componentContext, ToolbarComponent {
   // some logic
}
```

<div class="content-ad"></div>

```kotlin
@Composable
fun ToolbarUi(component: ToolbarComponent) {
   // 일부 UI
}
```

비슷하게, NextTestComponent, TestsComponent, TheoryComponent, ExamComponent, FeedbackComponent 및 각각의 UI를 생성할 것입니다.

## 부모 컴포넌트

화면 컴포넌트는 기능 블록 컴포넌트의 부모로 기능할 것입니다.


<div class="content-ad"></div>

Let's take a look at the interface declaration:

```kotlin
interface MainComponent {
  
    val toolbarComponent: ToolbarComponent
    
    val nextTestComponent: NextTestComponent
    
    val testsComponent: TestsComponent
    
    val theoryComponent: TheoryComponent
    
    val examComponent: ExamComponent
    
    val feedbackComponent: FeedbackComponent
    
}
```

Here, the MainComponent interface openly reveals its child components without hiding them. Now, in the implementation, we will make use of the `childContext` method from Decompose.

<div class="content-ad"></div>

```kotlin
class RealMainComponent(
    componentContext: ComponentContext
) : ComponentContext by componentContext, MainComponent {

    override val toolbarComponent = RealToolbarComponent(
        childContext(key = "toolbar")
    )

    override val nextTestComponent = RealNextTestComponent(
        childContext(key = "nextTest")
    )

    override val testsComponent = RealTestsComponent(
        childContext(key = "tests")
    )

    override val theoryComponent = RealTheoryComponent(
        childContext(key = "theory")
    )

    override val examComponent = RealExamComponent(
        childContext(key = "exam")
    )

    override val feedbackComponent = RealFeedbackComponent(
        childContext(key = "feedback")
    )
}
```

The `childContext` method creates a new child ComponentContext. It’s important that each child component has its own context. Decompose mandates that these child contexts have unique names, which we achieve by using the `key` parameter.

Now, all that remains is to add the UI, and then it’s all set:

```kotlin
@Composable
fun MainUi(component: MainComponent) {
    Scaffold(
        topBar = { ToolbarUi(component.toolbarComponent) }
    ) {
        Column(Modifier.verticalScroll()) {
            NextTestUi(component.nextTestComponent)

            TestsUi(component.testsComponent)

            TheoryUi(component.theoryComponent)

            ExamUi(component.examComponent)

            FeedbackUi(component.feedbackComponent)
        }
    }
}
```

<div class="content-ad"></div>

결과적으로 구성 요소의 코드는 간단하고 조밀합니다. 화면을 여러 부분으로 분해하지 않고는 이를 달성할 수 없었을 겁니다.

# 구성 요소 간 상호 작용 조직화

이상적인 경우, 자식 구성 요소는 서로 완전히 독립적이어야 합니다. 그러나 항상 그럴 수 있는 것은 아닙니다. 때로는 한 구성 요소에서 발생한 이벤트가 다른 구성 요소에서 특정 작업을 필요로 할 수 있습니다.

이전 예제의 화면을 고려해 보세요. 긍정적인 피드백을 남긴 후 '이론' 블록에서 사용자가 보너스 교육 자료를 받는 시나리오를 상상해 보세요. 이 특정 요구 사항은 실제 응용 프로그램의 일부가 아닙니다; 이것은 설명을 위해 만들어진 것입니다.

<div class="content-ad"></div>

피드백 구성 요소와 이론 구성 요소 간의 상호 작용을 조직해야 합니다. 일반적으로 떠오르는 첫 번째 생각은 RealFeedbackComponent에서 TheoryComponent에 대한 참조를 만드는 것입니다. 그러나 이것은 최적의 해결책이 아닙니다! 이것은 피드백 구성 요소가 주요 기능 이상의 작업을 처리하도록 하여 이론 자료를 관리하는 것으로 이어집니다. 이러한 구성 요소 간의 직접적인 링크를 계속 추가하면, 그들은 빠르게 지나치게 과부하되고 재사용할 수 없게 됩니다.

다른 방법을 취해 봅시다. 상호작용 책임을 부모 컴포넌트에 할당할 것입니다. 필요한 이벤트를 신호하기 위해 콜백을 사용할 것입니다.

다음과 같이 코드를 구성할 것입니다:

- TheoryComponent에는 보너스 교육 자료에 액세스할 수 있도록 unlockBonusTheoryMaterial이라는 메서드를 도입할 것입니다.
- RealFeedbackComponent에서는 적절한 시간에 트리거될 콜백 함수 onPositiveFeedbackGiven: () -> Unit을 생성자를 통해 전달할 것입니다.
- 그런 다음 RealMainComponent에서 이 두 구성 요소 사이에 연결을 설정할 것입니다:

<div class="content-ad"></div>

```kotlin
위 내용을 요약하자면, 컴포넌트 간 상호작용 가이드라인은 다음과 같습니다:

- 자식 컴포넌트간 직접적인 상호작용을 피해야 합니다.
- 자식 컴포넌트는 콜백을 사용하여 부모에게 알릴 수 있습니다.
- 부모 컴포넌트는 자식 컴포넌트의 메소드를 직접 호출할 수 있습니다.
```

![image](/assets/img/2024-07-10-Component-basedApproachImplementingScreenswiththeDecomposeLibrary_3.png)

<div class="content-ad"></div>

# 더 많은 자료

## 분해하기

- GitHub의 [분해하기](https://github.com): 라이브러리에 대한 간결한 개요, 이슈, 토론, 그리고 프로젝트를 스타로 표시하는 옵션.
- [분해하기 문서](https://decompose.dev): ComponentContext가 제공하는 추가 기능에 대해 알아보세요.

## 다른 라이브러리

<div class="content-ad"></div>

- RIBs: 이동 애플리케이션에 대한 구성 요소 기반 접근 방식의 최초 오픈 소스 구현 중 하나입니다.
- appyx: 주목할만한 제한 사항이 있는 현대 라이브러리 — Compose Multiplatform과만 통합됩니다.

## 클래식 스택

문서 "프래그먼트 간 통신하는 방법": 저자가 콜백을 사용하여 프래그먼트 간 통신을 어떻게 조직화하는지를 보여줍니다.

# 계속해서 번역 됩니다

<div class="content-ad"></div>

복잡한 화면에 대한 주제를 다뤘어요. 설명된 기술을 활용하면 어떤 복잡도의 화면도 효과적으로 다룰 수 있을 거예요.

다음으로, Decompose를 사용하여 내비게이션 구성에 초점을 맞출 거에요.