---
title: "JavaScript에서 변수와 타입 완벽 이해하기"
description: ""
coverImage: "/assets/img/2024-08-04-UnderstandingVariablesandTypesinJavaScript_0.png"
date: 2024-08-04 18:36
ogImage: 
  url: /assets/img/2024-08-04-UnderstandingVariablesandTypesinJavaScript_0.png
tag: Tech
originalTitle: "Understanding Variables and Types in JavaScript"
link: "https://medium.com/@eaglehead/understanding-variables-and-types-in-javascript-04b0b0aaa5ce"
---


## 원시 및 참조 유형 소개

![UnderstandingVariablesandTypesinJavaScript_0.png](/assets/img/2024-08-04-UnderstandingVariablesandTypesinJavaScript_0.png)

자바스크립트는 많은 프로그래밍 언어와 마찬가지로 변수를 기본적인 구성 요소로 사용합니다. 변수의 유형과 작동 방식을 이해하는 것은 자바스크립트를 숙달하는 데 매우 중요합니다. 이 글에서는 원시 유형과 참조 유형 사이의 차이를 살펴보고, 변수의 작동 방식을 탐구하며 이러한 개념을 설명하는 실제 예제를 제공할 것입니다.

## 변수란 무엇인가요?

<div class="content-ad"></div>

변수는 모든 프로그래밍 언어의 기초입니다. 이들은 코드 전체에서 변경하고 재사용할 수 있는 값을 저장하는 데 사용됩니다. JavaScript에서 변수는 다양한 유형의 값들을 보유할 수 있으며, 주된 목적은 참조하고 조작할 수 있는 데이터를 저장하는 것입니다.

```js
let age = 30;

// 나중에 'age'의 값을 변경할 수 있습니다:
age = 31;
```

이 과정은 변수 재할당이라고 알려져 있습니다.

## 기본 유형

<div class="content-ad"></div>

JavaScript에서는 여섯 가지 기본 데이터 유형이 있습니다:

- String: 텍스트에 사용됩니다.
- Number: 숫자에 사용됩니다(정수 및 부동 소수점 숫자 모두).
- Boolean: 참 또는 거짓을 나타냅니다.
- Null: 어떤 값이도 존재하지 않음을 의도적으로 나타냅니다.
- Undefined: 선언되었지만 아직 값이 할당되지 않은 변수를 나타냅니다.
- Symbol: ECMAScript 2015에 소개된 고유하고 변경할 수 없는 값입니다.

각 기본 유형의 예시는 다음과 같습니다:

```js
let name = "Hello world!"; // String
let isStudent = true; // Boolean
let score = 95; // Number
let data = null; // Null
let unassigned; // Undefined
let id = Symbol("id"); // Symbol
```

<div class="content-ad"></div>

## 참조 유형

자바스크립트에는 오직 하나의 참조 유형, 즉 객체가 있습니다. 이는 배열과 함수를 포함하며, 두 경우 모두 객체의 유형에 속합니다. 객체는 데이터의 집합 및 더 복잡한 엔티티를 저장하는 데 사용됩니다.

## 객체의 예시:

```js
let person = {
    name: "John",
    age: 30
};
```

<div class="content-ad"></div>

## 배열의 예시:

```js
let numbers = [1, 2, 3, 4, 5];
```

## 원시 타입이 어떻게 저장되는지

원시 값은 변수가 접근하는 위치에 직접 저장됩니다. 이는 원시 값을 변수에 할당할 때, 실제 값이 해당 변수와 관련된 메모리 공간에 저장된다는 의미입니다.

<div class="content-ad"></div>

예를 들어:

```js
let greeting = "Hello world!";
let count = 42;
let isActive = false;
```

이러한 각 값은 메모리의 고유한 위치에 저장됩니다.

## 참조 유형이 어떻게 저장되는지

<div class="content-ad"></div>

참조 유형은 다르게 저장됩니다. 객체를 변수에 할당할 때, 변수는 객체 자체가 아닌 객체가 저장된 메모리 위치를 가리키는 참조(또는 포인터)를 저장합니다.

## 예시:

```js
let car = {
    make: "Toyota",
    model: "Corolla"
};
```

여기서 car은 'make: "Toyota", model: "Corolla"' 객체를 직접 보관하지 않습니다. 대신, 이 객체가 저장된 메모리 위치를 가리키는 참조를 보관합니다.

<div class="content-ad"></div>

## 실제 예시: 변수 복사

참조 타입이 어떻게 작동하는지 설명하기 위해 객체를 복사하는 예시를 살펴보겠습니다:

```js
let original = { 
  a: 10,
  b: 25,
  c: 40,
};

let copy = original;
copy.a = 20;

console.log(original.a); // 출력: 20
```

이 예시에서 original과 copy 둘 다 메모리 내에서 동일한 객체를 가리킵니다. copy를 통해 값이 변경되면 original에도 영향을 미칩니다.

<div class="content-ad"></div>

## 결론

자바스크립트에서 기본(primitive) 타입과 참조(reference) 타입의 차이를 이해하는 것이 매우 중요합니다. 기본 타입은 변수의 메모리 위치에 직접 저장되는 반면 참조 타입은 메모리에 있는 객체의 참조를 저장합니다. 이 차이는 변수의 할당 및 조작 방식에 중요한 영향을 미칩니다.

요약하면:

- 기본 타입: 메모리에 직접 저장됨 (문자열, 숫자, 부울, null, undefined, 심볼).
- 참조 타입: 메모리 위치에 대한 참조로 저장됨 (객체, 배열, 함수).

<div class="content-ad"></div>

지금까지 자바스크립트 코드를 탐구하고 작성하는 과정에서 중요한 점을 기억해주세요. 이러한 개념을 숙달하면 응용프로그램에서 데이터를 효과적으로 관리할 수 있는 능력이 향상될 것입니다.

다음 글 "자바스크립트에서의 기본 값 유형 조사"에서는 자바스크립트의 각 기본 값 유형에 대해 더 깊이 파고들어 자세한 예제를 제공하고 특징을 탐구할 것입니다. 기대해주세요!