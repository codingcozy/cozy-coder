---
title: "Angular에서 부모와 자식 컴포넌트 간 양방향 바인딩 구현 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-09 10:37
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Implementing Two-Way Binding Between Parent and Child Components in Angular"
link: "https://medium.com/@arunima-anand/implementing-two-way-binding-between-parent-and-child-components-in-angular-31bc4d9d8a5b"
isUpdated: true
---

## Angular을 사용하여 사용자 지정 속성에 양방향 바인딩하는 방법

Angular 개발 세계에서 컴포넌트 간의 효과적인 통신은 동적이고 반응이 뛰어난 애플리케이션을 만드는 데 중요합니다. Angular가 제공하는 강력한 기능 중 하나는 양방향 바인딩입니다. 이를 통해 모델과 뷰 간의 데이터 동기화를 원활하게 할 수 있습니다. 'ngModel'을 사용한 단일 컴포넌트 내에서의 양방향 바인딩에 익숙할 것이지만, 부모 및 자식 컴포넌트 간에 양방향 바인딩을 구현하려면 개념을 조금 더 깊이 이해해야 합니다.

이 기사에서는 Angular에서 부모 및 자식 컴포넌트 간에 양방향 바인딩을 구현하는 방법을 탐색하고 널리 사용되는 ngModel 지시문에 대해 다뤄볼 것입니다.

# 'ngModel'을 사용한 양방향 바인딩 이해하기

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

부모-자식 간 통신에 들어가기 전에 ngModel에 대해 간단히 이야기해보겠습니다. Angular에서 ngModel 지시문은 템플릿 기반 폼에서 양방향 데이터 바인딩을 달성하는 간단하고 강력한 방법입니다. 이를 사용하면 입력 필드를 컴포넌트 클래스의 변수에 바인딩하여 입력의 변화가 변수에 즉시 반영되고 그 반대도 성립하게 됩니다.

다음은 기본적인 예시입니다:

```html
<!-- app.component.html -->
`input [(ngModel)]=”groceryItem” placeholder=”Enter item”/` `p`You need to buy: ' groceryItem '`/p`
```

```typescript
// app.component.ts
export class AppComponent '
groceryItem: string = ‘’;
'
```

이 예시에서는 컴포넌트의 groceryItem 변수가 입력 필드와 바인딩되어 있어, 입력 필드의 변경 사항이 즉시 groceryItem에 반영되고 groceryItem의 변경 사항도 입력 필드에 반영되는 것을 보장합니다.

# Angular의 양방향 바인딩 개념: 프로퍼티 바인딩과 이벤트 바인딩

Angular에서는 양방향 바인딩을 위해 프로퍼티 바인딩과 이벤트 바인딩을 모두 활용합니다.

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

부모 구성 요소로부터 자식 구성 요소로 데이터를 바인딩 할 수 있게 해주는 Angular의 Property binding(@Input())은 속성 이름을 대괄호에 넣어서 사용합니다. [propertyName]와 같이요. 예를 들어, [title] = “parentTitle”은 부모의 parentTitle 속성을 자식 구성 요소의 title 속성에 전달합니다.

Event binding(@Output())은 자식 구성 요소가 사용자 정의 이벤트를 발생시키고 부모 구성 요소가 그 이벤트를 수신하고 처리할 수 있도록 하는데 사용됩니다. (eventName)=“methodName()”과 같이 이벤트 이름을 괄호로 묶어 사용합니다. 예를 들어, (notify) = “onNotify($event)”은 부모 구성 요소에서 자식 구성 요소가 발생시킨 notify 이벤트를 수신하고, $event를 통해 전달된 데이터로 onNotify 메소드를 호출합니다. 이러한 바인딩은 Angular 애플리케이션의 구성요소 간에 원활한 통신과 상호작용을 가능케 합니다.

# 부모와 자식 구성 요소 간 양방향 바인딩 구현하기

ngModel을 사용하여 양식 입력 데이터를 모델에 직접 바인딩하는 것은 강력한 기능이지만, 항상 프로그래밍 목표와 일치하지는 않을 수 있습니다. 예를 들어 복잡한 응용 프로그램에서는 단일 양식 내에서만이 아니라 여러 구성 요소 간의 상태나 데이터 흐름을 관리해야 할 수도 있습니다. 부모와 자식 구성 요소 간에 데이터 바인딩이 필수적인 실제 예로는 동적 대시보드 애플리케이션이 있습니다. 부모 구성 요소는 사용자 활동의 요약을 표시하고, 자식 구성 요소는 이러한 활동을 날짜별로 필터링할 수 있게 해 줍니다. 사용자가 자식 구성 요소에서 날짜 범위를 선택하면, 부모 구성 요소는 선택한 범위 내의 활동을 표시하기 위해 표시를 업데이트해야 합니다. 이런 구성 요소 간의 통신은 애플리케이션의 데이터가 일관되고 사용자 상호작용에 반응적이 되도록 보장하며, 끊김 없는 직관적인 사용자 경험을 제공합니다.

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

부모 및 자식 컴포넌트 간의 양방향 바인딩을 구현하려면 Angular에서 @Input() 및 @Output() 데코레이터와 EventEmitter 클래스를 제공합니다.

부모 컴포넌트와 자식 컴포넌트 간 양방향 바인딩을 구현하는 예제를 살펴봅시다. 부모 컴포넌트는 작업 목록을 관리하고, 자식 컴포넌트는 새 작업 추가를 처리합니다.

단계 1: 자식 컴포넌트 생성

먼저, 사용자 입력을 처리할 자식 컴포넌트를 생성하십시오.

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

```typescript
// child.component.ts
import { Component, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "app-child",
  template: `
    <input [(ngModel)]="newTask" placeholder="Enter new task" />
    <button (click)="addTask()">Add Task</button>
  `,
})
export class ChildComponent {
  newTask: string = "";

  @Output() taskAdded = new EventEmitter<string>();

  addTask() {
    this.taskAdded.emit(this.newTask);
    this.newTask = "";
  }
}
```

Step 2: Update the Parent Component

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

다음으로, 부모 컴포넌트를 업데이트하여 작업 추가를 처리하십시오.

```typescript
// parent.component.ts
import 'Component' from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child (taskAdded)="onTaskAdded($event)"></app-child>
    <ul>
      <li *ngFor="let task of tasks">{{ task }}</li>
    </ul>
  `
})
export class ParentComponent {
  tasks: string[] = [];

  onTaskAdded(task: string) {
    this.tasks.push(task);
  }
}
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

Step 3: 구성 요소 등록하기

모듈에 두 구성 요소가 등록되어 있는지 확인하세요.

```typescript
// app.module.ts
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { FormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";
import { ParentComponent } from "./parent.component";
import { ChildComponent } from "./child.component";

@NgModule({
  declarations: [AppComponent, ParentComponent, ChildComponent],
  imports: [BrowserModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
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

Step 4: 결과 확인하기

이제 자식 컴포넌트의 입력란에 작업을 입력하고 "작업 추가"를 클릭하면 해당 작업이 상위 컴포넌트로 전송되어 작업 목록이 업데이트됩니다.

# 요약

Angular에서 부모 및 자식 컴포넌트간의 양방향 바인딩은 @Input() 및 @Output() 데코레이터와 EventEmitters를 사용하여 효율적으로 구현할 수 있습니다. ngModel은 단일 컴포넌트 내에서 양방향 바인딩을 달성하는 간편한 방법을 제공하지만, 부모-자식 통신 접근 방식은 컴포넌트가 모듈화되고 유지 보수 가능하도록 해줍니다.

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

위 기술들을 활용하여, 유동적이고 반응성 있는 Angular 애플리케이션을 만들어 나갈 수 있습니다.

드는 독자 여러분께, Angular의 양방향 데이터 바인딩에 관한 이 글에 대한 생각과 피드백을 공유할 것을 권장합니다. 여러분의 통찰은 이해와 명확성을 향상시키는 데 소중합니다. 만약 오류를 발견하거나 추가적인 주제에 대한 제안이 있다면, 아래 댓글로 언제든지 알려주시기 바랍니다. 참여해 주셔서 감사합니다! 즐거운 코딩하세요!

#Angular #WebDevelopment #TwoWayBinding #ParentChildCommunication #Component #NgModel
