---
title: "안드로이드에서 UI 리소스를 위한 효과적인 추상화 레이어 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-BuildinganeffectiveabstractionlayerforUIresourcesonAndroid_0.png"
date: 2024-08-17 00:26
ogImage:
  url: /assets/img/2024-08-17-BuildinganeffectiveabstractionlayerforUIresourcesonAndroid_0.png
tag: Tech
originalTitle: "Building an effective abstraction layer for UI resources on Android"
link: "https://medium.com/@michellbak/building-an-effective-abstraction-layer-for-ui-resources-on-android-1806448caf50"
isUpdated: true
updatedAt: 1723863574662
---

![BuildinganeffectiveabstractionlayerforUIresourcesonAndroid](/assets/img/2024-08-17-BuildinganeffectiveabstractionlayerforUIresourcesonAndroid_0.png)

안녕하세요! 안드로이드 개발자 여러분들께서는 도메인 모델, 유스케이스, 상태 매퍼 또는 뷰모델에서 다양한 UI 리소스를 참조해야 하는 상황에서 자주 있을 것입니다.

예를 들어 R.string.description 또는 R.drawable.logo와 같은 리소스 식별자를 사용하는 것이 이러한 상황에서 매우 흔합니다. 문자열 리소스나 드로어블(이미지)을 참조해야 하는 경우 등입니다.

이 방법은 작동하지만, 몇 가지 단점이 있습니다:

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

- 타입 안정성이 없어요
- 단위 테스트가 어려워요
- 유연성이 제한되어 있어요
- 비 UI 코드에서 자원을 사용하려면 종종 Context 인스턴스가 필요해요

이러한 문제를 해결하려면 추상화 계층을 추가해야 해요.

기본 사항부터 시작해서 UiResource라는 sealed class를 소개해볼까요:

```js
@Immutable
sealed class UiResource<T> : Serializable {
    abstract fun resolve(context: Context): T
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

우리는 기본 클래스를 갖고 있으니 이제 리소스별 코드를 추가할 시간입니다.

이 게시물에서는 주로 텍스트 리소스에 초점을 맞출 것이지만, 색상 리소스, 드로어블, 글꼴, 크기, 부울값 — 심지어 필요하다면 원시 리소스와 애니메이션에도 추상화 레이어를 사용할 수 있습니다.

그렇다면, 시작해보기 위한 간단한 예제를 보여드리겠습니다:

```js
@Immutable
sealed class TextResource : UiResource<String>(), Parcelable, Serializable {

    @Immutable
    @Parcelize
    data class RawString(val text: String) : TextResource

    override fun resolve(context: Context): String {
        return when (this) {
            is RawString -> text
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

새로운 클래스로 TextResource 객체를 생성하고 도메인 코드에서 사용할 수 있게 되었습니다.

솔직히 말해서, 위 예제는 그다지 유용하지 않습니다. 그래서 R.string.example과 R.plurals.example에 대한 문자열 리소스 식별자 및 수량 식별자를 지원하기 위해 그것을 변경해 봅시다:

```js
@Immutable
@Parcelize
data class Resource(
    @StringRes val id: Int,
    val formatArgs: List<TextResource> = emptyList(),
) : TextResource() {

    // Single argument를 허용하기 위한 보조 생성자
    constructor(
        @StringRes id: Int,
        formatArgs: TextResource,
    ) : this(
        id = id,
        formatArgs = listOf(formatArgs)
    )
}

@Immutable
@Parcelize
data class Quantity(
    @PluralsRes val id: Int,
    val quantity: Int,
    val formatArgs: List<TextResource>,
) : TextResource() {

    // 가변 인수를 허용하기 위한 보조 생성자
    constructor(
        @PluralsRes id: Int,
        quantity: Int,
        vararg formatArgs: TextResource,
    ) : this(
        id = id,
        quantity = quantity,
        formatArgs = formatArgs.toList()
    )
}
```

그리고 물론, 이러한 새로운 클래스들을 resolve(context: Context) 함수에서 처리할 수 있어야 합니다:

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
override fun resolve(context: Context): String {
    return when (this) {
        is RawString -> text

        is Resource -> {
            return context.getString(
                id, *formatArgs.map { it.resolve(context) }.toTypedArray()
            )
        }

        is Quantity -> {
            context.resources.getQuantityString(
                id, quantity, *formatArgs.map { it.resolve(context) }.toTypedArray()
            )
        }
    }
}
```

With this change, TextResource suddenly becomes a very useful abstraction layer. We can use it everywhere in our domain layer code and we won’t ever expose implementation details to any receiver when using it as a return type.

Receivers don’t know and shouldn’t care what kind of text resource they’re dealing with — they just know it’s a text resource, and eventually, when passed to UI code, it can be resolved to a readable string.

As you can imagine, you can create many different TextResource subclasses.

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

아마도 텍스트 콘텐츠의 대문자로 변환하거나 모두 대문자 또는 소문자로 변경하는 변환 서브클래스 세트가 있으면 좋을 것 같습니다. 또는 숫자 인수를 사용하는 구현이 있으면 어떨까요?

또는 여러 TextResource 인스턴스를 결합하는 클래스가 있으면 좋을 것 같습니다. 문자열 연결에 대해 연산자 오버로딩과 함께 이러한 클래스를 사용할 수도 있을 겁니다. 다음과 같은 것을 상상해보세요:

```js
operator fun plus(other: TextResource): TextResource {
	return TextResource.Combined(separator = " ", resources = listOf(this, other))
}
```

종합적으로 산술 연산자는 차원 리소스에 대해 매우 유용할 것입니다. 가능성은 (거의) 무한합니다.

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

# UI 코드에서 리소스 해결하기

`TextResource`는 변경 불가능한 클래스로 표시되며 `Parcelable`와 `Serializable`를 모두 구현합니다. 다시 말해, 이를 도메인 코드에서 사용할 수 있지만 UI 상태 모델에도 포함하는 것이 이상적입니다.

UI 코드에서 `TextResource` 객체를 사용하려면 `resolve(context: Context)` 함수를 호출해야 합니다.

만약 UI 코드가 뷰 기반인 경우, 이 작업은 매우 쉽고 간단합니다. 그냥 액티비티/프래그먼트/뷰 컨텍스트를 사용하면 됩니다.

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

만약 Compose를 사용 중이라면 LocalContext를 사용해야 합니다. 하지만 매번 텍스트 리소스를 해결할 때마다 그것을 작성하는 것이 조금 귀찮을 수 있습니다. 대신에, 이와 같은 확장 함수를 사용할 수 있습니다:

```js
@Composable
fun TextResource.resolve(): String {
    return this.resolve(LocalContext.current)
}
```

이 확장 함수를 사용하면 어떤 Compose 컨텍스트에서든 resolve()를 호출하여 텍스트 리소스가 올바르게 해결될 것이라는 것을 알 수 있습니다. 이는 원시 문자열, 문자열 리소스, 수량 리소스 또는 다른 것일지라도, 올바르게 해결될 것입니다.

# 도우미 함수들은 더욱 좋습니다.

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

항상 TextResource.RawString, TextResource.Resource 또는 TextResource.Quantity를 사용할 때마다 길고 지루한 작업이 될 수 있어요.

이를 해결하기 위해 몇 가지 도우미 함수를 도입할 수 있어요:

```js
fun textResource(
    text: String
) = TextResource.RawString(text)

fun textResource(
    @StringRes id: Int,
    vararg formatArgs: TextRes
) = TextResource.Resource(id, formatArgs.toList())

fun textResource(
    @PluralsRes id: Int,
    quantity: Int,
    vararg formatArgs: TextRes
) = TextResource.Quantity(id, quantity, formatArgs.toList())
```

비슷하게, 확장 함수를 사용하여 추상화 계층의 사용성을 더 향상시킬 수도 있어요:

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
fun TextResource?.orEmpty() = this ?: textResource("")

fun TextResource.lowercase() = TextResource.Lowercase(this)
```

나중에 제안된대로 Lowercase 하위 클래스를 생성한다는 전제를 가지고 있는 후자의 확장 함수입니다.

# 보너스: 더 나은 및 유용한 예외 받는 방법

문자열 자원 형식 매개변수와 함께 작업했다면, MissingFormatArgumentException을 언젠가는 마주쳤을 것이라고 확신합니다.

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

해당 예외는 형식 지정자와 해당 인수가 없는 경우 또는 인수 인덱스가 존재하지 않는 인수를 참조하는 경우에 throw되는 런타임 예외입니다.

이런 예외들을 실제로는 절대로 보지 못할 테지만, 개발 중에 발생하는 경우가 종종 있습니다. 불행히도, 기본 예외 메시지는 상당히 모호하며 무엇이 잘못되었는지 또는 어디서 발생했는지에 대한 정보가 충분하지 않습니다.

이를 개선하려면 resolve(context: Context) 함수를 수정할 수 있습니다. 예를 들어:

```js
try {
    context.getString(
        id, *formatArgs.map { it.resolve(context) }.toTypedArray()
    )
} catch (missingArgException: MissingFormatArgumentException) {
    val argumentCount = context.countStringResourceArguments(id)
    val resourceId = context.getResourceKey(id) ?: id
    throw MissingFormatArgumentException("문자열 리소스에 대한 인수가 누락되었습니다. 예상 수: $argumentCount. 실제 수: ${formatArgs.size}. 문자열 ID: $resourceId. 문자열 내용: ${context.getString(id)}")
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

이 코드는 두 가지 확장 기능을 사용합니다:

```js
/**
 * 문자열 리소스가 요구하는 인자 수를 계산합니다.
 *
 * @param stringResId 문자열 리소스의 ID입니다.
 * @return 문자열 리소스가 요구하는 인자 수입니다.
 */
fun Context.countStringResourceArguments(@StringRes stringResId: Int): Int {
    return countStringResourceArgs(getString(stringResId))
}

/**
 * 문자열에서 간단하고 위치 지정자 플레이스홀더를 찾아 문자열 값 인자, 정수(10진수) 값을 인자, 부동 소수점 값 인자를 계산합니다.
 */
private fun countStringResourceArgs(string: String): Int {
    var count = 0
    val pattern = Regex("%(\\d+\\$)?[sdf]") // %s, %d, %f, %1$s, %2$d 등과 일치합니다.
    pattern.findAll(string).forEach { _ -> count++ }
    return count
}
```

```js
/**
 * 주어진 리소스 ID의 리소스 키를 반환합니다.
 * 예: R.string.app_name -> app_name
 */
fun Context.getResourceKey(@AnyRes resId: Int): String? {
    try {
        return resources.getResourceEntryName(resId)
    } catch (e: NotFoundException) {
        e.printStackTrace()
        return null
    }
}
```

이 작은 변경으로 원래 예외를 catch하고 유용한 디버그 정보가 포함된 수정된 버전을 throw하게 됩니다.

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

여기에 있습니다!

UiResource와 TextResource 추상화를 소개함으로써, 도메인 레이어 코드 내에서 UI 리소스를 처리하기 위한 유연하고 안전한 방법을 만들어냈습니다.

이 추상화를 활용함으로써, 도메인 로직을 깔끔하고 Android 프레임워크와 독립적으로 유지할 수 있습니다(Context 안녕), 이를 통해 더 나은 아키텍처와 더 쉬운 테스팅을 촉진할 수 있습니다.

코딩 즐거움을 빕니다!

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

PS. 저의 동료 프레드릭 젠센에게 큰 감사를 전합니다. 그는 이 추상화 레이어를 퍼블릭(Android 팀)에 소개해 주었어요.
