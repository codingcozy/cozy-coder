---
title: "Angular Signals로 Reactive Form 변경 사항 처리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HandlingReactiveFormchangewithAngularSignals_0.png"
date: 2024-07-09 10:30
ogImage: 
  url: /assets/img/2024-07-09-HandlingReactiveFormchangewithAngularSignals_0.png
tag: Tech
originalTitle: "Handling Reactive Form change with Angular Signals."
link: "https://medium.com/stackademic/handling-reactive-form-change-with-angular-signals-69dae7cd3f78"
---


<img src="/assets/img/2024-07-09-HandlingReactiveFormchangewithAngularSignals_0.png" />

개발 중 매우 흔한 상황은 폼 변경을 처리하는 것입니다. 전통적인 방식은 this.form.valueChange를 처리하거나 특정 폼 속성의 변경을 처리하는 것이며, 이는 사각형 넓이 계산기의 예시에서 보여졌습니다.

```js
@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <h1>Rectangle area calculator</h1>
    <form [formGroup]='form'>
      Length: <input formControlName='length' type="number">
      <br><br>
      Width: <input formControlName='width' type="number">
    </form>
    <br>
    @if(area$ | async; as area) {
      <div>Area observable: {area}</div>
    }
  `,
  imports: [FormsModule, ReactiveFormsModule, AsyncPipe],
})
export class App {
  form = new FormGroup<{
    length: FormControl<number | null>;
    width: FormControl<number | null>;
  }>({
    length: new FormControl(0),
    width: new FormControl(0),
  });

  // reactive
  area$ = this.form.valueChanges.pipe(
    startWith(this.form.value),
    debounceTime(300),

    map((value) => {
      if (value.length != null && value.width != null) {
        return value.length * value.width;
      }

      return 0;
    })
  );
}
bootstrapApplication(App);
```

# 문제 설명 및 전통적인 해결책

<div class="content-ad"></div>

하지만, 이 전통적인 방법에는 한 가지 큰 단점이 있습니다: 뷰에 0의 결과를 표시할 수 없습니다. area$observable이 숫자 0을 발행하면, 뷰의 조건 @if(area$ | async; as areaValue)는 false를 반환하여 `div`Area observable: 'areaValue'`/div` HTML 블록이 렌더링되지 않습니다.

0의 결과를 처리하기 위해 일반적으로 async 파이프를 사용을 피합니다. 대신, 컨트롤러에서 observable을 구독하고 계산을 컨트롤러의 속성에 저장한 후, 이를 뷰에 보간합니다.

더 간단한 방법은 계산을 프로퍼티로 가진 객체로 래핑하는 것입니다. 이 방법으로 @if(area$ | async; as area)`condition`은 객체가 방출되는 즉시 true를 반환합니다. 그럼으로 결과가 0인 것을 뷰에 보간할 수 있습니다: `div`Area observable: 'area.value'`/div`.

아래의 예제에서 두 가지 전통적인 해결책을 사용한 예제를 확인해보세요:

<div class="content-ad"></div>

```js
@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <h1>사각형 면적 계산기</h1>
    <form [formGroup]="form">
      길이: <input formControlName="length" type="number">
      <br><br>
      너비: <input formControlName="width" type="number">
    </form>
    <br>
    @if(area$ | async; as area) {
      <div>비동기 파이프로 처리된 면적: {area}</div>
    }
    동기적 처리된 면적: {areaValue}<br>
    @if(areaObject$ | async; as areaObject) {
      <div>비동기 파이프로 처리된 결과를 객체로 포장한 면적: {areaObject.value}</div>
    }
  `,
  imports: [FormsModule, ReactiveFormsModule, AsyncPipe],
})
export class App implements OnInit {
  areaValue = 0;

  form = new FormGroup<{
    length: FormControl<number | null>;
    width: FormControl<number | null>;
  }>({
    length: new FormControl(0),
    width: new FormControl(0),
  });

  // 비동기 파이프로 처리된 면적:
  area$ = this.form.valueChanges.pipe(
    startWith(this.form.value),
    debounceTime(300),

    map((value) => {
      if (value.length != null && value.width != null) {
        return value.length * value.width;
      }

      return 0;
    })
  );

  // 동기적 처리된 면적
  ngOnInit() {
    this.area$.subscribe((area) => (this.areaValue = area));
  }

  // 비동기 파이프로 처리된 결과를 객체로 포장한 면적
  areaObject$ = this.area$.pipe(map((value) => ({ value })));

}
bootstrapApplication(App);
```

<img src="https://miro.medium.com/v2/resize:fit:1200/1*6RAEsUJyaw1i4Wv2EQze7w.gif" />

# Angular Signals를 활용한 현대적 솔루션

포장 메서드는 분명히 간단하며 스파게티 코드를 피할 수 있지만, Angular Signals를 사용하여 구현해 보겠습니다. 이를 위해 areaSignal이라는 새로운 프로퍼티를 추가하고 areaSignal = toSignal(this.area$);와 같이 초기화한 후 뷰에서 사용하면 됩니다.


<div class="content-ad"></div>

```js
@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <h1>사각형 넓이 계산기</h1>
    <form [formGroup]='form'>
      길이: <input formControlName='length' type="number">
      <br><br>
      너비: <input formControlName='width' type="number">
    </form>
    <br>
    @if(area$ | async; as area) {
      <div>Async 파이프로 연결된 넓이 observable: {area}</div>
    }
    
    넓이 시그널: {areaSignal()} <br>
  `,
  imports: [FormsModule, ReactiveFormsModule, AsyncPipe],
})
export class App implements OnInit {
  form = new FormGroup<{
    length: FormControl<number | null>;
    width: FormControl<number | null>;
  }>({
    length: new FormControl(0),
    width: new FormControl(0),
  });

  // Async 파이프로 연결된 넓이 observable:
  area$ = this.form.valueChanges.pipe(
    startWith(this.form.value),
    debounceTime(300),

    map((value) => {
      if (value.length != null && value.width != null) {
        return value.length * value.width;
      }

      return 0;
    })
  );

  //넓이 시그널
  areaSignal = toSignal(this.area$);
}

bootstrapApplication(App, {
  providers: [provideExperimentalZonelessChangeDetection()],
});
```

<img src="https://miro.medium.com/v2/resize:fit:1200/1*CdB3QdJukNoa6C0do6MVPw.gif" />

# StackBlitz에서 해보기

https://stackblitz.com/edit/stackblitz-starters-ey39la?file=src%2Fmain.ts


<div class="content-ad"></div>

제가 이전 글을 썼어요: Angular에서 NgRx SignalStore 상태 관리와 zoneless 앱.

제 LinkedIn: [링크](https://www.linkedin.com/in/serhii-zhydetskyi-80a7789b/)

## 유용한 링크:

Angular Signals: [링크](https://angular.dev/guide/signals)

<div class="content-ad"></div>

Angular Zoneless: [https://angular.dev/guide/experimental/zoneless](https://angular.dev/guide/experimental/zoneless)

Angular Reactive Forms: [https://angular.dev/guide/forms/reactive-forms](https://angular.dev/guide/forms/reactive-forms)

# Stackademic 🎓

Thank you for reading until the end. Before you go:

<div class="content-ad"></div>

- 작가에게 박수를 보내고 팔로우도 해주세요! 👏
- 우리를 팔로우해 주세요 X | LinkedIn | YouTube | Discord
- 다른 플랫폼에서도 만나보세요: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠를 만나보세요