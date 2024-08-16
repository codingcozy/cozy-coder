---
title: "Angular에서 컴포넌트를 제대로 생성하고 스타일링하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-CreatingandstylingcomponentsinAngularcorrectly_0.png"
date: 2024-07-06 11:05
ogImage: 
  url: /assets/img/2024-07-06-CreatingandstylingcomponentsinAngularcorrectly_0.png
tag: Tech
originalTitle: "Creating and styling components in Angular correctly"
link: "https://medium.com/stackademic/creating-and-styling-components-in-angular-correctly-52c93b062759"
isUpdated: true
---




![](/assets/img/2024-07-06-CreatingandstylingcomponentsinAngularcorrectly_0.png)

# 소개

Angular에서 컴포넌트는 사용자 인터페이스를 만들기 위한 기본적인 구성 요소입니다. Angular의 컴포넌트는 독립적이고 재사용 가능한 코드 블록으로, 템플릿(HTML 마크업), 스타일(CSS) 및 컴포넌트 로직(TypeScript)을 결합합니다.

언제든지 Angular 애플리케이션을 보고 있는지 알 수 있습니다. 그냥 DevTools를 열고 DOM을 분석하면 됩니다. 일반적인 div와 span 요소 외에도 app-_, ng-_, mat-\* 등 대체적으로 Angular 컴포넌트가 있습니다.

<div class="content-ad"></div>

생성하는 구성 요소는 모두 DOM 요소입니다. render()를 호출하는 컨테이너가 아니며, 뒤에 선택기 대신 정확한 템플릿을 전달합니다. 이 부분 때문에 Angular가 display:contents를 가진 요소를 생성한다고 생각할 수 있지만, 컴포넌트는 기본적으로 display:inline 스타일을 가지므로 요소에 무게가 있습니다.

# 가벼우면서 사용자 지정 가능한 컴포넌트

일부 개발자들은 이 부분을 놓치고 컴포넌트 요소와 함께 작업할 수 있도록 래퍼 요소를 만듭니다. 그래서 컴포넌트의 내용을 제어할 수 없습니다. 래퍼 스타일은 DOM 트리 아래에 위치하므로 ViewEncapsulation.None 또는 ::ng-deep를 활용하여 컴포넌트 요소에 접근할 방법을 찾아야 합니다.

당신의 컴포넌트는 이미 래퍼입니다. 스타일이 없을 뿐입니다. 그러므로 요소를 그룹화하고 정렬하기 위해 컴포넌트 내에 `div`/div` 래퍼를 만들지 마십시오. 컴포넌트의 스타일 파일에 필요한 설정과 함께 :host를 추가하는 것이 충분합니다.

<div class="content-ad"></div>

```js
:host {
  // 기본값은 div로 설정됩니다.
  display: block;
  // -- 블록 설정 --


  // 플렉스 컨테이너
  display: flex;
  // -- 플렉스 설정 --


  // 그리드 컨테이너
  display: grid;
  // -- 그리드 설정 --
}
```

데이터-\* 설정에도 동일하게 적용됩니다. 모든 컴포넌트 스타일 상태를 :host 안에서 선언하세요.

```js
:host {
  // 다른 스타일
  &[data-status="success"]{
    background: green;
  }
  &[data-status="warn"]{
    background: yellow;
  }
  &[data-status="error"]{
    background: red;
  }
}
```

그리고 입력을 통해 이를 제어할 수 있습니다.

<div class="content-ad"></div>

```js
@HostBinding('[attr.data-status]')
@Input()
public status: 'success' | 'warn' | 'error' | 'default' = 'default'
```

이것이 무엇을 하는 건가요?

- 불필요한 요소가 없어서 DOM이 가볍게 유지됩니다.
- wrapper-classes를 만들 필요가 없습니다.
- 부모 컴포넌트는 ViewEncapsulation.None이나 :ng-deep을 사용하지 않고 컴포넌트 엘리먼트와 직접 작업할 수 있으므로 당신의 컴포넌트에 스타일을 적용할 수 있습니다.

# 다시 만들지 말고, 더 나은 상속하세요

<div class="content-ad"></div>

예를 들어, 'table'을 생성하고 테이블의 'tr' 행을 구성 요소로 사용하고 싶다고 가정해 봅시다.

개발자가 테이블 요소를 지원하는 컴포넌트를 만들기 원할 때 기본적으로 사용하는 방법은 무엇인가요? — 스타일이 'tr'인 컴포넌트를 만드는 것.

```js
:host {
  display: table-row;
}
```

하지만 이 방법이 올바르지 않은 이유가 무엇인가요?

<div class="content-ad"></div>

컴포넌트 선택기는 템플릿 중에서 일치하는 항목을 찾아 요소 대신 컴포넌트를 생성하는 매개변수입니다. 즉, 네이티브 querySelector() 함수와 함께 작업할 때 허용된 태그, 속성, id, CSS 클래스 등을 선택기로 지정할 수 있습니다.

따라서 네이티브 동작을 복제하는 요소를 생성하려면 흔히 사용하는 태그 대신 tr[my-table-row]과 같이 지정할 수 있습니다.

템플릿에서는 다음과 같이 적용합니다

```js
<table>
   <tr *ngFor="let row of rows" my-table-row [data]="row"></tr>
</table>
```

<div class="content-ad"></div>

이렇게 하면 의미를 보존할 수 있고 표가 개별 부분으로 분해되도록 할 수 있어요. 구성 요소는 이미 `tr` 속성이 있기 때문에 :host에 따로 쓸 필요가 없어요.

이 방법은 td[my-table-cell]를 통해 셀에 적용할 수도 있어요.

# 호스트 속성

구성 요소가 요소인 것을 알고 있으면 구성 요소 내부에서 CSS 클래스나 속성을 적용할 수도 있어요. 전역 CSS 클래스를 상속하거나 자체 고유한 값으로 요소 식별자를 만드는 것과 같아요.

<div class="content-ad"></div>

이러한 경우에는 아마도 이렇게 할 것입니다

```js
@HostBinding('class.flex')
private readonly _classFlex = true

@HostBinding('attr.id')
private readonly _id = window.crypto.randomUUID()
```

하지만 이러한 값은 요소를 만들 때에만 필요하며, 다시 그릴 때 변경되지 않고 어떠한 방식으로도 상호 작용하지 않습니다.

이러한 정적 바인딩을 호스트에서 선언하고 컴포넌트 클래스에서 배제할 수 있습니다. 이러한 정적 바인딩을 @Component에서 선언하세요.

<div class="content-ad"></div>

```js
@Component({
 // ...설정
 host: {
   '[class.flex]': 'true',
   '[attr.id]': 'id'
 }
})
export class SomeComponent {
  public readonly id = window.crypto.randomUUID();
}
```

HostBinding을 통한 동일한 바인딩이지만, 클래스 코드가 더 깔끔해졌습니다. 이 접근 방식을 유지하기 위해 추가적인 import가 필요하지 않습니다.

동일한 방식으로 컴포넌트 요소에 애니메이션을 바인딩할 수도 있습니다. 이를 통해 컴포넌트 내부에 불필요한 요소를 만드는 것을 피할 수 있습니다.

```js
@Component({
 // ...설정
 host: {
   '[@fade]': '' // 속성 없이,
   '[@expand]': 'props'
 }
})
```

<div class="content-ad"></div>

# 믹스인

당신의 코드베이스에는 텍스트를 위한 전역 믹스인이 있다고 가정해보세요

```js
@mixin text-m(){
 font-size: 14px;
}


@mixin text-h2(){
 font-size: 16px;
 font-weight: bold;
}
```

공통 스타일 믹스인을 사용하고 싶은 컴포넌트를 개발 중입니다

<div class="content-ad"></div>

```css
.title {
  @include mixins.text-h2();
}

.description {
  @include mixins.text-m();
}
```

브라우저를 열어보면 모든 것이 작동하고 스타일이 적용된 아름다운 사진을 볼 수 있어요

![](/assets/img/2024-07-06-CreatingandstylingcomponentsinAngularcorrectly_1.png)

<div class="content-ad"></div>

잘못된 점은 무엇인가요? — 불필요한 중복 스타일을 만들었습니다.

이상적인 경우에는 전역 mixin이 있는 경우에는 해당 mixin을 사용하는 전역 CSS 선택자 .text-m 및 .text-h2를 가져야 합니다.

따라서, 전역 스타일을 반복하는 각 선택기에 대해 스타일을 만들지 않고 해당 요소에 적용하는 것이 옳습니다.

```js
- <p class="title">
+ <p class="title text-h2">
   Title
</p>
- <p class="description">
+ <p class="description text-m">
   Description
</p>
```

<div class="content-ad"></div>

새로운 셀렉터를 사용하여 이러한 요소의 특정성을 반영하는 다른 스타일을 적용하거나 사용하지 않는 것이 좋습니다.

```js
-@ 사용 ='../../styles/mixins';

.title {
-@ include mixins.text-h2()
+ line-height: 21px;
}

.description {
-@ include mixins.text-h2()
+ color: grey
}
```

스타일링 믹스인은 클래스 속성을 통해 요소에 스타일을 적용할 수 없을 때에만 사용해야 합니다. 디자인 킷에서 가져온 대부분의 스타일은 집중화되어 재활용되어야 합니다.

# 공유 스타일

<div class="content-ad"></div>

런타임에서 두 컴포넌트에서 반복되는 여러 CSS 클래스와 속성을 발견했다고 가정해봅시다.

이것이 코드 중복이며 크기를 증가시킨다는 것을 이해했고, 이 문제를 최적화해야 한다는 것을 알았습니다.

개발자로서 무엇을 할 것인가요? — 그 스타일들을 `domain`/shared/styles/text.scss 파일에 일반적인 CSS 스타일로 이동시키고 필요한 컴포넌트에서 재사용하는 것입니다.

- shared/styles/text.scss

<div class="content-ad"></div>

```js
.title {
  line-height: 21px;
}

.description {
  color: grey;
}
```

- Component 1

```js
@Component({
- styleUrl: './some-1.component.scss',
+ styleUrls: ['./some-1.component.scss', '../shared/styles/text.scss'],
})
```

- Component 2

<div class="content-ad"></div>

```js
@Component({
-  styleUrl: './some-2.component.scss',
+  styleUrls: ['./some-2.component.scss', '../shared/styles/text.scss'],
})
```

브라우저를 열어보니 현재 컴포넌트에는 이점 또는 영향이 없다는 것을 알게 되었어요. 잘 했어요!

하지만 실제로는 아무것도 변경되지 않았어요. 모든 게 그대로 있죠. 스타일이 중복되어 있었기 때문에 변한 건 없어요.

이제 Angular는 styleUrls에 각 파일마다 별도의 `style` 태그를 생성할 뿐이며 그 이상도 그 이하도 아닙니다.

<div class="content-ad"></div>

컴포넌트에서 스타일 반복을 줄이려면 주요 목표는 애플리케이션 내에서 스타일을 사용 가능하게 하고 재사용하는 것입니다.

이렇게 할 수 있는 방법은 3가지 있습니다:

- CSS 클래스가 다른 부분의 애플리케이션과 겹치지 않고 재사용을 위해 추상적으로 명명된 경우 - 해당 클래스를 styles.scss에 가져오세요.
- CSS 클래스가 다른 부분의 애플리케이션과 겹치지 않지만 특정하게 명명되었으며 전역적으로 사용하려는 경우 - CSS 클래스의 이름을 바꾸고 styles.scss에 가져오세요.
- 스타일이 애플리케이션의 다른 부분과 겹치는 경우 특정 부분에서만 필요한 경우 - ViewEncapsulation.None으로 이러한 요소의 공통 부모에 넣으세요.

중복 스타일이 문제가 아니라 CSS 사용을 제어하는 방법으로 고려하세요. 이러한 컴포넌트 중 하나를 제거하면 중복 코드도 함께 제거되고 스타일은 단일 인스턴스로 남게 됩니다.

<div class="content-ad"></div>

# 결론

보시다시피, Angular에서 컴포넌트를 생성하는 것이 항상 쉽고 최적화된 방법은 아닙니다. 처음에는 좋아보이는 몇 가지 방법이 실은 더 해를 끼칠 수 있습니다.

# 링크

- [Angular 공식문서: 뷰 캡슐화](https://angular.io/guide/view-encapsulation)
- [Angular 공식문서: 컴포넌트 스타일](https://angular.io/guide/component-styles#host)
