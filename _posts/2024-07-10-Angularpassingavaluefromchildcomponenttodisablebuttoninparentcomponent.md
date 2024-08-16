---
title: "Angular 자식 컴포넌트에서 부모 컴포넌트의 버튼 비활성화하는 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:52
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Angular passing a value from child component to disable button in parent component"
link: "https://medium.com/@fixitblog/solved-angular-passing-a-value-from-child-component-to-disable-button-in-parent-component-349101a0943c"
isUpdated: true
---





부모 요소의 버튼을 비활성화하거나 활성화하기 위해 @Output 및 EventEmitter를 사용하는 방법을 이해하는 데 어려움을 겪고 있습니다.

구체적으로 말하면, 텍스트가 포함된 컨테이너(자식 구성요소)가 맨 아래에서 스크롤될 때, 변수에 대한 부모 구성요소로 불리언 값을 전달하여 [disabled] 속성과 함께 사용하려고 합니다.

스크롤 계산은 이미 작동 중이며, @Output 및 EventEmitter 부분에만 문제가 있습니다.

다음은 코드입니다 - 이것이 자식 구성 요소입니다:

<div class="content-ad"></div>

```js
@Output() buttonDisabledEvent = new EventEmitter<boolean>();

scrolledToBottom() {
  if (
    // container is scrolled to bottom
  ) {
    this.buttonDisabledEvent.emit(false);
    // or something like this maybe?
    this.buttonDisabledEvent.emit(buttonDisabled = false);
  }
}
```

The parent html:

```js
<button
  type="button"
  class="lobyco-btn lobyco-btn-primary"
  [disabled]="buttonDisabled" <--- I need the new value to be used here
>
  Accept
</button>
```

원하는 내용이 명확히 전달되었기를 바라며, 무언가 부족한 점이 있으면 즉시 확인하고 수정하겠습니다. 도움 주셔서 많이 감사합니다!

<div class="content-ad"></div>

# 해결책

자식 컴포넌트에서 `this.buttonDisabledEvent.emit(false);`라고 말한 후에 

이제 ParentComponent HTML에서 이벤트를 캐치해야 합니다.
```js
<app-child (buttonDisabledEvent)="onButtonDisabled($event)"></app-child>

<button
  type="button"
  class="lobyco-btn lobyco-btn-primary"
  [disabled]="buttonDisabled" <--- 새 값이 여기에 사용되어야 합니다
>
  Accept
</button>
```

<div class="content-ad"></div>

위의 텍스트를 한국어로 번역해 드립니다:

부모 컴포넌트의 TS 파일은 아래와 같이 작성됩니다:

```js
export class ParentComponent {
  buttonDisabled = false;

  onButtonDisabled(buttonDisabled: boolean) {
    this.buttonDisabled = buttonDisabled;
  }
}
```

답변자 — kellermat

답변 확인자 — Mildred Charles (FixIt Admin)

<div class="content-ad"></div>

위 답변은 스택오버플로우에서 수집한 것으로, cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0의 라이센스를 따릅니다.