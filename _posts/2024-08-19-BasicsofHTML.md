---
title: "HTML 기초 웹 개발 입문자를 위한 필수 요소들"
description: ""
coverImage: "/assets/img/2024-08-19-BasicsofHTML_0.png"
date: 2024-08-19 01:49
ogImage: 
  url: /assets/img/2024-08-19-BasicsofHTML_0.png
tag: Tech
originalTitle: "Basics of HTML"
link: "https://medium.com/@saide2857/basics-of-html-571e504def06"
isUpdated: false
---



![HTML 기본](/assets/img/2024-08-19-BasicsofHTML_0.png)

HTML (HyperText Markup Language)은 웹 페이지를 만드는 데 사용되는 표준 언어입니다. HTML은 웹 페이지의 구조와 콘텐츠를 제공하며, CSS를 사용하여 스타일을 지정하고 JavaScript를 사용하여 상호 작용성을 부여합니다. HTML의 기본 사항을 알아보겠습니다:

# 1. HTML 문서 구조

HTML 문서는 선얹어로 시작해서 태그로 감싸인 일련의 요소로 이어집니다:


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
<!DOCTYPE html>
<html>
  <head>
    <title>My First Webpage</title>
  </head>
  <body>
    <h1>Welcome to My Website</h1>
    <p>This is a paragraph of text on my first webpage.</p>
  </body>
</html>
```

- `!DOCTYPE html`: HTML 문서 유형과 버전을 선언합니다. 브라우저가 문서가 HTML5로 작성되었음을 이해할 수 있도록 도와줍니다.
- `html`: 페이지의 모든 다른 HTML 요소를 포함하는 루트 요소입니다.
- `head`: 문서에 대한 메타 정보(제목, 스타일시트 또는 스크립트에 대한 링크 등)를 포함합니다.
- `title`: 웹페이지의 제목을 설정하며, 브라우저의 제목 표시줄이나 탭에 표시됩니다.
- `body`: 텍스트, 이미지, 링크 등과 같은 웹페이지의 콘텐츠를 포함합니다.

# 2. HTML 태그 및 요소

- 태그: `h1` 또는 `p`와 같이 꺽쇠 괄호로 둘러싸인 키워드입니다. 일반적으로 시작 태그 (`h1`)와 종료 태그 (`/h1`)의 쌍으로 구성됩니다.
- 요소: 시작 태그, 콘텐츠 및 종료 태그로 구성됩니다. 예시:
- `h1`Hello, World!`/h1`

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

# 3. HTML 속성

속성은 HTML 요소에 대한 추가 정보를 제공합니다. 이러한 속성들은 항상 여는 태그에 포함되며 이름="값" 쌍으로 작성됩니다:

```js
<a href="https://www.example.com">Visit Example</a>
<img src="image.jpg" alt="설명 텍스트">
```

- href: `a` (앵커) 태그에서 사용되며 링크의 대상을 지정합니다.
- src: `img` 태그에서 이미지의 소스를 지정합니다.
- alt: 이미지에 대한 대체 텍스트를 제공하며, 접근성을 위해 사용되거나 이미지가 표시되지 않을 때 대체로 사용됩니다.

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

# 4. HTML 헤딩

헤딩은 웹페이지에서 제목과 부제목을 만드는 데 사용됩니다:

```js
<h1>이것은 주요 제목입니다</h1>
<h2>이것은 부제목입니다</h2>
<h3>이것은 더 작은 부제목입니다</h3>
```

- `h1`부터 `h6`까지: 각기 다른 수준의 헤딩을 나타냅니다. `h1`이 가장 중요하고 `h6`이 가장 덜 중요합니다.

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

# 5. HTML 단락

단락은 텍스트 블록을 작성하는 데 사용됩니다:

```js
<p>This is a paragraph of text. HTML automatically adds some space before and after paragraphs.</p>
```

- `p`: 단락을 생성하기 위해 텍스트 블록을 감쌉니다.

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

# 6. HTML 줄 바꿈과 수평선

- 줄 바꿈 (`br`): 텍스트 내에서 줄 바꿈(새 줄)을 만드는 데 사용됩니다.
- 수평선 (`hr`): 주제를 분리하거나 수평선을 만드는 데 사용됩니다:

```js
<p>이것은 첫 번째 줄입니다.<br>이것은 두 번째 줄입니다.</p>
<hr>
<p>수평선 뒤의 텍스트입니다.</p>
```

# 7. HTML 주석

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

브라우저에는 주석이 표시되지 않으며 코드를 설명하는 데 사용됩니다:

```js
<!-- 이것은 주석입니다 -->
<p>이 텍스트는 보입니다.</p>
```

- `!-- Comment --`: 텍스트를 주석으로 처리할 때 사용합니다.

# 8. HTML 링크

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

링크를 통해 사용자들은 다른 페이지로 이동할 수 있어요:

```js
[Example](https://www.example.com)를 방문하려면 여기를 클릭해주세요.
```

- `a`: 앵커 태그는 하이퍼링크를 생성합니다. href 속성은 링크를 클릭했을 때 이동할 URL을 지정합니다.

# 9. HTML 이미지

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

이미지를 삽입하려면 `img` 태그를 사용하면 됩니다:

```js
<img src="image.jpg" alt="이미지에 대한 설명">
```

- `img`: 이미지를 삽입하는 데 사용됩니다. `src` 속성은 이미지 소스를 지정하고 `alt`는 설명 텍스트를 제공합니다.

# 10. HTML 목록

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

HTML은 정돈된 목록과 정돈되지 않은 목록을 지원합니다:

- 정돈되지 않은 목록(`ul`):


<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>


- 정돈된 목록(`ol`):

<ol>
  <li>First item</li>
  <li>Second item</li>
  <li>Third item</li>
</ol>


- `ul`: 원형 기호가 있는 정돈되지 않은 목록을 생성합니다.
- `ol`: 숫자가 있는 정돈된 목록을 생성합니다.
- `li`: 목록의 각 항목을 나타냅니다.

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

# 11. HTML 메타데이터

메타데이터는 다른 데이터에 대한 정보를 제공하는 데이터입니다. 이는 `head` 섹션 안에 배치되며, 문자 세트, 작성자 및 페이지 설명과 같은 정보를 포함합니다:

```js
<meta charset="UTF-8">
<meta name="description" content="HTML 기초 예제">
<meta name="keywords" content="HTML, 튜토리얼, 기초">
<meta name="author" content="Saide Hossain">
```

# 12. HTML 문서 구조 요약

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

간단한 모든 기본 요소를 결합한 HTML 문서가 여기 있어요:

```js
<!DOCTYPE html>
<html>
  <head>
    <title>나의 첫 HTML 페이지</title>
    <meta charset="UTF-8">
    <meta name="description" content="HTML 기초 학습">
    <meta name="keywords" content="HTML, 기초, 튜토리얼">
    <meta name="author" content="Saide Hossain">
  </head>
  <body>
    <h1>내 웹사이트에 오신 것을 환영합니다</h1>
    <p>이것은 나의 첫 웹페이지입니다. HTML을 배우고 있어요!</p>
    <p>HTML은 배우기 쉽고 사용하기 즐거운 언어입니다.</p>
    <hr>
    <h2>내가 좋아하는 웹사이트 몇 가지:</h2>
    <ul>
      <li><a href="https://www.example.com">Example.com</a></li>
      <li><a href="https://www.anotherexample.com">Another Example</a></li>
    </ul>
    <h2>내가 좋아하는 이미지:</h2>
    <img src="https://images.pexels.com/photos/287240/pexels-photo-287240.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1" width="300" alt="아름다운 풍경">
    <hr>
    <p>문의 사항은 <a href="mailto:saide2857@gmail.com">saide2857@gmail.com</a>으로 연락해주세요</p>
  </body>
</html>
```

# 주요 포인트들

- HTML은 콘텐츠를 구조화하기 위해 태그를 사용하는 것이 중요해요.
- 제목, 문단, 목록, 링크, 이미지 등의 기본 구성 요소가 있어요.
- 모든 HTML 문서는 올바른 구조가 필요하며 `!DOCTYPE html`로 시작하고 `html`, `head`, `body` 태그 내에서 콘텐츠를 감싸야 해요.

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

이 기본 사항을 활용하여 웹 페이지 제작을 시작할 수 있어요!

출처: HTML TUTE BLOG