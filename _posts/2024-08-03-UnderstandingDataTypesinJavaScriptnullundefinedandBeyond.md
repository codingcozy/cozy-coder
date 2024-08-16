---
title: "JavaScript 데이터 타입 완전 정복 null, undefined 및 그 이상"
description: ""
coverImage: "/assets/img/2024-08-03-UnderstandingDataTypesinJavaScriptnullundefinedandBeyond_0.png"
date: 2024-08-03 18:30
ogImage: 
  url: /assets/img/2024-08-03-UnderstandingDataTypesinJavaScriptnullundefinedandBeyond_0.png
tag: Tech
originalTitle: "Understanding Data Types in JavaScript null, undefined, and Beyond"
link: "https://medium.com/@maciejpoppek/understanding-data-types-in-javascript-null-undefined-and-beyond-f3674995c9f8"
isUpdated: true
---





![image](/assets/img/2024-08-03-UnderstandingDataTypesinJavaScriptnullundefinedandBeyond_0.png)

자바스크립트는 동적 타이핑과 유연성으로 유명한 다재다능한 프로그래밍 언어입니다. 다양한 데이터 유형을 이해하는 것은 효과적인 코드 작성에 중요합니다. 이 글에서는 자바스크립트에서의 null, undefined 및 기타 데이터 유형에 대해 탐구하며, 이러한 차이점, 사용 사례 및 동작을 살펴볼 것입니다.

# 자바스크립트의 기본 데이터 유형

자바스크립트에는 두 개의 범주로 분류할 수 있는 일곱 가지 주요 데이터 유형이 있습니다: 원시 타입(primitive types)과 객체(objects).

<div class="content-ad"></div>

## 1. 기본 유형

JavaScript에서 가장 기본적인 데이터 유형입니다. 다음을 포함합니다:

1.1 숫자: 정수와 부동 소수점 숫자를 모두 표현합니다.

```js
let age: number = 25;
let temperature: number = 36.6;
```

<div class="content-ad"></div>

1.2 문자열: 문자의 일련의 시퀀스를 나타냅니다.

```js
let name: string = "John Doe";
```

1.3 불리언: true 또는 false와 같은 논리적인 값을 나타냅니다.

```js
let isStudent: boolean = true;
```

<div class="content-ad"></div>

1.4 Null: 어떤 객체 값도 없음을 나타냅니다.

```js
let value: null = null;
```

1.5 Undefined: 값이 할당되지 않은 변수를 나타냅니다.

```js
let score: undefined;
console.log(score);
// 출력: undefined
```

<div class="content-ad"></div>

1.6 심볼: 고유 식별자를 나타냅니다.

```js
let symbol: symbol = Symbol('symbol');
```

1.7 BigInt: 임의 정밀도를 가진 정수를 나타냅니다.

```js
let bigIntNumber: bigint = 1234567890123456789012345678901234567890n;
```

<div class="content-ad"></div>

## 2. 객체

속성과 메소드의 모음입니다. 배열, 함수 및 일반 개체와 같은 복잡한 데이터 유형이 포함됩니다.

2.1 객체: 키-값 쌍의 모음.

```js
interface Person {
  name: string;
  age: number;
}

let person: Person = {
  name: "Alice",
  age: 30
};
```

<div class="content-ad"></div>

2.2 배열(Array): 값들의 순서가 정해진 모음입니다.

```js
let numbers: number[] = [1, 2, 3, 4, 5];
```

2.3 함수(Function): 특정 작업을 수행하기 위해 설계된 코드 블록입니다.

```js
function helloWorld(): void {
  console.log("Hello, world!");
}
```  

<div class="content-ad"></div>

# null 및 undefined 이해하기

## null

이것은 의도적으로 어떤 객체 값도 존재하지 않음을 나타내는 특별한 값입니다. 종종 변수가 비어 있어야 함을 나타내는 데 사용됩니다.

```js
let emptyValue: null = null;
console.log(emptyValue);
// 출력: null
console.log(typeof emptyValue);
// 출력: "object"
```

<div class="content-ad"></div>

null에 관한 주요 사항:

- null은 값이 없음을 나타내는 변수에 할당할 수 있는 할당 값입니다.
- null의 유형은 JavaScript의 과거 버그로 인해 개체입니다.

## undefined

은 변수가 방금 선언되었거나 인수가 제공되지 않은 함수 매개변수에 자동으로 할당되는 원시 값입니다.

<div class="content-ad"></div>

```js
let unassignedVariable;
console.log(unassignedVariable); // undefined

function exampleFunction(parameter) {
  console.log(parameter);
}
exampleFunction(); // undefined
```

undefined에 관한 중요한 점:

- 값이 없음을 나타냅니다.
- 변수가 선언되었지만 초기화되지 않은 경우 JavaScript에서 자동으로 undefined로 할당됩니다.
- 함수는 반환 값이 지정되지 않은 경우 undefined를 반환합니다.

null과 undefined의 차이점

<div class="content-ad"></div>

- Type: null은 객체이지만 undefined는 고유한 타입이다.
- 사용법: null은 값이 없음을 의도적으로 나타낼 때 사용된다. undefined는 변수가 선언되었지만 아직 값을 할당받지 않았음을 나타낸다.
- 할당: null은 개발자에 의해 명시적으로 변수에 할당된다. undefined는 JavaScript 엔진에 의해 할당된다.

# …

JavaScript의 서로 다른 데이터 유형, null 및 undefined를 이해하는 것은 효과적이고 버그 없는 코드를 작성하는 데 중요하다. 각 데이터 유형은 특정 목적을 제공하며 프로그램에서 다양한 종류의 값 및 상황을 관리하는 데 도움이 된다. 이러한 유형을 숙달함으로써 코드가 견고하고 유지보수가 용이함을 보장할 수 있다.

Pixabay의 CopyrightFreePictures의 이미지