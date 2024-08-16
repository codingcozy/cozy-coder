---
title: "단방향 데이터 흐름과 추상화로 Angular 컴포넌트 성능 높이는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-EnhancingAngularComponentswithUnidirectionalDataFlowandAbstraction_0.png"
date: 2024-07-10 00:40
ogImage: 
  url: /assets/img/2024-07-10-EnhancingAngularComponentswithUnidirectionalDataFlowandAbstraction_0.png
tag: Tech
originalTitle: "Enhancing Angular Components with Unidirectional Data Flow and Abstraction"
link: "https://medium.com/@bobrovice/enhancing-angular-components-with-unidirectional-data-flow-and-abstraction-d936fe8fb8e5"
isUpdated: true
---






![EnhancingAngularComponents](/assets/img/2024-07-10-EnhancingAngularComponentswithUnidirectionalDataFlowandAbstraction_0.png)

Angular 컴포넌트의 품질을 향상시키는 방법은 두 가지 모범 사례를 통해 달성할 수 있습니다: 단방향 데이터 흐름(UDF)을 활용하고 의존성 역전 원칙을 적용하여 추상화를 만드는 것입니다.

다음 컴포넌트를 고려해 보세요:

```javascript
@Component({
  selector: 'app-common',
  standalone: true,
  imports: [AsyncPipe],
  styleUrl: './common.component.scss',
  template: `
    <div class="container">
      <button
        class="red"
        [disabled]="redDisabled$ | async"
        (click)="redClick()"
      >Red</button>
      
      <button
        class="yellow"
        [disabled]="yellowDisabled$ | async"
        (click)="yellowClick()"
      >Yellow</button>
      
      <button
        class="green"
        [disabled]="greenDisabled$ | async"
        (click)="greenClick()"
      >Green</button>
    </div>
  `
})
export class CommonComponent {
  readonly redDisabled$ = new BehaviorSubject(false);
  readonly yellowDisabled$ = new BehaviorSubject(false);
  readonly greenDisabled$ = new BehaviorSubject(false);

  redClick() {
    this.redDisabled$.next(!this.redDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }

  yellowClick() {
    this.redDisabled$.next(!this.redDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }

  greenClick() {
    this.yellowDisabled$.next(!this.yellowDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }
}
```

<div class="content-ad"></div>

현재의 redClick, yellowClick 및 greenClick 메서드 구현은 로직을 컴포넌트와 긴밀하게 결합시킵니다. 기본 기능에는 편리해 보일 수 있지만, 나중에 문제가 발생할 수 있습니다. 컴포넌트는 주로 HTML, CSS 및 버튼 상태와 같은 UI 관련 사항에 주로 초점을 맞춰야 합니다. 이러한 메서드에 캡슐화된 비즈니스 로직은 클라이언트 요구에 따라 변경될 수 있습니다. 이는 로직에 대한 변경이 여러 테스트를 다시 작성하고 모든 기능이 제대로 작동하는지 버튼 클릭을 통해 UI를 수동으로 테스트해야 한다는 것을 의미합니다. 또한, 컴포넌트가 외부 로직에 의존하는 경우 전반적인 복잡성이 상당히 증가합니다.

# 단방향 데이터 흐름 (UDF)

UDF에서 로직은 부모 컴포넌트로 상위로 이동되어 하위 컴포넌트는 순수히 프리젠테이셔널하게 됩니다. 이전 컴포넌트가 UDF로 변환되었을 때의 모습은 다음과 같습니다:

```js
@Component({
  selector: 'app-udf',
  standalone: true,
  imports: [AsyncPipe],
  styleUrl: './udf.component.scss',
  template: `
    <div class="container">
      <button
        class="red"
        [disabled]="redDisabled"
        (click)="onRedClick.emit()"
      >Red</button>
      
      <button
        class="yellow"
        [disabled]="yellowDisabled"
        (click)="onYellowClick.emit()"
      >Yellow</button>
      
      <button
        class="green"
        [disabled]="greenDisabled"
        (click)="onGreenClick.emit()"
      >Green</button>
    </div>
  `
})
export class UdfComponent {
  @Input() redDisabled: boolean | null = false;
  @Input() yellowDisabled: boolean | null = false;
  @Input() greenDisabled: boolean | null = false;

  @Output() onRedClick = new EventEmitter<void>();
  @Output() onYellowClick = new EventEmitter<void>();
  @Output() onGreenClick = new EventEmitter<void>();
}
```

<div class="content-ad"></div>

UDF에서는 모든 로직이 상위 컴포넌트로 이동됩니다. 이로 인해 상위 컴포넌트가 복잡해지고 관리하기 어려워질 수 있습니다. UDF는 하위 컴포넌트를 더 깔끔하게 만들어줍니다. 이는 하위 컴포넌트의 관심사를 프레젠테이션으로 분리하여 중앙화된 로직을 만드는 것인데, 이는 변경이나 의존성에 취약한 "더러운" 공간이 될 수 있습니다. 이 복잡성은 테스트를 방해하고 유지 관리성을 낮출 수 있습니다. 이상적으로는 이러한 로직을 완전히 추출하여 하위 및 상위 컴포넌트 모두 깨끗하게 유지하고 각자의 업무에 집중할 수 있는 방법이 있으면 좋겠습니다.

# 디펜던시 인젝션으로 추상화

부모 컴포넌트를 로직으로 어지럽히지 않도록 DI를 활용할 수 있습니다. 해결책은 간단합니다: 컴포넌트와 전용 로직 클래스 사이의 다리 역할을 하는 추상 클래스를 생성하는 것입니다.

추상화를 정의하세요:

<div class="content-ad"></div>

```js
export abstract class IComponentAbstraction {
  abstract readonly redDisabled$: Observable<boolean>;
  abstract readonly yellowDisabled$: Observable<boolean>;
  abstract readonly greenDisabled$: Observable<boolean>;

  abstract redClick(): void;
  abstract yellowClick(): void;
  abstract greenClick(): void;
}
```

이 추상화를 사용하도록 구성 요소를 업데이트하십시오:

```js
@Component({
  selector: 'app-abstractions',
  standalone: true,
  imports: [AsyncPipe],
  styleUrl: './abstractions.component.scss',
  template: `
    <div class="container">
      <button 
        class="red"
        [disabled]="abstraction.redDisabled$ | async"
        (click)="abstraction.redClick()"
      >빨강</button>
      
      <button
        class="yellow"
        [disabled]="abstraction.yellowDisabled$ | async"
        (click)="abstraction.yellowClick()"
      >노랑</button>
      
      <button
        class="green"
        [disabled]="abstraction.greenDisabled$ | async"
        (click)="abstraction.greenClick()"
      >초록</button>
    </div>
  `
})
export class AbstractionsComponent {
  constructor(readonly abstraction: IComponentAbstraction) {}
}
```

추상화를 구현하십시오:

<div class="content-ad"></div>

```typescript
@Injectable({ 
  providedIn: 'root'
})
export class LogicImpl implements IComponentAbstraction {
  readonly redDisabled$ = new BehaviorSubject(false);
  readonly yellowDisabled$ = new BehaviorSubject(false);
  readonly greenDisabled$ = new BehaviorSubject(false);

  redClick() {
    this.redDisabled$.next(!this.redDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }

  yellowClick() {
    this.redDisabled$.next(!this.redDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }

  greenClick() {
    this.yellowDisabled$.next(!this.yellowDisabled$.value);
    this.greenDisabled$.next(!this.greenDisabled$.value);
  }
}
```

AppModule 혹은 standalone AppComponent에서 구현을 제공하는 것을 잊지 마세요:

```typescript
providers: [
  { provide: IComponentAbstraction, useExisting: LogicImpl }
]
```

# 결론

<div class="content-ad"></div>

일방향 데이터 흐름을 채택하고 추상화를 생성함으로써 UI 구성 요소에서 논리를 분리하여 테스트 및 유지 관리를 더 쉽게 할 수 있습니다. 이 방법을 통해 구성 요소는 표현에 집중하는 동시에 논리는 필요에 따라 독립적으로 테스트하고 수정할 수 있습니다.