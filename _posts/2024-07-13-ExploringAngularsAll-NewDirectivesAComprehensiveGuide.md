---
title: "Angular의 새로운 지시어 완벽 가이드 최신 기능 탐구"
description: ""
coverImage: "/assets/img/2024-07-13-ExploringAngularsAll-NewDirectivesAComprehensiveGuide_0.png"
date: 2024-07-13 23:30
ogImage:
  url: /assets/img/2024-07-13-ExploringAngularsAll-NewDirectivesAComprehensiveGuide_0.png
tag: Tech
originalTitle: "Exploring Angular’s All-New Directives: A Comprehensive Guide"
link: "https://medium.com/@abhigyana/exploring-angulars-all-new-directives-a-comprehensive-guide-da98a6b2d88d"
isUpdated: true
---

인기 있는 프론트엔드 웹 프레임워크 인 Angular는 계속해서 발전하며 각 릴리스마다 새로운 기능과 개선 사항을 소개하고 있어요. 최근 업데이트에서 강화된 주요 영역 중 하나는 새로운 지시문(directives)을 소개한 것입니다. 이러한 지시문들은 DOM을 더 강력하고 유연하게 조작하고 Angular 애플리케이션의 기능을 향상시키는 더 강력하고 유연한 방법을 제공하여 개발자들의 업무를 더욱 쉽게 만들고 있어요. 이 게시물에서는 Angular에서 가장 최근에 도입된 일부 지시문들을 살펴보고, 이를 활용하여 더 효율적이고 동적인 웹 애플리케이션을 만드는 방법을 탐색해 볼 거에요.

![이미지](/assets/img/2024-07-13-ExploringAngularsAll-NewDirectivesAComprehensiveGuide_0.png)

# 지시문이란 무엇인가요?

새로운 지시문에 대해 들어가기 전에, Angular에서 지시문이 무엇인지 간단히 되짚어볼까요? 지시문은 DOM 요소에 부착된 특수 표시물로, Angular의 HTML 컴파일러에게 해당 요소에 지정된 동작을 부여하도록 지시하는 것입니다 (또는 DOM 요소 및 해당 자식 요소를 변환시킬 수도 있습니다). 지시문은 Angular의 핵심 구성 요소 중 하나로, HTML의 기능을 향상시켜 새로운 구문을 사용하여 HTML을 확장할 수 있는 기능을 제공하고 있어요.

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

# 새로운 지시사항

## 1. ngClass - 향상됨

ngClass 지시사항이 더 유연하도록 업그레이드되었습니다. 이제 개발자들은 조건에 따라 CSS 클래스를 추가하거나 제거할 수 있습니다. 이 지시사항은 오랫동안 존재해왔지만 최근 업데이트로 더욱 강력해졌습니다.

```js
<div [ngClass]="{'class-name': condition}">컨텐츠</div>
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

이 예시에서는 조건이 참일 때 div에 class-name이 추가됩니다. 향상된 버전은 복잡한 표현식을 지원하며 구성 요소 로직과 더 잘 통합됩니다.

## 2. Async 파이프를 사용한 ngIf

ngIf 지시문은 이제 async 파이프와 원활하게 작동하여 비동기 데이터 스트림을 처리하기가 더 쉬워졌습니다. 이 조합은 템플릿 내에서 observable 데이터 처리를 간단하게 만듭니다.

```js
<div *ngIf="data$ | async as data">
  { data }
</div>
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

이 지시문을 사용하면 템플릿에서 직접 observable을 구독하고 해당 값이 사용 가능할 때 렌더링할 수 있습니다.

## 3. ngSwitch - 향상된 기능

ngSwitch 지시문은 더 직관적이고 강력해졌습니다. 개발자들은 범위 표현식에 따라 조건부로 DOM 구조를 교체할 수 있게 되었습니다.

```js
<div [ngSwitch]="expression">
  <div *ngSwitchCase="'case1'">Case 1</div>
  <div *ngSwitchCase="'case2'">Case 2</div>
  <div *ngSwitchDefault>Default case</div>
</div>
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

**4. ngTemplateOutlet**

새롭게 개선된 ngSwitch 지시자는 이제 더 복잡한 시나리오를 쉽게 처리할 수 있어요. 요소를 조건부로 표시하는 더 견고한 방법을 제공합니다.

ngTemplateOutlet 지시자를 사용하면 준비된 TemplateRef에서 임베디드 뷰를 삽입할 수 있어요. 이는 동적이고 재사용 가능한 컴포넌트를 생성하는 데 특히 유용합니다.

```js
<ng-template #templateRef>
  <div>템플릿 콘텐츠</div>
</ng-template>

<div *ngTemplateOutlet="templateRef"></div>
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

ngTemplateOutlet을 사용하면 템플릿 콘텐츠를 동적으로 삽입하여 구성 요소의 유연성과 재사용성을 향상시킬 수 있어요.

아직 시도해보지 않으셨다면, 이러한 새로운 지시문들을 Angular 프로젝트에 통합해 보세요. 제공된 예제들을 활용해보고, 개발 과정을 어떻게 간소화하고 향상시킬 수 있는지 알아보세요.
