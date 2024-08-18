---
title: "프론트엔드 초보 개발자를 위한 JavaScript JSON 정리"
description: ""
coverImage: "/assets/img/2024-07-28-AllAboutJavaScriptJSONforBeginners_0.png"
date: 2024-07-28 14:07
ogImage:
  url: /assets/img/2024-07-28-AllAboutJavaScriptJSONforBeginners_0.png
tag: Tech
originalTitle: "All About JavaScript JSON for Beginners"
link: "https://medium.com/@sarahisdevs/all-about-javascript-json-for-beginners-72346b9d5063"
isUpdated: true
---

<img src="/assets/img/2024-07-28-AllAboutJavaScriptJSONforBeginners_0.png" />

안녕하세요 독자👋 혹시 잘 지내고 있나요?😊

이전 글에서는 DOM 인터페이스의 복잡성에 대해 살펴보았어요. 오늘은 JavaScript JSON의 세계로 뛰어들어볼까요? 준비하시라구요!🔥

## JavaScript JSON이란 무엇인가요?🤔

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

JSON은 JavaScript Object Notation의 약자로, 사람이 쉽게 읽고 기계가 처리하기 쉬운 가벼운 데이터 교환 형식입니다. 간단히 말해, 데이터 저장과 전송에 사용되는 텍스트 기반 형식입니다.

예를 들어보겠습니다:

```js
{
    "name": "John Doe",
    "age": 30,
    "city": "New York"
}
```

JSON은 컴퓨터 간에 데이터를 효율적으로 전송하는 데 중요합니다.

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

주요 JSON 구조는 다음과 같습니다:

- 이름/값 쌍의 컬렉션
- 값들의 순서가 있는 목록

## JSON을 사용하는 이유?🤷‍♂️

변수에 값을 할당할 때 변수는 실제 값이 저장된 메모리 위치를 가리키는 것이 아니라, 실제 값을 보유하지 않습니다. 데이터 전송 중에는 이 메모리 위치를 공유해야 하는데, 이는 비실용적이고 위험합니다. JSON은 데이터를 표준화된 인간이 읽을 수 있는 형식으로 직렬화하여 시스템과 플랫폼 간에 원활한 통신을 위해 이러한 딜레마를 해결합니다.
