---
title: "React에서 useContext를 사용하는 이유"
description: ""
coverImage: "/assets/img/2024-08-19-WhyandWhentoUseuseContextinReact_0.png"
date: 2024-08-19 02:18
ogImage: 
  url: /assets/img/2024-08-19-WhyandWhentoUseuseContextinReact_0.png
tag: Tech
originalTitle: "Why and When to Use useContext in React"
link: "https://medium.com/@komalmraut/why-and-when-to-use-usecontext-in-react-20f19a8d7fce"
isUpdated: true
updatedAt: 1724034979689
---


리액트 개발에서 여러 컴포넌트 간에 데이터를 공유하는 것은 때로는 전화로 게임하는 것 같은 느낌일 수 있어요. 그 게임을 기억하나요? 옆 사람에게 메시지를 속삭이고, 그 사람이 다시 전달하는 식으로 진행되는 그 게임 말이에요. 마지막 사람에게 도착할 때, “I love this song”이 “I am stoned”로 어떻게 변해있을지 상상해보세요.

![이미지](/assets/img/2024-08-19-WhyandWhentoUseuseContextinReact_0.png)

비슷하게, 여러 컴포넌트 층을 통과하는 프롭을 전달하는 것은 “prop drilling”이라고 불릴 수 있는데, 이는 코드를 혼란스럽고 유지보수하기 어렵게 만들 수 있어요. 하지만 걱정하지 마세요. React에는 이에 대한 편리한 해결책이 있습니다: useContext 훅.

## 문제: Prop Drilling

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

프롭 드릴링은 데이터를 여러 레이어의 컴포넌트를 통해 전달할 때 발생하는 현상이에요. 때때로 일부 컴포넌트가 데이터가 필요하지 않더라도 데이터를 전달하는 경우도 있어요!

다음은 예시에요:

```js
// App.js
const App = () => {
  const theme = 'dark';
  return <ParentComponent theme={theme} />;
};

// ParentComponent.js
const ParentComponent = ({ theme }) => {
  return <ChildComponent theme={theme} />;
};

// ChildComponent.js
const ChildComponent = ({ theme }) => {
  return <DeeplyNestedComponent theme={theme} />;
};

// DeeplyNestedComponent.js
const DeeplyNestedComponent = ({ theme }) => {
  return <div>The current theme is {theme}</div>;
};
```

이 코드에서는 theme 프롭이 여러 컴포넌트를 통해 DeeplyNestedComponent에 전달되고 있어요. 앱이 커질수록 이것은 우리의 전화 게임처럼 복잡해질 수 있어요.

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

## 해결책: useContext

모든 계층을 통해 props를 전달하는 대신, useContext를 사용하면 중간 단계를 건너뛰고 데이터를 필요한 곳에서 직접 공유할 수 있습니다.

이전 예제를 어떻게 간소화할 수 있는지 살펴봅시다:

```js
import React, { createContext, useContext } from 'react';

// 컨텍스트 생성
const ThemeContext = createContext();

const App = () => {
  const theme = 'dark';

  return (
    <ThemeContext.Provider value={theme}>
      <DeeplyNestedComponent />
    </ThemeContext.Provider>
  );
};

// DeeplyNestedComponent.js
const DeeplyNestedComponent = () => {
  const theme = useContext(ThemeContext);
  return <div>현재 테마는 {theme}입니다.</div>;
};
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

이 예시에서 DeeplyNestedComponent는 prop 드릴링이 필요 없이 컨텍스트에서 주제에 직접 액세스할 수 있습니다.

## useContext를 사용하는 시기는?

이제 useContext가 무엇이며 어떻게 작동하는지 아셨으니, 언제 사용해야 하는지 알아봅시다. useContext를 사용하는데 유용한 몇 가지 시나리오는 다음과 같습니다:

- 전역 상태 관리:
여러 컴포넌트에서 동일한 데이터에 액세스해야 하는 경우 useContext가 매우 유용합니다. 사용자 인증 상태, 테마 설정 또는 언어 설정과 같은 데이터를 관리하는 경우입니다. 마치 응용 프로그램 전체에 대한 유니버설 리모컨을 갖고 있는 것과 비슷합니다.
- Prop 드릴링 회피:
필요하지 않은 많은 컴포넌트 계층을 통해 데이터를 전달해야 할 때, useContext가 문제 해결을 도와줍니다. 중요한 메모를 회사 내 모든 부서를 통과시키는 대신 필요로 하는 사람에게 직접 전달하는 상황을 상상해 보세요.
- 테마 설정:
앱에서 여러 테마를 지원하는 경우, useContext를 사용하여 필요한 모든 컴포넌트에 현재 테마를 제공할 수 있습니다. 마치 앱 전체의 테마를 한 번에 변경하는 빛 스위치를 갖고 있는 것과 비슷합니다.
- 로컬라이제이션:
앱이 여러 언어를 지원하는 경우, useContext를 사용하여 현재 언어 및 번역을 모든 컴포넌트에 제공할 수 있습니다. 모두가 동일한 것을 이해할 수 있도록 하는 것이 telephone game처럼 아무도 이해하지 못하는 상황을 방지할 수 있습니다.

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

## useContext의 장단점

장점:

- 데이터 공유를 간편화: 프롭 드릴링의 번거로움을 피하고 코드를 깔끔하게 유지할 수 있습니다.
- 중앙 집중식 상태 관리: 여러 컴포넌트가 액세스해야 하는 전역 상태에 적합합니다.
- 테마 및 지역화가 쉬워짐: 애플리케이션 전체에 일관된 테마 또는 언어 환경을 적용하기가 보다 간단해집니다.

단점:

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

- 과용은 복잡성으로 이어질 수 있습니다: 많은 컨텍스트 사용은 앱을 디버깅하기 어렵게 할 수 있습니다.
- 성능에 대한 고려사항: 컨텍스트 값의 빈번한 업데이트는 불필요한 다시 렌더링을 유발할 수 있어 성능에 영향을 줄 수 있습니다.
- 디버깅에 대한 도전: 프롭을 사용하는 것과 비교하여 큰 애플리케이션에서는 문제를 추적하기 어려울 수 있습니다.

## 결론

React에서 useContext는 프롭 드리리을 피하는 데 도움이 되는 강력한 도구로, 전역 상태를 관리하고 앱 전반에서 데이터를 공유하기 쉽게 만들어줍니다. 컴포넌트 트리를 간소화시켜줍니다. 그러나 모든 도구와 마찬가지로 적당히 사용하는 것이 가장 좋습니다. 간단하게 유지하고 코드를 깔끔하게 유지하세요. 여러 층을 통해 프롭을 전달할 때는 useContext가 해결책이라는 것을 기억하세요. 데이터가 필요한 곳으로 정확히 전달되도록 보장하는 편리한 훅입니다.

독서해 주셔서 감사합니다! 도움이 되셨다면 동료 개발자와 공유하고 React가 제공하는 많은 도구들을 계속 탐험해보세요.

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

행복한 코딩하세요! 🎉