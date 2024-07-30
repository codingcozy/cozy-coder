---
title: "2024년, React v19의 놀라운 기능 총정리 꼭 알아야 할 모든 것"
description: ""
coverImage: "/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_0.png"
date: 2024-07-30 17:18
ogImage: 
  url: /assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_0.png
tag: Tech
originalTitle: "Mind-blowing Features of React v19 All You Need To Know in 2024"
link: "https://medium.com/javascript-in-plain-english/mind-blowing-features-of-react-v19-all-you-need-to-know-in-2024-4ffb52767cef"
---


저는 TypeScript 개발자로서, 매일 사용하는 도구들의 최신 기술 발전을 기대합니다. React는 현대 웹 개발의 중심 요소이며, React v19의 출시로 큰 발전을 경험하고 있습니다. 🔮

이번 버전은 개발을 간소화하고 성능을 개선하며 전반적인 개발 경험을 단순화하는 다양한 새로운 기능과 개선 사항을 제공합니다.

이러한 첨단 기능을 자세히 살펴보고 프로젝트에 혁명을 일으킬 수 있는지 살펴보겠습니다.

# 1. React Server Components 🚀

<div class="content-ad"></div>

리액트 서버 컴포넌트(RSC)는 정말 멋진 기술이에요. 이 컴포넌트들은 빌드 시간에 렌더링되거나 각 요청마다 렌더링되어 클라이언트 측의 작업 부담을 줄이고 성능을 향상시킬 수 있어요.

클라이언트가 모든 처리를 담당하는 대신 서버에서 처리할 수 있도록 RSC가 제공되어, 미리 렌더링된 컴포넌트를 클라이언트에 전달할 수 있는 상황을 상상해보세요.

예시:

![이미지 설명](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_0.png)

<div class="content-ad"></div>

서버 측 렌더링을 활용하면 초기로드 시간을 크게 줄일 수 있어 사용자 경험이 원활해질 수 있어요.

## 2. 작업

작업은 전이, 오류 처리, 낙관적 업데이트 및 보류 중인 상태를 자동으로 처리하여 비동기 작업을 간단하게 만듭니다. 이 기능은 비동기 작업과 관련된 보일러플레이트 코드의 많은 부분을 추상화합니다.

예시:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_1.png)

행동은 양식 제출 및 기타 비동기 작업 처리 과정을 간소화하여 코드를 보다 깔끔하고 유지보수하기 쉽게 만듭니다.

## 3. 전체 커스텀 요소 지원

React v19은 이제 커스텀 요소를 완벽하게 지원하여 웹 구성 요소의 원활한 통합을 가능하게 합니다. 이는 React의 기능을 손상시키지 않고 타사 라이브러리나 기존 코드를 통합하는 데 유용합니다.


<div class="content-ad"></div>

예시:


![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_2.png)


이 지원을 통해 React와 사용자 정의 요소의 장점을 모두 활용하여 응용 프로그램 개발 범위를 넓힐 수 있습니다.

# 4. 문서 메타데이터 지원

<div class="content-ad"></div>

문서 메타데이터 관리가 React v19에서 더욱 간편해 졌어요. 컴포넌트 내에서 제목, 링크 및 메타 태그를 렌더링할 수 있고, React가 자동으로 해당 내용을 문서 헤드에 적절한 위치에 배치해줍니다.

예시:

`<img src="/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_3.png" />`

이 기능을 통해 메타데이터 관리가 더욱 효율적이고 직관적으로 이루어집니다.

<div class="content-ad"></div>

# 5. 우선순위 설정이 포함된 스타일시트

React v19에서는 스타일시트 우선순위 설정을 소개합니다. 이를 통해 개발자들은 스타일시트가 적용되는 순서를 제어할 수 있습니다. 특히 타사 스타일을 처리하거나 특정 스타일이 다른 스타일을 덮어쓸 수 있도록 하는 데 유용합니다.

예시:

![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_4.png)

<div class="content-ad"></div>

이 기능은 복잡한 스타일링 시나리오를 관리하는 것을 간단하게 만들어줍니다. 당신의 스타일이 의도한 대로 적용되도록 보장합니다.

## 6. 어떤 컴포넌트에서도 비동기 스크립트 렌더링하기 🔮

이제 React가 중복을 처리하면서 어떤 컴포넌트에서도 비동기 스크립트를 렌더링할 수 있습니다. 이를 통해 스크립트가 중복으로 로드되지 않고 효율적으로 로드됩니다.

예시:

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_5.png)

이 기능은 제3자 서비스 또는 분석을 통합하는 데 특히 유용합니다.

# 7. prefetchDNS, preconnect, preload, preinit을 통한 리소스 미리 로드하기 👏

React v19에서는 prefetchDNS, preconnect, preload, preinit과 같은 다양한 전략을 사용하여 리소스를 미리 로드할 수 있습니다. 이는 중요한 리소스가 빨리 로드되어 성능을 향상시키는 데 도움이 됩니다.

<div class="content-ad"></div>

예시:

<img src="/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_6.png" />

중요한 리소스를 미리 로드하면 로딩 시간을 크게 줄일 수 있고 사용자 경험을 향상시킬 수 있어요.

# 8. 예기치 못한 태그 처리가 개선되었어요

<div class="content-ad"></div>

React v19은 `head`와 `body`에서 예상치 못한 태그를 건너뛰어 제3자 스크립트와의 호환성을 향상시켰습니다. 이로 인해 불일치 오류를 피하고 외부 통합이라도 응용 프로그램이 원활하게 실행되도록 보장할 수 있습니다.

예시:

<img src="/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_7.png" />

이 기능은 외부 서비스와 통합할 때 응용 프로그램의 견고성을 향상시킵니다.

<div class="content-ad"></div>

# 9. 더 나은 오류 보고

React v19에서는 향상된 오류 보고 기능을 제공하여 중복 오류를 자동으로 제거하고 더 나은 오류 처리를 위한 onCaughtError 및 onUncaughtError 루트 옵션을 제공합니다.

예시:

![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_8.png)

<div class="content-ad"></div>

개선된 오류 보고는 문제를 더 효과적으로 처리할 수 있도록 해줍니다. 이는 더 안정적인 애플리케이션을 이끌어냅니다.

# 새로운 지시문 💖 😱

# 1. ‘use client’

`use client` 지시문은 클라이언트에서만 실행되는 코드를 표시하여 클라이언트와 서버 로직 사이의 명확한 분리를 보장합니다.

<div class="content-ad"></div>

예시:

![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_9.png)

## 2. 'use server'

'Use server' 디렉티브는 클라이언트 측 코드에서 호출할 수 있는 서버 측 함수를 표시하여 서버 측 로직 통합을 용이하게 합니다.

<div class="content-ad"></div>

예시:

![이미지](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_10.png)

## 새로운 API

### 1. 사용

<div class="content-ad"></div>

API를 사용하면 렌더링에서 약속과 컨텍스트와 같은 리소스를 읽을 수 있어요.

예시:

![Image](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_11.png)

## 2. Prop으로 ref 사용

<div class="content-ad"></div>

Refs는 이제 간단한 prop으로 변경되어 사용하기가 더 간편해졌습니다.

예시:


<img src="/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_12.png" />


# 3. 클린업이 포함된 Ref 콜백

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경했습니다.

<div class="content-ad"></div>

Markdown 형식으로 변경할 때 `<table>` 태그를 사용해야 합니다.

<div class="content-ad"></div>

useDeferredValue은 이제 초기 값을 받아 더 많은 유연성을 제공합니다.

예시:


![Example](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_15.png)

# 새로운 훅


<div class="content-ad"></div>

# 1. useActionState

JS가 아직 실행되지 않았을 경우, 폼 상태를 선언하고 gracefully하게 저하시킨다.

예시:


![image](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_16.png)


<div class="content-ad"></div>

# 2. useFormStatus

양식의 상태를 쉽게 가져옵니다.

예시:

![image](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_17.png)

<div class="content-ad"></div>

# 3. useOptimistic

비동기 요청이 진행 중일 때 최적화된 최종 상태를 낙관적으로 표시합니다.

예시:

![image](/assets/img/2024-07-30-Mind-blowingFeaturesofReactv19AllYouNeedToKnowin2024_18.png)

<div class="content-ad"></div>

# 결론

React v19에는 개발을 간단하게 만들고 성능을 향상시키며 React를 이전보다 유연하고 견고하게 만드는 강력한 기능과 개선 사항이 포함되어 있습니다.

TypeScript 개발자로서, 이러한 새로운 도구와 기능은 우리의 생산성과 애플리케이션 품질을 크게 향상시킬 수 있습니다.

이 기능들을 자세히 알아보고 실험해보며 다음 프로젝트를 어떻게 변화시킬 수 있는지 확인해보세요!

<div class="content-ad"></div>

행복한 코딩하세요!

# 평문으로 말해보자 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 clapping하고 팔로우해 주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기