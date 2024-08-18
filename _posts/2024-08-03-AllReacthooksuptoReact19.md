---
title: "React 19에 새로 추가된 React 훅 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-03 18:43
ogImage:
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "All React hooks  up to React 19"
link: "https://medium.com/@upekshadilshan000/all-react-hooks-up-to-react-19-5c0bbba534a5"
isUpdated: true
---

## 리액트 훅 완전 가이드.

![image](https://miro.medium.com/v2/resize:fit:1400/1*HOahzVAKstNAfQwHP6_G3Q.gif)

훅이 소개되기 전에는 클래스 컴포넌트가 상태 및 라이프사이클 메서드를 관리하는 주요 방법이었습니다. 훅은 리액트 16.8 버전에서 소개되었습니다. 클래스와 관련된 다양한 문제를 방지하고 상태와 사이드 이펙트를 보다 간결하고 재사용 가능하게 관리할 수 있습니다.

## 리액트 훅을 사용해야 하는 경우?

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

만약 React에서 함수 컴포넌트를 만들고 나중에 상태 또는 부가 효과를 추가해야 한다면, Hooks를 사용할 수 있어요.

## 고려해야 할 Hooks 규칙:

- 먼저 Hooks를 import해주세요.
- Hooks는 함수 컴포넌트 내에서만 사용할 수 있어요.
- Hooks는 함수 컴포넌트의 최상위 레벨에서만 사용할 수 있어요. (컴포넌트 내의 함수 안에 넣을 수 없어요)
- Hooks는 조건문 안에 넣을 수 없어요.

## Hooks 카테고리

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

리액트 19 버전까지는 총 20가지 내장 후크가 있습니다. 이 후크들은 목적에 따라 07가지 주요 카테고리로 분류할 수 있어요.

- State Hooks: State 후크는 컴포넌트의 로컬 상태를 유지하는 데 사용될 수 있어요. 사용자 입력과 같은 정보를 기억하는 데 유용해요. useState, useReducer
- Context Hooks: Context 후크는 프롭스로 전달하지 않고 먼 부모 컴포넌트로부터 정보를 전달받는 데 사용돼요. useContext
- Ref Hooks: Ref 후크는 렌더링에 사용되지 않는 일부 정보를 컴포넌트가 보유할 수 있도록 해줘요. DOM 노드나 타임아웃 ID와 같은 것이 이에 해당돼요. useRef, useImperativeHandle
- Effect Hooks: Effect 후크는 컴포넌트가 외부 시스템에 연결하고 동기화할 수 있도록 해줘요. 네트워크, 브라우저 DOM, 기타 React가 아닌 코드 처리를 포함해요. useEffect, useLayoutEffect, useInsertionEffect
- Performance Hooks: 성능 후크는 불필요한 작업을 건너뛰어 다시 렌더링 성능을 최적화하는 데 사용돼요. useMemo, useCallback, useTransition, useDeferredValue
- Form Hooks: 폼 작업을 할 때 사용될 수 있어요. useFormStatus, useFormState
- Other Hooks: useDebugValue, useId, useSyncExternalStore, useActionState, use, useOptimistic

제 유튜브 채널에서는 이 react 후크들을 하나씩 다루게 될 거예요. 그래서 제 채널도 구독해주세요. 그렇게 되면 콘텐츠를 놓치지 않을 거예요.

그리고 React 개발자라면 도움이 될 수 있는 몇 가지 블로그 포스트도 있어요.

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

다음 React 블로그를 기다려주세요. 👋👋👋👋

React나 코딩 관련 내용을 배우고 싶다면, 제를 팔로우하는 것을 고려해보세요. 지식을 새롭게 갱신하고 프로그래밍 관련 새로운 정보를 습득할 수 있습니다. 🤩🤩🤩😃
