---
title: "React에서 인덱스를 Key로 사용하면 안되는 이유"
description: ""
coverImage: "/assets/img/2024-08-04-WhynottouseindexforReactKeysDoyoureallygetwhatshappeningunderthehood_0.png"
date: 2024-08-04 18:43
ogImage: 
  url: /assets/img/2024-08-04-WhynottouseindexforReactKeysDoyoureallygetwhatshappeningunderthehood_0.png
tag: Tech
originalTitle: "Why not to use index for React Keys Do you really get whats happening under the hood"
link: "https://medium.com/@deshwaljaivardhan/why-not-to-use-index-for-react-keys%EF%B8%8F-do-you-really-get-whats-happening-under-the-hood-3d9da500192c"
isUpdated: true
---






![이미지](/assets/img/2024-08-04-WhynottouseindexforReactKeysDoyoureallygetwhatshappeningunderthehood_0.png)

eslint이 왜 index를 키로 사용하는 것에 이렇게 고집을 부리는 걸까요❌? React에서 객체를 매핑할 때 왜 필요한 걸까요? 키 없이도 앱은 잘 작동하는데, 귀찮게 뭘 할까요?

아마도 당신이 요소를 삭제하거나 추가하는 일이 없는 정적 리스트를 가지고 있기 때문에 index를 키로 사용하는 문제에 직면하지 못했을 수도 있습니다. 실제로 다른 경우에는 React가 변경사항을 비교하고 렌더링하기 위해 키가 필요합니다.

책장에 진열된 장난감 목록이 있다고 상상해보세요. 장난감의 순서를 변경하거나 일부를 제거하려면, 어떤 장난감을 어떻게 움직여야 하는지 몰라도 혼란스러울 것입니다. 라벨(또는 키)은 선반 전체를 망치지 않고 정확히 어떤 장난감을 이동하거나 제거해야 하는지 쉽게 알 수 있게 해줍니다.


<div class="content-ad"></div>

## 인덱스가 문제가 될 수 있는 이유❗

## 🔄항목 재정렬:

인덱스를 키로 사용하고 목록의 항목을 재정렬하는 경우, React가 어떤 항목이 변경되었는지 혼란스러워할 수 있습니다. 키는 순서에 기반하므로 재정렬 후에는 동일한 인덱스가 다른 항목을 가리킬 수 있습니다. 이는 잘못된 항목이 업데이트되거나 DOM에서 이동되는 등 예기치 않은 동작을 초래할 수 있습니다.

예를 들어, 좋아하는 색상 목록이 있다고 가정해 봅시다:

<div class="content-ad"></div>

```js
const items = ["Apple", "Banana", "Cherry"];
```

초기 렌더링:

- Index 0 - “Apple” (key=0)
- Index 1 - “Banana” (key=1)
- Index 2 - “Cherry” (key=2)

만약 “Banana”와 “Cherry”를 교체하면:

<div class="content-ad"></div>

- 인덱스 0 - ` "사과" (키=0)
- 인덱스 1 - ` "체리" (키=1)
- 인덱스 2 - ` "바나나" (키=2)

React는 "바나나"와 "체리"가 자리를 바꾼 것으로 인식하여 키가 동일한 상태에서 컴포넌트를 잘못 재사용할 수 있습니다.

## ➕ 추가 또는 제거 ➖

리스트에 항목을 추가하거나 제거할 때 인덱스를 키로 사용하면 React가 어떤 항목이 변경되었는지 잘못 해석할 수 있습니다. 목록에서 항목이 제거되면 그 이후의 모든 항목이 인덱스를 변경하므로 React는 제거된 항목 이후의 모든 항목이 변경되었다고 생각합니다.

<div class="content-ad"></div>

```js
import React, { useState } from 'react';

function TodoList() {
  const [tasks, setTasks] = useState([
    "장보기",
    "개 산책시키기",
    "책 읽기"
  ]);

  return (
    <ul>
      {tasks.map((task, index) => (
        <li key={index}>{task}</li>
      ))}
    </ul>
  );
}

function addTask() {
  setTasks([
    "장보기",
    "개 산책시키기",
    "이메일 쓰기", // 새로운 작업을 중간에 삽입
    "책 읽기"
  ]);
}
```

Initial Render:

- "장보기" (키=0)
- "개 산책시키기" (키=1)
- "책 읽기" (키=2)

새로운 작업 추가 후:

<div class="content-ad"></div>

- "식료품 사기" (key=0)
- "개 산책 시키기" (key=1)
- "이메일 쓰기" (key=2) - 새로운 아이템입니다.
- "책 읽기" (key=3)

키는 인덱스를 기반으로 하기 때문에 React는 키가 2인 아이템이 "책 읽기"에서 "이메일 쓰기"로 변경되었고, 키가 3인 아이템은 새로 추가되었다는 것을 알 수 있습니다. 이로 인해 React는 다음과 같은 작업을 수행합니다:

- 키가 2인 이전 아이템("책 읽기")을 제거합니다.
- 키가 2인 새로운 아이템("이메일 쓰기")을 삽입합니다.
- 키가 3인 새로운 아이템("책 읽기")을 추가합니다.

사실상, React는 "이메일 쓰기"를 삽입하는 것만이 실제 변경 사항이지만, 변경된 인덱스(2)부터 리스트 끝까지의 아이템을 제거하고 다시 삽입해야 합니다.

<div class="content-ad"></div>

리액트는 삽입 지점 다음의 모든 항목을 다시 렌더링하며 새 항목뿐만 아니라 모두 렌더링합니다. 특히 각 항목이 복잡한 렌더링 로직이나 부작용을 가지고 있는 경우에는 비효율적일 수 있습니다.

## ♻️ 컴포넌트 상태와 라이프사이클:

리액트에서 컴포넌트는 상태를 가질 수 있으며, 해당 상태는 컴포넌트가 DOM에서 제거되지 않는 한 유지됩니다. 인덱스 기반 키를 사용하고 목록이 변경된 경우, 리액트는 이전 상태를 새 항목과 잘못 연결할 수 있습니다. 왜냐하면 리액트는 동일한 키(인덱스)를 갖는 항목이 전과 동일하다고 가정하기 때문입니다. 이는 특히 항목에 상호작용 요소가 있는 경우 예기치 않은 동작과 버그로 이어질 수 있습니다.

# 인덱스 사용이 적절할 때 ✅

<div class="content-ad"></div>

일부 시나리오에서는 색인을 키로 사용하는 것이 수용될 수 있습니다. 특히 다음 경우에 해당됩니다:

- 항목 목록이 정적일 때: 목록의 길이나 순서가 변경되지 않는 경우입니다.
- 항목 자체가 정적일 때: 각 항목의 내용이 변경되지 않는 경우입니다.
- 고유 식별자가 없는 경우: 다른 고유 식별자를 키로 사용할 수 없는 경우입니다.

고유 식별자를 키로 사용하는 것은 React의 조정 알고리즘이 효율적으로 작동하고 의도한 변경 내용이 DOM에 정확하게 반영되도록 보장합니다.