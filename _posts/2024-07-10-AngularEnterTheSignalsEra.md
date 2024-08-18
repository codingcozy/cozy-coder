---
title: "Angular 시그널 시대에 들어서다"
description: ""
coverImage: "/assets/img/2024-07-10-AngularEnterTheSignalsEra_0.png"
date: 2024-07-10 00:35
ogImage:
  url: /assets/img/2024-07-10-AngularEnterTheSignalsEra_0.png
tag: Tech
originalTitle: "Angular: Enter The Signals Era"
link: "https://medium.com/@mrkrsticmilos/angular-enter-the-signals-era-357b5c8d871f"
isUpdated: true
---

Angular 이 선보인 기능 중 몇 가지를 소개해 드리겠습니다.

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

- 시그널
- 새로운 제어 흐름
- 로컬 템플릿 변수
- 지연 가능한 뷰
- 신호 지원을 포함한 RxJS-interop 패키지
- 통합된 컨트롤 상태 변경 이벤트

여기서는 신호에 대해 쓰며, 그것들이 얼마나 멋진지 알 수 있게 해줄 거에요. 제가 무슨 얘기를 하는 걸까요? 좀 더 자세히 살펴보도록 하죠. :)

## 신호

Angular 커뮤니티를 따르고 있다면, 신호에 대해 들어보거나 읽은 적이 있을 것입니다. 그리고 JavaScript가 신호를 네이티브로 지원할 것으로 예상되어 큰 일이라고 할 수 있어요.

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

제안된 표준과 Angular의 구현 사이에는 약간의 차이가 있지만, 그에 대해 얘기하려고 온 것은 아닙니다.

시그널은 렌더링 업데이트를 최적화하는 새로운 반응적인 기본 요소입니다. 시그널 변수를 정의하는 것은 간단합니다: 초기값과 함께 signal 함수를 호출하기만 하면 됩니다.

```js
const count = signal(0);
```

시그널의 값 읽기도 간단합니다. 이렇게 getter 함수를 호출하면 됩니다:

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

```js
console.log(`현재 카운트 값은 ${count()}`);
// 현재 카운트 값은 0
```

그 두 가지 메소드 set 및 update를 사용하여 시그널의 값을 설정하고 업데이트할 수 있습니다. 두 메소드의 차이점은 set은 새 값으로 인수를 취하고 update는 이전 값을 입력 인수로 취하는 함수 핸들러를 취한다는 것입니다.

```js
count.set(1);
console.log(`설정 후: ${count()}`);
// 설정 후: 1

count.update((currentValue) => currentValue + 1);
console.log(`업데이트 후: ${count()}`);
// 업데이트 후: 2
```

템플릿에서 시그널의 값을 표시하려면 위의 예시처럼 getter 함수를 호출하기만 하면 됩니다.

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

```js
@Component({
  selector: 'app-root',
  standalone: true,
  template: `
    <p>Count { count() }</p>
    <button (click)="increment()">Increment</button>
  `,
})
export class App {
  readonly count = signal<number>(1);

  increment(): void {
    this.count.update((currentCount: number) => currentCount + 1);
  }
}
```

알고 있어요, 아마도 템플릿에서 함수 표현식을 호출하는 것이 성능에 안 좋다고 들어봤을 거에요. 그러나 Signal은 조금 다릅니다. Signal의 값이 변경될 때에만 새로운 값으로 리렌더링됩니다. 다시 말해, Signals의 값은 메모이즈됩니다. 순수 파이프와 유사합니다.

좋아요, 그럼 여기서 리액티브한 것은 무엇일까요?

## React

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

아니요, 저는 인기가 높지만 덜 성숙한 Frontend 도구인 React에 대해 얘기하고 있지 않아요. 농담은 떠나서, 두 도구 모두 놀라운 기능을 가지고 있지만 제 의견으로는 서로 다른 목적을 제공하고 있어요. :)

이미 언급했듯이, Signal은 새로운 반응형 기본 요소입니다. 이는 신호 값 변경에 쉽게 반응할 수 있다는 뜻이에요.

위에서 살펴본 count Signal 예제는 Writeable Signal이라고 불립니다. 그 이름에서 알 수 있듯이 새 값을 쓸 수 있습니다 (설정 및 업데이트).

계산된 신호와 이펙트 함수를 살펴보도록 하죠. 반응형 요소들이에요.

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

## 계산

계산된 신호는 읽기 전용입니다. 계산된 신호의 값은 다른 신호 값에 따라 달라집니다.

```js
const isEven = computed(() => count() % 2 === 0);
```

이전에 정의된 가변(count) 신호의 getter 함수를 호출했다는 점을 주목해주세요. 계산된 신호는 제공된 함수 내에서 읽은 신호의 값 변경을 추적하며, 신호가 변경될 때마다 새 값이 계산됩니다.

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

```js
@컴포넷({
  selector: 'app-root',
  standalone: true,
  template: `
    <p>Count { count() }</p>
    <p>{ isEven() ? 'Even' : 'Odd' }</p>
    <button (click)="increment()">Increment</button>
  `,
})
export class App {
  readonly count = signal<number>(1);
  readonly isEven = computed(() => this.count() % 2 === 0);

  increment(): void {
    this.count.update((currentCount: number) => currentCount + 1);
  }
}
```

## 지호

지호 설정을 트랙한 분석시승으로 시작적을 할고싶다. 함께하는 농초에 있으는 지호를 양자하는 것으로, 최종 정수 프로세스가 발생할 때 번영을 시작해서 재목시스템에서 출레되고 있습니다.

```js
constructor() {
    effect(() =>
      console.log(
        `Count is ${this.count()} and is ${this.isEven() ? 'even' : 'odd'}`
      )
    );
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

위에서 count 및 isEven 신호 게터를 모두 호출했음을 알 수 있습니다. count가 변경되면 isEven도 변경됩니다. count를 1에서 2로 업데이트하면 isEven도 count와 동기화되어 계산되며 모든 것이 다시 안정되면 효과가 트리거됩니다. 이는 RxJS 및 동기 값을 해결하는 데 문제가 있는 다이아몬드 문제를 해결하는 매우 멋진 방법입니다.

다이아몬드 문제에 대한 자세한 내용은 별도의 기사가 필요합니다. 그러나 기본적으로 옵저버블 소스가 여러 소스를 조합하고 동기적으로 업데이트되는 경우 의도하지 않은 동작을 일으킬 수 있는 중간 값이 생길 수 있습니다.

## 새 I/O

신호 소개로 인해 구성요소 및 디렉티브에 대한 새로운 신호 기반 입력 및 신호와 함께 더 예쁜보이는 새로운 출력 함수 API가 생겼습니다. 구 방식과 새 방식을 비교해 봅시다.

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

```js
// 이전 방식: Input 및 Output 데코레이터와 함께
@Input()
value: number = 0;
@Output()
valueChange: EventEmitter<number> = new EventEmitter<number>();

// 새로운 방식 #1: 새로운 입력 신호 및 출력 함수와 함께
value = input<number>(0);
valueChange = output<number>();

// 새로운 방식 #2: 2-way 데이터 바인딩을 지원하는 신호 기반 모델과 함께
value = model<number>(0);
```

새로운 방식은 코드가 적고 읽기 쉽기 때문에 이전처럼 사용할 수 있습니다:

```js
// value는 변수입니다
<child [value]="value" (valueChange)="handleValueChange($event)"/>

// value는 신호입니다
<child [value]="value()" (valueChange)="handleValueChange($event)"/>

// 양방향 바인딩; value는 쓰기 가능한 신호 또는 비-신호 변수입니다
<child [(value)]="value"/>
```

input`T`()와 model`T`()의 차이점은 model이 쓰기 가능한 신호이고 input이 읽기 전용이라는 점입니다. 두 가지 모두 다른 사용 사례에서 유용합니다.

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

마지막으로, 새로운 출력 기능은 신호에 기반하지 않습니다.

## 결론

신호에 대한 나의 열정을 공유해 주셨으면 좋겠습니다. Angular 뿐만 아니라 현대 프런트엔드 개발의 미래가 되기 때문입니다. 신호를 사용하면 On Push Change Detection Strategy를 자연스럽게 사용할 수 있고 Angular 애플리케이션을 더욱 효율적으로 만들 수 있습니다.

또한 이 글이 유익하게 느껴진다면, 다른 누군가에게도 유용할 것이라고 생각하신다면, 동료나 네트워크와 공유해 주시면 감사하겠습니다.

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

나의 멘토들에게 이 쓰기 과정을 이끌어 주셔서 특별한 감사를 드립니다.

- Bonnie Brennan, Angular Consultant & Mentor @ Tech Stack Nation
- PhD Pavle Kostić, CTO @ Capaciteam

여성 우선. :)

주목해 주셔서 감사합니다. 즐거운 하루 되시길 바랍니다.
