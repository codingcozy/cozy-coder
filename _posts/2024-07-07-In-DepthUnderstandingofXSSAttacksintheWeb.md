---
title: "웹에서의 XSS 공격에 대한 심층 이해"
description: ""
coverImage: "/assets/img/2024-07-07-In-DepthUnderstandingofXSSAttacksintheWeb_0.png"
date: 2024-07-07 13:39
ogImage:
  url: /assets/img/2024-07-07-In-DepthUnderstandingofXSSAttacksintheWeb_0.png
tag: Tech
originalTitle: "In-Depth Understanding of XSS Attacks in the Web"
link: "https://medium.com/the-first-digit/in-depth-understanding-of-xss-attacks-in-the-web-a3aa91e1080a"
isUpdated: true
---

<img src="/assets/img/2024-07-07-In-DepthUnderstandingofXSSAttacksintheWeb_0.png" />

먼저, XSS 공격이 어떻게 생성되는지 살펴봅시다. HTML은 가장 일반적인 웹 언어로, 매우 유연합니다. 언제든지 HTML을 수정할 수 있습니다. 그러나 이 유연성은 해커에게 기회를 제공하기도 합니다. 악의적 입력을 제공함으로써 해커들은 악의적 JavaScript 스크립트를 브라우저에 삽입하여 개인 정보를 탈취하거나 여러분을 가장하여 행동을 수행할 수 있습니다. 바로 이것이 XSS 공격(Cross-Site Scripting)의 원리입니다.

이 외에도 Reflective XSS, DOM-Based XSS, 그리고 Persistent XSS 세 가지 유형의 XSS 공격에 대해 배워야 합니다. 각각을 좀 더 자세히 살펴보겠습니다.

# Reflective XSS

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

검색 페이지가 있습니다. 키워드를 입력하고 '검색' 버튼을 클릭하면 페이지에 '검색 결과: XXX'가 표시됩니다.

<img src="/assets/img/2024-07-07-In-DepthUnderstandingofXSSAttacksintheWeb_1.png" />

예를 들어, PHP에서 이 웹 페이지의 서버 측 구현 로직은 다음과 같습니다:

```js
<!DOCTYPE html>
<html>
  <body>
    <form role="search" action="" method="GET">
      <input type="text" name="search" placeholder="검색어를 입력하세요">
      <button type="submit">검색</button>...
```
