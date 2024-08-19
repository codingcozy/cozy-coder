---
title: "fit-content CSS 함수 정리"
description: ""
coverImage: "/assets/img/2024-08-19-Decodingthefit-contentCSSFunction_0.png"
date: 2024-08-19 01:50
ogImage: 
  url: /assets/img/2024-08-19-Decodingthefit-contentCSSFunction_0.png
tag: Tech
originalTitle: "Decoding the fit-content CSS Function"
link: "https://medium.com/@divjotdavy/decoding-the-fit-content-css-function-1e0c5c1f6ce2"
isUpdated: true
updatedAt: 1724033226405
---


![Fit-content() CSS Function](/assets/img/2024-08-19-Decodingthefit-contentCSSFunction_0.png)

이 글에서는 fit-content() CSS 함수를 살펴보겠습니다. 이 함수는 최근 CSS에 추가된 것으로, CSS 그리드 모듈에서 주로 사용되어 그리드 레이아웃을 구성합니다. 일관된 그리드 레이아웃과 구조를 만드는 데 가장 유용한 도구 중 하나입니다. 이 기능이 어떻게 작동하는지 살펴보고 때로는 원하는 결과를 제공하지 않는 이유를 알아볼 것입니다.

# fit-content()는 무엇을 하는가요?

그리드 요소의 grid-template-columns 및 grid-template-rows 속성에서 사용되어 그리드 항목의 차원을 지정합니다. MDN에 따르면, "fit-content() CSS 함수는 주어진 크기를 형식에 맞추는데, 이는 주어진 수식에 따라 가능한 크기에서 값을 판단합니다 (최대 크기, 최소 크기, 인수)." 좋아요, 그런데 이게 무슨 뜻이죠?

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

간단히 말해서, fit-content()은 그리드 항목을 최소 내용보다 크고 최대 내용보다 작을 경우 인수에 맞게 크기를 조정합니다. 콘텐츠가 최대 크기보다 클 경우에는 인수가 무시되고 그리드 항목은 최대 내용으로 자체를 설정합니다.

이는 요소의 높이와 너비를 모두 조정하는 데 사용될 수 있습니다. 이 기사에서는 두 가지 경우를 살펴보겠습니다. 우선 높이를 조정하는 것부터 시작해보죠.

예를 들어봅시다. 간단한 레이아웃이 있다고 가정해보겠습니다–

```js
<div class="grid">
  <div class="item">
   Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
  </div>
</div>
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

간단한 .grid 컨테이너 요소에 .item 그리드 항목을 추가했어요. 다음 CSS 코드를 추가했어요 -

```js
.grid {
    display: grid;
    height: 300px;
    width: 500px;
}

.item {
    padding: 1rem;
    font-family:'Arial', sans-serif;
    background-color: antiquewhite;
}
```

.grid 컨테이너에는 너비가 500픽셀이고 높이가 300픽셀이에요. 또한 배경색, 일부 타이포그래피 및 패딩을 그리드 항목에 추가하여 조금 더 멋지게 보이게 했어요. 결과는 이렇게 보여야 해요 - 

<img src="/assets/img/2024-08-19-Decodingthefit-contentCSSFunction_1.png" />

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

위에서 보듯이, .grid 컨테이너에는 샘플 텍스트가 있는 .item 그리드 아이템이 있습니다. 컨테이너 요소의 크기는 500*300px입니다. 이제 그리드 항목의 높이를 콘텐츠에 맞게 조정하는 방법을 살펴보겠습니다.

## 행에 대한 fit-content() 함수

fit-content() 함수가 그리드 항목의 높이에 어떻게 영향을 미치는지 보려면 다음 CSS를 .grid 컨테이너 요소에 추가해 보세요 -

```js
.grid {
  display: grid;
  width: 500px;
  height: 300px;
  grid-template-rows: fit-content(10%);
}
.
.
.
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

안녕하세요! 표 태그를 Markdown 형식으로 변경하시라고 하셨네요. 

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

지금은 최대 크기가 그리드 항목이 달성할 수 있는 최대 높이입니다. 이전과 비슷하게, 여기에서 그리드 항목의 최대 가능한 높이를 얻으려면 height: max-content CSS 속성을 사용할 수 있습니다. 콘텐츠가 더 이상 확장되지 않으므로 최대 크기는 106px이 될 것입니다. 이제 우리의 식에 따라 최소 크기와 최대 크기가 있습니다. 우리는 10%를 인수로 사용했습니다. 컨테이너 높이가 300px이므로 절대적으로 인수는 (300의 10%) 30px가 될 것입니다. 이를 우리의 식에 사용하면 -

```js
min(106, max(106, 30)); // 결과: 106
```

따라서 공식에 따른 콘텐츠의 높이는 106px입니다. 최소 콘텐츠와 최대 콘텐츠 값이 같기 때문에 인수의 값이 무엇이든 상관없이 결과는 항상 같을 것입니다. 우리의 그리드 항목은 항상 콘텐츠에 맞게 맞출 것입니다.

지금까지 fit-content()를 사용하여 그리드 항목의 높이를 콘텐츠에 맞게 조절하는 방법을 살펴보았습니다. 이제 fit-content()가 그리드 항목의 너비에 어떻게 영향을 미치는지 살펴봅시다.

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

## fit-content()를 사용한 열

여기서 fit-content() 함수가 정말 빛을 발합니다. 이 함수를 열 기반 레이아웃에 적용합니다. .grid 컨테이너의 CSS를 다음과 같이 수정해보세요 -

```js
.grid {
  display: grid;
  width: 500px;
  height: 300px;
  grid-template-columns: fit-content(10%);
}
.
.
.
```

원하는 브라우저에서 레이아웃 변경 사항을 확인하세요. 그리드 항목의 너비가 줄어들고, 높이는 내용을 맞추기 위해 비례적으로 증가했음을 알 수 있습니다.

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


![그리드 항목이 너비를 계산하는 방법을 살펴보겠습니다. 위의 코드에서 fit-content에 동일한 10%를 인수로 제공했습니다. 이 경우, 최소 사이즈는 width: min-content를 사용하여 계산됩니다. 텍스트 내용의 최소 너비를 계산하기 위해 컨텐츠는 가능한 매 회마다 감싸져 가장 긴 단어가 될 때까지 넓어져서 가장 작은 너비를 얻습니다. 그리드 항목에 width: min-content를 적용하면 최소 너비는 약 115px 정도 될 것입니다.](/assets/img/2024-08-19-Decodingthefit-contentCSSFunction_5.png)


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

그리드 항목의 최대 크기는 width: max-content를 사용하여 계산됩니다. 텍스트의 경우 줄 바꿈을 무시하고 모든 텍스트를 한 줄에 놓고 계산됩니다. 이 경우 최대 크기는 1680픽셀까지 확장됩니다.

![이미지](/assets/img/2024-08-19-Decodingthefit-contentCSSFunction_6.png)

따라서 최소 크기는 115px이고 최대 크기는 1680px입니다. 또한 우리는 10%를 인수로 사용했습니다. 절대값으로는 (500의 10%) 50px입니다. 이러한 모든 값을 우리의 공식에 적용하면 다음과 같습니다 -

```js
min(1680, max(115, 50)) // 결과: 115
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

그리드 항목은 너비를 115픽셀로 계산하게 됩니다.

이제 코드를 바꿔서 fit-content()에 다른 값 할당해보겠습니다. 인자를 200픽셀로 변경하세요 -

```js
.grid {
    display: grid;
    height: 300px;
    width: 500px;
    grid-template-columns: fit-content(200px);
}
.
.
.
```

이제 그리드 항목이 200픽셀의 너비를 갖게 됨을 알 수 있습니다.

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

아래는 Markdown 형식의 텍스트입니다.

이미지를 확인해보세요.

```js
min(1680, max(115, 200)); //결과: 200
```

여기에는 여러분이 직접 값을 바꿔가며 결과를 확인할 수 있는 펜이 있습니다. 함께 놀아보세요.

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

# 마무리

그래서, 그게 다야! fit-content()는 CSS의 최근 추가 사항이며 브라우저 지원이 의문스러울 수 있습니다. 그러나 여전히 CSS에서 강력한 도구로 복잡한 레이아웃을 만드는 데 사용할 수 있습니다. 저는 제작한 테마 중 하나에서 효과적으로 사용하여 주요 뉴스 영역을 만들었습니다.

이 글에서 무언가를 배울 수 있었으면 좋겠어요. 웹 개발 관련 팁, 트릭 및 튜토리얼에 관심이 있다면, 제 사이트인 IndiThemes.com을 확인해보세요. 여기에서는 웹 개발 관련 기사를 정기적으로 게시하는 것 외에도 WordPress 테마를 포함한 서비스를 제공합니다. 다음에 또 만나요!