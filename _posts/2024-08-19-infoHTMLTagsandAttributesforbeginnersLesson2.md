---
title: "HTML 태그와 속성 설명  초보자를 위한 레슨 2"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 01:53
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "infoHTML Tags and Attributes for beginners  Lesson 2"
link: "https://medium.com/@jvcodes/html-tags-and-attributes-for-beginners-lesson-2-44e88b1d485e"
isUpdated: false
---


HTML은 웹의 기본 요소이며 제목, 단락, 목록, 링크, 인용 부분 등 텍스트 구조를 통해 웹 문서를 구성하는 데 사용됩니다.

소스 코드 다운로드

HTML은 가장 쉬운 프로그래밍 언어로, 개발자가 배워야 할 첫 번째 언어입니다. 이 강의에서는 모든 웹 디자이너에게 중요한 기본 태그와 속성만 설명합니다.

이 글의 끝에는 이러한 태그와 속성을 적용하여 초보적인 웹 페이지를 만들 수 있는 위치에 있게 될 것입니다.

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

# HTML 태그와 속성이란?

HTML에서 태그는 HTML 문서 내에서 객체를 만드는 데 사용됩니다. 이러한 태그는 꺾쇠 괄호 ` ` 안에 포함됩니다. 예를 들어 `p` 태그는 텍스트 단락을 정의하는 데 사용될 수 있습니다. 대부분의 HTML 태그는 여는 태그(예: `p`)와 닫는 태그(예: `/p`)로 구성된 쌍으로 이루어져 있습니다. 그러나 닫는 태그를 필요로하지 않는 Self-closing 태그도 있는데, 예를 들어 `br` 태그가 있습니다.

속성은 기본 HTML 태그에 대한 확장으로, 태그와 관련된 더 구체적인 세부 정보를 제공합니다. 속성은 항상 여는 태그에 포함되며 이름/값 쌍으로 구성되어야 합니다. 예를 들어 `img` 태그에서 사용되는 'src'는 해당 이미지의 소스를 나타냅니다.

이제 HTML 초급 과정에서 주로 다루게 될 HTML 태그와 속성을 함께 살펴보겠습니다.

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

# 태그 설명 목록

- 단락 태그
- 목록 태그
- 제목 태그
- 링크 태그
- 이미지 태그
- 줄 바꿈 태그
- 굵게, 기울임, 밑줄 태그
- 큰 글씨와 작은 글씨 태그
- 수평선 태그
- 아래 첨자 및 위 첨자 태그
- 미리 서식 지정된 태그

# 단락 태그 (`p`)

'`p`' 태그는 텍스트 단락을 정의하는 데 사용됩니다. 어떤 워드 처리 프로그램에서 입력하는 다른 단락에 비해 위아래로 약간의 공간을 추가합니다.

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
<p>이것은 텍스트 단락입니다.</p>
```

속성들

align: 단락 텍스트의 정렬을 지정합니다. 가능한 값은 left, right, center, justify입니다.

속성을 사용한 예제

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
<p style="text-align: centre;">이 단락은 가운데 정렬됩니다.</p>
<p style="text-align: left;">이 단락은 가운데 정렬됩니다.</p>
<p style="text-align: right;">이 단락은 가운데 정렬됩니다.</p>
<p style="text-align: justify;">이 단락은 가운데 정렬됩니다.</p>
```

# 리스트 태그 (`li`, `ol`, `ul`)

리스트는 관련 항목을 그룹화하는 데 사용됩니다. HTML은 순서가 지정된 목록 (`ol`)과 정렬되지 않은 목록 (`ul`)을 지원합니다. 목록 항목은 `li` 태그를 사용하여 정의됩니다.

정렬되지 않은 목록 예제

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
* Item 1
* Item 2
* Item 3
```

Ordered list example

```js
1. First item
2. Second item
3. Third item
```

# Heading Tag (`h1` to `h6`)

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

가장 일반적으로 사용되는 태그는 제목 태그입니다. 이 태그들은 콘텐츠 내에서 순서를 정리하고 SEO를 위해 도움이 되는 제목을 만드는 데 사용됩니다. 일반적인 제목 또는 부제목 계층 구조가 `h1`부터 `h6`까지 있으며, `h1`이 가장 중요합니다.

```js
# 주요 제목
## 부제목
### 서브 부제목
#### 제목
##### 제목
###### 제목
```

## 링크 태그 (`a`)

가장 중요한 것 중 하나는 하이퍼링크를 만드는 데 사용되는 `a` 태그입니다. `a` 태그의 경우 유일하게 필수 매개변수는 링크의 위치를 나타내는 href입니다.

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
[방문하기](https://www.example.com)
```

속성

- href: 링크가 가리키는 페이지의 위치 또는 URL(Uniform Resource Locator)을 제공합니다.
- target: 링크된 문서가 열리는 위치를 명시합니다. _blank는 새 탭에서 문서가 열립니다.

속성과 함께 예시

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


[새 탭에서 예시 열기](https://www.example.com){:target="_blank"}


# 이미지 태그 (`img`)

Html 문서에 이미지를 삽입하는 방법은 `img` 태그를 사용하는 것입니다. 이 태그는 입력된 후 자동으로 HTML 태그를 닫습니다.


![이미지 설명](image.jpg)


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

속성

- src: 이미지의 경로를 제공합니다.
- alt: 이미지를 설명하는 텍스트를 제공하거나 이미지가 작동하지 않을 때 이미지의 대체물을 제공합니다.
- width: 이미지의 가로 크기를 넓이를 표시하여 제공합니다.
- height: 이미지의 높이를 설정합니다.

속성을 사용한 예시

```js
<img src="image.jpg" alt="이미지 설명" width="500" height="300">
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

# 줄 바꿈 태그 (`br`)

이 태그는 텍스트 내에서 줄 바꿈을 생성하는 데 사용됩니다. 텍스트의 계속이 다음 줄에서 시작됩니다. 다음 새 줄을 입력하면 자동으로 닫히는 태그입니다.

```js
이것은 줄 바꿈입니다<br>새 줄이 여기서 시작됩니다.
```

# 굵은 글꼴, 이탤릭체, 밑줄 태그 (`b`, `i`, `u`)

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

이것들은 텍스트에 스타일을 적용하는 데 사용되는 태그들이에요.

```js
**진한 텍스트**
*기울임꼴 텍스트*
<ins>밑줄 텍스트</ins>
```

# 크고 작은 태그 (`big`, `small`)

Big 태그는 텍스트의 글꼴 크기를 증가시키고 Small 태그는 텍스트의 글꼴 크기를 줄입니다.

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
<big>This text is big.</big>
<small>This text is small.</small>
```

# 수평선 태그 (`hr`)

`hr` 태그는 콘텐츠를 구분하는 데 사용할 수있는 수평 선을 삽입하는 데 사용됩니다.

```js
<p>첫 번째 단락.</p>
<hr>
<p>두 번째 단락.</p>
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

# 아래첨자 및 윗첨자 태그 (`sub`, `sup`)

이것들은 각각 아래첨자 위치와 윗첨자 위치에 쓰여진 스크립트를 정의하는 데 사용되는 태그들입니다.

```js
H<sub>2</sub>O (물)
E=mc<sup>2</sup> (아인슈타인의 방정식)
```

# 서식이 적용된 텍스트 태그 (`pre`)

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

`pre`태그는 텍스트를 정렬하여 Notepad에 입력한 대로 보존하고 보이게합니다.

```js
<pre>
  이것은 서식이 있는 텍스트입니다.
    들여쓰기가 보존됩니다.
</pre>
```

# 소스 코드 다운로드

간편함을 위해, 이 문서에서 설명하는 모든 예시 코드들은 무료로 다운로드할 수 있습니다. 이 튜토리얼에서 설명하는 모든 단계의 소스 코드를 다운로드하려면 아래 버튼을 클릭하세요.