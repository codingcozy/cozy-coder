---
title: " 웹사이트를 전문가 수준으로 보이게 만들어주는 간단한 CSS 한 줄"
description: ""
coverImage: "/assets/img/2024-08-17-HowaSimpleCSSOne-linerCanMakeYourWebsiteLookLikeaDesignPros_0.png"
date: 2024-08-17 00:03
ogImage:
  url: /assets/img/2024-08-17-HowaSimpleCSSOne-linerCanMakeYourWebsiteLookLikeaDesignPros_0.png
tag: Tech
originalTitle: "How a Simple CSS One-liner Can Make Your Website Look Like a Design Pros"
link: "https://medium.com/gitconnected/how-a-simple-css-one-liner-can-make-your-website-look-like-a-design-pros-64aa870bf4b7"
isUpdated: true
updatedAt: 1723863710112
---

![How a Simple CSS One-liner Can Make Your Website Look Like a Design Pro](/assets/img/2024-08-17-HowaSimpleCSSOne-linerCanMakeYourWebsiteLookLikeaDesignPros_0.png)

Adding a background blur to your website can create a cool, glass-like effect that makes your design look more modern and stylish.

The best part? You can do it with just one line of CSS code!

In this short guide, I will show you how.

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

# 왜 배경 흐림을 사용해야 하나요?

배경 흐림은 인기 있는 효과입니다:

- 유리 효과 생성: 배경을 겹겹이 덮어 유리처럼 보이도록 만들어줍니다. 이는 웹 디자인에서 매우 유행입니다.
- 깊이를 추가합니다: 웹사이트에 레이어가 더해져 3D 느낌을 줍니다.
- 중요한 콘텐츠 강조: 배경을 흐리게 하는 것으로 주요 콘텐츠를 더욱 두드러지게 만들어줍니다.

# 배경 흐림을 위한 간단한 CSS 코드

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

여기 배경을 흐리게 만드는 CSS 한 줄이 있어요:

```js
backdrop-filter: blur(10px);
```

이 코드는 적용한 요소 뒤에 흐린 효과를 만듭니다. 10px는 얼마나 희미한지를 제어하는 부분이에요. 이 숫자를 변경하여 더 희미하거나 또는 덜 희미하게 만들 수 있어요.

## 사용 방법

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

여기에 이 코드를 프로젝트에 사용하는 방법이 있어요:

컨테이너로 내용을 감쌀 때: 내용을 div(컨테이너) 안에 넣어주세요.

```js
<div class="blurred-background">
    <!-- 내용을 여기에 넣어주세요 -->
</div>
```

그리고 CSS를 추가해주세요.

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

```css
.blurred-background {
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.3); /* Adds a frosted glass effect */
}
```

여기서는 blurred-background 클래스가 지정된 div 내부 콘텐츠 뒤에 흐릿하고 유리처럼 보이는 배경을 만듭니다.

# 브라우저 호환성

backdrop-filter CSS 속성은 크롬, 엣지, 사파리, 파이어폭스를 포함한 대부분의 최신 브라우저에서 지원됩니다. 그러나 오래된 브라우저나 일부 모바일 브라우저에서는 작동하지 않을 수 있습니다.

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

인터넷 익스플로러와 오페라 미니에서는 지원되지 않습니다.

더 많은 세부 정보를 확인하려면 이 웹사이트를 방문해보세요:

# 결론

그겣이에요! 단 한 줄의 CSS로 웹사이트에 멋진 유리 효과를 만들 수 있습니다.

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

당신의 디자인이 보다 현대적이고 세련된 느낌을 줄 수 있는 쉬운 방법입니다. 다양한 블러 값으로 실험해보며 당신의 사이트에 완벽한 룩을 찾아보세요.

디자인을 즐기세요!
