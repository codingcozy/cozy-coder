---
title: "새로운 JavaScript 기능 탐험 Records와 Tuples 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-30-ExploringtheNewJavaScriptFeatureRecordsandTuples_0.png"
date: 2024-07-30 17:26
ogImage: 
  url: /assets/img/2024-07-30-ExploringtheNewJavaScriptFeatureRecordsandTuples_0.png
tag: Tech
originalTitle: "Exploring the New JavaScript Feature Records and Tuples"
link: "https://medium.com/javascript-in-plain-english/exploring-the-new-javascript-feature-records-and-tuples-8345e1e968dd"
---


![2024-07-30-ExploringtheNewJavaScriptFeatureRecordsandTuples_0.png](/assets/img/2024-07-30-ExploringtheNewJavaScriptFeatureRecordsandTuples_0.png)

자바스크립트는 지속적으로 진화하며, 개발자 생산성과 코드 가독성을 향상시키는 새로운 기능을 소개하고 있습니다. 레코드(record)와 튜플(tuple)은 자바스크립트의 두 가지 새로운 기능으로, 변경할 수 없는 데이터 구조를 제공하여 상태 관리와 데이터 처리를 간소화합니다. 본 문서에서는 레코드와 튜플의 기본 개념과 구문을 소개하고, 다양한 예제를 통해 그들의 강력한 기능과 응용 프로그램을 시연하겠습니다.

# 1. 소개

## 레코드

<div class="content-ad"></div>

레코드는 변경할 수 없는 키-값 쌍의 모음으로, JavaScript 객체와 유사하지만 불변이며 깊은 동등성을 가집니다. 레코드는 #로 접두사가 붙은 객체 형식의 리터럴 구문을 사용하여 생성됩니다.

```js
const record = #{
  key1: 'value1',
  key2: 'value2'
};
```

## 튜플

튜플은 변경할 수 없는 순서가 있는 리스트로, JavaScript 배열과 유사하지만 불변이며 깊은 동등성을 가집니다. 튜플은 #로 접두사가 붙은 배열 형식의 리터럴 구문을 사용하여 생성됩니다.

<div class="content-ad"></div>

```js
const tuple = ['value1', 'value2'];
```

# 2. 예시

## 예시 1: 상태 관리

React와 같은 프론트엔드 프레임워크에서 상태 관리는 일반적인 요구사항입니다. 불변인 레코드와 튜플은 상태가 실수로 수정되는 것을 효과적으로 방지하여 응용 프로그램의 안정성을 향상시킬 수 있습니다.

<div class="content-ad"></div>

```js
const initialState = #{
  count: 0,
  items: #['item1', 'item2']
};

const newState = {
  ...initialState,
  count: initialState.count + 1,
  items: [...initialState.items, 'item3']
};

console.log(newState); // #{
                       //   count: 1,
                       //   items: #['item1', 'item2', 'item3']
                       // }
```

## 예시 2: 데이터 전달

대규모 애플리케이션에서 데이터 전달은 복잡할 수 있습니다. 레코드와 튜플을 사용하면 데이터가 이동 중에 수정되지 않아 데이터 일관성과 신뢰성을 보장할 수 있습니다.

```js
function processData(data) {
  // 데이터가 레코드인 것으로 가정
  const processedData = {
    ...data,
    timestamp: Date.now()
  };
  return processedData;
}

const data = #{
  id: 1,
  name: '샘플'
};

const result = processData(data);

console.log(result); // #{
                     //   id: 1,
                     //   name: '샘플',
                     //   timestamp: 1627392929313
                     // }
```

<div class="content-ad"></div>

## 예제 3: 함수형 프로그래밍

레코드와 튜플은 함수형 프로그래밍에서 불변 데이터 구조의 개념과 잘 어울립니다. 레코드와 튜플을 사용하면 순수 함수의 구현을 간단하게 할 수 있어 기능적인 코드를 작성하고 유지하는 것이 더 쉬워집니다.

```js
const increment = (tuple) => {
  return #[...tuple, tuple[tuple.length - 1] + 1];
};

const nums = #[1, 2, 3];
const newNums = increment(nums);

console.log(newNums); // #[1, 2, 3, 4]
```

## 예제 4: 깊은 동등성

<div class="content-ad"></div>

레코드와 튜플은 깊은 동등성을 가지고 있어요. 이는 비교가 표면적인 부분을 넘어서서 내부 구조까지 확인한다는 것을 의미해요.

```js
const record1 = #{
  key1: 'value1',
  key2: #{
    nestedKey: 'nestedValue'
  }
};

const record2 = #{
  key1: 'value1',
  key2: #{
    nestedKey: 'nestedValue'
  }
};

console.log(record1 === record2); // true
```

## 예제 5: 조합 사용

레코드와 튜플은 함께 사용할 수 있어요. 이는 데이터 구조의 유연성과 안정성을 향상시키는 데 도움이 돼요.

<div class="content-ad"></div>

```js
const user = #{
  id: 1,
  name: 'John Doe',
  roles: #['admin', 'user']
};

const updatedUser = {
  ...user,
  roles: [...user.roles, 'editor']
};

console.log(updatedUser); // #{
                          //   id: 1,
                          //   name: 'John Doe',
                          //   roles: #['admin', 'user', 'editor']
                          // }
```

레코드와 튜플은 JavaScript에 강력한 불변 데이터 구조를 소개하여 데이터 처리의 보안성과 신뢰성을 크게 향상시킵니다. Records와 Tuples를 사용하면 개발자들은 상태를 더 쉽게 관리하고, 데이터를 전달하고, 함수형 프로그래밍을 구현할 수 있습니다. 이 글에서 Records와 Tuples의 기본 개념과 구문을 소개하고, 다양한 시나리오를 통해 사용법을 보여줌으로써 독자들이 이러한 새로운 기능을 더 잘 이해하고 적용할 수 있도록 도왔습니다. JavaScript가 계속 발전함에 따라 Records와 Tuples은 분명히 다양한 응용 프로그램에서 더 중요한 역할을 할 것입니다.

# 간단하게 설명하기 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 계속하기 전에:  

<div class="content-ad"></div>

- 작가를 클릭하고 팔로우해주세요! ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기