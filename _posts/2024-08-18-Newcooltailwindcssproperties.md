---
title: "최신 Tailwind CSS 속성들 기능 정리"
description: ""
coverImage: "/assets/img/2024-08-18-Newcooltailwindcssproperties_0.png"
date: 2024-08-18 10:26
ogImage: 
  url: /assets/img/2024-08-18-Newcooltailwindcssproperties_0.png
tag: Tech
originalTitle: "New cool tailwind css properties"
link: "https://medium.com/@shreyvijayvargiya26/new-cool-tailwind-css-properties-eeea98808df1"
isUpdated: true
updatedAt: 1723951816721
---

접근성을 고려하여 발전하는 모습이 너무 좋아요 😊

![새로운 쿨 테일윈드 CSS 속성](/assets/img/2024-08-18-Newcooltailwindcssproperties_0.png)

Tailwind CSS의 새로운 기능부터 살펴봅시다. 버전 3 이후에 무엇이 변경되었는지 살펴볼게요. 버전 3.4가 최신 버전인 것 같아요.

우리 대부분은 레포지토리의 패키지를 업데이트하지 않는 경우가 많죠. 필요하거나 의무적으로 진행할 때에만 업데이트를 하게 됩니다 😃

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

그 느낌을 이해할 수 있어요. 그래서 대부분은 새 저장소를 만들 때만 패키지를 업그레이드할 거라고 생각해요 😆

본론으로 돌아와서, 저는 지난 4년간 Tailwind CSS를 사용해왔고 그것을 사랑하고 있어요. 대부분의 사람들은 Tailwind CSS를 작성하는 것만으로 프론트엔드 개발자로 성장하지 못할 것이라고 주장할 수도 있어요.

시간이 흐르면서, 제가 경험한 바에 의하면 Tailwind CSS는 간단한 CSS 작성에 관련된 몇 가지 문제를 야기할 수 있지만, 그것은 대체로 구문적인 문제이며 요소에 스타일을 주는 핵심 개념이나 애니메이션 추가 등은 여전히 동일해요.

단지 구문의 차이일 뿐이에요 🌟

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
JSX에서 CSS
transform: “”,

Tailwind CSS
transform: transition-all,
```

기본 사항을 이해하면 구문이 큰 차이를 만들지 않을 것입니다.

애니메이션을 작성할 때는 JSX에서 CSS를 선호하고 간단한 스타일링을 위해서는 Tailwind를 사용하며, 이것이 대부분의 저장소가 개발자가 선호하는 문법으로 작성할 수 있도록 유연성을 제공하는 이유입니다.

# Tailwind CSS에서 무엇이 새롭고 멋진가요?

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

테일윈드 개발자들은 접근성과 애니메이션에 매우 열심히 작업하고 있어요.

개발자들은 Tailwind CSS를 사용하여 간단한 CSS나 GSAP 또는 framer-motion과 같은 다른 애니메이션 라이브러리로 할 수 있는 복잡한 애니메이션을 작성하는 데 대한 우려를 제기했습니다.

## Divide

https://tailwindcss.com/docs/divide-style

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

대부분의 경우 우리는 열 사이에 구분선을 추가하고 싶어합니다. 그게 Divider 속성이 하는 일입니다. Flex 순서로 배치된 요소들에 경계 두께를 추가하여 나누는 모양을 만듭니다.

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_1.png)

# 간격

https://tailwindcss.com/docs/place-content#space-between

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

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_2.png)

# 선형 그라데이션

https://tailwindcss.com/docs/background-image#linear-gradients

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_3.png)

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

# 스크린 리더

[스크린 리더 가이드](https://tailwindcss.com/docs/screen-readers)

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_4.png)

# 캐럿 스타일링

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

https://tailwindcss.com/docs/caret-color

![screenshot](/assets/img/2024-08-18-Newcooltailwindcssproperties_5.png)

# 스냅 제어

https://tailwindcss.com/docs/scroll-snap-type

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

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_6.png)

# Resize

https://tailwindcss.com/docs/resize

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_7.png)

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

# 외형

![이미지](/assets/img/2024-08-18-Newcooltailwindcssproperties_8.png)

# 강조 색상

https://tailwindcss.com/docs/accent-color

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

<img src="/assets/img/2024-08-18-Newcooltailwindcssproperties_9.png" />

오늘은 여기까지입니다. 다음 시간에 뵙겠습니다.

팔로우하고 구독해도 좋아요!!

Shrey
