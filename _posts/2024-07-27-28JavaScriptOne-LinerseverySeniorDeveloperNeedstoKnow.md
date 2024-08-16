---
title: "시니어 개발자가 꼭 알아야 할 28개의 JavaScript 한 줄 짜리 코드"
description: ""
coverImage: "/assets/img/2024-07-27-28JavaScriptOne-LinerseverySeniorDeveloperNeedstoKnow_0.png"
date: 2024-07-27 14:04
ogImage: 
  url: /assets/img/2024-07-27-28JavaScriptOne-LinerseverySeniorDeveloperNeedstoKnow_0.png
tag: Tech
originalTitle: "28 JavaScript One-Liners every Senior Developer Needs to Know"
link: "https://medium.com/@matemarschalko/28-javascript-one-liners-every-senior-developer-needs-to-know-e74bdedc3b3b"
isUpdated: true
---






![image](/assets/img/2024-07-27-28JavaScriptOne-LinerseverySeniorDeveloperNeedstoKnow_0.png)

# 1. 임시 변수 없이 값 교환하기

```js
let a = 1, b = 2;

[a, b] = [b, a];

// 결과: a = 2, b = 1
```

이 한 줄 코드는 임시 변수 없이 a와 b의 값을 교환하는데 배열 해체를 활용합니다. 이는 코드를 더 깔끔하고 간결하게 만드는 깔끔한 트릭입니다. [a, b] = [b, a] 구문은 오른쪽의 배열을 해체하여 그 값을 왼쪽에 할당하여 값들을 교환합니다.


<div class="content-ad"></div>

# 2. 데이터 더 쉽게 접근하기 위한 객체 비구조화

```js
const {name, age} = {name: 'John', age: 30};

// 출력: name = 'John', age = 30
```

여기서 객체 비구조화는 객체에서 이름과 나이 속성을 변수에 직접 추출하는 데 사용됩니다. 이 접근 방식은 객체 속성에 쉽게 접근하고 코드 가독성을 향상시킵니다.

# 3. 간편하게 객체 복제하기

<div class="content-ad"></div>

```js
const originalObj = {name: 'Jane', age: 22};

const clonedObj = {...originalObj};

// Output: clonedObj = {name: 'Jane', age: 22}
```

스프레드 연산자 (...)는 originalObj의 shallow clone을 만드는 데 사용됩니다. 이 기술은 원래 객체의 모든 열거 가능한 속성을 새 객체로 복사합니다.