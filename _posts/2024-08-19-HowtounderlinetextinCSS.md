---
title: "CSS에서 텍스트에 밑줄을 넣는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-HowtounderlinetextinCSS_0.png"
date: 2024-08-19 01:43
ogImage: 
  url: /assets/img/2024-08-19-HowtounderlinetextinCSS_0.png
tag: Tech
originalTitle: "How to underline text in CSS "
link: "https://medium.com/@frontendinterviewquestions/how-to-underline-text-in-css-f5b7604d4316"
isUpdated: false
---


<img src="/assets/img/2024-08-19-HowtounderlinetextinCSS_0.png" />

더 많은 질문과 답변을 보시려면 저희 웹사이트 Frontend Interview Questions을 방문해주세요.

텍스트에 밑줄을 그어 강조하거나 중요한 콘텐츠를 강조하는 것은 웹페이지에서 일반적으로 사용되는 디자인 기술입니다. HTML `u` 태그를 사용하여 텍스트에 밑줄을 그을 수 있지만, CSS는 동일한 효과를 얻을 수 있는 더 유연하고 현대적인 방법을 제공하여 디자인에 더 잘 맞게 모양을 사용자 정의할 수 있습니다. 이 안내서에서는 CSS에서 텍스트에 밑줄을 그는 다양한 방법을 예제와 함께 안내해드립니다.

# 기본 방법: text-decoration 속성

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

가장 간단한 방법은 CSS에서 텍스트에 밑줄을 그어주는 것인데, 그 방법은 text-decoration 속성을 사용하는 것입니다. 이 속성을 사용하면 밑줄, 윗줄 및 취소선과 같은 다양한 텍스트 꾸밈을 추가할 수 있습니다.

## 구문:

```css
요소 {
    text-decoration: underline;
}
```

## 예시:

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
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS로 텍스트에 밑줄 그리기</title>
    <style>
        .underline {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <p>CSS를 사용하여 <span class="underline">밑줄이 그어진 텍스트</span>의 예시입니다.</p>
</body>
</html>
```

이 예시에서 `span` 요소 내의 텍스트는 text-decoration: underline; 규칙을 사용하여 밑줄이 그어집니다.

# 밑줄 스타일 맞춤 설정

CSS를 이용하면 밑줄의 색상, 스타일 및 위치를 변경하여 사용자 정의할 수 있습니다. 이러한 사용자 정의는 추가적인 속성을 사용하여 구현할 수 있습니다.

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

## 밑줄 색상 변경하기

기본적으로 밑줄은 텍스트의 색상을 상속받습니다. 그러나 text-decoration-color 속성을 사용하여 밑줄의 색상을 다르게 설정할 수 있습니다.

## 예시:

```js
.underline-custom-color {
    text-decoration: underline;
    text-decoration-color: red;
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

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>사용자 지정 색상으로 밑줄</title>
    <style>
        .underline-custom-color {
            text-decoration: underline;
            text-decoration-color: red;
        }
    </style>
</head>
<body>
    <p>This is an example of <span class="underline-custom-color">underlined text with a custom color</span> using CSS.</p>
</body>
</html>
```

이 예제에서는 밑줄이 빨간색이며 텍스트의 원래 색상을 유지합니다.

# 밑줄 스타일 변경

또한 text-decoration-style 속성을 사용하여 밑줄 스타일을 변경할 수 있습니다. 이 속성을 사용하면 솔리드, 점선, 대시, 물결, 이중 등 여러 가지 밑줄 스타일 중에서 선택할 수 있습니다.

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

## 예시:

```js
.underline-dashed {
    text-decoration: underline;
    text-decoration-style: dashed;
}
```

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashed Underline</title>
    <style>
        .underline-dashed {
            text-decoration: underline;
            text-decoration-style: dashed;
        }
    </style>
</head>
<body>
    <p>This is an example of <span class="underline-dashed">dashed underlined text</span> using CSS.</p>
</body>
</html>
```

여기에서 텍스트는 대시로 밑줄이 그어져 다른 시각적 효과를 제공합니다.

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

# 보다 세밀한 제어를 위해 border-bottom 사용하기

밑줄의 모양을 더 세밀하게 제어하려면 text-decoration 대신 border-bottom 속성을 사용할 수 있습니다. 이 방법을 사용하면 밑줄의 두께, 색상 및 위치를 더 정확하게 조절할 수 있습니다.

## 예시:

CSS:

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
.custom-underline {
    border-bottom: 2px solid blue;
    display: inline;
}
```

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom Underline with Border-Bottom</title>
    <style>
        .custom-underline {
            border-bottom: 2px solid blue;
            display: inline;
        }
    </style>
</head>
<body>
    <p>이것은 CSS의 border-bottom 속성을 사용한 <span class="custom-underline">사용자 정의 밑줄 텍스트의 예시</span>입니다.</p>
</body>
</html>
```

이 예시에서는 border-bottom 속성을 사용하여 밑줄을 만들었습니다. 두께를 2px로 설정하고 색상을 파란색으로 지정할 수 있습니다. 이 방법은 밑줄의 모양을 더 세밀하게 제어해야 할 때 유용합니다.

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

CSS에서 텍스트에 밑줄을 넣는 것은 여러 방법으로 간단하게 수행할 수 있습니다. 디자인 요구에 따라 방법이 다양합니다. text-decoration 속성이 가장 간단한 방법입니다. 그러나 더 많은 사용자 정의를 위해 밑줄의 색상, 스타일을 조정하거나 더 많은 제어를 위해 border-bottom 속성을 사용할 수도 있습니다. 이러한 기술을 이해하면 웹페이지의 디자인에 맞는 완벽한 밑줄 효과를 적용할 수 있습니다.

고지: 여기에는 비용이 발생하지 않는 이상 제휴 링크가 포함되어 있습니다. 지원해 주셔서 감사합니다!

여기 구매하세요:- Angular에 대한 실용적이고 포괄적인 안내서