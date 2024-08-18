---
title: "초보 개발자라면 반드시 알아야 할 React Context API 사용법"
description: ""
coverImage: "/assets/img/2024-08-04-ReactContextAPI_0.png"
date: 2024-08-04 18:45
ogImage:
  url: /assets/img/2024-08-04-ReactContextAPI_0.png
tag: Tech
originalTitle: "React Context API"
link: "https://medium.com/@aliriza.ihn/react-context-api-91c3f6a5e896"
isUpdated: true
---

React Context API는 상태를 관리하고 구성 요소 트리를 통해 데이터를 전달하는 강력한 도구입니다. prop drilling 없이 컴포넌트 간에 값을 공유하는 방법을 제공합니다.

## Context API란?

Context API를 사용하면 컨텍스트를 생성하여 컴포넌트 트리에서 데이터를 제공하고 사용할 수 있습니다. 현재 사용자, 테마 또는 언어 설정과 같은 전역 데이터를 공유하는 데 유용합니다.

## 컨텍스트 생성하기

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

컨텍스트를 생성할 때 React.createContext 함수를 사용합니다. 이 함수는 Provider 및 Consumer 컴포넌트가 포함된 Context 객체를 반환합니다.

![React Context API Image](/assets/img/2024-08-04-ReactContextAPI_0.png)

## 데이터 제공하기

Provider 컴포넌트는 컨텍스트 데이터를 모든 자식 컴포넌트에서 사용할 수 있도록 만듭니다. Provider로 컴포넌트 트리를 감싸고 공유하려는 값을 전달합니다.

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

<img src="/assets/img/2024-08-04-ReactContextAPI_1.png" />

## 데이터 소비

컨텍스트 데이터를 소비하기 위해 함수 컴포넌트에서 Consumer 컴포넌트 또는 useContext 훅을 사용할 수 있습니다.

- Consumer 컴포넌트 사용:

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

<img src="/assets/img/2024-08-04-ReactContextAPI_2.png" />

2. Using the useContext Hook:

<img src="/assets/img/2024-08-04-ReactContextAPI_3.png" />

Using the useContext hook makes the code cleaner and easier to read.

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

## 컨텍스트 업데이트

컨텍스트 데이터를 업데이트해야 할 경우, 컨텍스트 값에 함수를 제공하고 컴포넌트에서 해당 함수를 호출할 수 있습니다.

![이미지 1](/assets/img/2024-08-04-ReactContextAPI_4.png)

![이미지 2](/assets/img/2024-08-04-ReactContextAPI_5.png)

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

이 예시에서 setUser은 컨텍스트에 의해 제공되는 함수로 사용자 데이터를 업데이트하는 데 사용됩니다.

## Best Practices

- 컨텍스트를 세분화 유지: 큰 컨텍스트를 만드는 것을 피하십시오. 대신 응용 프로그램의 다른 부분에 대해 여러 개의 작은 컨텍스트를 만드세요.
- 컨텍스트를 절약하게 사용: 글로벌로 액세스해야 하는 데이터에 대해서만 컨텍스트를 사용하세요. 로컬 상태의 경우에는 훅이나 클래스 상태와 함께 계속 상태 관리를 사용하세요.
- 컨텍스트 값 메모화: useMemo를 사용하여 컨텍스트 값 메모화하고 불필요한 다시 렌더링을 피하세요.

![React Context API](/assets/img/2024-08-04-ReactContextAPI_6.png)

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

## 결론

React Context API는 전역 상태를 관리하고 프롭 전달을 피하는 데 강력한 도구입니다. 컨텍스트를 만들고 제공하고 소비하는 방법을 이해하면 유지보수가 용이하고 확장 가능한 React 애플리케이션을 만들 수 있습니다. 프로젝트에 Context API를 통합하여 상태 관리 능력을 향상시켜보세요!
