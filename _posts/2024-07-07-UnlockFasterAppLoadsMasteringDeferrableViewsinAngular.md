---
title: "앱 로딩 속도를 높이는 방법 2024년 Angular에서 Deferrable Views 마스터하기"
description: ""
coverImage: "/assets/img/2024-07-07-UnlockFasterAppLoadsMasteringDeferrableViewsinAngular_0.png"
date: 2024-07-07 23:02
ogImage: 
  url: /assets/img/2024-07-07-UnlockFasterAppLoadsMasteringDeferrableViewsinAngular_0.png
tag: Tech
originalTitle: "Unlock Faster App Loads: Mastering Deferrable Views in Angular"
link: "https://medium.com/@hmidihamdi7/unlock-faster-app-loads-mastering-deferrable-views-in-angular-dc693c8653f5"
---



<img src="/assets/img/2024-07-07-UnlockFasterAppLoadsMasteringDeferrableViewsinAngular_0.png" />

Angular 17 이전에는 게으른 로딩이 주로 어플리케이션의 초기 번들 크기를 줄이는 방법으로 모듈에만 제한되어 있었습니다. 게으른 로딩 기능을 더 향상시키기 위해, Angular 커뮤니티는 컴포넌트, 디렉티브 및 파이프에 대한 유사한 기능을 요청했습니다.

이 요청의 가치를 인식한 Angular 팀은 Angular 17에서 지연 로딩을 위한 뷰(views) 기능을 도입했습니다. 이 강력한 기능을 사용하면 특정 컴포넌트, 디렉티브 및 파이프의 로딩을 실제로 필요할 때까지 지연시킬 수 있습니다. 이는 초기 로드 시간과 전반적인 성능을 크게 향상시킵니다.

지연 로딩 기능을 위해 Angular 17에서 도입된 내장 제어 흐름 구문을 활용합니다. 이 새로운 구문에 대해 더 알고 싶다면, 제 최신 게시물을 확인해보세요.


<div class="content-ad"></div>

```js
@defer {
  <heavy-component />
}
```

# Deferrable views의 장점

Deferrable views는 커뮤니티 요청을 해결하기 위해서만이 아니라 사용자에게 상당한 성능 향상도 제공하기 위해 도입되었습니다. 이러한 혜택에는 다음이 포함됩니다:

- 번들 크기 감소: 이는 초기 페이지로드가 더 빨라진다는 것을 의미합니다. 사용자는 응용 프로그램과 상호 작용하기 전에 필요없는 코드를 다운로드하기를 기다릴 필요가 없게 됩니다.
- 향상된 코어 웹...