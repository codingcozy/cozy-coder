---
title: "파이썬 매직 메소드에 대해 알았으면 좋았을 9가지 혹독한 진실"
description: ""
coverImage: "/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_0.png"
date: 2024-08-18 10:38
ogImage:
  url: /assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_0.png
tag: Tech
originalTitle: "9 Things I Regret Not Knowing Earlier About Python Magic Methods"
link: "https://medium.com/@zlliu/9-things-i-regret-not-knowing-earlier-about-python-magic-methods-48903e679d8f"
isUpdated: false
---

![Image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_0.png)

# 1) Numerical Operators

By default, custom objects have no support for numerical operators like +, -, \*, etc.

![Image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_1.png)

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

그러나 이러한 동작을 해제하려면 특정한 매직 메서드를 정의할 수 있습니다:

- **add**은 객체 + 값을 할 때의 동작을 정의합니다.
- **sub**은 객체 - 값을 할 때의 동작을 정의합니다.
- **mul**은 객체 \* 값을 할 때의 동작을 정의합니다.
- **truediv**은 객체 / 값을 할 때의 동작을 정의합니다.
- **floordiv**은 객체 // 값을 할 때의 동작을 정의합니다.

![이미지](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_2.png)

이외에도 많은 숫자 매직 메서드가 있지만, 여기에는 가장 많이 사용되는 몇 가지가 있습니다.

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

실생활 예시 — 파일 경로를 다루는 Python의 pathlib에서는 /를 Path 객체와 함께 사용하여 경로를 결합할 수 있습니다. 이것은 /가 실제로는 나눗셈 연산자임에도 불구하고 사용할 수 있는 것입니다.

![Python Magic Methods](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_3.png)

이는 Path가 **truediv** 매직 메소드를 구현했기 때문에 가능합니다.

# 2) 반사적 숫자 연산자

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

대부분의 숫자 연산자에는 반전된 버전이 있습니다.

- **add**는 object + value에 대해 처리합니다.
- **radd** (반전된 **add**)는 value + object에 대해 처리합니다.
- **sub**는 object - value에 대해 처리합니다.
- **rsub** (반전된 **sub**)는 value - object에 대해 처리합니다.

![이미지](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_4.png)

# 3) 복합 할당 연산자

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

우리는 object + 5와 5 + object를 다루는 방법을 알고 있습니다. 그러나 object += 5는 어떨까요?

우리는 복합 할당 연산자를 위한 매직 메서드도 가지고 있습니다.

- **iadd**는 object += value를 다룹니다.
- **isub**는 object -= value를 다룹니다.
- **imul**는 object \*= value를 다룹니다.
- **itruediv**는 object /= value를 다룹니다.
- **ifloordiv**는 object //= value를 다룹니다.

![image]("https://사이트주소/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_5.png")

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

만약 우리의 사용 사례가 object = object + value 와 object += value를 구분해야 하는 경우 도움이 될 수 있습니다.

## 4) **init** VS **new**

**init**과 **new**은 클래스에서 객체를 초기화하기 위한 중요한 매직 메소드입니다. 그러나 둘은 같지 않습니다.

**new**는 우리의 객체를 생성합니다.

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

**init**은 새로 생성된 객체에 속성을 할당합니다.

다음은 우리가 양에 **new**와 **init**를 둘 다 오버라이드 하는 간단한 예제입니다:

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_6.png)

**new**와 **init** 사이의 주요 차이점:

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

- **new** 메소드는 **init** 메소드보다 먼저 실행됩니다.
- **new** 메소드는 인스턴스를 반환합니다. 반면에 **init** 메소드는 None을 반환해야 합니다.
- **new** 메소드는 인스턴스를 생성하고 반환합니다. **init** 메소드는 새로 생성된 인스턴스에 속성(예: 이름, 나이)을 할당하는 역할을 합니다.
- **new** 메소드는 클래스 메소드이고, **init** 메소드는 인스턴스 메소드입니다.

각 메소드를 언제 사용해야 하는지:

- 객체의 생성을 제어하고 싶을 때는 **new**를 사용합니다. 중요한 사용 사례 중 하나는 싱글톤을 생성할 때입니다. 클래스가 하나의 인스턴스만 가질 수 있는 경우입니다.
- 객체를 초기화하고 속성을 할당해야 할 때는 **init**를 사용합니다. 일반적으로 이것을 **new**보다 더 자주 사용합니다.

# 5) **getattr** 대 **getattribute**

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

두 가지 메소드는 object.attribute를 사용하여 객체의 속성에 액세스하는 방법과 관련이 있습니다. 하지만 약간의 차이가 있습니다.

**getattr**은 객체에 속성이 존재하지 않을 때에만 호출됩니다.

![](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_7.png)

**getattribute**는 속성이 존재 여부와 관계 없이 호출됩니다.

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

<img src="/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_8.png" />

# 잠깐 멈춰주세요

최근에 책을 썼어요 — 파이썬에 대해 몰랐던 101가지

작가로서 저를 지원하고 싶다면 여기를 확인해보세요!

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

링크: https://payhip.com/b/vywcf

# 6)**getattribute** VS **setattr** VS **delattr**

다음은 간단한 빈 클래스입니다

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_9.png)

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

- 먼저 dog.name을 문자열 값 'rocky'으로 설정합니다.
- 그런 다음 dog.name 속성을 읽습니다.
- 다음으로, del 키워드를 사용하여 dog.name을 삭제합니다. 이후에는 dog에는 더 이상 name 속성이 없습니다.

특정 매직 메서드를 사용하여 이러한 속성 설정, 속성 가져오기 및 속성 삭제 작업을 제어할 수 있습니다.

- **getattribute** 메서드는 속성에 액세스하는 방법을 정의합니다.
- **setattr** 메서드는 속성 값을 설정하는 방법을 정의합니다.
- **delattr** 메서드는 del 키워드를 사용하여 속성을 삭제하는 방법을 정의합니다.

![이미지](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_10.png)

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

모든 3개의 사용자 정의 정의에서 super()를 사용합니다. 모든 경우에 super()를 사용합니다. super()는 상위 클래스를 반환하며, 우리는 상위 클래스의 메서드를 사용할 수 있게 해줍니다.
이것은 이러한 메서드를 재정의할 때 중요합니다. 그렇지 않으면 최대 재귀 깊이가 초과된 오류가 발생합니다.

# 7) **str** 대 **repr**

**str**은 str(object)를 호출할 때 반환되는 문자열을 제어합니다.

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

**repr**은 repr(object)를 호출할 때 반환되는 문자열을 제어합니다.

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_11.png)

처음 보면 **str**과 **repr**이 거의 똑같아 보이지만 다른 방식으로 호출된다는 것 같습니다 - str()로 하나는 repr()로 하나는.

이는 틀리지 않습니다. 사실 **str**과 **repr**은 정확히 그런 것입니다. 두 가지 간의 차이는 각각의 사용 사례에 있습니다.

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

- **str**은 최종 사용자가 쉽게 이해하고 읽을 수 있도록 의도된 것입니다.
- **repr**은 주로 디버깅 및 개발에 사용됩니다.

실제 예시 — datetime 객체

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_12.png)

**str**과 **repr**의 차이를 주목해보세요 (두 경우 모두 문자열이지만)

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

- `__str__`은 간단한 사람이 읽기 쉬운 문자열을 반환합니다.
- `__repr__`은 디버깅을 위해 정확히 동일한 datetime 개체를 재생성하는 데 사용할 수 있는 문자열을 반환합니다.

## 8) `__call__`

기본적으로, 사용자 지정 객체는 함수처럼 호출할 수 없습니다.

하지만, `__call__` 메서드를 정의하면 우리 객체를 함수처럼 호출할 수 있습니다.

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

![image 1](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_13.png)

When we define `__call__`, we can call our dog object just like a function. This way, we can also access the dog's object attributes.

One practical application of this is using classes as decorators.

![image 2](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_14.png)

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

# 9) **invert**

우리는 객체 앞에 ~를 추가할 때의 동작을 정의하기 위해 **invert** 매직 메소드를 사용할 수 있습니다.

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_15.png)

실제 예시 - Pandas 시리즈는 **invert** 매직 메소드를 사용하여 ~를 사용하여 부울 시리즈를 뒤집을 수 있습니다. True가 False로, False가 True로 변환됩니다.

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

![image](/assets/img/2024-08-18-9ThingsIRegretNotKnowingEarlierAboutPythonMagicMethods_16.png)

# Substack 더 보기

Python 매직 메서드에 대해 이전에 알았더라면 후회했을 17가지 사항:

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

이해하기 쉽고 명확했길 바라요

# 만약 나를 창작자로 지원하길 원하신다면

- https://zlliu.substack.com/에서 내 Substack 뉴스레터에 가입해주세요. 저는 주간으로 파이썬과 관련된 이메일을 보냅니다.
- 제 책인 '파이썬에 대해 절대 알지 못했던 101가지'를 구매해주세요. https://payhip.com/b/vywcf
- 50번 박수로 이 스토리에 찬사를 보내주세요.
- 저에게 생각을 말씀해주는 댓글을 남겨주세요.
- 이야기 중 가장 좋아하는 부분을 강조해주세요.

감사합니다! 이 작은 행동들이 큰 도움이 되고, 진심으로 감사드립니다!

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

YouTube: [https://www.youtube.com/@zlliu246](https://www.youtube.com/@zlliu246)

LinkedIn: [https://www.linkedin.com/in/zlliu/](https://www.linkedin.com/in/zlliu/)
