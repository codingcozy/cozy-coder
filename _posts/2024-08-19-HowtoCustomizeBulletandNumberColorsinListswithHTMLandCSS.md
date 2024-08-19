---
title: "HTML과 CSS를 사용하여 목록 불릿과 번호 색상을 커스텀하는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-HowtoCustomizeBulletandNumberColorsinListswithHTMLandCSS_0.png"
date: 2024-08-19 01:52
ogImage: 
  url: /assets/img/2024-08-19-HowtoCustomizeBulletandNumberColorsinListswithHTMLandCSS_0.png
tag: Tech
originalTitle: "How to Customize Bullet and Number Colors in Lists with HTML and CSS"
link: "https://medium.com/@alfonsoramos.1803/how-to-customize-bullet-and-number-colors-in-lists-with-html-and-css-2d533134356a"
isUpdated: true
updatedAt: 1724033307315
---


웹 디자인에서 목록은 정보를 명확하고 효과적으로 구성할 수 있는 기본 요소입니다. 그러나 때로는 사이트 디자인에 더 잘 어울리도록 모양을 맞추어야 할 필요가 있습니다. 이 글에서는 HTML과 CSS만을 사용하여 순서 없는 목록(`ul`)의 점의 색상과 순서 있는 목록(`ol`)의 숫자의 색상을 변경하는 방법을 안내해 드리겠습니다.

# 간단한 해결책

처음에는 복잡해보일 수 있지만, 목록의 점과 숫자의 색상을 사용자 정의하는 것은 ::marker 가상 요소 덕분에 간단합니다. 이 가상 요소를 사용하면 목록의 마커에 특정 스타일을 적용할 수 있습니다. 점, 숫자 또는 다른 기호인지에 관계없이 목록의 마커에 특정 스타일을 적용할 수 있습니다.

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

# 코드 예시

이 사용자 정의를 구현하는 데 필요한 코드는 다음과 같습니다:

# HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lists with Custom Colors</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h2>Unordered List</h2>
    <ul class="custom-ul">
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ul>

    <h2>Ordered List</h2>
    <ol class="custom-ol">
        <li>Item A</li>
        <li>Item B</li>
        <li>Item C</li>
    </ol>
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

# CSS

```js
/* 순서 없는 목록의 불릿 색상 변경 */
.custom-ul {
    list-style-type: disc; /* 불릿을 사용하도록 설정 */
}

.custom-ul li::marker {
    color: blue; /* 불릿의 색상 변경 */
}

/* 순서 있는 목록의 숫자 색상 변경 */
.custom-ol {
    list-style-type: decimal; /* 숫자를 사용하도록 설정 */
}

.custom-ol li::marker {
    color: red; /* 순서 있는 목록의 숫자 색상 변경 */
}
```

# 코드 설명

- ::marker 가상 요소: 이 가상 요소는 목록 (`ul` 및 `ol`)의 마커를 선택하고 스타일을 적용하는 데 사용됩니다.
- `ul`에서 불릿 사용자 정의: .custom-ul li::marker 에서 'color: blue;'를 사용하여 순서 없는 목록의 불릿 색상을 파란색으로 지정합니다.
- `ol`에서 숫자 사용자 정의: .custom-ol li::marker 에서 'color: red;'를 사용하여 순서 있는 목록의 숫자 색상을 빨간색으로 변경합니다.

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

# 결론

HTML 및 CSS를 사용하여 목록의 불릿과 번호의 색상을 사용자 정의하는 것은 생각보다 쉽습니다. ::marker 가상 요소를 사용하면 목록의 모양을 간단하고 효과적으로 수정하여 웹사이트의 스타일에 더 적합하게 만들 수 있습니다. 이 해결책이 도움이 되었으면 좋겠습니다!