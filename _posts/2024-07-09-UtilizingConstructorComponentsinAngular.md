---
title: "Angular에서 Constructor 컴포넌트 활용 방법"
description: ""
coverImage: "/assets/img/2024-07-09-UtilizingConstructorComponentsinAngular_0.png"
date: 2024-07-09 10:34
ogImage: 
  url: /assets/img/2024-07-09-UtilizingConstructorComponentsinAngular_0.png
tag: Tech
originalTitle: "Utilizing Constructor Components in Angular"
link: "https://medium.com/@bobrovice/utilizing-constructor-components-in-angular-2289cd950051"
isUpdated: true
---






![Constructor components in Angular](/assets/img/2024-07-09-UtilizingConstructorComponentsinAngular_0.png)

Angular의 Constructor 구성 요소들은 ng-content의 모든 가능성을 최대한 활용할 수 있는 강력한 방법을 제공합니다. 이러한 구성 요소들은 응용 프로그램의 계층 구조를 단순화하고, 여러 @Input 속성의 필요성을 줄이며, 핵심 기능에 초점을 맞추어 구성 요소의 사용자 정의를 향상시킵니다. 이 안내서는 Angular에서 constructor 구성 요소를 사용하고 어떻게 그로부터 혜택을 얻을 수 있는지 설명합니다.

# 예시 시나리오: 간단한 카드 구성 요소

다음과 같이 보통의 카드 구성 요소를 고려해 보겠습니다:


<div class="content-ad"></div>

```js
<div class="container">
  <span>{ title }</span>
  <div class="body">{ content }</div>
  <div>
    <button [disabled]="buttonsDisabled">OK</button>
    <button [disabled]="buttonsDisabled">CANCEL</button>
  </div>
</div>
```

이 예제에서 title, content 및 buttonsDisabled는 @Input 속성입니다. 카드에는 CSS 클래스로 정의된 container 및 body가 있습니다. 그러나 title 및 버튼과 같은 요소는 카드의 핵심 구조의 일부가 아니라 별도의 기능입니다.

# ng-content로 리팩토링하기

비핵심 요소를 ng-content로 대체하여 카드 컴포넌트를 간단하게 만들고 더 유연하게 만들 수 있습니다. 이렇게 하면 카드가 동적 콘텐트의 컨테이너로 작동할 수 있습니다. 동적 콘텐트는 부모 컴포넌트에서 제공할 수 있습니다.

<div class="content-ad"></div>

여기에 리팩토링된 카드 컴포넌트의 모습을 소개합니다:

```js
<div class="container">
  <ng-content select="[header]"></ng-content>
  <div class="body">
    <ng-content select="[body]"></ng-content>
  </div>
  <ng-content select="[footer]"></ng-content>
</div>
```

이 버전에서는 @Input 속성이 더 이상 필요하지 않습니다. 대신 ng-content가 동적 콘텐츠를 위한 자리 표시자 역할을 하며, 부모 컴포넌트에서 header, body, footer와 같은 속성을 사용하여 지정된 내용을 나타냅니다.

# 리팩토링된 컴포넌트 사용하기

<div class="content-ad"></div>

리팩토링된 카드 컴포넌트를 사용하려면 부모 컴포넌트에서 다음과 같이 내용을 정의할 수 있어요:

```js
<app-constructor-card>
  <app-titles-container header></app-titles-container>
  <app-contents-container body></app-contents-container>
  <app-buttons-container footer></app-buttons-container>
</app-constructor-card>
```

각 컨테이너(titles-container, contents-container, buttons-container)는 각각의 로직을 캡슐화하고 독립적으로 테스트할 수 있어요.

<img src="https://miro.medium.com/v2/resize:fit:852/1*WqeRE4DdLnuQqJzZWw7mdw.gif" />

<div class="content-ad"></div>

# 생성자 컴포넌트의 이점

- 단순화된 컴포넌트 계층 구조: ng-content를 사용하여 @Input 속성을 여러 수준을 통과하지 않아도 되므로 복잡성이 줄어듭니다.
- 향상된 사용자 정의: 중복 속성 없이 컴포넌트는 핵심 기능에 집중하므로 더 유연한 디자인을 가능케 합니다.
- 향상된 테스트성: 컴포넌트의 각 부분을 독립적으로 테스트할 수 있습니다. 예를 들어 생성자-카드가 각 부분(헤더, 본문, 푸터)을 렌더링할 수 있는지와 각 부분이 전체 카드 로직을 고려하지 않고 올바르게 기능하는지만 테스트하면 됩니다.
- 단일 책임 원칙 준수: 특정 작업을 처리하도록 설계된 컴포넌트로 유지 관리성과 가독성이 향상됩니다.

# 결론

생성자 컴포넌트와 ng-content를 채택하면 의도된 기능에만 집중하는 매우 커스터마이즈 가능하고 유지보수성이 높은 컴포넌트를 만들 수 있습니다. 이 접근 방식은 컴포넌트 계층 구조를 간단화하는 것 뿐만 아니라 Angular 애플리케이션의 유연성과 테스트 가능성을 향상시킵니다. 다른 컴포넌트에도 이러한 원칙을 적용하여 핵심 기능과 분리할 수 있는 기능을 식별하여 모듈성과 명확성을 더욱 향상시킬 수 있습니다.