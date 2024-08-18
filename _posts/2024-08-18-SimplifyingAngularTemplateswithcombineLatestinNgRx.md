---
title: "NgRx에서 combineLatest를 활용하여 Angular 템플릿을 간편하게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-SimplifyingAngularTemplateswithcombineLatestinNgRx_0.png"
date: 2024-08-18 11:11
ogImage: 
  url: /assets/img/2024-08-18-SimplifyingAngularTemplateswithcombineLatestinNgRx_0.png
tag: Tech
originalTitle: "Simplifying Angular Templates with combineLatest in NgRx"
link: "https://medium.com/stackademic/simplifying-angular-templates-with-combinelatest-in-ngrx-d6a0d4dd1184"
isUpdated: false
---



![image](/assets/img/2024-08-18-SimplifyingAngularTemplateswithcombineLatestinNgRx_0.png)

Angular과 NgRx를 사용할 때 가장 흔한 과제는 상태 셀렉터들을 깔끔하고 가독성있게 효과적으로 관리하는 것입니다. combineLatest 함수는 이러한 문제에 강력한 해결책을 제공하며, 특히 Angular의 async 파이프와 ng-container와 함께 사용할 때 뛰어난 결과를 보여줍니다. 이 접근법을 사용하면 더 간결하고 유지보수가 쉬운 템플릿을 생성할 수 있어 가독성과 성능을 모두 향상시킬 수 있습니다.

## combineLatest 이해하기

RxJS의 combineLatest 함수는 NgRx가 의존하는 것으로, 여러 개의 옵저버블을 결합하는 데 사용됩니다. NgRx 셀렉터와 함께 사용할 때 상태의 여러 조각을 단일 옵저버블 스트림으로 결합할 수 있어 구성 요소에서 이를 구독할 수 있게 해줍니다.


<div class="content-ad"></div>

여기에 기본적인 예제가 있습니다:

```js
data$ = combineLatest([
  sample1: this.#store.select(sampleSelector1)),
  sample2: this.#store.select(sampleSelector2))
])
```

이 예제에서 data$는 selector 중 하나가 새 값이 발생할 때마다 sample1과 sample2를 포함한 객체를 발행하는 observable입니다. 이 결합된 observable은 Angular 템플릿에서 직접 사용할 수 있습니다.

# ng-conteiner와 async 파이프 활용하기

<div class="content-ad"></div>

가장 효과적인 방법 중 하나는 Angular 템플릿에서 combineLatest를 사용하는 것입니다. 이 방법은 ng-container와 async 파이프를 사용하는 것으로 시작합니다. 이 접근 방식은 템플릿을 간소화하는데 도움이 되며, 비동기 구독 수를 줄이는 방법으로 성능을 향상시킵니다.

다음은 구현하는 방법입니다:

```js
<ng-container *ngIf="data$ | async as data">
  <div>{ data.sample1 }</div>
  <div>{ data.sample2 }</div>
</ng-container>
```

# 장점

<div class="content-ad"></div>

combineLatest을 ng-container 및 async pipe와 함께 사용하는 것은 여러 가지 이점을 제공합니다:

- 단일 구독: combineLatest를 사용하면 템플릿에 async pipe가 하나만 필요하므로 구독 수가 줄어들고 템플릿 로직이 간소화됩니다.
- 깔끔한 템플릿: ng-container 지시문을 사용하면 단일 데이터 객체 내에서 모든 결합된 상태 선택기에 직접 액세스할 수 있어 템플릿을 간결하고 가독성 있게 유지할 수 있습니다.
- 성능 향상: 더 적은 구독은 더 적은 오버헤드를 의미하며, 특히 복잡한 구성 요소에서는 성능이 향상됩니다.
- 보일러플레이트 감소: observable$ | async as value" 지시문을 여러 개 관리하는 대신 하나만 관리하므로 템플릿의 보일러플레이트 코드 양이 줄어듭니다.

# 모두 함께 사용하기

두 가지 다른 선택기에서 사용자 세부 정보 및 환경 설정을 표시하는 구성 요소를 고려해보세요. combineLatest 없이는 템플릿이 이와 같이 보일 수 있습니다:

<div class="content-ad"></div>

```js
<div *ngIf="user$ | async as user">
  <div>{ user.name }</div>
</div>
<div *ngIf="preferences$ | async as preferences">
  <div>{ preferences.theme }</div>
</div>
```

combineLatest을 사용하면 다음과 같이 간소화할 수 있습니다:

```js
<ng-container *ngIf="data$ | async as data">
  <div>{ data.user.name }</div>
  <div>{ data.preferences.theme }</div>
</ng-container>
```

이 접근 방식은 더 깔끔하게 보이며 컴포넌트가 복잡해지더라도 더 효율적으로 확장할 수 있습니다.


<div class="content-ad"></div>

# 결론

Angular 템플릿에서 ng-container 및 async pipe와 함께 combineLatest를 사용하면 NgRx에서 상태 선택기를 더 효율적이고 깨끗하게 유지할 수 있는 방법을 제공합니다. 이 패턴을 채택하면 템플릿의 복잡성을 줄이고 코드를 더 쉽게 읽고 작성하며 유지할 수 있습니다.

다음 Angular 프로젝트에서 이 접근 방식을 시도해보고 코드베이스에 얼마나 큰 영향을 미칠 수 있는지 경험해보세요!

더 많은 Angular 팁과 트릭을 기대해주세요! 👨‍💻

<div class="content-ad"></div>

이 글을 즐겨 보셨다면 연결하고 싶으시다면 망설이지 마시고 LinkedIn에서 저를 팔로우해주세요!