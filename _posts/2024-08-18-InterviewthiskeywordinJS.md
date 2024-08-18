---
title: "자바스크립트에서의 this 란 무엇일까"
description: ""
coverImage: "/assets/img/2024-08-18-InterviewthiskeywordinJS_0.png"
date: 2024-08-18 11:00
ogImage: 
  url: /assets/img/2024-08-18-InterviewthiskeywordinJS_0.png
tag: Tech
originalTitle: "Interview this keyword in JS"
link: "https://medium.com/@opensrc0/interview-this-keyword-in-js-f6b015d69c4c"
isUpdated: true
updatedAt: 1723951533251
---

![이미지](/assets/img/2024-08-18-InterviewthiskeywordinJS_0.png)

이 글에서는 자바스크립트 함수 내의 this 키워드에 대해 이야기합니다. 함수는 호출될 때 항상 객체를 참조합니다.

객체 없이 직접 호출된 함수는 전역 객체로 간주됩니다. 즉, window 객체입니다.

this 키워드의 어려운 문제들을 해결해 봅시다. 궁금한 점 있으면 언제든지 댓글 남겨주세요. 빠른 대답해 드릴게요.

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

예제로 이해해봅시다.

## 1. 질문:

```js
let emp = {
  name: "John",
  getName: function () {
    console.log(this);
  },
};

emp.getName();
```

## 답변:

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
{name: 'John', getName: ƒ} // emp object
```

## 설명:

getName 함수가 emp 객체에서 호출되었기 때문에 이것은 emp 객체 자체입니다.

## 2. 질문:

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
function foo() {
  console.log(this);
}

foo();
```

## 답변:

```js
window; // window 객체
```

## 설명:

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
foo() === window.foo();
```

함수를 객체 없이 호출할 때는 `window` 객체에 의해 호출된 것을 의미하며, 이러면 `foo` 안에서 `this` 키워드는 `window`를 가리킵니다.

## 3. 질문

```js
function foo() {
  console.log(this);

  function bar() {
    console.log(this);
  }

  bar();
}

foo();
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

## Answer:

```js
window;
window;
```

헷갈리시면 설명해 드릴게요.

## 설명:

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
function foo() {
  console.log(this);

  function bar() {
    console.log(this);
  }

  window.bar(); // window
}

window.foo(); // window
```

foo와 bar를 호출할 때 window를 추가했는데, window없이 호출할 때와 동일합니다.

우리가 논의한 대로, this는 함수를 호출한 객체입니다.

어디에서 호출했는지 중요하지 않습니다. 호출한 객체에 의해 결정됩니다.

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

인터뷰-2: 자바스크립트에서 값에 의한 호출 및 참조에 대한 대화

인터뷰-3: 당신은 자바스크립트에서 이 키워드를 읽고 있습니다

인터뷰-4: 자바스크립트에서 호이스팅 및 데드 존
