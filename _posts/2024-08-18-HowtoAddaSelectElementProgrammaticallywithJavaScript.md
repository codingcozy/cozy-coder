---
title: "자바스크립트로 프로그래밍적으로 셀렉트 요소를 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-HowtoAddaSelectElementProgrammaticallywithJavaScript_0.png"
date: 2024-08-18 10:46
ogImage: 
  url: /assets/img/2024-08-18-HowtoAddaSelectElementProgrammaticallywithJavaScript_0.png
tag: Tech
originalTitle: "How to Add a Select Element Programmatically with JavaScript"
link: "https://medium.com/javascript-in-plain-english/how-to-add-a-select-element-programmatically-with-javascript-78b3b7ae3246"
isUpdated: false
---


<img src="/assets/img/2024-08-18-HowtoAddaSelectElementProgrammaticallywithJavaScript_0.png" />

가끔은 JavaScript를 사용하여 프로그래밍 방식으로 select 요소를 추가하고 싶을 때가 있습니다.

이 문서에서는 JavaScript를 사용하여 프로그래밍 방식으로 select 요소를 추가하는 방법을 살펴보겠습니다.

# JavaScript를 사용하여 프로그래밍 방식으로 Select 요소 추가하기

<div class="content-ad"></div>

자바스크립트를 사용하여 프로그래밍적으로 선택 요소를 추가하려면, document.createElement 메소드를 사용하여 요소를 생성할 수 있어요.

그런 다음 appendChild 메소드를 사용하여 부모 요소에 자식 요소를 추가할 수 있어요.

예를 들어, 다음과 같은 HTML을 작성할 수 있어요:

```js
<div>
</div>
```

<div class="content-ad"></div>

선택 요소에 상위 요소를 추가합니다.

그런 다음 다음과 같이 옵션 요소가 포함된 선택 요소를 추가할 수 있습니다:

```js
const myParent = document.querySelector('div');
const array = ["Volvo", "Saab", "Mercades", "Audi"];
const selectList = document.createElement("select");
selectList.id = "mySelect";
myParent.appendChild(selectList);
for (const a of array) {
  const option = document.createElement("option");
  option.value = a;
  option.text = a;
  selectList.appendChild(option);
}
```

다음과 같이 div 요소를 가져옵니다:

<div class="content-ad"></div>

```js
const myParent = document.querySelector('div');
```

이후 선택 요소를 생성합니다:

```js
const selectList = document.createElement("select");
```

그런 다음 선택 요소의 id 속성을 설정합니다:

<div class="content-ad"></div>

```js
selectList.id = "mySelect";
```

다음으로, 우리는 select 요소를 다음과 같이 div의 자식으로 추가합니다:

```js
myParent.appendChild(selectList);
```

그런 다음, 다음과 같이 select 요소에 옵션 요소를 추가합니다:

<div class="content-ad"></div>

```js
for (const a of array) {
  const option = document.createElement("option");
  option.value = a;
  option.text = a;
  selectList.appendChild(option);
}
```

우리는 for-of 루프를 사용하여 배열을 반복합니다.

루프 내에서 `option`을 사용하여 option 요소를 만드는 document.createElement을 호출합니다.

option.value를 설정하여 option 요소의 value 속성을 설정합니다.

<div class="content-ad"></div>

그리고 우리는 옵션 텍스트를 표시하기 위해 텍스트 속성을 설정합니다.

다음으로, selectList.appendChild를 호출하여 옵션을 선택 요소의 자식으로 추가합니다.

# 결론

JavaScript로 프로그래밍적으로 선택 요소를 추가하려면 document.createElement 메서드를 사용하여 요소를 생성할 수 있습니다.

<div class="content-ad"></div>

그럼 appendChild 메서드를 사용하여 자식 요소를 부모 요소에 추가할 수 있어요.