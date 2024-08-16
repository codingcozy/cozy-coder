---
title: "프론트엔드 프레임워크의 상태 관리 쉽게 배우는 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-StateManagementinFrontendFrameworksAnEasyGuide_0.png"
date: 2024-07-10 00:41
ogImage: 
  url: /assets/img/2024-07-10-StateManagementinFrontendFrameworksAnEasyGuide_0.png
tag: Tech
originalTitle: "State Management in Frontend Frameworks: An Easy Guide"
link: "https://medium.com/@fakecoder.in/state-management-in-frontend-frameworks-an-easy-guide-0d6608e877c3"
isUpdated: true
---





애플리케이션의 상태 관리는 효율적인 웹 애플리케이션을 구축하는 데 중요합니다. 앱이 복잡해지면 상태를 효과적으로 처리함으로써 애플리케이션이 원활하게 작동하고 유지보수하기 쉬워집니다. 이 안내서에서는 React, Vue, Angular의 세 가지 인기 있는 프론트엔드 프레임워크 및 그와 함께 자주 사용되는 상태 관리 라이브러리인 Redux, Vuex, NgRx에 대해 살펴봅니다.

# 상태 관리란?

상태 관리는 앱 내에서 시간이 지남에 따라 변경될 수 있는 데이터를 처리하는 것을 말합니다. 사용자 입력, UI 요소 및 API에서 수신한 데이터와 같이 변할 수 있는 데이터를 효과적으로 처리하는 것은 다양한 앱 구성 요소가 데이터를 공유하고 동기화하여 상호 작용하는 사용자 인터페이스를 구축하기 쉽게 만들어줍니다.

<div class="content-ad"></div>

# 리액트와 리덕스

# 리액트의 내장 상태 관리

페이스북에서 만든 리액트는 컴포넌트를 사용하여 사용자 인터페이스를 구축하는 데 사용되는 라이브러리입니다. 리액트는 useState와 useReducer 훅을 사용하여 내장 상태 관리를 제공하며 이를 통해 컴포넌트 내에서 로컬 상태를 관리할 수 있습니다.

```js
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>당신은 {count} 번 클릭했습니다</p>
      <button onClick={() => setCount(count + 1)}>
        클릭하세요
      </button>
    </div>
  );
}
```

<div class="content-ad"></div>

리액트의 내장 상태 관리 기능은 작은 앱에 적합하지만, 대규모 앱의 경우 Redux와 같이 더 강력한 도구가 필요할 수 있습니다.

# Redux: 예측 가능한 상태 컨테이너

Redux는 대규모 앱에서 상태를 관리하기 위한 라이브러리입니다. 모든 상태를 위한 중앙 집중식 저장소를 제공하며, 상태를 예측 가능하고 디버그하기 쉽게 만드는 패턴을 따릅니다.

## Redux의 주요 개념

<div class="content-ad"></div>

- Store: 모든 상태를 보유하는 진리의 단일 소스입니다.
- Actions: 일어난 일을 설명하는 일반 객체입니다.
- Reducers: 현재 상태와 액션을 가져와 새로운 상태를 반환하는 함수들입니다.

## Redux의 예시

```js
// actions.js
export const increment = () => ({ type: 'INCREMENT' });
export const decrement = () => ({ type: 'DECREMENT' });

// reducer.js
const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

export default counterReducer;
```

# Vue와 Vuex

<div class="content-ad"></div>

# Vue의 내장 상태 관리

Vue는 Evan You에 의해 만들어진 사용자 인터페이스를 구축하기 위한 프레임워크입니다. Vue는 데이터 옵션과 반응적 속성을 사용하여 내장 상태 관리를 제공합니다.

```js
<template>
  <div>
    <p>{ count }</p>
    <button @click="increment">증가</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0
    };
  },
  methods: {
    increment() {
      this.count++;
    }
  }
};
</script>
```

더 큰 앱의 경우, Vuex를 추천합니다.

<div class="content-ad"></div>

# Vuex: Vue를 위한 중앙 집중식 상태 관리

Vuex는 Vue 앱에서 상태를 관리하기 위한 라이브러리입니다. 중앙 집중식 저장소를 제공하며 Redux와 유사한 패턴을 따릅니다.

## Vuex의 주요 개념

- State: 모든 상태를 보유하는 진리의 유일한 원천입니다.
- Mutations: 상태를 직접 변경하는 함수입니다.
- Actions: 비동기 작업을 포함하고 변이를 커밋할 수 있는 함수입니다.
- Getters: 유도된 상태를 반환하는 계산된 속성입니다.

<div class="content-ad"></div>

## Vuex 예시

```js
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    },
    decrement(state) {
      state.count--;
    }
  },
  actions: {
    increment({ commit }) {
      commit('increment');
    },
    decrement({ commit }) {
      commit('decrement');
    }
  },
  getters: {
    count: state => state.count
  }
});
```

# Angular과 NgRx

# Angular의 내장 상태 관리

<div class="content-ad"></div>

Google이 만든 Angular은 웹 애플리케이션을 구축하는 플랫폼입니다. Angular는 서비스를 사용하여 내장 상태 관리를 제공하며 더 복잡한 시나리오를 위해 @ngrx/store 라이브러리를 사용할 수 있습니다.

```js
// counter.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class CounterService {
  private count = 0;

  getCount() {
    return this.count;
  }

  increment() {
    this.count++;
  }

  decrement() {
    this.count--;
  }
}
```

대규모 앱의 경우 NgRx가 더 나은 선택입니다.

# NgRx: Angular를 위한 반응형 상태 관리

<div class="content-ad"></div>

앵귤러에서 상태를 관리하기 위한 반응형 라이브러리 세트인 NgRx는 Redux 원칙에 기반을 두고 있어요.

## NgRx의 중요한 개념

- 스토어(Store): 모든 상태를 보유하는 진실의 단일 원본.
- 액션(Actions): 상태 변경에 대한 설명.
- 리듀서(Reducers): 상태 전환을 처리하는 함수.
- 셀렉터(Selectors): 상태를 쿼리하기 위한 함수.

## NgRx의 예시

<div class="content-ad"></div>

```js
// actions.ts
import { createAction } from '@ngrx/store';

export const increment = createAction('[Counter] Increment');
export const decrement = createAction('[Counter] Decrement');

// reducer.ts
import { createReducer, on } from '@ngrx/store';
import { increment, decrement } from './actions';

export const initialState = 0;

const _counterReducer = createReducer(initialState,
  on(increment, state => state + 1),
  on(decrement, state => state - 1)
);

export function counterReducer(state, action) {
  return _counterReducer(state, action);
}
```

# 결론

상태 관리는 현대 웹 개발의 중요한 부분입니다. 올바른 접근 방식과 라이브러리를 선택하면 앱을 유지 보수하고 확장하기 쉬워집니다. React와 Redux, Vue와 Vuex, 또는 Angular와 NgRx를 사용하고 있던 중이건, 이 도구들을 이해하면 더 나은 앱을 개발할 수 있습니다. 각 프레임워크와 라이브러리는 각자의 강점을 가지고 있으므로 프로젝트의 요구 사항에 맞는 것을 선택하시기 바랍니다.

상태 관리를 숙달하면 사용자를 기쁘게 할 수 있는 고품질의 인터랙티브 앱을 개발할 준비가 될 것입니다.
