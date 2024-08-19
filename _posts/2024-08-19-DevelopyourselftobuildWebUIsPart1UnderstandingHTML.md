---
title: "웹 UI를 만들기 위해 알아둬야할 HTML"
description: ""
coverImage: "/assets/img/2024-08-19-DevelopyourselftobuildWebUIsPart1UnderstandingHTML_0.png"
date: 2024-08-19 01:58
ogImage: 
  url: /assets/img/2024-08-19-DevelopyourselftobuildWebUIsPart1UnderstandingHTML_0.png
tag: Tech
originalTitle: "Develop yourself to build Web UIs Part 1  Understanding HTML"
link: "https://medium.com/@marinalabs/develop-yourself-to-build-web-uis-part-1-understanding-html-41f1498baabf"
isUpdated: true
updatedAt: 1724033266206
---


웹 개발은 오늘날 가장 필수적인 기술 중 하나입니다. 이는 브라우저를 통해 접근할 수 있는 사용자 친화적이고 매력적인 웹사이트를 만드는 것을 포함합니다. 웹 개발자가 되기 위한 첫 번째 단계는 HTML을 이해하는 것입니다.

![HTML 이미지](/assets/img/2024-08-19-DevelopyourselftobuildWebUIsPart1UnderstandingHTML_0.png)

HTML (하이퍼텍스트 마크업 언어)은 모든 웹 페이지의 기초입니다. 이는 웹페이지의 구조를 결정하고 콘텐츠가 브라우저에서 어떻게 표시되는지를 결정하는 표준 언어입니다. 페이지의 외관은 CSS(계단식 스타일 시트)에 의해 결정되고 기능은 JS(자바스크립트)에 의해 담당하지만, HTML은 기본 뼈대나 구조를 담당합니다.

본 강좌의 이 부분에 진입하기 전에, 여정에서 사용될 유명하고 반복되는 용어를 이해하는 것이 중요합니다. 이는 진행함에 따라 개념을 이해하는 데 도움이 될 뿐만 아니라, 저자가 설명을 쉽게 하도록 도와줍니다 ;-)

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

## 용어 이해하기

- 프로그래밍 언어: 컴퓨터가 실행할 수 있는 특정 구문(프로그래밍 언어의 방식)으로 작성된 지시 사항의 모음입니다. 컴퓨터는 오직 이진 코드(1 또는 0)만을 이해한다는 점을 기억하세요. 우리(인간)는 컴퓨터가 논리를 이해하고, 또한 트레이드 오프를 찾기 위해 프로그래밍 언어를 만들었습니다. 즉, 우리가 코딩하기 쉽고, 컴퓨터가 이해하기 쉽게 만든 것입니다.
- 컴파일러: 프로그래밍 언어로 작성된 코드를 컴퓨터가 이해하고 실행할 수 있는 기계어로 번역하는 도구입니다.
- 구문: 프로그래밍 언어의 구조를 정의하는 규칙입니다. 문장에서 단어가 배열되는 방식으로 생각해 보세요.
- 주석: 코드 내에 있는 코드 일부가 무엇을 하는지 설명하는 메모입니다. 주석은 다른 개발자(또는 미래의 본인)가 코드 논리를 이해하는 데 도움이 됩니다.
- DOM (문서 객체 모델): DOM은 HTML 문서의 트리 형태 표현입니다. HTML의 각 태그는 이 트리에서 노드가 됩니다. 예를 들어, HTML에 `body` 태그와 `p` (문단)이 포함된 경우, 브라우저는 본문 노드를 만들고 이를 자식으로 가지는 단락 노드를 생성합니다.
- 자식: 진행하면서 이를 이해할 것입니다. 다른 요소 내에 중첩된 요소입니다. 예를 들어, HTML에서 `div` 태그 내에 있는 단락 태그(`p`)는 div의 자식으로 간주됩니다.
- 블록 수준 요소: 진행하면 소개될 것입니다. 일반적으로 요소가 전체 사용 가능 폭을 차지할 기능을 설명하는 용어입니다. 

## HTML로 활기차게 시작하기

HTML은 Hyper Text Markup Language의 약자입니다.

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

- 하이퍼텍스트: HTML의 다른 문서를 연결하는 능력을 가리킵니다.
- 마크업 언어: 텍스트를 주석 처리하기 위해 태그를 사용하며, 브라우저에서 어떻게 표시해야 하는지 정의합니다.

다음은 HTML 문서의 기본 구조입니다:

```js
<!DOCTYPE html>
<html>
  <head>
    <title>HTML 튜토리얼</title>
  </head>
  <body>
    <p>안녕, 세상!</p>
  </body>
</html>
```

- 태그: HTML에서 태그는 요소를 정의하는 데 사용됩니다. 태그는 `<html>`이나 `<p>`와 같이 꺾쇠 괄호로 둘러싸여 있습니다.
- 요소: 여는 태그와 닫는 태그로 구성되며 내용을 포함할 수 있습니다. 예를 들어, `<p>안녕, 세상!</p>`은 문단 요소입니다.

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

## HTML 문서 구조

모든 HTML 문서는 기본적인 구조를 따릅니다:

- `!DOCTYPE html`: 문서 유형 및 HTML 버전을 선언합니다.
- `html`: 다른 HTML 요소를 포함하는 루트 요소입니다.
- `head`: 문서에 대한 메타 정보를 포함하며, 제목 및 스타일시트 링크를 담고 있습니다.
- `title`: 웹페이지의 제목을 설정하며, 브라우저의 제목 표시줄이나 탭에 표시됩니다.
- `meta`: 문자 집합, 저자 및 뷰포트 설정과 같은 HTML 문서에 대한 메타데이터를 제공합니다. 자체 종료 태그입니다.
- `style`: HTML 요소를 스타일링하는 CSS 코드를 삽입합니다.
- `script`: 웹페이지에 상호 작용을 추가하기 위한 JavaScript 코드를 삽입합니다.
- `body`: 웹페이지에서 사용자에게 보이는 콘텐츠를 감싸는 요소입니다.

## 자주 사용되는 HTML 요소

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

많이 사용하는 기본 HTML 요소들을 소개해드릴게요:

- `p``/p`: 단락을 정의합니다.
- `div``/div`: 다른 요소들을 그룹화할 때 사용하는 블록 수준 요소입니다.
- `span``/span`: 스타일링을 위해 텍스트를 그룹화하는 인라인 요소입니다.
- `header``/header`: 섹션의 소개 콘텐츠나 내비게이션 링크를 나타냅니다.
- `h1``/h1` ~ `h6``/h6`: `h1`이 가장 높은 수준이고 `h6`이 가장 낮은 수준인 헤딩입니다.
- `br`: 줄 바꿈을 삽입합니다 (닫는 태그가 필요없는 self-closing tag입니다).
- `form``/form`: 사용자 입력을 위한 HTML 폼을 만드는 데 사용됩니다.
- `input`: 일반적으로 폼 내에서 사용되는 입력 필드를 생성합니다.
- `select``/select`: 드롭다운 리스트를 생성합니다.
- `label``/label`: 폼 요소에 텍스트 라벨을 연결합니다.
- `table``/table`: 테이블을 정의합니다.
- `tr``/tr`: 테이블 내의 한 행을 정의합니다.
- `td``/td`: 테이블 행 내의 셀을 정의합니다.
- `th``/th`: 테이블 행 내의 헤더 셀을 정의합니다.
- `ul``/ul`: 정렬되지 않은 (글머리 기호가 있는) 목록을 정의합니다.
- `ol``/ol`: 순서대로 번호가 매겨진 목록을 정의합니다.
- `li``/li`: 목록 항목을 정의합니다.

## 첫 HTML 파일 만들기

HTML 파일을 만들기 위해서는 메모장이나 VS Code와 같은 텍스트 편집기를 사용하시면 됩니다. 간단한 예시를 들겠습니다:

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

- 텍스트 편집기를 열고 다음 코드를 입력하세요:

```js
<!DOCTYPE html>
<html>
<head>
  <title>HTML Tutorial</title>
</head>
<body>
  <h1>Example Number 1</h1>
  <p>Hello, world!</p>
</body>
</html>
```

- 파일을 .html 확장자로 저장하세요 (예: index.html)
- 웹 브라우저에서 파일을 열어 첫 번째 HTML 웹페이지를 확인해보세요!
- 코드를 검사하려면 Google Chrome에서 Ctrl + Shift + C를 눌러 개발자 도구를 열고 DOM 구조를 탐색하세요.
- 개발자 도구의 네트워크 탭으로 이동하여 브라우저 탭을 새로고침해주세요.

사진에서 저장한 이름으로 요청이 있는 것을 확인할 수 있습니다.

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

아래는 당신이 작성한 코드를 찾을 수 있는 응답 탭입니다.

이제 파일을 열면 저장한 HTML 파일을 실행하기 시작했고, 컴퓨터는 브라우저에서 파일을 실행시키고 싶어했습니다. 브라우저가 보여줄 것이 필요했고, 파일이 실행된 위치에서 브라우저에게 요청을 보냈습니다. 파일은 브라우저에 코드를 제공했고, 이것이 응답 섹션에 발견됐습니다. HTML 파일이었기 때문에, 브라우저는 위에서 아래로 HTML 코드를 읽기 시작했습니다. 이 과정을 파싱이라고 합니다. 파싱 중에 브라우저는 다른 HTML 태그(예: `html`, `head`, `body` 등)를 만나며 이 태그들을 기반으로 하는 DOM이라는 구조를 만들기 시작합니다. 브라우저가 DOM을 구축하는 동안 화면에 내용을 렌더링하게 됩니다.

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

## 표 만들기

이번에는 HTML에서 간단한 표를 만들어 보겠습니다:

- 동일한 HTML 파일을 열고 `body` 태그 안에 다음 코드를 추가하세요:

```js
<p>표 예제</p>
| Name   | Power     | Is Kurama Present |
|--------|-----------|-------------------|
| Naruto | Rasengan  | Yes               |
| Sasuke | Sharingan | No                |
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

- 파일을 저장하고 브라우저를 새로 고침하여 표가 표시되는지 확인하세요.

표 제목이 문단 태그에 의해 렌더링되는 것을 알 수 있습니다. 대신 `caption` 태그를 사용할 수도 있는데, 이는 표의 제목을 가운데 정렬합니다. `caption` 태그를 사용해보고 변경을 확인하려면 새로고침하세요.

`caption` 태그는 `table` 시작 태그 바로 뒤에 사용되어야 합니다.

이제 HTML에서 기본 표를 성공적으로 만들었습니다. HTML 구문에 더 익숙해지기 위해 추가적인 행과 열에 대해 실험해 보세요.

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

## 마무리

HTML로 웹 개발의 첫걸음을 밟으셨군요! HTML을 마스터하는 비결은 연습입니다. 다양한 요소들을 실험하고, 자신만의 웹페이지를 만들어보세요. 실수를 두려워하지 마세요 — 모든 에러는 학습의 기회입니다.

기억하세요, 이것은 시작에 불과합니다. 이 기초 위에 더 쌓아나가면 곧 더 복잡하고 동적인 웹사이트를 만들 수 있을 겁니다. 우리 함께 웹을 더 나은 곳으로 만들어봅시다. 한 줄의 코드씩.

본 문서는 IT와 기계 공학 분야에서 경험을 쌓은 전문가 Anantha Krishnan이 작성했습니다. 풀스택 개발에 대한 배경과 기계 및 전기 시스템에 대한 열정을 갖고 있는 Anantha Krishnan은 이제 자신의 전문 분야에서 초보자들을 돕기 위한 교육 콘텐츠 작성에 주력하고 있습니다.