---
title: "많이 사용하지 않지만 유용한 HTML 태그 5가지"
description: ""
coverImage: "/assets/img/2024-08-18-5HTMLtagsyouhaventbeenusingbutprobablyshouldbe_0.png"
date: 2024-08-18 10:27
ogImage: 
  url: /assets/img/2024-08-18-5HTMLtagsyouhaventbeenusingbutprobablyshouldbe_0.png
tag: Tech
originalTitle: "5 HTML tags you havent been using, but probably should be"
link: "https://medium.com/@kyleabrady/5-html-tags-you-havent-been-using-but-probably-should-be-fd08973e2c67"
isUpdated: true
updatedAt: 1723950982091
---


웹 페이지를 개발하는 사람이라면 확실히 `a`, `span`, `form`, `table`, `ul` 태그를 담은 HTML 태그 도구 상자가 있을 것입니다. 모든 웹 페이지에는 하나의 `h1` 태그가 있어야 합니다. 페이지에 적어도 몇 개의 `div` 태그가 없다면, 무슨 일을 하고 있는 건지요?

하지만 여러분께 웹 페이지에서 실제로 큰 차이를 만들 수 있는 다섯 가지 HTML 태그 목록을 공유하고 싶습니다.

<img src="/assets/img/2024-08-18-5HTMLtagsyouhaventbeenusingbutprobablyshouldbe_0.png" />

# 목록에 없는 것

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

하지만 먼저 사용하지 않아도 되는 다섯 가지 HTML 태그를 살펴보기 전에, 사용하지 말아야 할 몇 가지 태그에 대해 먼저 살펴보겠습니다.

## 사용하지 말아야 할 #1 — `blink`

현대 브라우저는 `blink` 태그를 인식하거나 지원조차 하지 않습니다. 이전에 이 태그가 렌더링했던 번쩍이는 효과는 화려하지만 사용성이 좋지 않아서 다행입니다.

텍스트의 깜박이 효과는 여전히 CSS나 JavaScript를 통해 페이지에 추가할 수 있지만, 사용자에게 이러한 경험을 제공하는 것은 피하는 것이 좋습니다.

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

좋아요, `blink` 태그가 사라졌네요!

## 사용 금지 - `marquee` 태그

`blink` 태그만큼 좋지 않은 것이라면 `marquee` 태그인데요, 여전히 브라우저에서 지원은 되지만 폐기되었고 웹 표준에서 제거되었습니다. 이 태그는 포함된 텍스트가 설정 가능한 속도와 방향으로 웹페이지 컨테이너 위를 스크롤하는 기능을 제공합니다.

`blink` 태그와 마찬가지로, `marquee` 태그의 행동은 웹페이지의 접근성과 일반적인 사용 편의성에 심각한 문제를 일으킵니다.

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

만약 현재 이 HTML 태그 중 하나를 사용 중이라면, 그만두세요. 사용 중이 아니라면, 잘했어요! 계속해서 사용하지 않도록 해주세요.

이제 좋은 소식을 들어보세요.

알고 계세요? 사실 여기에는 당신이 사용하지 않았겠지만 아마도 사용해야 할 일곱 가지의 HTML 태그 목록이 있습니다. 하지만 몇 가지는 다른 것과 관련이 있기 때문에 리스트 크기를 줄이기로 결정하여 다섯 가지라고 말했어요. 당신의 행운한 날이에요!

# #1 — 두 가지 태그 한 가지로: `details` 와 `summary`

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

당신의 웹페이지에 빠르고 쉬운 아코디언을 추가하여 추가 정보를 볼 수 있고 숨길 수 있습니다. `details`와 `summary` 태그만 있으면 됩니다.

`summary` 태그 내의 텍스트는 접는 섹션의 제목으로 작동합니다. 클릭하거나 키보드로 활성화되면 `details` 태그의 나머지 내용이 나타나거나 숨겨집니다.

```js
<details>
  <summary>네 번째 수정안: 수색과 압수</summary>
  <p>
    사람들이 자신의 몸, 집, 서류 및 효력에 대한 무차별적인 수색과 압수로부터 안전할 권리는 침해되어서는 안 되며, 일각인 사물이나 물건을 압수하기 위한 장소를 명백하게 기술하는 증언이나 단언에 의한 합리적인 원인에 기초해 발부되는 영장이 없어서는 안 된다.
  </p>
</details>
```

`details` 태그는 초기에 페이지 로드시 추가 정보가 표시되는지 여부를 제어하는 선택적인 `open` 속성이 있습니다.

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

# #2 — 슈퍼칼리프라질리스틱익스피알리도셔스 도우미: `wbr`

파일 경로나 긴 단어에 특히 유용한 `wbr` 태그는 단어 간 줄 바꿈 가능성을 식별하기 위해 사용됩니다. 이를 통해 브라우저가 자체적으로 줄 바꿈 전략을 사용하는 대신 텍스트를 원하는 위치에서 잘라내고 나누도록 시도할 수 있습니다.

```js
<p>
  Hydro<wbr>chloro<wbr>fluoro<wbr>carbons are sometimes used as aerosol propellants.
</p>
```

`br` 태그와 달리 줄 바꿈을 강제하지 않는 `wbr` 태그는 사용자 브라우저에서 표시해야 할 경우 줄 바꿈이 필요한 위치의 제안만 제공합니다.

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

# #3 — 일어나요, 낮아지세요: `sup`와 `sub`

`sup`와 `sub` 태그는 각각 인라인 텍스트를 위 첨자 및 아래 첨자로 렌더링합니다. 이 태그 내의 텍스트 값은 올려진 또는 내려진 기준선으로 표시됩니다.

`sup` 태그를 사용한 위 첨자 텍스트의 예시로는 서수 표시자(-st, -nd, -rd, -th)가 있습니다.

```js
크리스마스는 12월 25일에 축하됩니다.
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

아래는 `sub` 태그를 사용한 아래 첨자 텍스트의 예시입니다.

```js
물: H<sub>2</sub>O
```

`sup` 태그와 `sub` 태그는 동시에 사용하여 텍스트를 높이고 낮추는 것은 불가능합니다. 왜 텍스트를 동시에 올리고 내리려고 하는지 잘 모르겠네요. 그냥 아무것도 하지 않고 기본 베이스라인에 맞춰 텍스트를 작성하세요.

Credit where credit is due: `cite` (#4)

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

창의적인 작품의 제목을 인용할 때 `cite` 태그를 사용하여 의미론적인 방식으로 표시할 수 있습니다. `cite` 태그는 책, 연극, 영화, 웹사이트, 페이스북 게시물, TV 프로그램 및 노래와 같은 다양한 창의적인 작품을 참조할 때 사용할 수 있습니다.

```js
허먼 멜빌의 <cite>모비 딕</cite>은 "내 이름은 이스마엘"로 시작합니다.
```

## #5 — 간결하게 유지하기: `abbr`

`abbr` 태그는 웹 페이지의 본문에서 약어나 머리글자를 정의하는 데 사용됩니다. 이 태그는 매우 적절하게 '약어'라는 단어의 약어입니다.

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

이 태그를 사용하면 사용자가 주어진 약어나 두문자어의 의미를 알 수 있도록 해당 태그의 title 속성을 통해 전체 확장 버전을 표기합니다.


<abbr title="Self Contained Underwater Breathing Apperatus">SCUBA<abbr>


HTML5 출시 이후 `acronym` 태그는 사용이 중지되었으므로 그 대체로 `abbr` 태그가 권장됩니다.

# 이 HTML 태그들을 자유롭게 사용하세요!

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

이 HTML 태그들이 익숙할 수도 있고, 크게 사용되지 않을 수도 있지만, 당신의 웹페이지를 더욱 효과적으로 개선할 수 있는 스마트한 방법일 수 있습니다.

다른 흥미로운 HTML 태그나 표준에서 벗어난 태그를 찾아서 댓글로 남겨주세요!