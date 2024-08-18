---
title: "Swift 고성능 개발 딕셔너리 키 사용법"
description: ""
coverImage: "/assets/img/2024-07-14-SwiftHighPerformanceDictionaryKeys_0.png"
date: 2024-07-14 00:09
ogImage:
  url: /assets/img/2024-07-14-SwiftHighPerformanceDictionaryKeys_0.png
tag: Tech
originalTitle: "Swift High Performance: Dictionary Keys"
link: "https://medium.com/the-swift-cooperative/high-peformance-dictionary-keys-370dd0b525ac"
isUpdated: true
---

![Swift High Performance Dictionary Keys](/assets/img/2024-07-14-SwiftHighPerformanceDictionaryKeys_0.png)

이 기사는 모든 상황에서 빠르고 효율적인 딕셔너리 키를 만드는 방법을 깊게 탐구하며, 왜 문자열 기반 키가 그 관점에서 종종 실패하는지를 다룹니다.

제 오픈 소스 의존성 주입 라이브러리인 Factory에서 가져온 실제 예제와 성능 데이터가 포함되어 있습니다.

준비되셨나요? 함께 알아보도록 하겠습니다.

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

# 캐싱

어떤 응용 프로그램에서 공통적으로 사용되는 방법은 값비싼 항목을 생성한 다음 그 항목을 키 기반의 사전에 캐시하고, 나중에 해당 항목이 존재하는지 확인하여 있을 경우 캐시된 값을 반환하는 것입니다.

그 중요한 부분은 아래와 같이 구현될 수 있습니다:

```js
class ItemRepository {
    private var cache = [String:Item]()
    func item(forKey key: String) -> Item? {
        if let item = cache[key] {
            return item
        }
        let item = Item(key: key)
        cache[key] = item
        return item
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

우리 모두가 이것과 비슷한 변형을 본 적이나 해 본 적이 있습니다.

하지만 여기서 우리가 가지고 있는 근본적인 문제는 Swift String을 우리의 사전 키로 사용하는 것이 최적의 선택이 아니라는 것입니다. 사실 그렇지 않습니다.

**Strings**

Swift String은 값 타입이며, 그것은 좋은 것입니다! 우리에게 그렇게 말해 왔으니까요.

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

In reality, a Swift String has value semantics. It behaves like a value type, but behind the scenes, it’s actually a reference type, with an associated string buffer allocated on the heap.

This means that when you create a new String, a heap allocation occurs. When you pass a String to another function or hand it off to another object to be stored, its reference count must be incremented and decremented as needed.

And it also means that it needs to be deallocated when it goes out of scope.

But it gets worse.

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

# 유니코드

스위프트 문자열은 유니코드 기반으로, 문자열은 확장된 그래폼 클러스터 또는 문자들의 컬렉션입니다 (내부 저장 메커니즘은 일반적으로 16비트 정수 배열입니다).

우리에게 주는 문제는 유니코드 문자 표현이 주어진 문자, 예를 들어 “é”과 같이 배열 내에서 단일 정수 값으로 매핑될 수도 있고 매핑되지 않을 수도 있다는 것입니다.

스위프트 문서는 문자열의 동일성을 비교하는 것은 항상 유니코드 표준 표현을 사용한다고 말합니다. 고려해보세요.

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

Unicode 스칼라 값 "\u301"은 앞의 문자에 강세를 포함하도록 수정합니다. 따라서 "e\u301"은 단일 Unicode 스칼라 값 "é"와 동일한 정규 형식을 갖습니다.

두 문자열은 "문자 수"나 문자 수는 같지만 실제로 다른 버퍼 길이를 가지고 있습니다.

간단히 말해서, 문자열 동등성 확인은 간단한 바이트 대 바이트 비교가 아닙니다.

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

안녕하세요! 오늘은 해싱과 관련된 문제에 대해 알아보겠습니다. cafe1과 cafe2는 동일한 문자열 표현을 가지고 있기 때문에 두 값이 동일한 값으로 해싱됩니다. 문자열을 해싱할 때는 "문자" 단위로 진행되기 때문에 이런 문제가 발생합니다.

그렇다면 왜 이것이 중요한지 궁금할 수 있습니다. 이어서 딕셔너리 조회에 대해 알아보겠습니다.

# 딕셔너리 조회

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

Tarot 이미지가 있는 모든 것은 사전이 어떻게 내부적으로 작동하는지에 달려 있습니다. 이것은 우리가 조회 속도를 향상시키고 싶다면 이해해야 하는 매우 중요한 사항입니다.

일반적으로 사전은 해시 테이블로 구현되어 있어 키 조회가 빠릅니다 (일반적으로 O(1) 시간).

테이블에서 항목을 찾으려면 먼저 키를 해시해야 합니다. 대부분의 구현은 해시 값의 함수를 사용하여 해시 테이블에 색인화합니다. 그 위치에 테이블이 비어 있으면 아무것도 저장되지 않았다는 것을 알려주고 해당 결과를 보고합니다.

그러나 해당 위치의 테이블에 항목이 있는 경우 그곳에 저장된 키와 검색 키를 비교하여 일치하는지 확인해야 합니다.

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

친구야, 기억해야 할 건, 동일한 키는 동일한 값으로 해싱되어야 하지만 다른 키가 동일한 값으로 해싱될 수도 있다는 거야.

그래서 우리는 키를 비교해서 일치하는지 확인해야 해. 만약 일치한다면, 우리는 값을 찾은 거니까 당첨이야! 값을 반환할 수 있어.

하지만, 일치하지 않는다면, 해시 충돌이 발생했을 수 있고, "버킷" 목록에서 저장된 값들 중에 일치하는 값이 있는지 확인해야 해.

어쨌든, 더 많은 키 비교가 필요하겠지.

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

# Lessons

So, what have we learned and how does that help us? We've discovered that Swift Strings not only present challenges with heap allocation and reference counting but also have complexity issues related to hashing and comparing Unicode characters and strings.

It's important to keep in mind that we aim for dictionary lookups in O(1) time. However, hashing and comparing strings typically occur in O(N) time, where N represents the length of the string.

In simpler terms, Swift Strings aren't ideal choices as dictionary keys.

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

그렇다면, 최상의 해시 키는 무엇이 될까요? 여기에는 정수가 떠오르네요. 이는 참조 부담이 없는 단일 값(단어)이며, 쉽고 빠르게 복사, 해싱 및 비교할 수 있습니다.

하지만 해시 키 충돌을 최소화하기 위해서는 가능한 한 고유해야 합니다. 왜냐하면 해시 키 충돌은 우리의 성능에 상당한 영향을 줄 수 있기 때문이죠.

# Factory

여기에 Factory가 등장합니다. 이는 저의 GitHub에서 제공되는 컨테이너 기반 의존성 주입 라이브러리입니다.

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

팩토리는 각 팩토리 정의에서 계산된 키를 사용하여 팩토리 등록의 변경을 확인하고 사용자가 설정한 옵션을 확인하며 범위 지정/캐시된 인스턴스를 확인합니다.

좋든 나쁘든, 팩토리는 원래 다음과 같이 보이는 문자열 기반 키를 사용했습니다.

그 키에는 서비스의 이름과 유형이 포함되어 있습니다. 둘 다 팩토리 정의를 서로 구분하는 데 필요합니다.

다음과 같은 팩토리 정의를 고려해보세요.

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

```swift
extension Container {
    var myService: Factory<MyServiceType> {
        Factory(self) { MyService() }.cached
    }
    var anotherService: Factory<MyServiceType> {
        Factory(self) { MyService() }.cached
    }
    var simpleService: Factory<SimpleService> {
        Factory(self) { SimpleService() }
    }
}
```

미서비스(myService)와 애나더서비스(anotherService) 모두 같은 프로토콜 유형을 기반으로합니다. 따라서 유형을 기반으로 키를 만들면 작동하지 않습니다. 이 서비스에 대해 중복 키가 있고 이는 캐시된 값 및 기타 설정과 충돌을 발생시킵니다.

아니요, 키는 고유한 값이어야합니다. 그러므로 다른 값이 필요합니다. 함수 이름과 같은 다른 값을 사용하여 두 서비스를 구분해야합니다. 그러나 이 값을 어떻게 가져와야 할까요?

쉽습니다. 겉보기에 기본 팩토리의 공개 이니셜 라이저가 여기에 있습니다.

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

안녕하세요! 이 코드는 제가 보유한 Tarot 구조체를 설명하고 있습니다.

Tarot에서 중요한 부분은 `key` 매개변수인데요, 이 값은 기본적으로 Swift의 #function 값을 사용합니다.

#function 값은 해당 변수나 함수를 감싸고 있는 부모의 이름을 반환합니다. 예를 들어, `myService`와 같은 이름이겠죠? 이 값을 generic type T와 함께 사용하여 위에서 보여진 key를 생성합니다.

```swift
self.key = "\(key)<\(String(reflecting: T.self))>"
```

저 같은 Tarot 전문가에게 있어서 여러분의 코드는 정말 흥미로워요! 계속하여 즐겁게 코딩해보세요! 이 코드가 여러분에게 행운을 가져다 줄 거라 믿어요. 😉🔮✨

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

아무튼, 이 기능은 작동하지만 문자열 보간과 매번 타입 이름을 String 값으로 변환하는 비용이 비싼 String(reflecting:) 연산을 고려하면 속도가 느립니다.

그래서 이제 우리가 가지고 있는 모든 것과 알게 된 모든 것을 고려할 때, 어떻게 이를 개선할 수 있을까요?

## 개선

우리의 예제는 타입 정보와 키 정보를 모두 가진 단일 값 타입이 필요하며, 최상의 성능을 얻기 위해서 두 가지 모두 자체 값 타입이어야 합니다.

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

Well, if we want to combine value types into a single value type we can use as a key, then we're going to need to combine them together in a struct. This means we need something that looks like…

```swift
public struct FactoryKey: Hashable {
    public let type: ???
    public let key: ???
}
```

Note the Hashable conformance, which is required for any value that aspires to be a dictionary key.

### Type Information

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

간단한 값에 유형 정보를 저장하는 것은 상대적으로 간단한 작업인 것으로 나타납니다.

```swift
public init(type: Any.Type, key: StaticString) {
    self.type = ObjectIdentifier(type)
    self.key = ???
}
```

여기서는 Swift의 `ObjectIdentifier`를 사용하여 전달된 유형에 대한 고유 식별자를 얻습니다. 코드를 살펴보면, `ObjectIdentifier`는 \_value: Builtin.RawPointer라는 단일 값이 있는 구조체입니다.

유형 포인터가 처리 포인터나 참조 유형이 아닌 원시 포인터인 경우, 값이 애플리케이션의 수명 동안 유효할 것으로 추론할 수 있습니다(그리고 설명서도 명시합니다).

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

위에서 더 좋은 점은 ObjectIdentifier도 Hashable 및 Equatable을 따르며, 각각에 대한 간단한 구현이 있다.

# 주요 정보

위에서 노트를 보면 초기화 프로그램에서 이전에 얻은 StaticString에 직접 키를 전달하고 있다는 것을 알 수 있습니다.

이제 당신이 무엇을 생각하고 있는지 알겠습니다. 우리는 문자열이 나쁘다고 말했는데, 왜 StaticString을 다루고 있을까요?

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

하지만 이곳에서 뚜렷한 차이가 있습니다. StaticString은 Strings가 아닙니다. 사실, 다음 내부 구조를 갖는 순수 값 타입입니다.

```swift
public struct StaticString: Sendable {
    var _startPtrOrData: Builtin.Word
    var _utf8CodeUnitCount: Builtin.Word
    var _flags: Builtin.Int8
    ...
}
```

그래서 우리의 주요 정의는 다음과 같이 보입니다.

```swift
public struct FactoryKey: Hashable {
    let type: ObjectIdentifier
    let key: StaticString
    public init(type: Any.Type, key: StaticString) {
        self.type = ObjectIdentifier(type)
        self.key = key
    }
    ...
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

하지만 문제가 있어요. StaticString은 Sendable로 표시되어 있지만 Hashable이나 Equatable로 표시되어 있지 않아요. 그럼 우리는 어떻게 해야 할까요?

물론 우리가 직접 준수를 추가하면 되죠.

## StaticString의 해싱과 비교

내 원래 목표는 단순히 utf8Start를 추출하고 저장하며 비교하는 것이었는데, 몇몇 사람들과 Swift 포럼에서 여러 번 논의한 후에 그 아이디어에 반대하기로 결정했어요.

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

먼저, 원래 정의에서 \_startPtrOrData를 주목해야 합니다. 그 값은 문자열을 가리킬 수도 있고, 단일 유니코드 스칼라 값일 수도 있습니다. 스칼라일 때 문자열 포인터를 가져오려고 시도하면 충돌이 발생할 수 있습니다.

이제, 나는 그것에 대해 너무 걱정하지 않았습니다. 왜냐하면 스칼라 값을 갖는 StaticString을 만들려면 많은 노력을 기울여야 하고, #function 메서드가 그렇게 할 가능성은 낮을 것이라고 생각했기 때문이었습니다. 하지만 그런 일이 일어날 수도 있습니다.

둘째, 그리고 좀 더 중요한 점은, 항상 동일한 문자열을 가리키는 포인터를 성상 보장하지 못했습니다. 한 번 포인터가 확보되면 목적지는 항상 바뀌지 않기 때문에 상황은 괜찮지만, 구현 또는 문서 모두 여러 호출이 항상 동일한 포인터를 반환할 것을 보장하지 않았습니다.

요컨대 말하자면, Hashable 및 Equatable에 대해 다음과 같은 구현이 만들어졌습니다.

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

## 해시 가능

이게 해시 가능이에요. 간결하고 귀엽죠.

```swift
public struct FactoryKey: Hashable {
    ...
    @inlinable public func hash(into hasher: inout Hasher) {
        hasher.combine(self.type)
    }
    ...
}
```

제 해시 함수는 키를 해시하지 않고 타입만 해싱해요. 무수한 테스트와 성능 테스트를 거쳐서(나중에 자세히 설명할게요), 키를 해싱하지 않는 것이 더 좋은 성능을 내는 것으로 결정했어요.

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

이미 언급한 바와 같이, 사전 조회는 해시(hash)를 사용한 뒤 동등성(equality)을 확인하며, 비교 중에는 두 문자열을 비교하면서 이동할 것입니다.

또한, 앞서의 예제와는 다르게 대부분의 팩토리(Factory) 정의는 각기 다른 유형을 정의하며, 이는 차례로 다른 해시 키를 생성할 것입니다.

동일한 값을 항상 동일한 해시 키로 생성해야 하는 Hashable 요구 사항과 일치함을 기억하세요.

## Equatable

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

```swift
public struct FactoryKey: Hashable {
    ...
    @inlinable public static func == (lhs: Self, rhs: Self) -> Bool {
        guard lhs.type == rhs.type && lhs.key.hasPointerRepresentation == rhs.key.hasPointerRepresentation else {
            return false
        }
        if lhs.key.hasPointerRepresentation {
            return lhs.key.utf8Start == rhs.key.utf8Start || strcmp(lhs.key.utf8Start, rhs.key.utf8Start) == 0
        } else {
            return lhs.key.unicodeScalar.value == rhs.key.unicodeScalar.value
        }
    }
}
```

Equal conformance는 약간 복잡하지만 여전히 비교적 간단합니다. 우리는 먼저 두 유형을 비교합니다. 왜냐하면 그것이 가장 빠른 확인이기 때문입니다.

그런 다음 두 키가 동일한 표현을 가지고 있는지 확인합니다. 그렇지 않으면 일치하지 않으므로 종료합니다.

마지막으로 두 문자열 또는 스칼라 값을 비교합니다. 일치하면 키가 일치합니다.

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

간단한 최적화 팁을 하나 드리겠습니다. lhs.key.utf8Start == rhs.key.utf8Start 에서 이미지 태그를 마크다운 형식으로 변경하면 더 효율적입니다.

## 비최적화된 최적화 방법

저는 키 문자열을 해시 값에 포함시키면 사용 사례에 살짝 나쁜 성능을 보인다고 언급했었는데, 여기 구현 방법이 궁금하신 경우를 위해 코드를 첨부합니다.

```js
public func hash(into hasher: inout Hasher) {
    hasher.combine(self.type)
    if key.hasPointerRepresentation {
        hasher.combine(bytes: UnsafeRawBufferPointer(start: key.utf8Start, count: key.utf8CodeUnitCount))
    } else {
        hasher.combine(key.unicodeScalar.value)
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

I've also experimented with precomputing and storing the hash value in a different version, but it unfortunately didn't improve performance as much as I had hoped.

In the end, I attempted another version using precomputed enumerated types, but this approach still ended up being just a tad slower than the most efficient implementation.

```swift
public enum KeyType {
    case string(UnsafePointer<UInt8>)
    case scalar(UInt32)
}
public let type: ObjectIdentifier
public let key: KeyType
```

## Implementation

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

이제 최종, 완벽한 구현은 이렇게 됩니다.

```swift
public struct FactoryKey: Hashable {
    @usableFromInline let type: ObjectIdentifier
    @usableFromInline let key: StaticString
    @inlinable public init(type: Any.Type, key: StaticString) {
        self.type = ObjectIdentifier(type)
        self.key = key
    }
    @inlinable public func hash(into hasher: inout Hasher) {
        hasher.combine(self.type)
    }
    @inlinable public static func == (lhs: Self, rhs: Self) -> Bool {
        guard lhs.type == rhs.type && lhs.key.hasPointerRepresentation == rhs.key.hasPointerRepresentation else {
            return false
        }
        if lhs.key.hasPointerRepresentation {
            return lhs.key.utf8Start == rhs.key.utf8Start || strcmp(lhs.key.utf8Start, rhs.key.utf8Start) == 0
        } else {
            return lhs.key.unicodeScalar.value == rhs.key.unicodeScalar.value
        }
    }
}
```

우리는 최적화된 해시 및 비교 함수를 사용하여 저장 공간이 약 네 개의 단어인 순수한 값 타입을 얻었습니다.

성능 향상을 위해 모든 함수에는 @inlineable로 표시되어 있습니다.

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

하지만 정확히 얼마나 빠를까요?

## 성능

다양한 딕셔너리를 채우고 확인하기 위해 많은 테스트 코드를 작성했고, 각각 백만 회 이상의 반복을 수행하여 의미 있는 성능 숫자를 얻을 수 있도록 수십 차례의 평균을 계산했습니다.

다음은 결과입니다.

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

FactoryKey9 = 0.5267935395240784s - scalar 테스트와 포인터 표현가드를 가진 staticstring

FactoryKey6 = 0.5381311357021332s - scalar 테스트를 가진 staticstring
FactoryKey5 = 0.559116518497467s - enum keytype

FactoryKey2 = 0.6407256484031677s - 해시 키 문자열을 가진 staticstring과 scalar 테스트
FactoryKey3 = 0.7152477920055389s - 타입과 키에 대한 해시값 사전 계산을 가진 staticstring과 scalar 테스트

StringKey = 2.964924430847168s - 문자열

FactoryKey9은 우리가 논의해 온 버전입니다. 버전 5와 6은 백만 개의 키 확인 및 해결 사이에 겨우 소수점 몇 초만 차이가 납니다.

해시에 키 문자열을 포함하면 0.1초 정도의 차이가 나고, 해시값을 사전 계산하고 저장하려고 시도하면 그 두 배 이상 차이가 납니다.

그러나 모든 이러한 메커니즘은 원래 문자열 기반 접근 방식을 사용하는 것보다 4배에서 5배 빠릅니다.

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

# 완료 블록

그럼 이만 마치도록 하죠. 사전 성능에 신경을 쓸 거라면 가능하다면 사전과 세트 키에 값 유형을 사용하세요.

그리고 문자열 데이터로 작업하고 있다면 해당 데이터를 값 유형(열거형?)으로 쉽게 변환할 방법이 있는지 확인하세요.

이 변경에 대한 많은 동기부여는 WWDC 2016의 'Understanding Swift Performance' 비디오를 다시 보고 난 후 나왔습니다.

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

이미 몇 년이 지났지만, Swift가 값 타입, 참조 타입, 존재론 및 메소드 호출 및 호출 방식을 관리하는 방식에 대해 설명하고 그 배후를 엿볼 수 있는 훌륭한 일을 합니다.

Factory에 대한 주요 변경 코드는 새롭고, 본 글 작성 시 Repository의 develop 브랜치에서 사용할 수 있습니다. 자유롭게 확인해보세요.

언제나 같이 해요! 좋아요 버튼을 누르고, 아래 질문이나 댓글을 남겨주세요.

정말로요. Medium은 프로모션과 지급 알고리즘을 변경했기 때문에, 좋아요 버튼을 누르고 댓글을 달면 엄청난 차이를 만들어냅니다.

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

다음에 뵙겠습니다.

참고: 이 글은 '빠른 종속성 주입 시리즈(The Swift Dependency Injection Series)'의 일부입니다.
