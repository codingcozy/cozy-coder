---
title: "반드시 알아야하는 CSS 팁 3가지"
description: ""
coverImage: "/assets/img/2024-08-17-3CSSOne-LinersICantLiveWithout_0.png"
date: 2024-08-17 00:06
ogImage: 
  url: /assets/img/2024-08-17-3CSSOne-LinersICantLiveWithout_0.png
tag: Tech
originalTitle: "3 CSS One-Liners I Cant Live Without"
link: "https://medium.com/gitconnected/3-css-one-liners-i-cant-live-without-8f181f8d2772"
isUpdated: true
updatedAt: 1723863526842
---


<img src="/assets/img/2024-08-17-3CSSOne-LinersICantLiveWithout_0.png" />

CSS를 정말 잘 알고 있다고 생각하시나요? 생각을 다시 해보세요!

아마도 들어보지 못한 3가지 CSS 원라이너를 소개합니다:

# 1. 적응형 그리드 레이아웃

<div class="content-ad"></div>

```css
.dynamic-grid { grid-template-columns: repeat(auto-fit, minmax(calc(100vw / 10), 1fr)); }
```

어떤 화면 크기에도 완벽하게 맞게 조절되는 그리드를 원해 보셨나요?

이 트릭이 여러분의 새로운 가장 친한 친구가 될 거예요. 뷰포트 너비에 따라 자동으로 조절되는 반응형 그리드를 생성합니다.

# 2. clamp()를 사용한 유동적인 텍스트 크기

<div class="content-ad"></div>

```css
.fluid-text { font-size: clamp(1rem, 2vw + 1rem, 3rem); }
```

만약 작은 화면에서 글자가 너무 커서 고민한 적이 있거나 큰 화면에서는 글자가 너무 작아 보였다면, clamp()가 도움이 될 수 있어요.

이 한 줄로 텍스트 크기를 유동적으로 만들어, 뷰포트의 변화에 따라 아름답게 크기가 조절돼요.

# 3. 멋진 배경 블렌딩들

<div class="content-ad"></div>

```js
.blend-background { background: url('image.jpg') no-repeat center center; background-size: cover; background-blend-mode: multiply; }
```

배경 이미지를 더 돋보이게 만들거나 멋진 오버레이 효과를 만들고 싶나요?

background-blend-mode 속성은 배경 이미지를 해당 색상과 혼합하여 디자인에 마법 같은 터치를 더해줍니다. 예를 들어 multiply는 이미지를 어둡게 만들어 세련된, 우울한 느낌을 줍니다.

이 팁들이 웹 디자인 여정을 조금 더 원활하고 재미있게 만들어줄 것을 희망합니다.

<div class="content-ad"></div>

도전해보세요! 단 몇 초만에 여러분의 디자인이 한 단계 더 발전할 거예요!

코딩을 즐기세요!