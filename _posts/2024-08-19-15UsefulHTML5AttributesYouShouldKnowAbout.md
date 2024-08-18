---
title: "15가지 유용한 HTML5 속성들을 알아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_0.png"
date: 2024-08-19 01:56
ogImage: 
  url: /assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_0.png
tag: Tech
originalTitle: "15 Useful HTML5 Attributes You Should Know About"
link: "https://medium.com/@kaderbiral26/15-useful-html5-attributes-you-should-know-about-acb8774e4188"
isUpdated: false
---


![Table](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_0.png)

웹 개발의 세계는 끊임없는 혁신과 발전으로 가득 차 있습니다. 이러한 발전 중 하나가 HTML5에 의해 가져온 기능입니다. HTML5는 웹 페이지를 더 인상적이고 기능적으로 만드는 일련의 속성을 제공합니다. 이러한 속성은 웹 개발자들에게 더 많은 유연성, 성능 및 사용자 경험을 제공하기 위해 설계되었습니다. 본 문서에서는 HTML5의 잘 알려지지 않은 15가지 유용한 속성을 살펴보겠습니다.

## 1. contenteditable

contenteditable 속성은 사용자가 요소의 내용을 편집할 수 있도록 만듭니다.

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
<div contenteditable="true">
    편집 가능한 텍스트입니다.
</div>
```

<img src="/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_1.png" />

## 2. spellcheck

스펠체크 속성은 텍스트 입력 또는 텍스트 필드의 맞춤법을 확인합니다. 사용자가 입력한 텍스트가 올바른지 확인하고 잘못 입력된 단어를 표시하도록 이 속성을 사용할 수 있습니다.

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
<p spellcheck="true" contenteditable="true">
     여기에 콘텐츠를 추가하세요.
</p>
```

![Useful HTML5 Attributes](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_2.png)

## 3. 번역

translate 속성은 텍스트 조각이 번역되는지 여부를 나타내는 데 사용됩니다. 예를 들어, 로고, 회사 이름 또는 브랜드 이름 등이 있습니다.

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
<footer>
    <small>© 2024 <span translate="no">회사명</span> All right reserved.</small>
</footer>
```

<img src="/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_3.png" />

## 4. multiple

`multiple` 속성은 양식 요소에 동시에 여러 옵션을 선택할 수 있는 속성입니다. 일반적으로 `select`와 `input` 태그와 함께 사용됩니다.

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

```html
<label for="cars">좋아하는 자동차를 선택하세요:</label>
<select id="cars" name="cars" multiple>
    <option value="volvo">볼보</option>
    <option value="mercedes">메르세데스</option>
    <option value="peugeot">푸조</option>
    <option value="audi">아우디</option>
</select>
```

![Useful HTML5 Attributes You Should Know About](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_4.png)

## 5. reversed

reversed 속성은 번호가 매겨진 목록에서 사용되는 속성입니다. 이 속성은 목록 항목의 순서를 반대로 바꾸는 데 사용됩니다. 일반적으로 `ol` (순서가 지정된 목록) 요소 내부에서 사용됩니다.


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


1. Strawberry
2. Cherry
3. Peach


<img src="/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_5.png" />

## 6. draggable

드래그 앤 드롭을 허용할지 여부를 결정하는 속성입니다. 이 속성을 사용하면 사용자가 마우스를 사용하여 항목을 드래그 앤 드롭할 수 있습니다.


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
<p draggable="true">이것은 끌어서 놓기 가능한 텍스트입니다.</p>
```

📌 끌어서 놓기 속성은 부울(boolean)이 아니라 열거형 속성입니다. HTML 사양에 따르면 이 속성을 사용할 때 "true" 또는 "false" 값을 명시적으로 지정해야 합니다.

## 7. accesskey

accesskey 속성을 사용하면 사용자가 키보드에서 특정 키 조합(일반적으로 [Alt] 키와 함께 글자 또는 숫자)을 눌러 특정 요소에 빠르게 액세스할 수 있습니다.

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


[Contact](https://www.example.com){:accesskey="c"} - [Alt]+C를 눌러 액세스할 수 있습니다.

![Useful HTML5 Attributes You Should Know About](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_6.png)

## 8. poster

poster 속성을 사용하면 비디오 플레이어가 로드할 비디오와 관련된 포스터 이미지를 지정할 수 있습니다. 포스터 이미지는 비디오 플레이어가 로드되는 동안 표시될 수 있으며 사용자에게 비디오의 미리보기를 제공합니다.


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


<video width="380" height="240" poster="video_poster.jpg" controls>
  <source src="video.mp4" type="video/mp4">
  <source src=" video.ogg" type="video/ogg">
  Your browser does not support the video tag. 
</video>


![Useful HTML5 Attributes](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_7.png)

## 9. onerror

`onerror` 속성은 외부 파일(이미지 또는 다른 미디어 요소)을로드하는 동안 오류가 발생할 경우 트리거되는 JavaScript 코드를 지정하는 데 사용됩니다.


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


![](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_8.png)

## 10. theme-color

theme-color 속성은 웹 페이지의 테마 색상을 웹 브라우저에 지정합니다. 이 색상은 브라우저 인터페이스에 통합되어 사용자가 브라우저에서 탐색하는 동안 특정 테마 색상을 제공합니다.


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
<meta name="theme-color" content="#007bff">
```

![Image](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_9.png)

## 11. favicon

파비콘은 웹 사이트의 브라우저 탭이나 즐겨찾기에 나타나는 작은 아이콘입니다. 사용자가 웹 사이트 간에 쉽게 이동할 수 있도록 도와줍니다.

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

```html
[마크다운 형식으로 변경]
```

![Touch-icons](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_10.png)

## 12. 터치 아이콘

터치 아이콘(touch-icons)은 스마트폰이나 태블릿의 홈 화면에 웹 사이트나 웹 애플리케이션을 위한 바로 가기 아이콘으로 추가됩니다. 이 아이콘을 통해 사용자는 웹 사이트나 애플리케이션에 빠르게 액세스할 수 있습니다.

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


[link rel]="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png"
[link rel]="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png"
[link rel]="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png"

![Example Image](/assets/img/2024-08-19-15UsefulHTML5AttributesYouShouldKnowAbout_11.png)

## 13. loading

The loading attribute is used to control the loading behavior of a resource by the browser. This attribute can delay or prioritize the loading of resources while the browser waits for the page to load or be processed. This is used to improve page performance and enhance user experience.


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

`img`, `iframe`, `script`, `link rel=”stylesheet” href=”styles.css”`와 같은 요소에서 특히 사용됩니다.

```js
<!-- 예시 1: 이미지 로딩 시 "lazy" 사용 예시. -->
<img src="image.jpg" loading="lazy">

<!-- 예시 2: 이미지 로딩 시 "eager" 사용 예시. -->
<img src="image.jpg" loading="eager">
```

🔸예시 1: loading="lazy" ➔ 이것은 페이지 로딩이 완료될 때까지 이미지의 가시성을 지연시킵니다. 사용자가 이미지를 볼 수 있을 때만 로드됩니다. 이는 페이지 로딩 속도를 높이고 사용자 경험을 향상시킬 수 있습니다. 특히 페이지에 많은 큰 이미지가 있거나 스크롤을 내릴 때 특히 유용할 수 있습니다.

🔸예시 2: loading="eager" ➔ 이것은 브라우저에 즉시 이미지를 처리하도록 지시합니다. 사용자는 즉시 이미지를 볼 수 있지만 페이지 로딩 시간이 늘어날 수 있습니다. 특히 이미지가 크거나 이미지 수가 많은 페이지에서 유용합니다.

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

## 14. async

async 어트리뷰트는 브라우저에서 스크립트 파일을 다운로드하는 동안 HTML 콘텐츠의 처리를 멈추지 않습니다. 스크립트 파일이 준비되면 다운로드가 완료될 때 처리됩니다.

```js
<script src="example.js" async></script>
```

## 15. defer

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

defer 속성은 브라우저가 스크립트 파일을 다운로드하는 동안 HTML 처리를 계속할 수 있게 합니다. 스크립트 파일은 HTML 내용을 처리한 후에 실행됩니다. defer로 로드된 스크립트는 순차적으로 실행됩니다.

```js
<script src="example.js" defer></script>
```

# 결론

그 결과로, HTML5의 15가지 잘 알려지지 않았지만 매우 유용한 기능에 대한 이 기사는 웹 개발자의 더 효과적이고 효율적인 작업에 기여하고자 합니다. 이러한 기능들은 초보 개발자에게 새로운 정보를 제공할 뿐만 아니라, 경험豊富한 개발자들이 작업 흐름을 개선하는 데 도움을 줍니다. HTML5가 제공하는 이러한 깊은 기능을 탐색함으로써 웹 애플리케이션을 보다 강력하고 접근성이 좋고 사용자 친화적으로 만들 수 있습니다.