---
title: "쿠키 동의 아직도 무법천지"
description: ""
coverImage: "/assets/img/2024-08-13-CookieconsentistheWildWest_0.png"
date: 2024-08-13 11:11
ogImage: 
  url: /assets/img/2024-08-13-CookieconsentistheWildWest_0.png
tag: Tech
originalTitle: "Cookie consent is the Wild West"
link: "https://medium.com/dev-genius/cookie-consent-is-the-wild-west-06ff229c9900"
---


쿠키를 거부할 수 있는 권리가 있어요... 정말일까요?

![Cookie consent popup](/assets/img/2024-08-13-CookieconsentistheWildWest_0.png)

영국에 계시면 아마도 쿠키 동의 팝업을 수백만 번 본 적이 있을 거예요. 보통 페이지가 처음으로 로드될 때 나타나는 모달 창이에요. 종종 콘텐츠를 많이 가리며 상호작용할 수 있도록 만들어져요.

![Cookie consent popup](/assets/img/2024-08-13-CookieconsentistheWildWest_1.png)

<div class="content-ad"></div>

대부분의 경우, 큰 초록색 '수락' 버튼을 찾아서 전 문서에서 하던 일을 계속하면 될 거예요. 가끔 의심스러운 웹사이트에 들어가면(음, 나도 판단하지 않을게요), 누군가가 여러분을 추적하지 못하게 쿠키 수집을 거절할 수도 있어요.

중요한 점은 쿠키가 최종 사용자의 동의를 통해 작동해야 한다는 것입니다 — 이것이 개인정보보호 및 전자 통신 규정(PECR)에서 말하는 것입니다.

그러니까 사용자가 동의하면, 웹사이트가 설정하기를 원하는 쿠키를 받고, 거절하면 아무 것도 설정되지 않나요? 
사실 그렇지 않아요.

# 필수 쿠키

<div class="content-ad"></div>

그 전에 수상한 웹사이트에 들어가보고 쿠키를 거부한 적이 있었던 것 기억나요? 하지만 사용자 동의와 상관 없이 설정되는 특정 쿠키를 막을 수 없다는 사실에 놀랄지도 모르겠네요.

사용자 동의에 영향을 받지 않는 '필수 쿠키'라는 카테고리가 있어요.

쿠키 동의 팝업을 거부함으로써 필수 쿠키를 거부할 수 없어요. 이러한 쿠키를 비활성화할 수 있는 유일한 방법은 브라우저 설정에서 모든 쿠키를 비활성화하는 것뿐이에요.

필수 쿠키는 사용자 동의 동작과 상반됩니다. 쿠키를 거부해도 모든 쿠키가 거부되지 않는다는 사실을 처음 알았을 때, 조금 속은 듯한 느낌이 들었어요.
대부분의 웹사이트는 사용자를 당황시키거나 돌아서게 만들 수 있기 때문에 선택에 관계 없이 쿠키를 설정할 것이라는 사실을 언급하지 않을 거예요. '모든 쿠키 거부'라고 표시된 버튼이 실제로 모든 쿠키를 거부하지 않기 때문에 그 버튼이 잘못되었다고 말해봐도 좋을 것 같아요.
예상대로 정보 위원회(ICO)는 훨씬 명확한 동의 팝업을 가지고 있으며, '비필수 쿠키 거부'라는 구문을 사용합니다. 이 버튼을 클릭하면 실제로 그 버튼을 클릭했을 때 무슨 일이 벌어질지 정확히 설명한 것이에요.

<div class="content-ad"></div>

중요한 쿠키들은 사용자 상호작용/정보를 수집하거나 추적하지 않고 웹사이트가 올바르게 작동하기 위해 필요한 경우에만 중요한 쿠키로 분류될 수 있으므로 그것에 대해 걱정할 필요가 없을 것 같아요. 필요한 쿠키의 한 예로는 언어 환경 선택이나, 역설적으로 쿠키 팝업에 동의했는지 거부했는지 여부가 있을 수 있어요.

내 의견으로는 중요한 쿠키의 정의는 다소 해석의 여지가 있고 추가 쿠키가 설정되고 그것이 웹사이트가 올바르게 작동하는 데 필요한지에 대한 논란의 여지가 있을 수도 있어요. 전반적으로 중요한 쿠키들은 해로울 바 없고, 사용자의 익명성을 유지하면서 웹사이트 전반적인 경험을 가능하게 해줄 것이에요.

# 동의 존중

그래서 쿠키 동의 팝업을 거절했는데, 몇 가지 중요한 쿠키들은 여전히 설정될 거라고 알고 있어요. 그게 전부일까요?

<div class="content-ad"></div>

잘…

네, 그게 맞아요. 하지만 쿠키 동의가 제대로 존중된다는 가정하에 그렇죠. 그런데 항상 그렇지는 않아요. 특정한 스크립트가 자체적으로 쿠키를 설정하며 사용자 동의를 제대로 존중하지 않을 때가 있어요. 사용자 동의를 제대로 존중하지 않는 예제를 살펴보겠습니다. 걱정 마세요, 이후에 어떻게 수정하는지 알려줄게요.

쿠키 동의 준수하지 않는 웹사이트를 '이름을 공개하고 비난하는' 것이 합법적인지 확신하지는 못해서, 실제 웹사이트 URL은 비공개로 유지하는 게 모두에게 더 편할 것 같아요. 하지만 이 웹사이트는 매일 수천 건의 페이지 조회가 예상되는 인기 있는 영국 웹사이트에요. 웹사이트의 닉네임을 'Webby'로 지어볼게요.

![이미지](/assets/img/2024-08-13-CookieconsentistheWildWest_2.png)

<div class="content-ad"></div>

웹사이트 Webby는 꽤 평범하고 잘 디자인된 사이트로, 특별한 점은 없어 보입니다.

Chrome에서 웹사이트 Webby를 처음 방문하고 쿠키로 이동하면 이미 사용자 동의 없이 설정된 많은 쿠키 목록이 표시됩니다.

![이미지](/assets/img/2024-08-13-CookieconsentistheWildWest_3.png)

내 스파이더 감각이 작동되며 사용자 쿠키 동의 권한이 아직 주어지지 않았는데도 설정되는 많은 쿠키들이 있음을 느낍니다. 특히, _vwo 쿠키가 인기 있는 A/B 테스팅 스위트인 Visual Web Optimizer와 관련된 행동을 추적한다는 것을 알고 있기 때문입니다.

<div class="content-ad"></div>

ICO가 이 행동에 대해 매우 명확하게 설명하고 있어요.

알았어 알앉, 웨비가 미리 몇 가지 쿠키를 설정해두는 거죠. 큰 문제는 아닐 거에요. 분명 사용자 쿠키 동의를 취할 때 이 쿠키들은 존중할 거예요.

이제 그걸 테스트해볼게요.

다음은 웨비의 쿠키 동의 배너가 어떻게 생겼는지에 대한 내용입니다.

<div class="content-ad"></div>


![Cookie consent screenshot 1](/assets/img/2024-08-13-CookieconsentistheWildWest_4.png)

You can see that the necessary cookies box is not toggleable — that’s expected due to how necessary cookies function.
The Preferences, Statistics, and Marketing categories have been toggled off. Clicking Ok means that any cookies related to these categories should be unset.

*Clicking the Ok button*

![Cookie consent screenshot 2](/assets/img/2024-08-13-CookieconsentistheWildWest_5.png)


<div class="content-ad"></div>

성공입니다! 우리는 예상대로 _vwo 쿠키가 제거되었다는 것을 확인할 수 있습니다.
그런데 VWO 쿠키가 필수 쿠키가 아님을 어떻게 아는 건가요? 해당 쿠키는 웹 사이트 자체의 쿠키 정책 페이지에서 '통계' 항목에 속합니다.

![쿠키 동의 팝업](/assets/img/2024-08-13-CookieconsentistheWildWest_6.png)

따라서 쿠키를 거부했을 때 VWO 쿠키는 보이지 않을 것으로 예상됩니다. 그리고 기쁜 소식은, 현재 그렇게 보입니다.

페이지를 새로고침해서 다시 한 번 확인해 보겠습니다.

<div class="content-ad"></div>


![Cookie consent issue](/assets/img/2024-08-13-CookieconsentistheWildWest_7.png)

Uh oh. VWO cookies have been set again, even though cookie consent for statistic cookies has explicitly not been given.

## This is an issue.

Webby is not respecting cookie consent, and this potentially has major implications in terms of the ICO and GDPR.


<div class="content-ad"></div>

# 어떻게 이 문제를 해결할까요

어떻게 이 무작위 영국 웹 사이트에 숨어있는 쿠키 상호 작용을 발견했는지 궁금할 수도 있습니다. 합리적인 질문이에요.

최근에 VWO와 많이 작업하고 있었는데, 특히 VWO SmartCode 라는 것과 많이 작업했어요.
SmartCode는 웹 사이트에 추가되면 VWO 기능을 사용할 수 있게 해주는 JavaScript 스크립트에요. 
이 스크립트는 VWO가 당신의 웹 사이트를 '볼' 수 있게 하고, 추적 데이터를 당신의 사이트에서 VWO로 전달할 수 있게 하며, 또한 VWO 쿠키를 설정할 수 있도록 합니다. 작은 스크립트로서 당신의 사이트 헤더에 두렵지 않게 자리 잡고 있어요.

SmartCode의 문제점, 아니면 SmartCode를 구현하는 문제는, 이 스크립트를 단순히 생으로 웹 사이트에 삽입하는 것만으로는 쿠키 동의를 자동으로 준수하지 않는다는 점이에요.
이 스크립트는 모든 쿠키 동의 팝업 위에 있고, 사이트가 처음로드될 때 쿠키가 사전에 활성화되어 있는 것처럼 보이는 이유이며, 더 중요한 것은 새로 고침할 때 쿠키가 돌아오는 이유이기도 해요.

<div class="content-ad"></div>

이 문제는 VWO SmartCode를 내 프로젝트에 어떻게 구현할지 테스트하는 중에 발견한 문제였어요. 다른 사람들도 이런 문제에 빠졌는지 궁금해서 빠르게 찾아보니 Webby에서도 같은 문제가 있었어요.

저는 Webby가 VWO SmartCode를 쿠키 동의 메커니즘을 통해 삽입하는 대신 직접 DOM에 주입하고 있다고 생각해요.

이 문제를 해결하려면 스크립트를 쿠키 동의 관리자를 통해 삽입해야 합니다. 이렇게 하면 허락 없이 추적 쿠키가 설정되지 않도록 할 수 있어요. 그게 다에요. 이 변경을 하면 미리 활성화된 VWO 쿠키가 더 이상 발생하지 않고, 새로 고침할 때 쿠키가 다시 나타나지 않게 되겠죠.

# 마무리

<div class="content-ad"></div>

사용자들은 필수가 아닌 쿠키를 거부함으로써 실제로 필요하지 않은 쿠키가 설정되는 것을 막는 기능을 신뢰할 수 있어야 합니다. 이것은 단지 상식적인 문제입니다. 평균 사용자는 쿠키 동의가 예상대로 작동하는지를 믿어야 합니다. 대부분의 사람들은 당신의 웹 사이트 쿠키를 살펴볼 생각을 하지 않을 것입니다. 심지어 나도 우연히 이 예시를 찾았습니다. 그곳에 얼마나 오랫동안 있었는지 알 수 없습니다. 익명 유지를 희망한 사용자들에 대해 실수로 얼마나 많은 데이터가 전송되었을지 모른다면 어떨지요? 케임브리지 애널리티카만큼 스캔들은 아니라고 주장하진 않지만, 이러한 것들은 중요합니다. 인터넷 익명성은 점점 더 중요해지고 있습니다. 웹비의 창조자들이 VWO의 오용을 전혀 모르고 있는 것도 매우 가능성이 높습니다. 저는 이것이 악의가 아닌 무능에서 비롯된 문제라고 생각합니다. 이것이 오류를 변명하지는 않지만, 쿠키 동의는 심각한 주제이며 개발자들도 고민하는 부분입니다. 동일한 실수를 저지르고 있는 수백 개, 아니 수천 개의 다른 웹 사이트가 있을 것으로 예상됩니다.

웹 사이트를 소유하고 계신다면, 지금 쿠키 준수 여부를 확인해보는 것이 좋습니다. 혹시 내가 계속해서 당신에 대해 이야기하고 있었을지도 모르겠네요 👀

그런데, 이 모든 정보는 웹비 소유자들에게 이메일로 전송되었습니다. 지금쯤에는 문제가 해결된 것일지도 모릅니다.

이 기사를 좋아하셨거나 유용하게 여기셨다면, 저를 팔로우해주세요. 또는 커피 한 잔 사주시는 방법으로 저를 지원해주셔도 됩니다! 모든 지원은 매우 감사히 받고, 완전히 선택적입니다.