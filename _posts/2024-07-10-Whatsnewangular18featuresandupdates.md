---
title: "2024년 최신 Angular 18 기능 및 업데이트 사항 알아보기"
description: ""
coverImage: "/assets/img/2024-07-10-Whatsnewangular18featuresandupdates_0.png"
date: 2024-07-10 00:53
ogImage: 
  url: /assets/img/2024-07-10-Whatsnewangular18featuresandupdates_0.png
tag: Tech
originalTitle: "What’s new angular 18 features and updates?"
link: "https://medium.com/@angularminds/whats-new-angular-18-features-and-updates-1142b3be03ff"
isUpdated: true
---






![Image](/assets/img/2024-07-10-Whatsnewangular18featuresandupdates_0.png)

2024년 5월 22일에 출시된 Angular 18은 인기있는 웹 개발 프레임워크에 흥미로운 새로운 기능과 개선 사항을 가져왔습니다. 이 블로그 포스트에서는 이러한 개선 사항을 자세히 살펴보며, 개발자들이 성능이 뛰어나고 견고하며 개발자 친화적인 Angular 애플리케이션을 구축하는 데 어떻게 도움이 되는지 탐구합니다.

1. 시그널을 이용한 Zoneless 변경 감지

Angular 18의 가장 중요한 변경 사항 중 하나는 시그널을 이용한 zoneless 변경 감지의 도입입니다. 이전에 Angular은 변경 감지에 Zone.js를 사용했으며, 이는 오버헤드와 복잡성을 도입할 수 있었습니다. Zoneless 변경 감지는 더 가볍고 효율적인 접근 방식을 제공하여 응용 프로그램의 성능 향상을 이끌 수 있습니다.


<div class="content-ad"></div>

Signals는 Angular 18의 새로운 개념으로, 값이 변경될 때 변경 감지를 트리거하는 Observables로 작동합니다. 이는 응용 프로그램 상태 업데이트를 관리하는 더 세밀하고 반응적인 방법을 제공합니다.

2. 함수 기반의 라우트 리디렉션

라우트 리디렉션은 Angular 응용 프로그램에서 흔히 사용되는 방법입니다. Angular 18에서는 함수를 사용하여 라우트 리디렉션을 정의할 수 있는 새로운 방법이 도입되었습니다. 이를 통해 더 동적이고 유연한 리디렉션 로직을 구현할 수 있습니다. 예를 들어, 이제 사용자를 인증 상태 또는 다른 응용 프로그램 조건에 따라 리디렉션할 수 있습니다.

3. 개선된 개발자 도구

<div class="content-ad"></div>

이 Angular 18 기능은 향상된 툴링을 통해 개발자 경험을 개선합니다. Angular DevTools 확장 프로그램은 이제 애플리케이션 성능, 의존성 관리, 상태 변경에 대한 더 나은 통찰력을 제공합니다. 이는 개발자가 애플리케이션을 디버깅하고 최적화하는 데 큰 도움이 될 수 있습니다.

4. 향상된 Angular Material 및 CDK

Angular Material은 Angular용 UI 구성 요소를 개발하는 인기 있는 라이브러리이며 Angular CDK(Component Dev Kit)는 유틸리티 디렉티브와 동작의 컬렉션입니다. 이러한 라이브러리들은 Angular 18과 완전히 호환되고 최적화되도록 안정성 업데이트를 받았습니다.

5. 서버 측 렌더링(SSR) 향상

<div class="content-ad"></div>

검색 엔진 최적화(SEO) 혜택이 필요한 애플리케이션이나 초기 페이지로드 속도가 빨라야 하는 경우, 서버 측 렌더링(SSR)은 귀중한 기술입니다. Angular 18에서는 SSR에 대한 개선 사항을 소개하는데, 이는 클라이언트 측 동작을 서버에서 모방하여 사용자 경험을 더 부드럽게 만드는 이벤트 리플레이를 포함하고 있습니다.

6. 향상된 국제화(i18n) 지원

세계적인 관객을 대상으로 하는 애플리케이션을 구축하려면 견고한 국제화 지원이 필요합니다. Angular 18에서는 새로운 @angular/localize 패키지를 소개하여 번역 관리 및 서로 다른 로캘에 따라 날짜, 통화 및 숫자의 형식을 구성하는 프로세스를 간소화했습니다.

7. TypeScript 5.4 지원

<div class="content-ad"></div>

Angular 18은 이제 JavaScript의 인기 있는 상위 집합인 TypeScript 5.4를 완전히 지원합니다. 이를 통해 개발자들은 Angular 애플리케이션 내에서 TypeScript 5.4에서 소개된 새로운 기능과 개선 사항을 활용할 수 있습니다.

8. 추가 기능

위에서 언급한 주요 하이라이트를 넘어서, Angular 18에는 다양한 다른 개선 사항이 있습니다:

- ng-content에서 기본 콘텐츠: Angular 18의 이 기능을 사용하면 프로젝트된 콘텐츠가 없는 경우 컴포넌트의 ng-content 프로젝션 내에서 대체 콘텐츠를 표시할 수 있습니다.
- 향상된 빌드 성능: Angular 18의 빌드 프로세스가 최적화되어 애플리케이션의 빌드 시간을 단축시킬 수 있습니다.
- WebAssembly 지원: 아직 개발자 미리보기 단계이지만, WebAssembly 지원은 Angular 애플리케이션에서 성능 최적화에 대한 새로운 가능성을 탐색할 수 있게 합니다.

<div class="content-ad"></div>

결론

Angular 18에서의 새로운 기능들은 기존 애플리케이션을 업그레이드하거나 다음 프로젝트에 Angular을 고려할 동기부여 요인을 제시합니다. 성능 및 개발자 경험의 향상부터 개선된 라우팅 및 국제화 지원까지, Angular 18은 개발자들에게 현대적이고 확장 가능하며 사용자 친화적인 웹 애플리케이션을 구축할 수 있는 기회를 제공합니다.

Angular에 대한 추가 정보는 아래 링크를 확인해보세요:

- https://www.angularminds.com/blog/category/angular