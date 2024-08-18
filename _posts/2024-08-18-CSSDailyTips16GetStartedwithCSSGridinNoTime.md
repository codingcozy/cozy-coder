---
title: "CSS 그리드 시작하기 신속하게 익히는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-CSSDailyTips16GetStartedwithCSSGridinNoTime_0.png"
date: 2024-08-18 10:23
ogImage:
  url: /assets/img/2024-08-18-CSSDailyTips16GetStartedwithCSSGridinNoTime_0.png
tag: Tech
originalTitle: "CSS Daily Tips 16 Get Started with CSS Grid in No Time"
link: "https://medium.com/dev-genius/css-daily-tips-16-get-started-with-css-grid-in-no-time-f6c34f57f12a"
isUpdated: false
---

![이미지](/assets/img/2024-08-18-CSSDailyTips16GetStartedwithCSSGridinNoTime_0.png)

CSS Grid은 웹 개발에 혁명을 일으키며, 복잡하고 응답성 있는 레이아웃을 쉽게 만들 수 있는 강력한 시스템을 제공합니다. 초보자든 경험있는 개발자든 CSS Grid를 사용하면 작업 흐름을 간소화하고 웹 디자인을 향상시킬 수 있습니다. 이 글에서는 CSS Grid의 기본을 안내하여 곧바로 시작할 수 있는 지식과 자신감을 제공할 것입니다.

# CSS Grid란 무엇인가요?

CSS Grid 레이아웃 또는 일반적으로 CSS Grid로 불리는 것은 복잡한 웹 레이아웃을 빠르고 쉽게 만들 수 있는 2차원 레이아웃 시스템입니다. Flexbox와 달리, 단순한 1차원 레이아웃(행 또는 열 기반)에 최적화된 것과는 달리 CSS Grid는 행과 열 모두 다룰 수 있어 복잡한 웹 디자인을 만드는 데 이상적입니다.

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

# CSS Grid의 주요 개념

- 그리드 컨테이너: 그리드 항목을 보유하는 부모 요소입니다. display: grid 또는 display: inline-grid로 정의됩니다.
- 그리드 항목: 그리드 컨테이너 내의 자식 요소들입니다.
- 그리드 라인: 그리드를 행과 열로 나누는 선들입니다.
- 그리드 트랙: 두 인접한 그리드 라인 사이의 공간으로, 본질적으로 행과 열을 나타냅니다.
- 그리드 셀: 행과 열의 교차점에 의해 형성된 그리드 내의 단일 공간입니다.

# 기본 그리드 설정

# 그리드 컨테이너 생성하기

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

그리드 컨테이너를 만들려면 부모 요소에 display: grid를 적용해야 해요.

```js
.container {
    display: grid;
}
```

# 그리드 열과 행 정의하기

그리드의 구조를 정의하려면 grid-template-columns와 grid-template-rows를 사용하세요.

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
.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr; /* 세 개의 동일한 너비 열 생성 */
    grid-template-rows: 100px 100px; /* 높이가 각각 100px인 두 개의 행 생성 */
}
```

# 예: 기본 그리드 레이아웃

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .container {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 100px 100px;
            gap: 10px;
        }
        .item {
            background-color: #3498db;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    </style>
    <title>Basic Grid Layout</title>
</head>
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
    </div>
</body>
</html>
```

해설:

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

- 기본 그리드 레이아웃: 각 그리드 항목이 셀 내에서 수직 및 수평으로 중앙 정렬된 세 개의 열과 두 개의 행으로 그리드를 생성합니다.

## 그리드 항목 위치 지정

## 그리드 라인 사용

그리드 라인은 그리드를 나누는 수평 및 수직 라인입니다. 이러한 라인을 사용하여 그리드 항목을 배치할 수 있습니다.

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
.item {
    grid-column-start: 1;
    grid-column-end: 3; /* 두 개의 열을 차지합니다 */
    grid-row-start: 1;
    grid-row-end: 3; /* 두 개의 행을 차지합니다 */
}
```

# Shorthand 속성 사용하기

코드를 더 간결하게 작성하기 위해 shorthand 속성 grid-column 및 grid-row를 사용할 수도 있습니다.

```js
.item {
    grid-column: 1 / 3; /* 두 개의 열을 차지합니다 */
    grid-row: 1 / 3; /* 두 개의 행을 차지합니다 */
}
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

# 예시: 그리드 라인을 사용한 위치 지정

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .container {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: 100px 100px;
            gap: 10px;
        }
        .item1 {
            background-color: #e74c3c;
            grid-column: 1 / 3;
            grid-row: 1 / 3;
        }
        .item2 {
            background-color: #2ecc71;
            grid-column: 3 / 5;
        }
        .item3 {
            background-color: #9b59b6;
        }
    </style>
    <title>그리드 라인을 사용한 위치 지정</title>
</head>
<body>
    <div class="container">
        <div class="item1">1</div>
        <div class="item2">2</div>
        <div class="item3">3</div>
        <div class="item4">4</div>
        <div class="item5">5</div>
        <div class="item6">6</div>
    </div>
</body>
</html>
```

설명:

- 그리드 라인을 사용한 위치 지정: 첫 번째 항목을 두 열과 두 행에 걸쳐 배치하고, 두 번째 항목은 두 열에 걸쳐 배치하며, 나머지는 개별 그리드 셀을 차지합니다.

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

# 고급 그리드 기술

# 그리드 템플릿 영역

그리드 템플릿 영역을 사용하면 그리드 레이아웃의 특정 영역에 이름을 지정하여 복잡한 레이아웃을 더 읽기 쉽고 관리하기 쉽게 만들 수 있습니다.

```js
.container {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar content content"
        "footer footer footer";
    grid-template-columns: 1fr 2fr 2fr;
    grid-template-rows: auto;
}
.header {
    grid-area: header;
    background-color: #3498db;
}
.sidebar {
    grid-area: sidebar;
    background-color: #2ecc71;
}
.content {
    grid-area: content;
    background-color: #e74c3c;
}
.footer {
    grid-area: footer;
    background-color: #9b59b6;
}
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

# 예시: 그리드 템플릿 영역

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .container {
            display: grid;
            grid-template-areas:
                "header header header"
                "sidebar content content"
                "footer footer footer";
            grid-template-columns: 1fr 2fr 2fr;
            grid-template-rows: auto;
            gap: 10px;
        }
        .header {
            grid-area: header;
            background-color: #3498db;
            color: white;
            padding: 10px;
        }
        .sidebar {
            grid-area: sidebar;
            background-color: #2ecc71;
            color: white;
            padding: 10px;
        }
        .content {
            grid-area: content;
            background-color: #e74c3c;
            color: white;
            padding: 10px;
        }
        .footer {
            grid-area: footer;
            background-color: #9b59b6;
            color: white;
            padding: 10px;
        }
    </style>
    <title>그리드 템플릿 영역</title>
</head>
<body>
    <div class="container">
        <div class="header">헤더</div>
        <div class="sidebar">사이드바</div>
        <div class="content">콘텐츠</div>
        <div class="footer">푸터</div>
    </div>
</body>
</html>
```

설명:

- 그리드 템플릿 영역: 이름이 지정된 그리드 영역을 사용하여 복잡한 레이아웃을 정의하며, CSS를 더 가독성 있고 유지보수하기 쉽게 만듭니다.

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

# 반응형 그리드 레이아웃

CSS Grid를 사용하면 화면 크기에 따라 그리드 구조를 조정하여 반응형 레이아웃을 손쉽게 만들 수 있어요.

## 예시: 반응형 그리드 레이아웃

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 10px;
        }
        .item {
            background-color: #34495e;
            color: white;
            padding: 20px;
            text-align: center;
        }
    </style>
    <title>Responsive Grid Layout</title>
</head>
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
    </div>
</body>
</html>
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

해당 내용을 친근한 톤으로 번역하겠습니다:

### 테이블 태그를 Markdown 형식으로 변경합니다.

설명:

- 반응형 그리드 레이아웃: 사용 가능한 공간에 따라 열의 수를 조정하는 반응형 그리드를 만들기 위해 repeat(auto-fill, minmax(150px, 1fr))를 사용합니다.

# 아이템 정렬과 정렬

CSS Grid는 그리드 항목을 가로 및 세로로 모두 정렬하고 정렬하는 데 사용할 수 있는 속성을 제공합니다.

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
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: 100px;
    align-items: center; /* 항목을 세로로 정렬합니다 */
    justify-items: center; /* 항목을 가로로 정렬합니다 */
}
```

# 예제: 항목 정렬 및 정렬하기

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: 100px;
            align-items: center;
            justify-items: center;
            gap: 10px;
        }
        .item {
            background-color: #2980b9;
            color: white;
            padding: 20px;
        }
    </style>
    <title>항목 정렬 및 정렬하기</title>
</head>
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
    </div>
</body>
</html>
```

해설:

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

- 항목 정렬 및 정렬: align-items와 justify-items를 사용하여 그리드 항목을 그리드 셀 내에서 가로 및 세로 중앙에 정렬합니다.

## 여기까지 읽어 주셔서 축하합니다! 만약 이 글을 좋아하셨다면 퓨나 또는 팔로우를 눌러주세요. 미래 게시물을 위해!

## 또한 매일 HTML 팁과 트릭을 업로드합니다. 제 프로필을 탐색하거나 여기에서 찾아보십시오:

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

CSS Grid은 복잡하고 반응형 웹 레이아웃을 간단하게 만들어 주는 강력한 도구입니다. 그리드 컨테이너, 그리드 항목 및 위치 지정의 기본 원리를 이해하면 동적이고 적응 가능한 디자인을 빠르게 만들 수 있습니다. 또한 그리드 템플릿 영역, 반응형 레이아웃 및 정렬 속성과 같은 고급 기술을 사용하면 CSS Grid의 능력을 최대로 발휘할 수 있습니다.

오늘 CSS Grid를 실험해 보세요. 이를 통해 웹 디자인 작업 흐름을 변화시키고 멋진 반응형 레이아웃을 만들 수 있습니다. “CSS Daily Tips” 시리즈에서 더 많은 팁과 요령을 기대해 주시고, CSS Grid의 끝없는 가능성을 계속 탐험해 보세요!
