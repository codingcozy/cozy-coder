---
title: "알면 좋은 것과 나쁜 것 알아두어야 하는 프론트엔드 도구와 기술"
description: ""
coverImage: "/assets/img/2024-08-18-TheGoodandtheBadFront-EndToolsandTechniquesYouShouldKnow_0.png"
date: 2024-08-18 11:07
ogImage: 
  url: /assets/img/2024-08-18-TheGoodandtheBadFront-EndToolsandTechniquesYouShouldKnow_0.png
tag: Tech
originalTitle: "The Good and the Bad Front-End Tools and Techniques You Should Know"
link: "https://medium.com/gitconnected/the-good-and-the-bad-front-end-tools-and-techniques-you-should-know-bd8acc9aa980"
isUpdated: false
---


![image](/assets/img/2024-08-18-TheGoodandtheBadFront-EndToolsandTechniquesYouShouldKnow_0.png)

지난 몇 년 동안, $1.7B GenAI 스타트업에서 일하면서 다양한 프론트엔드 도구 및 기술을 접했습니다.

회사가 성장함에 따라, 우리 팀은 전 세계의 엔지니어들이 기여하는 조직화된 프론트엔드 저장소를 유지하는 방법에 대한 다양한 결정을 내려야 했습니다.

이 글에서는 이러한 도구에 대한 제 경험을 공유하고 "❌ 고려" 또는 "✅ 지지" 중 하나로 평가하겠습니다.

<div class="content-ad"></div>

- ❌ 고려해 보세요: 프로젝트에 대한 대안을 고려하는 것을 추천드립니다.
- ✅ 지지합니다: 망설임 없이 이것을 프로젝트에서 재사용할 것입니다.

# 코드 품질과 도구

## ESLint

🔍 고려

<div class="content-ad"></div>

설명: JavaScript 코드에서 문제를 식별하고 해결하여 일관된 코딩 표준을 시행하는 정적 코드 분석 도구입니다.

긍정적인 점:
ESLint는 버그를 찾아내고 일관된 코딩 표준을 분산된 팀 전체에 적용하는 데 도움을 줍니다. React를 위한 여러 규칙들로 확장 가능합니다.

부정적인 점:
ESLint는 느릴 수 있습니다. 제 경험상, 빌드 파이프라인 시간에 상당한 영향을 미치며, 캐싱 및 변경된 파일에만 실행하는 기술을 사용해도 그 시간을 감소시키기 어렵습니다.

일상적인 작업 흐름에서, 저는 커밋 전에 Git 후크로 ESLint를 실행합니다. 파일을 약간만 수정해도 ESLint가 보통 5-15초가 걸리는데, 이는 작은 커밋을 선호하는 저에게는 귀찮은 일일 수 있습니다.

<div class="content-ad"></div>

ESLint의 잠재적인 대체품으로 biome을 고려해 볼 수 있습니다. biome은 현재 ESLint 규칙을 모두 지원하지는 않지만 상당히 빠릅니다.

## Git 훅

✅ 지지

설명:
Husky나 pre-commit과 같은 도구로 관리되는 Git 훅은 코드베이스에서 작업하는 모든 사람이 변경 사항을 커밋하기 전에 동일한 작업을 수행하도록 보장합니다.

<div class="content-ad"></div>

긍정적으로 말씀드리자면, pre-commit Git 훅은 팀 전반에 걸쳐 일관된 관행을 강제함으로써 풀 리퀘스트를 올릴 때 빌드가 깨지는 횟수를 줄이는 데 도움을 줍니다. 여기에는 pre-commit 훅에 포함할 수 있는 몇 가지 작업이 있습니다:

- 변경된 파일 형식 맞추기
- 변경된 파일에 ESLint 실행하기
- 미임포트 파일 확인하기
- 순환 종속성 확인하기
- 커밋 메시지를 접두어(예: 티켓 번호)와 함께 형식 맞추기, 이렇게 함으로써 봇이 자동으로 Jira/Linear 티켓을 연결할 수 있습니다.

## TypeScript

✅ 지지합니다

<div class="content-ad"></div>

설명:
최근 몇 년 동안 TypeScript의 인기가 상당히 증가했지만, Svelte 창조자인 Rich Harris와 같은 사람들은 때로는 필수가 아니라고 주장합니다.

![이미지](/assets/img/2024-08-18-TheGoodandtheBadFront-EndToolsandTechniquesYouShouldKnow_1.png)

긍정적 측면:
전 세계의 엔지니어들과 함께 대규모 분산 코드베이스에서 작업 중이라면 TypeScript를 고려할 가치가 있습니다. ESLint보다 빠르게 실행되며, 출시하는 코드에 강력한 확신을 제공하고, 리팩토링을 쉽게 만듭니다.

부정적 측면:
TypeScript는 tsc를 실행해야 하므로 CI 파이프라인에 일부 추가 빌드 시간을 추가할 수 있습니다.

<div class="content-ad"></div>

또한, 특히 react-hook-form과 같은 복잡한 유형의 패키지를 다룰 때 지루할 수도 있습니다. 이러한 경우에 대해 TypeScript 특정 문제를 해결하는 데 종종 JavaScript 전용 프로젝트에는 존재하지 않는 버그를 디버깅해야 했습니다.

## 미사용 파일 식별 도구

✅ 추천

설명:
Unimported(더 이상 유지되지 않음)와 knip과 같은 도구는 코드베이스에서 사용되지 않는 파일을 식별하는 데 도움을 줍니다.

<div class="content-ad"></div>

긍정적:
코드를 정리할 때 사용하지 않는 파일을 놓치기 쉽습니다. 이러한 도구들은 삭제해도 되는 파일을 감지하여 코드베이스를 깨끗하고 효율적으로 유지하는 데 도움을 줍니다.

이 글을 쓰는 동안, Unimported가 더 이상 유지되지 않는 것을 알았습니다. 저는 직접 knip을 사용한 적은 없지만, Unimported에서 knip으로 마이그레이션하고 추가 기능을 탐색하는 노력을 할 가치가 있는 것으로 보입니다.

# 빌드 및 패키지 관리

## Vite

<div class="content-ad"></div>

### 환영합니다

소개:
빠른 개발 및 최적화된 프로덕션 빌드를 제공하는 차세대 프론트엔드 빌드 도구입니다.

긍정적인 점:
바벨에서 Vite로 전환하면 생산성이 크게 향상될 수 있습니다. 제 경험에 따르면 빌드 시간이 5분에서 40초로 줄었습니다.

게다가 Vite의 '즉시 서버 시작' 기능은 로컬 개발 서버를 즉시 시작하여 매일 소중한 시간을 절약할 수 있습니다.

<div class="content-ad"></div>

## 러나

✅ 추천

설명:
러나는 JavaScript 모노 레포지토리의 관리를 간단하게 만들어줍니다.

장점:
지금까지 러나를 사용하며 원활한 경험을 해왔어요. 주요 문제 없이 JavaScript 모노 레포지토리를 관리하는 데 신뢰할 만하고 안정적인 도구입니다.

<div class="content-ad"></div>

## 프로토콜 버퍼

✅ 지지

설명:
언어 중립적이고 플랫폼 중립적이며 확장 가능한 구조화된 데이터 직렬화 메커니즘으로, 분산 시스템 간에 효율적으로 데이터를 정의하고 교환하는 데 사용됩니다.

장점:
프로토콜 버퍼는 특히 수십 명의 엔지니어로 이루어진 대규모 코드베이스에서 API 표준에 대해 분산된 팀을 정렬하는 데 탁월한 도구입니다. 이들은 일관적이고 효율적인 데이터 형식을 제공함으로써 복잡한 응용 프로그램을 구축하는 프로세스를 간소화합니다.

<div class="content-ad"></div>

부정적인 측면:
그러나 Protocol Buffers를 설정하는 것은 복잡할 수 있고, 시스템에 익숙하지 않은 사람들에게는 도전적일 수 있는 학습 곡선이 있습니다.

부가 정보:
Protocol Buffers와 같은 코드 생성 도구가 실제로 어떻게 작동하는지 궁금했던 적이 있나요? Google Summer of Code 프로젝트 중에 Google의 도구 프레임워크에 기여했는데, 이를 깊이 있게 탐구하게 되었습니다. 해당 프레임워크는 현재 GitHub에서 약 300개의 별을 받고 있습니다.

# 테스트

## Jest를 사용한 단위 테스트

<div class="content-ad"></div>

**표를 고쳐주세요**

설명:
JavaScript 테스팅 프레임워크.

장점:
애플리케이션의 비즈니스 로직에 중점을 둔 유닛 테스트는 버그를 발견하는 데 효과적입니다. 예를 들어, 백엔드에서 데이터를 변환하거나 다양한 객체로 복잡한 API 요청을 생성하는 상황에서 유닛 테스트가 빛을 발합니다.

우리는 SWC를 사용하여 Jest의 유닛 테스트 실행 시간을 4분에서 1분 정도로 크게 줄였어요.

<div class="content-ad"></div>

Vitest를 사용하는 것 또한 테스트 속도를 향상시키는 좋은 방법일 수 있습니다.

부정적인 측면:
나는 React 컴포넌트에 대한 유닛 테스트를 작성하는 것을 좋아하지 않아요, 특히 버튼을 클릭하여 팝오버를 열거나 하는 단순한 상호작용을 위한 것들에 대해서 말이에요. 이러한 테스트는 구현 및 유지에 상당한 시간이 필요하나, 그에 비해 제한적인 가치를 제공합니다.

## Playwright

✅ 추천

<div class="content-ad"></div>

내용:
작가의 인기가 지난 몇 년 동안 상당히 증가했으며, 그로 인해 Playwright가 E2E에 대한 올바른 선택임을 확신하게 되었습니다.

![이미지](/assets/img/2024-08-18-TheGoodandtheBadFront-EndToolsandTechniquesYouShouldKnow_2.png)

장점:
나는 우리의 E2E 테스트 인프라를 구축하는 역할을 맡았고, 그 프레임워크로 Playwright를 선택했습니다. 내 경험은 매우 긍정적이었습니다. 가장 중요한 기능을 다루는 몇 가지 포괄적인 E2E 테스트에 초점을 맞추고, 각 구성 요소에 대한 작은 단위 테스트를 구현하기보다는 사용자의 경로를 따르도록 권장합니다.

우리는 관련 있는 E2E 테스트가 풀 리퀘스트에서 트리거되도록 시스템을 설정했으며, 실패하는 경우 엔지니어는 문제를 분석하기 위해 직접 Trace Viewer에 연결됩니다.

<div class="content-ad"></div>

부정적:
E2E 테스트 유지하는 일이 조금 지루할 수 있어요. 처음에는 개발자들이 새로운 기능과 함께 E2E 테스트를 작성하도록 했지만, 코드베이스가 발전함에 따라 기존 테스트를 유지하고 새로운 테스트를 작성할 전용 QA 팀이 필요하다는 점을 깨달았어요.

## UI 프레임워크와 라이브러리

### Mantine

✅ 추천

<div class="content-ad"></div>

**설명:**
Mantine은 완전한 오픈 소스 React 컴포넌트 라이브러리입니다.

**장점:**
Mantine은 이해하기 쉽고 프로젝트에 쉽게 적응하고 통합할 수 있는 우수한 컴포넌트 세트를 제공합니다. 라이브러리는 완벽하게 작동하며 주요 개발자가 Discord에서 매우 활발하게 활동하여 강력한 커뮤니티 지원을 제공합니다. 제 경험에서 Mantine과 관련된 어떤 문제도 발견하지 못했습니다.

매우 추천할 수 있습니다.

**## Highcharts**

<div class="content-ad"></div>

### 고려사항

**설명:**
다양한 대화형 시각화를 만들 수 있는 포괄적 차트 라이브러리입니다.

**장점:**
Highcharts에는 다양한 사용 사례를 다루는 복잡한 차트를 구축할 수 있는 잘 문서화된 API가 있습니다.

**단점:**
그러나 Highcharts는 유료 라이브러리이며, 대부분의 사용 사례에는 Chart.js와 같은 무료 대안이 충분할 수 있습니다.

<div class="content-ad"></div>

또 다른 단점은 TypeScript의 타입과 인터페이스가 하나의 거대한 파일로 통합되어 있어서 IDE의 성능을 상당히 느리게 만들 수 있다는 것입니다. 그 결과로 종종 온라인 문서를 의존해야 했습니다.

게다가 특정한 사용 사례에 대해서는 Highcharts가 제 요구사항을 충족시키지 못했기 때문에 필요한 차트를 구축하기 위해 D3를 사용해야 했습니다.

## D3

❌ 고려해 보세요

<div class="content-ad"></div>

**설명:**
복잡하고 사용자 정의된 데이터 시각화를 만들기 위한 유연한 JavaScript 라이브러리입니다.

**긍정적인 점:**
D3는 좀 더 간단한 라이브러리로 다루기 어려운 복잡한 계층 차트를 생성할 때 매우 가치 있는 도구입니다. 매우 유연하며 다양한 예제와 함께 상세한 문서가 제공되어 있어 사용자를 안내해줍니다.

**부정적인 점:**
하지만, D3는 높은 학습 곡선을 가지고 있습니다. 당신이 필요로 하는 것을 달성할 수 없는 상위 차트 라이브러리가 없을 때에만 사용하는 것을 추천합니다.

라이브러리의 복잡성이 매우 높기 때문에 문서의 튜토리얼과 예제에 많은 의존을 하게 될 것입니다.

<div class="content-ad"></div>

## dnd 킷

✅ 퍼스트<br>

설명:
dnd 킷은 드래그 앤 드롭 기능을 구현하는 라이브러리입니다.

긍정적인 면:
dnd 킷은 사용자가 폼 요소를 드래그하고 다시 배치할 수 있는 복잡한 중첩된 양식을 구축하는 데 훌륭하다고 생각합니다. 이러한 시나리오에서 제게 잘 도움이 되었습니다.

<div class="content-ad"></div>

# 상태 관리

## TanStack Query

✅ 추천

설명:
TanStack Query는 React 애플리케이션에서 API 호출과 서버 상태를 효과적으로 관리하는 강력한 라이브러리입니다.

<div class="content-ad"></div>

긍정적인 피드백:
TanStack Query를 사용하면 항상 긍정적인 경험만을 누릴 수 있어요. 데이터 검색, 캐싱 및 동기화를 간편하게 처리해주어, 직접 검색 로직을 구현하는 것보다 훨씬 쉽게 만들어줘요. 이것은 의심할 여지없이 모든 프론트엔드 프로젝트에 포함되어야 할 가장 중요한 라이브러리 중 하나에요.

## TanStack Table

✅ 추천

설명:
TanStack Table은 강력하고 사용자 정의 가능한 헤드리스 UI 라이브러리로, 테이블을 구축하는 데 사용됩니다.

<div class="content-ad"></div>

긍정적인 면:
TanStack Table을 사용한 경험은 전적으로 긍정적이었습니다. 이는 특정한 필요에 맞는 견고한 테이블 컴포넌트를 만들 수 있는 유연성을 제공합니다.

## Zustand

✅ 지지

설명:
Zustand은 React 애플리케이션을 위한 간단하고 확장 가능한 상태 관리 라이브러리입니다.

<div class="content-ad"></div>

긍정적:
Zustand은 React Context와 비교해 더 쉽고 확장 가능하기 때문에 React 프로젝트에서 상태를 관리하는 데 좋은 선택입니다.

## React hook form

❌ 고려 사항

Description:
React Hook Form은 React 애플리케이션에서 폼을 빌드하고 관리하는 인기 있는 라이브러리입니다.

<div class="content-ad"></div>

친절한 말투로 번역:

긍정적:
해당 라이브러리는 잘 작동하며 GitHub 커뮤니티가 크기 때문에 복잡한 폼을 만드는 데 필요한 기능을 제공합니다.

부정적:
TypeScript와 함께 사용할 때 문제를 겪었습니다. 특히 복잡한 폼의 유형 안정성 유지에 어려움을 겪었습니다. 재귀 폼 및 해당 유형 처리도 까다로웠습니다.

TanStack Form이나 Mantine Form과 같은 대안들은 TypeScript 관련 문제가 덜 발생할 수 있습니다.

# 유틸리티 라이브러리

<div class="content-ad"></div>

## Lodash

✅ Approve

설명:
Lodash는 편리한 함수들로 코드를 더 깔끔하고 읽기 쉽게 만들어줍니다.

긍정적:
Lodash 함수들은 코드를 보다 간결하고 가독성 있게 만들어줍니다. 제가 자주 사용하는 함수로는 groupBy, cloneDeep 등이 있습니다.

<div class="content-ad"></div>

내 코드를 더 깔끔하게 만들고 싶을 때는 종종 GPT에게 Lodash 함수를 사용하여 리팩토링해달라고 요청하곤 하는데, 결과적으로 대부분 개선이 있었습니다.

## Madge

✅ 추천

설명:
Madge는 코드 베이스에서 순환 종속성을 감지하고 방지하기 위한 도구입니다.

<div class="content-ad"></div>

긍정적인 측면:
Jest로 작성된 단위 테스트는 저장소 내에서 순환 종속성이있을 때 예기치 않은 실패를 일으킬 수있으므로 부실할 수 있습니다. Madge를 통합하여 이러한 문제를 피할 수 있습니다. Madge는 빠르기도 하기 때문에 작업 과정에 상당한 시간을 추가하지 않습니다.

부정적인 측면:
순환 종속성을 피하기 위해서는 많은 파일을 다른 위치로 리팩토링해야할 수도 있습니다. 새로운 기능을 시작할 때 파일을 어디에 놓고, 그들이 어떻게 참조되는지에 대해 신중해야 합니다. 미리 고려하지 않으면 나중에 전체 파일 구조를 재조직해야할 수도 있습니다.

## 덜

✅ 추천

<div class="content-ad"></div>

설명:
Less는 스타일 파일을 더 유지보수하기 쉽고 이해하기 쉽도록 도와주는 CSS 전처리기입니다.

긍정적인 측면:
Less를 사용하면 복잡한 스타일 작업을 할 때 스타일 파일을 더 쉽게 읽고 관리할 수 있습니다.

부정적인 측면:
IDE에서 Less 구문을 완전히 지원하지 않는 경우가 있어서 "선언으로 이동(Go to declaration)"과 같은 작업이 항상 작동하지는 않습니다.

저는 CSS 클래스 이름을 다루는 대신 Mantine와 같은 라이브러리를 사용하여 JSX 내에서 직접 DOM 요소를 스타일링하는 것이 더 효율적이라고 생각했습니다.

<div class="content-ad"></div>

예를 들어, 저는 이 방법을 선호합니다:

```js
// Mantine을 사용하여 패딩, 마진 및 배경을 가진 플렉스박스 사용
const BoxExample = () => {
  return (
    <Flex p="1rem" mb="1rem" bg="white" justify="center">
      Mantine Flex로 스타일링됨
    </Box>
  );
};
```

다음 예제보다:

```js
// styles.less
.styled-div {
  padding: 1rem
  background-color: white
  margin-bottom: 1rem
  display: flex
  justify-conent: center
}

const LessExample = () => {
  return <div className="styled-div">LESS로 스타일링됨</div>;
};
```

<div class="content-ad"></div>

저의 흔한 방식은 Mantine에서 제공하는 속성 (예: p, mb 등)을 최대한 활용하여 스타일을 적용하고, 필요할 때만 CSS 또는 Less로 되돌아가는 것입니다.

이 방식을 채택하면 레이아웃을 빠르게 이해할 수 있고, 외부 스타일 파일이 필요한 경우를 줄일 수 있습니다. JSX 코드에 속성이 과도하게 사용되어 가독성을 떨어뜨리는 것을 방지하면서 컴포넌트의 레이아웃을 이해하기 쉽게 유지할 수 있는 좋은 균형을 이룹니다. Tailwind처럼 가독성이 떨어지는 문제를 피하는 데에도 도움이 됩니다.

더 알아보기: