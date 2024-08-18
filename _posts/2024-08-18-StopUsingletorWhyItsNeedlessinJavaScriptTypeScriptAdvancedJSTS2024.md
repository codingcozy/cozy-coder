---
title: "JavaScript와 TypeScript에서 let 사용을 중지해야 하는 이유  JavaScript TypeScript 고급 2024"
description: ""
coverImage: "/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_0.png"
date: 2024-08-18 10:59
ogImage:
  url: /assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_0.png
tag: Tech
originalTitle: "Stop Using let or Why Its Needless in JavaScript TypeScript  Advanced JS TS 2024"
link: "https://medium.com/javascript-in-plain-english/stop-using-let-or-why-its-needless-in-javascript-typescript-advanced-js-ts-2024-fe3be70287d2"
isUpdated: false
---

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_0.png)

이미 let 키워드를 수백만 번 정도 만났을 가능성이 높습니다. 이것은 처음에는 은총 같은 것처럼 보이는 JavaScript (그리고 확장되어 TypeScript까지)의 기능 중 하나입니다.

들고 있는 블록 범위 변수를 선언하는 방법이 마침내 있습니다! var이 호이스팅으로 혼란을 야기하고 직관적이지 않은 버그를 만드는 시대는 끝났습니다.

하지만 여기에 주목할 점이 있습니다: 2024년에는 코드에서 let을 사용하는 것이 가장 불필요한 일 중 하나일 수 있습니다.

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

그렇다고요. 이제 let을 사용하는 것을 그만두는 시간입니다.

const로 바꾸어야 하는 경우와 가끔씩 let이 TypeScript 파일에서 여전히 자리를 차지해야 하는 이유에 대해 안내해 드릴게요.

# let의 부흥과 몰락 🐑

우선 과거로 돌아가봅시다. ES6 이전에는 var을 사용했었죠. 기능적이었지만 심각하게 결함이 있었습니다. var은 블록 스코프를 존중하지 않았는데요, for 루프 내에서 var를 선언해도 해당 변수가 루프 외부에서 계속 접근 가능했습니다.

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

혼란을 불러일으키고 버그 코드를 유발하는 것은 바로 요것. ES6(혹은 ECMAScript 2015)은 let과 const를 도입함으로써 게임을 바꿨어. 갑자기 우리에게는 블록 단위 변수가 생겼지!

더 이상 var 악몽은 없어! let은 문제를 해결해 줘서 순식간에 인기를 끌었어. 몇 년간 씨름해 온 문제들을 해결해 주니까 말이야.

그런데 여기서 한 가지, const가 동시에 소개되었는데, 대부분의 상황에서 우월한 옵션인 것으로 평가되고 있어.

# 왜 const가 기본값이 되어야 하는 이유 🐠

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

자, 함께 고려해 봅시다: 변경 불가능성은 최고입니다. 우리의 대부분의 일상 프로그래밍에서는 예측 가능하고 이해하기 쉽며 부작용이 없는 코드를 작성하기 위해 노력합니다.

이것이 const의 역할입니다. const를 사용하면 이 값이 변경되지 않을 것이라고 자신과 다른 사람들에게 말하는 것입니다.

그것은 보장입니다 - 변수가 코드의 중간에서 새 값으로 갑자기 바뀌지 않을 것이라는 약속입니다.

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_1.png)

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

이렇게 하면 코드를 읽고 유지하는 것이 더 쉬워집니다. const로 선언된 변수를 한눈에 보면 즉시 그 목적을 파악할 수 있습니다: 이 값은 고정되어 있다는 것을 알 수 있어요.

반면에 let은 불확실성을 도입합니다. let으로 선언된 변수를 볼 때 나중에 값이 변경될지 확신할 수 없습니다. 그 변수를 정신적으로나 도구를 통해 추적해야 하는데, 이는 인지 부담을 더하게 됩니다.

내 경험상 코드를 검토할 때, let으로 선언된 변수를 const로 쉽게 변경할 수 있는 경우가 자주 있어요. 개발자들이 변수가 나중에 변경되지 않을 경우에도 습관적으로 let을 기본으로 선택하는 듯합니다.

# let 사용의 문제점 🐲

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

let 키워드의 주요 문제는 불필요한 가변성을 초래한다는 것입니다. let을 사용하여 무언가를 선언하는 것은 너무 쉽지만 후에 그것이 처음에 왜 가변이어야 하는지 잊기 쉽습니다.

다음은 간단한 예시입니다:

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_2.png)

이 예시에서 let을 사용하는 것은 해 Harmless 해 보일 수 있지만, 자신에게 물어보세요 - userCount가 정말 가변이어야 했나요?

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

당신이 쓴 코드를 const로 바꾸고 로직을 다시 구성했으면 코드가 더 명확하고 기능적일까요?

| <img src="/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_3.png" /> |
| :-----------------------------------------------------------------------------------------------------------: |

와우! 더 깨끗한 코드, 덜 가변성이 있고, 쉽게 따라가기가 쉬워졌어요.

# 언제 let을 사용해야 할까요? 🎒

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

그래서 let을 완전히 버릴까요? 그렇지 않아요. let에 대한 합리적인 사용 사례가 있습니다. 하지만 생각하는 것보다 그 수가 더 적고 희소합니다.

- 반복 횟수: 무언가를 반복해야 할 때 let이 종종 필요합니다.

for 루프가 좋은 예입니다:

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_4.png)

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

- 여기서 i는 각 반복마다 변경되어야 하므로 let을 사용하는 것이 옳은 선택입니다.
- 변경 가능한 변수: 값이 실제로 변경되어야 하는 변수가 있는 경우에는 const 대신 let을 사용하는 것이 적절합니다.

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_5.png)

이 경우, 재할당은 로직에 중요합니다.

하지만 이러한 시나리오는 생각보다 덜 흔합니다. 종종 let이 필요하다고 느껴지는 것은 const로 재구성되어 명확성이나 기능을 잃지 않을 수 있습니다.

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

# 더 나은 코드를 위한 도구와 팁들 🎥

코드를 더 향상시키고 싶으신가요? let 사용을 최소화하고 const의 힘을 극대화하기 위한 도구와 팁 몇 가지를 소개합니다:

- 린터: ESLint가 당신의 친구입니다. ESLint를 설정하여 불필요한 let 사용 시 경고 또는 오류를 표시하도록 설정할 수 있습니다. 이렇게 하면 let을 사용하기 전에 두 번 생각하게 됩니다.
- 리팩터링 도구: Prettier나 VSCode의 내장 리팩터링 도구와 같은 도구를 사용하면 빠르게 let을 const로 변환할 수 있습니다. 그냥 우클릭하고 마법이 일어나는 걸 보세요.
- 동료 코드 리뷰: 팀원들에게 코드 리뷰 과정에서 let 사용에 대해 질문하도록 권유해보세요. "이것이 정말 가변이어야 할까요?"라고 물어보는 것은 팀 내에서 불변성에 대한 사고 방식을 심어줄 수 있습니다.

# let 선언 완화의 고급 수준 💖

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

혼란을 해결하는 다른 방법을 찾고 있다면, 선언 부분이 당신을 구조해 주는 구명조끼가 될 수도 있어요.

이것을 읽기 어렵게 만드는 이유는 초기 값을 머릿속에 유지하고 읽는 동안 발생하는 변이를 지켜봐야 한다는 점입니다.

![이미지](/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_6.png)

해결 방법: 함수를 호출하세요.

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

<img src="/assets/img/2024-08-18-StopUsingletorWhyItsNeedlessinJavaScriptTypeScriptAdvancedJSTS2024_7.png" />

let을 사용하지 말고 formattedAddress 변수를 변이시키는 대신, 형식을 처리하는 함수를 호출하여 일찍 반환할 수 있습니다. 이렇게 하면 가변성이 없어지고 코드가 더 깔끔해집니다.

# 결론: 불변성 수용하기

요약하자면, 현대의 JavaScript와 TypeScript에서는 let은 규칙보다는 예외여야 합니다. const를 기본으로 사용함으로써 코드를 더 예측 가능하고 읽기 쉽게 만들며 버그 발생 가능성을 줄일 수 있습니다. 물론 let이 필요한 경우도 있지만, 그런 경우는 신중하게 고려해야 합니다.

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

키보드 위에 손가락이 떠다니면서 'let'을 입력하려고 할 때, 한 순간 멈춰서 '이것이 정말 가변이어야 할까?' 물어보세요. 대부분의 경우 그 답은 아닐 겁니다.

2024년에는 적어도 불필요한 곳에서는 'let' 사용을 그만두는 해가 되었으면 좋겠네요.

독해해 주셔서 감사합니다! 이 글이 도움이 되었다면 동료들과 공유해 주시고, 댓글에서 계속 대화를 이어나가요.

즐거운 코딩하세요!

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

# 친근한 한국어 번역 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수로 격려하고 팔로우해주세요 ️👏️️
- 저희를 팔로우해주세요: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼에서도 만나보세요: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 즐겨보세요
