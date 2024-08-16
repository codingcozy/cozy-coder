---
title: "Angular FormArray 구현 단계별 튜토리얼"
description: ""
coverImage: "/assets/img/2024-07-10-AStep-by-StepTutorialonAngularFormArrayImplementation_0.png"
date: 2024-07-10 00:47
ogImage: 
  url: /assets/img/2024-07-10-AStep-by-StepTutorialonAngularFormArrayImplementation_0.png
tag: Tech
originalTitle: "A Step-by-Step Tutorial on Angular FormArray Implementation"
link: "https://medium.com/front-end-weekly/a-step-by-step-tutorial-on-angular-formarray-implementation-ce09cec51b01"
isUpdated: true
---






<img src="/assets/img/2024-07-10-AStep-by-StepTutorialonAngularFormArrayImplementation_0.png" />

Angular의 FormArray는 Angular 애플리케이션에서 동적 폼을 관리하는 강력한 도구입니다. 이를 사용하면 폼 컨트롤의 수가 고정되어 있지 않고 런타임 중에 동적으로 변경될 수 있는 시나리오를 처리할 수 있습니다. 본 강좌에서는 FormArray를 깊게 파헤쳐 구현 단계별 안내를 제공할 것입니다. Angular 프로젝트 설정부터 중첩 FormArrays 처리까지 모두 다룰 예정입니다.

# 섹션 1: FormArray 이해하기

# 1.1 Reactive Forms 소개


<div class="content-ad"></div>

Angular은 두 종류의 양식을 제공합니다: 템플릿 기반 양식과 반응형 양식. 반응형 양식은 더 견고하고 더 나은 제어를 제공하여 복잡한 상황에 적합합니다. 반응형 양식은 FormGroup 및 FormControl 클래스를 중심으로 구축되며 FormArray는 이러한 클래스들의 확장으로, 동적 양식 컨트롤을 다루기 위해 설계되었습니다.

# 1.2 FormArray 사용 시기

FormArray는 양식 컨트롤의 수가 가변적인 양식을 처리할 때 필수적입니다. 예를 들어, 쇼핑 카트용 양식을 생성하고 사용자가 동적으로 항목을 추가하거나 제거할 수 있는 시나리오를 상상해보세요. 카트의 각 항목에는 이름, 수량, 가격과 같은 여러 속성이 있을 수 있습니다. FormArray를 사용하면 이러한 상황을 우아하게 처리할 수 있습니다.

# 섹션 2: Angular 프로젝트 설정하기

<div class="content-ad"></div>

# 2.1 Angular CLI 설치하기

만약 Angular CLI를 아직 설치하지 않았다면, 다음 명령어를 사용하여 설치할 수 있습니다:

```js
npm install -g @angular/cli
```

# 2.2 새로운 Angular 프로젝트 생성하기

<div class="content-ad"></div>

Angular CLI를 사용하여 새로운 Angular 프로젝트를 생성해보겠습니다:

```js
ng new angular-form-array-tutorial
cd angular-form-array-tutorial
```

# 섹션 3: FormArray를 사용한 반응형 폼 생성

# 3.1 컴포넌트 생성

<div class="content-ad"></div>

정리를 위해 새로운 form 컴포넌트를 생성해보겠습니다:

```js
ng generate component form-array-example
```

# 3.2 필요한 모듈 가져오기

form-array-example.component.ts 파일을 열고 필요한 모듈을 import하세요.

<div class="content-ad"></div>

```js
// form-array-example.component.ts

import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormArray, Validators } from '@angular/forms';

@Component({
  selector: 'app-form-array-example',
  templateUrl: './form-array-example.component.html',
  styleUrls: ['./form-array-example.component.css']
})
export class FormArrayExampleComponent implements OnInit {

  myForm: FormGroup;

  constructor(private fb: FormBuilder) { }

  ngOnInit(): void {
    this.myForm = this.fb.group({
      items: this.fb.array([]) // Initialize an empty FormArray
    });
  }

  // Other form-related methods will go here

}
```

## 3.3 Basic Form Structure

이제 form-array-example.component.html 파일에 간단한 폼 구조를 생성해 봅시다:

```js
<!-- form-array-example.component.html -->

<form [formGroup]="myForm">
  <!-- Form controls go here -->
</form>
```

<div class="content-ad"></div>

# 섹션 4: 동적으로 양식 컨트롤 추가하기

# 4.1 항목 추가 메서드 생성하기

form-array-example.component.ts 파일에서, 동적으로 양식 컨트롤을 추가하는 메서드를 만들어봅시다. 이를 addItem이라고 부를 것입니다:

```js
// form-array-example.component.ts

addItem() {
  const item = this.fb.group({
    // 여기에 양식 컨트롤을 정의하세요
  });

  // 새로운 FormArray를 FormArray에 추가합니다
  this.items.push(item);
}

// 'items' FormArray를 가져오는 도우미 메서드
get items() {
  return this.myForm.get('items') as FormArray;
}
```

<div class="content-ad"></div>

# 4.2 HTML에 버튼 추가하기

당신의 컴포넌트의 HTML (form-array-example.component.html)에 addItem 메서드를 호출하는 버튼을 추가해주세요:

```js
<!-- form-array-example.component.html -->

<button (click)="addItem()">Add Item</button>
```

# 4.3 폼 컨트롤 커스터마이징하기

<div class="content-ad"></div>

addItem 메서드를 확장하여 양식 컨트롤을 사용자 정의하실 수 있습니다. 예를 들어, `name`과 `quantity` 필드에 유효성 검사를 추가해보겠습니다:

```js
// form-array-example.component.ts

addItem() {
  const item = this.fb.group({
    name: ['', Validators.required],
    quantity: [1, [Validators.required, Validators.min(1)]],
    // 필요에 따라 추가적인 양식 컨트롤을 추가하세요
  });

  this.items.push(item);
}
```

이제 당신의 양식 컨트롤에는 유효성 검사 규칙이 적용되었습니다. 필요에 따라 규칙을 사용자 정의하여 변경하십시오.

# 섹션 5: FormArray 유효성 검사

<div class="content-ad"></div>

## 5.1 Validation의 중요성

어떤 형식이든 Validation은 중요합니다. 우리의 형식 컨트롤에 몇 가지 기본적인 유효성을 추가해 봅시다. validators를 포함하여 addItem 메소드를 업데이트해 보세요:

```js
// form-array-example.component.ts

addItem() {
  const item = this.fb.group({
    name: ['', Validators.required],
    quantity: [1, [Validators.required, Validators.min(1)]],
    // 필요한만큼 더 많은 폼 컨트롤을 추가하세요
  });

  this.items.push(item);
}
```

## 5.2 유효성 규칙 사용자 정의하기

<div class="content-ad"></div>

특정 요구 사항에 맞게 검증 규칙을 맞춤화하세요. Validators.required 및 Validators.min과 같은 유효성 검사기는 단순한 예시일 뿐입니다. 필요에 맞게 더 복잡한 유효성 검사 논리를 추가할 수 있습니다.

# 섹션 6: 중첩 FormArrays 사용하기

# 6.1 중첩 FormArrays 소개

실제 상황에서는 각 항목이 하위 항목 목록을 가진 중첩 FormArrays가 필요한 경우가 발생할 수 있습니다. 예를 들어, 각 항목에는 하위 항목 목록이 있을 수 있습니다. 우리의 예제에 중첩 FormArray를 포함하여 확장해 봅시다.

<div class="content-ad"></div>

```js
// form-array-example.component.ts

ngOnInit(): void {
  this.myForm = this.fb.group({
    items: this.fb.array([
      this.fb.group({
        name: ['', Validators.required],
        quantity: [1, [Validators.required, Validators.min(1)]],
        subItems: this.fb.array([]) // 중첩된 FormArray
      })
    ])
  });
}

// 'subItems' FormArray를 'item' 내부에서 가져오는 도우미 메서드
getSubItems(itemIndex: number) {
  return (this.items.at(itemIndex) as FormGroup).get('subItems') as FormArray;
}

addSubItem(itemIndex: number) {
  const subItem = this.fb.group({
    // 여기서 하위 항목 폼 컨트롤을 정의하세요
  });

  // 새로운 하위 항목 폼 그룹을 중첩된 FormArray에 추가합니다
  this.getSubItems(itemIndex).push(subItem);
}
```

# 6.2 HTML 구조 업데이트

HTML에서 중첩된 FormArray를 수용할 수 있도록 구조를 업데이트하십시오:

```js
<!-- form-array-example.component.html -->

<div *ngFor="let item of items.controls; let i = index">
  <div>
    <label>품목 { i + 1 }:</label>
    <input formControlName="name" placeholder="이름">
    <input formControlName="quantity" placeholder="수량">
    <button (click)="addSubItem(i)">하위 항목 추가</button>
  </div>

  <div formArrayName="subItems">
    <div *ngFor="let subItem of getSubItems(i).controls; let j = index">
      <!-- 여기에서 하위 항목 컨트롤을 표시합니다 -->
    </div>
  </div>
</div>
```

<div class="content-ad"></div>

이제 Angular FormArray 구현에 대한 심층적인 튜토리얼을 마치겠습니다. 리액티브 폼을 설정하는 방법을 배웠습니다.

# 읽어 주셔서 감사합니다!

이 기사가 유용하셨기를 바랍니다. 궁금한 점이나 제안 사항이 있으면 댓글을 남겨주세요. 여러분의 피드백이 나를 더 나아지게 하는 데 도움이 됩니다.

구독을 잊지 마세요⭐️

<div class="content-ad"></div>

페이스북 페이지: [DesignTechWorld](https://www.facebook.com/designTechWorld1)

인스타그램 페이지: [Tech-Design](https://www.instagram.com/techd.esign/)

유튜브 채널: [Tech.Design](https://www.youtube.com/@tech..Design/)

트위터: [sumit_singh2311](https://twitter.com/sumit_singh2311)