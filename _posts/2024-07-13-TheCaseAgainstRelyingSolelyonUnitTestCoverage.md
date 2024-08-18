---
title: "단위 테스트 커버리지에만 의존하지 말아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-13-TheCaseAgainstRelyingSolelyonUnitTestCoverage_0.png"
date: 2024-07-13 23:32
ogImage:
  url: /assets/img/2024-07-13-TheCaseAgainstRelyingSolelyonUnitTestCoverage_0.png
tag: Tech
originalTitle: "The Case Against Relying Solely on Unit Test Coverage"
link: "https://medium.com/better-programming/the-case-against-relying-solely-on-unit-test-coverage-1cb3977e38ac"
isUpdated: true
---

![TheCaseAgainstRelyingSolelyonUnitTestCoverage](/assets/img/2024-07-13-TheCaseAgainstRelyingSolelyonUnitTestCoverage_0.png)

유닛 테스트 커버리지의 가치는 소프트웨어 엔지니어들 사이에서 오랫동안 논란의 여지가 있었습니다. 어떤 사람들은 새 코드의 모든 부분이 제대로 테스트되도록 보장한다고 주장합니다. 그러나 다른 사람들은 커버리지가 단순히 플레이스보로 작용하는 의미 없는 지표라고 주장합니다.

내 의견은 두 가지 모두 맞다고 생각합니다.

유닛 테스트가 코드 일부를 커버한다고 해서 버그가 없음을 보장하는 것은 당연하지 않을 수 있습니다. 유닛 테스트는 개발자의 기대에 따라 제어된 환경에서 실행됩니다. 불행하게도, 실제 환경은 예측할 수 없는 것입니다.

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

비즈니스 로직은 동일한 코드 라인을 실행하는 여러 복잡한 상태를 가질 수 있습니다. 하나의 조건만을 테스트하는 것은 코드를 철저히 테스트하지 않는다는 것을 의미합니다. 비록 커버리지 숫자가 그렇게 말해주지만요.

예를 들어봅시다. 이 간단한 함수를 살펴봅시다:

```js
fun foo(val x: Int, val y: Int) : Int {
  var sum = 0
  if (x > 10) {
    result += x
  }
  if (y > 10) {
    result += y
  }
  return sum
}
```

이 함수에는 두 가지 조건문이 있어 네 가지 가능한 상태를 발생시킵니다. 우리는 하나의 상태만을 cover해도 100% 코드 커버리지를 달성할 수 있습니다. 이것을 보여주는 테스트 함수가 여기 있습니다.

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
@Test
fun fooTest() {
  val x = 20
  val y = 50
  val sum = foo(x,y)
  Truth.assertThat(sum).isEqualTo(70)
}
```

당신의 코드 건강 지표를 보면 실제로 다뤄야 하는 경우의 25%만 커버했음에도 100%의 코드 커버리지를 보여줄 것입니다. 이것은 간단한 예제일 뿐이지만, 비슷한 상황이 프로덕션 코드에서 발생할 수 (그리고 발생할 것) 있습니다. 이 함수를 조금 더 흥미롭게 만들어 봅시다.

```js
fun divideByFoo(val x: Int, val y: Int, val z: Int ) : Int {
  var quotient = z
  if (x > 0) {
    result /= x
  }
  if (y >= 0) {
    result /= y
  }
  return quotient
}
```

한 번의 테스트로 100% 커버리지를 달성해 봅시다.

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
@Test
fun divideByFooTest() {
  val x = 1
  val y = 2
  val z = 90;
  val sum = divideByFoo(x,y, z)
  Truth.assertThat(sum).isEqualTo(45)
}
```

여기서는 제가 2번째 조건문에서 '≥' 대신에 '`'를 사용하면서 어리석은 실수를 범했습니다. 이제 제로로 나눌 수 있고 100%의 테스트 커버리지를 갖고 있어도 예외를 발생시킬 수 있습니다! 이것은 테스트 품질을 결정하는 데 이 지표에 독립적으로 의존하는 것의 순진함을 보여줍니다. 테스트를 작성할 때 가능한 많은 입력을 고려하는 것이 매우 중요합니다. 너무 많은 입력을 평가하기 어렵다면 구현을 분리하는 것을 고려해보세요. 너무 많은 일을 하는 함수들은 테스트하기 어려울 수 있습니다.

빠진 경계 상황만이 부실한 테스트의 유일한 원인은 아닙니다. 부실한 어설션(assertion)도 많이 발견되는 죄악 중 하나입니다. 개발자들은 취약한 어설션을 사용하여 품질이 낮은 테스트를 작성하면서 커버리지 숫자를 부풀릴 수 있습니다. 유닛 테스트가 실패 또는 성공하는 것은 그 테스트가 하는 어설션에 달려 있습니다. 저자가 직접 이를 제어하며, 이것은 종종 실제로 커버되는 라인과는 아무 관련이 없을 수도 있습니다.

다시 한 번 예를 살펴봅시다

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
@Test
fun fooTest() {
  val x = 20
  val y = 50
  val sum = foo(x,y)
  Truth.assertThat(sum).isNotNull()
}
```

결과가 null이 아닌지 확인하는 것은 실제로 함수가 제대로 작동하는지 확인하는 데 아무런 도움이 되지 않습니다. 이것은 테스트가 통과된 것처럼 보이도록 하는 해킹에 더 가깝습니다. 모든 테스트는 반환값이나 데이터베이스 업데이트와 같은 특정 코드를 검증하는 "강력한" 어서션을 가져야 합니다.

이러한 표준을 강요하는 것은 테스트에 대한 범죄를 쉽게 면할 수 있기 때문에 어렵습니다. 높은 품질의 테스트를 작성하는 것에 대한 책임을 따르고 서로 책임질 수 있도록 개발팀이 체제를 따라야 합니다.

## 돌연변이로 테스트하라

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

지금까지 우리는 100% 커버리지를 달성하면서도 경우를 놓치는 테스트를 작성하는 것이 얼마나 쉬운지 살펴보았습니다. 만약 이러한 테스트를 찾아내는 방법이 있다고 말한다면 어떨까요? 바로 돌연변이 테스트입니다.

돌연변이란 소스 코드에서의 단일 변경을 의미하며 당신의 테스트에서의 결함을 찾아내기 위한 것입니다. 이를 하는 방법은 여러 가지가 있습니다. 값의 변경, 조건문의 반전, 심지어 전체 줄 삭제 등이 될 수 있습니다.

이 수정은 이상적으로는 테스트가 실패하도록 만들어야 합니다. 실패는 테스트가 돌연변이에 의해 도입된 버그를 감지하고 잡은 것을 나타냅니다.

만약 여러분의 테스트가 실패하지 않았다면, 그건 나쁜 것만은 아닙니다. 돌연변이가 관련이 없는 줄을 변경했을 수도 있습니다. 그러나 일반적으로 돌아남은 돌연변이는 더 나은 테스트를 작성해야 한다는 것을 의미합니다.

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

**충분한 검사를 보장하기 위해, 여러 분의 유전자 변형체를 생성하는 것이 좋습니다. 각각은 작은 변화 중 하나만 가지고 있어야 합니다. 테스트 스위트에 변이를 적용하는 것을 쉽게 만들어 주는 많은 라이브러리와 프레임워크가 있습니다. 우리 팀에서는 새로운 테스트가 병합되기 전에 자동으로 변이에 의해 검토됩니다. 우리는 모든 생존한 변이를 제거하려 하지 않지만, 그것들은 표시됩니다.**

**## 커버리지가 모두 나쁜 것은 아닙니다**

**불평을 하면서도, 나는 여전히 코드 커버리지가 어떤 가치를 제공한다고 믿습니다. 그렇지 않으면 우리 팀은 그것을 사용하지 않을 것입니다.**

**커버리지 지표는 목표가 되었을 때만 속이는 것입니다. 엔지니어들은 자신들의 영향을 양적으로 표현하는 것을 좋아합니다. 코드 커버리지는 낮은 과일처럼 쉽게 얻을 수 있는 것입니다. "프로젝트 코드 커버리지를 x% 증가시켰다"라고 성과 평가에서 써 넣는 것은 실제로는 생성자를 호출하는 테스트를 작성한 것뿐이기 때문에 누구에게도 도움이 되지 않습니다.**

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

테스트 전략의 결과물로 높은 코드 커버리지를 가져와야 하며, 최종 목표가 되어서는 안 됩니다. 각 엣지 케이스를 철저하게 테스트하면 높은 커버리지 수치에 도달하게 될 겁니다. 시간을 적당히 투자하여 테스트 전략을 계획하는 것이 중요합니다. 테스트 주도 개발을 채택할 필요는 없습니다.

단위 테스트 커버리지는 테스트 계획의 약점을 드러낼 수도 있습니다. 기존의 결함이 발견되는 지점으로 이어집니다. 개발자가 새 코드를 병합할 때마다 커버리지를 다시 계산해야 합니다. 커버리지 감소는 심각한 문제로 취급해야 합니다. 추가 테스트를 추가하지 않은 채로 코드를 제출하는 일은 거의 없어야 합니다.

## 통합 테스트도 빼 놓지 마세요

높은 커버리지 수치와 견고한 단위 테스트 스위트를 갖고 있더라도, 통합 테스트는 필수입니다. 소프트웨어는 거의 결합된 부분들의 모음이 아닙니다. 이러한 부분들은 보통 함께 작동해야 합니다.

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

유닛 테스트가 모든 상황에 대한 해답이어서는 안 됩니다. 어떤 코드는 유당 테스트로 철저히 테스트하기 어려울 수도 있습니다. 데이터베이스 트랜잭션이나 비즈니스 로직 확인만큼 간단한 것이 아니기 때문입니다. 이러한 상황은 테스트 도구 상자에서 다른 것을 찾도록 신호를 주어야 합니다.

고수준의 테스트 프레임워크는 크게 발전했습니다. 개발자들은 코드가 예상대로 작동하는지 확인하기 위해 선택할 수 있는 무수히 많은 도구들을 가지고 있습니다. 이 도구들을 숙달하여 팀의 코드베이스에 도입하는 것은 의미 있는 영향을 줄 수 있는 환상적인 방법입니다. 통합 테스트에 경험이 많지 않은 엔지니어들이 놀랄 정도로 많은데, 이는 가치 있는 기술이라고 할 수 있습니다.

사용하는 정확한 프레임워크는 프로젝트의 기술 스택에 따라 다를 것입니다. 저는 주로 Android 앱을 다루는데, 대부분의 종단 간 테스트에 Espresso를 사용합니다. 이 강력한 프레임워크는 전체 사용자 경로를 확인하는 데 확장할 수 있습니다.

```kotlin
fun simpleTest() {
  onView(withId(R.id.button)).perform(click())
  onView(withText("Button clicked")).check(matches(isDisplayed()))
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

에스프레소는 강력한 것들을 매우 적은 코드로 처리할 수 있는 몇 가지 추가적인 프레임워크가 내장되어 있습니다. 예를 들어, 사용자의 저장 목록에 항목을 추가하고 변경 사항이 UI에 반영되는지 확인하는 이와 같은 테스트가 만들어질 수 있습니다. 이 과정은 데이터베이스 트랜잭션, API 호출, UI 레이어 및 비즈니스 로직에 의존하는 흐름일 가능성이 높습니다.

```kotlin
fun addToSavedListTest() {
  openMyListPage()
  val listSize = getListSize()
  openAppHomePage()
  getFirstCard().perform(click())
  openSavedListPage()
  checkListSize(listSize)
}
```

통합 테스트도 여전히 단점을 갖고 있습니다. 더 많은 규모에서 작동하기 때문에, 단위 테스트보다 훨씬 더 작성하기 어렵고, 실행하는 데 더 오랜 시간이 걸리며, 끊임없이 오류가 발생하기 쉽습니다. 통합 테스트는 테스트되는 각 모듈과 그들 사이의 상호작용을 철저히 이해해야 합니다. 한 조각이 깨지면 전체 테스트가 무너집니다. 실패를 디버깅하는 것이 어려워질 수 있습니다. 대규모 기업에서는 제품 팀이 통합 테스트 프레임워크를 구축하고 유지하는 데 전문 엔지니어 팀을 배치하기도 합니다.

## 교훈

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

테스트 커버리지는 제대로 활용할 때 가치 있는 지표가 될 수 있어요. 결함을 발견하고 트렌드를 강조하며 전반적인 테스트 강도를 엿볼 수도 있죠. 하지만 완벽한 것이 아니며, 너무 의존하면 오히려 해로울 수 있어요.

좋은 테스트 계획은 쉽게 테스트할 수 있도록 코드를 구성하는 데서 시작돼요. 테스트를 작성하는 개발자는 커버리지 목표만 충족시키는 대신 엣지 케이스와 실패 시나리오를 다루는 데 초점을 맞춰야 해요. 변이(mutations)는 이러한 기준을 강화하는 데 도움이 될 거예요. 마지막으로, 단위 테스트 이외의 다른 테스트도 제품 전반적인 안정성을 강화하기 위해 통합돼야 해요.

저는 안드로이드 분야에서의 테스트 경험을 많이 쌓아왔어요. 다른 사람들이 테스트에 접근하는 방법을 배우는 것에 항상 흥미를 느껴요. 여러분의 생각을 댓글로 남겨주세요. 읽어 주셔서 감사해요.

저와 소통하고 싶다면, 이미 팔로우하고 있지 않다면 트위터에서 저를 팔로우해보세요.
