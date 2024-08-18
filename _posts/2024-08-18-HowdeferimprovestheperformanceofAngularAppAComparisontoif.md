---
title: "Angular 앱의 성능을 향상시키는 defer의 효과 if와의 비교"
description: ""
coverImage: "/assets/img/2024-08-18-HowdeferimprovestheperformanceofAngularAppAComparisontoif_0.png"
date: 2024-08-18 11:11
ogImage: 
  url: /assets/img/2024-08-18-HowdeferimprovestheperformanceofAngularAppAComparisontoif_0.png
tag: Tech
originalTitle: "How defer improves the performance of Angular App A Comparison to if"
link: "https://medium.com/angular-simplified/optimizing-angular-performance-with-defer-a-comparison-to-if-2d25f2dd1984"
isUpdated: false
---


<img src="/assets/img/2024-08-18-HowdeferimprovestheperformanceofAngularAppAComparisontoif_0.png" />

앵귤러에서는 버전 17에서 @if 및 @defer를 도입했습니다. 둘 다 특정 조건이 참인 경우 화면에 요소를 렌더링합니다. 이제 두 가지가 조건부 렌더링을 한다면 두 가지 사이에는 무엇이 다른 것인지 궁금할 것입니다. 언제 어떻게 사용해야 할까요? 이를 통해 애플리케이션 성능을 향상하는 방법에 대해 알아봅시다.

# @if과 @defer 이해

## @if

<div class="content-ad"></div>

`@if`은 조건이 참일 때 컴포넌트를 렌더링하고 거짓인 경우 숨기는 기본 제어 흐름 데코레이터입니다.

```js
@if(conditon){
 <div>
   조건이 참일 때 표시할 내용
 </div>
}
```

## `@defer`

`@defer` 지시어는 조건이 true일 때 컴포넌트를 DoM에 렌더링합니다. 컴포넌트 렌더링을 true 조건이 될 때까지 지연시킬 수 있습니다.

<div class="content-ad"></div>


@defer (condition) {
  <large-component />
}


# @if, @switch, @defer 비교

![이미지](/assets/img/2024-08-18-HowdeferimprovestheperformanceofAngularAppAComparisontoif_1.png)

# 언제 어떤 것을 사용해야 할까요?


<div class="content-ad"></div>

- **@if**: 즉시 렌더링이 필요하고 컴포넌트가 비교적 가벼운 간단한 조건에 이상적입니다.
- **@defer**: 복잡한 컴포넌트나 초기 사용자 경험에 중요하지 않은 컴포넌트에 가장 적합합니다.

# 결론

@if와 @defer는 조건부 렌더링의 목적을 달성하는 데 사용됩니다. 그러나 성능 영향은 다릅니다. 사용 사례를 신중히 고려함으로써 개발자는 애플리케이션 성능을 최적화하고 사용자 경험을 향상시키기 위해 @defer를 활용할 수 있습니다.