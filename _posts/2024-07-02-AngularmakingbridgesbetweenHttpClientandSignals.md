---
title: "Angular에서 HttpClient와 Signals 연결하는 방법"
description: ""
coverImage: "/assets/img/2024-07-02-AngularmakingbridgesbetweenHttpClientandSignals_0.png"
date: 2024-07-02 23:00
ogImage:
  url: /assets/img/2024-07-02-AngularmakingbridgesbetweenHttpClientandSignals_0.png
tag: Tech
originalTitle: "Angular: making bridges between HttpClient and Signals"
link: "https://medium.com/@IgorPak-dev/angular-making-bridges-between-httpclient-and-signals-a7a50c15ad9b"
isUpdated: true
---

![Angular Making Bridges Between HttpClient and Signals](/assets/img/2024-07-02-AngularmakingbridgesbetweenHttpClientandSignals_0.png)

18번 릴리스와 새로운 Angular 프레임워크의 다가오는 릴리스부터, 전체 Angular 커뮤니티가 zoneless 앱과 매일 코드에서 RxJS의 사용량을 줄이는 등의 중요한 변화를 경험하고 있습니다. RxJS를 신중하게 사용할지 여부를 고려하는 것이 중요하지만, 응용 프로그램에서 사용하는 반응적 메커니즘의 주요 개념을 모두 이해하고 활용해야 합니다. 상태에는 시그널을 사용하고 이벤트 및 복잡한 로직을 관리하기 위해 RxJS를 사용하는 것을 권장합니다.

그런데 먼저, 이 기사의 주제를 소개하겠습니다: HttpClient 기반 서비스와 시그널 기반 구성 요소를 연결할 때 어떻게 상호 작용해야 하는지 알아봅시다. 이는 HttpClient의 Observable 기반 응답에서 시그널 기반 구성 요소에서 사용되는 속성까지의 전환을 관리하는 것을 포함합니다.

HttpClient 대신 fetch API를 사용하는 논의 속에서, 제가 주는 조언을 다시 강조하고 싶습니다: HttpClient를 사용하지 않아도 됩니다. 우리가 혜택을 얻을 수 있는 유용한 기능을 제공합니다. Observable을 Signal로 변환하는 도구가 있으므로 이전에 없던 변환 작업을 처리할 수 있습니다.

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

## 직접적인 RxJS 구독

첫 번째로, 가장 분명한 방법은 구독을 사용하는 것입니다. HTTP 호출은 값이 하나인 옵저버블이기 때문에 구독을 걱정할 필요가 크지 않습니다. 또한 RxJS 오류 처리 패턴을 활용할 수 있습니다(파이프나 오류 콜백을 통해). 가장 분명하고 쉬운 방법입니다.

```js
public data = signal<number[]>([])
....
constructor(private testService: TestService) {}
....
public ngOnInit() {
  this.testService.getItems().subscribe((items) => {
    // 신호를 덮어쓰기 또는 생성
    this.data = signal(items);
    // 또는 값 직접 설정
    this.data.set(items);
  });
}
```

## 프로미스 활용

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

두 번째 방법은 promise를 활용하는 것입니다. 표준 then/catch 또는 async/await를 사용할 수 있습니다. 이 부분은 조심해야 합니다, 특히 async/await의 특이사항을 잊거나 모르는 경우에는 더욱 그렇습니다. RxJS의 firstValueFrom 함수를 사용하여 첫 번째 노출된 값을 observable로 변환할 수 있습니다(대부분의 API 응답은 단일 값이므로 잘 맞습니다). 여기 두 가지 주의할 점이 있습니다:

- async/await 구문을 사용하면 프로미스가 값을 반환하거나 오류가 발생할 때까지 함수 실행이 중단됩니다. 따라서 코드의 나머지 부분은 여전히 실행을 기다리고 있습니다. async/await 함정에 빠지지 않도록 주의하세요.
- firstValueFrom을 사용하면 즉시 observable 소스로 구독됩니다. 이는 문제가 될 수 없지만, 이를 지연 시키고 싶다면 제대로 작동하지 않을 수 있습니다.

```js
public async ngOnInit() {
  // observable 소스를 즉시 구독합니다
  this.data = signal(await firstValueFrom(this.testService.getItems()));
  // 주의! 서비스가 값을 반환할 때까지 코드가 실행되지 않습니다
```

코드 실행을 중단하지 않고 싶다면 특별한 기능 래퍼를 사용할 수 있습니다. 또 다른 방법은 IIFE를 사용하는 것인데, 아마 더 일관성이 있을 것입니다.

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
public async initData() {
  this.data = signal(await firstValueFrom(this.testService.getItems()));
}

public ngOnInit() {
  this.initData();
  ....
}
```

```js
public ngOnInit() {
  (async () => {
    this.data = signal(await firstValueFrom(this.testService.getItems()));
    console.log('data inited');
  })();
  ....
}
```

다른 방법은 old-faithful then을 사용하는 것입니다. 위의 모든 프로미스 경우에서 then 콜백 실행의 특정 사항을 잊지 마십시오. 이것은 고유한 특징을 가지고 있으며 경우에 따라 예측할 수 없는 결과를 줄 수 있습니다.

```js
public ngOnInit() {
  firstValueFrom(this.testService.getItems()).then((items) => {
    this.data = signal(items);
  });
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

## rxjs-interop에서 toSignal 사용하기

rxjs-interop에서 세 번째 방법인 toSignal을 살펴보았습니다. 간단히 말하면, 이 함수는 소스 observable을 구독하고 모든 값을 시그널에 푸시하는 기능을 합니다. 그러나 HttpClient 응답을 신호로 변환하는 가장 tricky한 방법입니다. 이에 관해 명심해야 할 몇 가지 사항이 있습니다:

- firstValueFrom과 비슷하게, toSignal은 실행과 동시에 즉시 구독합니다.
- 기본값을 제공하지 않으면 첫 번째 값(undefined)을 즉시 발행합니다.
- API 호출을 라이프사이클 훅이나 컴포넌트 클래스 메서드의 어딘가에서 수행하는 경우 'Error: NG0203: toSignal() can only be used within an injection context such as a constructor, a factory function, a field initializer, or a function used with `runInInjectionContext.' 에러가 발생할 수 있습니다. 이 경우에는 인젝터를 주입하거나 runInInjectionContext를 사용해야 합니다.
- 읽기 전용 시그널을 제공합니다. 따라서 나중에 시그널과 상호 작용할 예정이라면(설정/업데이트) 여기서 발생 가능한 장애물을 처리해야 합니다.

```js
private _injector = inject(Injector);

public ngOnInit() {
  this.data = toSignal(this.testService.getItems(), {
    // 인젝터에 대한 참조 제공
    injector: this._injector,
    // 기본값 제공
    initialValue: [],
  });

  // 또는 runInInjectionContext 사용
  runInInjectionContext(this._injector, () => {
    toSignal(this.testService.getItems(), {
      initialValue: [],
    });
  });
  ....
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

## 결론:

현재 상황에서는 표준 observable 구독 방법을 사용하는 것이 가능한 위험과 어려움을 피할 수 있는 가장 좋은 방법으로 보입니다. Promise를 사용하는 것도 편리할 수 있지만, 당신과 팀원들이 async/await 및 기타 promise에 대해 잘 알고 있어야 합니다. toSignal 사용은 가장 번거로워 보입니다.

유용한 링크:
https://angular.dev/guide/signals
https://angular.dev/guide/di/dependency-injection-context#run-within-an-injection-context
