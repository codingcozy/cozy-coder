---
title: "Kotlin 시일드 클래스Sealed Classes 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-KotlinSealedclasses_0.png"
date: 2024-07-10 01:53
ogImage:
  url: /assets/img/2024-07-10-KotlinSealedclasses_0.png
tag: Tech
originalTitle: "Kotlin Sealed classes"
link: "https://medium.com/@guruprasadhegde4/kotlin-sealed-classes-9dcb75dea0df"
isUpdated: true
---

이미지 링크는 아래와 같이 표기해주세요.

![이미지 설명](/assets/img/2024-07-10-KotlinSealedclasses_0.png)

코틀린의 sealed class는 제한된 클래스 계층 구조를 정의할 수 있는 특별한 종류의 클래스입니다. sealed class의 주요 특징은 어떤 클래스가 하위 클래스가 될 수 있는지를 제한한다는 점입니다. sealed class의 모든 직접적인 하위 클래스는 sealed class 자체가 선언된 파일 내에 선언되어야 하지만, sealed class 내에 중첩될 수도 있고 파일 내에서 별도로 정의될 수도 있습니다. 이 제한은 when 표현식에서 sealed class를 사용할 때 컴파일러가 완전한(Exhaustive) 체크를 수행할 수 있도록 해주어 모든 가능한 하위 클래스가 고려되어야 함을 보장합니다.

sealed class는 sealed 키워드를 사용하여 정의됩니다. 기본 구문은 다음과 같습니다:

```js
sealed class ClassName {
    // 하위 클래스와 속성
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

## 주요 특징

- Subclassing 제한: 동일 파일에 정의된 클래스만이 sealed 클래스를 확장할 수 있습니다.
- Exhaustive when 문: Kotlin 컴파일러는 when 표현식에서 모든 가능한 경우가 처리되었는지 확인할 수 있습니다.
- 기본적으로 추상화: Sealed 클래스는 직접 인스턴스화할 수 없으며 기본적으로 추상화되어 있습니다.
- 또한 sealed 클래스는 다른 독특한 특징을 가지고 있는 데, 그것은 생성자가 기본적으로 보호되어 있다는 것입니다.

## Sealed 클래스의 예시를 보겠습니다. 어떻게 사용하는 것이 관용적인 방법인지 살펴봅시다.

```kotlin
sealed class UserEvent {

    data object LoginClicked : UserEvent()

    data object LogOutCLicked : UserEvent()
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

질문이 있으신가요? Sealed 클래스를 사용하면 사용자 동작 종류에 따라 작업을 수행해야 할 때 UserEvent의 가능성이 두 가지로 소진되고 UserEvent 인스턴스가 정확히 두 옵션 중 하나임을 확인할 수 있습니다.

만약 sealed 클래스를 사용하지 않으면 가능성이 두 개 이상으로 소진되고 else 문을 포함해야 합니다.

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

## 만일 sealed 클래스가 없다면 가능성을 모두 다 다룬(exhaust) 것이 없습니다.

가능성을 완전히 다 다루는 것의 사용 사례는 무엇인가요? 동일한 페이지에 비밀번호를 잊어버렸을 때와 같은 다른 버튼이 있다고 상상해보세요. 사용자가 클릭하면 안 되는 비밀번호를 클릭하지 않도록 보장하고 싶다면요.

만일 sealed class가 없다면, 다른 사람이 새로운 UserEvent를 정의할 수 있습니다:

```kotlin
sealed class UserEvent {
    data class LoginClicked : UserEvent()
    data class LogOutClicked : UserEvent()
    data class ForgotPasswordClicked : UserEvent()
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

```kotlin
fun main() {

    fun handleUserEvent(event: UserEvent) : String {
        return when(event) {
            is UserEvent.LoginClicked -> "Login clicked"
            is UserEvent.LogOutCLicked -> "Logout clicked"
            else -> { "The user has clicked forgot password."}
        }
    }
}
```

Your handleUserEvent method will take action on this unexpected UserEvent since it is included in the else statement.

Similar to that, in your program, it's beneficial to cover all possibilities to reduce the number of states your application can be in and decrease the likelihood of bugs arising from undefined behaviors in certain states.

This is why sealed classes are necessary in your code.

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

## 생성자

**sealed class**는 생성자를 포함하거나 상속할 수 있습니다. 이 생성자들은 sealed class 자체의 인스턴스를 생성하는 데 사용되는 것이 아니라 그 하위 클래스들을 위한 것입니다.

sealed class의 생성자는 하위 클래스들이 어떻게 인스턴스화될지를 제어하는 어떤 가시성 수정자(public, private, protected, 또는 internal)든 사용할 수 있습니다.

```kotlin
sealed class HttpMethod {

    class Get(val path: String) : HttpMethod()

    class Post(val path: String, val body: String) : HttpMethod()

    class Put(val path: String) : HttpMethod()

    data object Delete : HttpMethod()
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

```kotlin
HttpMethod은 우리의 sealed 클래스입니다.
Get, Post 및 Put은 각각 관련 데이터(path, body)를 보관하기 위한 생성자를 가진 데이터 클래스입니다.
Delete은 특정 데이터가 필요하지 않기 때문에 객체입니다.

당신은 이 sealed 클래스를 이전 예제에서 보여준 것과 유사한 방식으로 사용할 것입니다. 서로 다른 HTTP 메서드 인스턴스를 처리하기 위해 when 표현식을 사용할 것입니다.

## 생성자 가시성 제어

만약 위 예제에서 Post 인스턴스의 생성을 특정 모듈 내에서만 허용하게 제한하고 싶다면, 생성자를 internal로 선언할 수 있습니다.
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

**Markdown 코드:**

```kotlin
sealed class HttpMethod {

    class Success internal constructor(val path: String): HttpMethod()

    class Failure : HttpMethod()
}
```

세aled 클래스 내부에 Enum을 사용할 수 있습니다.

Enum과 sealed 클래스는 고정된 값 집합을 표현하는 데 일부 유사성을 공유하지만 서로 다른 목적을 가지고 있고 독특한 기능을 갖고 있습니다. 효과적으로 결합하는 방법을 살펴보겠습니다.

시나리오: 결제 방법 모델링
전자 상거래 앱을 구축하고 다양한 결제 방법을 나타내야 한다고 상상해보십시오. Enum을 사용하여 사용 가능한 옵션을 정의할 수 있습니다.

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

이제 각 결제 방법마다 연관된 특정 데이터가 있을 수 있습니다. 여기서 sealed 클래스가 필요하게 됩니다.

```kotlin
sealed class PaymentDetails {

    class CreditCard(
        val cardNumber: String,
        val expiryDate: String,
        val cvv: String
    ) : PaymentDetails()

    class PhonePe(
        val phoneNumber: String,
        val amount: Double
    ) : PaymentDetails()

    class GPay(
        val phoneNumber: String,
        val amount: Double
    ) : PaymentDetails()
}
```

```kotlin
// totalCount의 값을 센다
fun main() {

    fun paymentUsage(paymentDetails: PaymentDetails) : String {
        return when(paymentDetails) {
            is PaymentDetails.CreditCard -> "Payment done through credit card"
            is PaymentDetails.PhonePe -> "Payment done through PhonePe"
            is PaymentDetails.GPay -> "Payment done through GPay"
        }
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

**조합하기**

이제 지불 유형과 해당 세부 정보를 캡슐화하기 위해 데이터 클래스를 사용할 수 있습니다.

```kotlin
enum class PaymentType {
    CREDIT_CARD,
    PHONE_PAY,
    GOOGLE_PAY
}

sealed class PaymentDetails {

    class CreditCard(
        val cardNumber: String,
        val expiryDate: String,
        val cvv: String
    ) : PaymentDetails()

    class PhonePe(
        val phoneNumber: String,
        val amount: Double
    ) : PaymentDetails()

    class GPay(
        val phoneNumber: String,
        val amount: Double
    ) : PaymentDetails()
}

data class PaymentInfo (val paymentType: PaymentType, val paymentDetails: PaymentDetails)

fun main() {

    val paymentInfo = PaymentInfo(
        paymentType = PaymentType.PHONE_PAY,
        paymentDetails = PaymentDetails.PhonePe("1234567890", 100.0)
    )

    fun paymentUsage(paymentInfo: PaymentInfo): String {
        return when (paymentInfo.paymentType) {
            PaymentType.CREDIT_CARD -> "Credit card payment"
            PaymentType.PHONE_PAY -> "PhonePe payment"
            PaymentType.GOOGLE_PAY -> "GPay payment"
        }
    }
}
```

**고정 옵션용 열거형:**
정의된 변경 빈도가 낮은 값 집합(예: 지불 유형)인 경우 enums를 사용합니다.

**유연한 데이터용 Sealed Classes:**
각 값(예: 지불 세부 정보)에 다른 관련 데이터가 있을 때 sealed classes를 사용합니다.

**조합된 파워:**
enums와 sealed classes를 결합하여 복잡한 데이터 모델을 구조화하고 타입 안전하게 표현할 수 있습니다.

자세한 정보는 공식 사이트를 방문해보세요: [Kotlin 공식 문서 - Sealed Classes](https://kotlinlang.org/docs/sealed-classes.html#inheritance)

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

행운을 빕니다!!
