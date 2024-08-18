---
title: "2024년 Angular의 현황 Signals와 Observables 비교"
description: ""
coverImage: "/assets/img/2024-07-10-TheStateofAngularSignalsandObservables_0.png"
date: 2024-07-10 01:00
ogImage:
  url: /assets/img/2024-07-10-TheStateofAngularSignalsandObservables_0.png
tag: Tech
originalTitle: "The State of Angular: Signals and Observables"
link: "https://medium.com/@albertobasalo/the-state-of-angular-signals-and-observables-dd964caf3fe4"
isUpdated: true
---

![Image](/assets/img/2024-07-10-TheStateofAngularSignalsandObservables_0.png)

Change detection has evolved more in the first six months of this year than in the previous six years. All thanks to the signal type and its interaction with the observable.

In this article, I summarize the recommended use of both techniques to improve the work experience for both users and programmers.

# 📻 Signals

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

- 템플릿은 가능한 최소한의 처리 비용으로 신호를 사용하여 업데이트해야 합니다.
- 신호는 변경 가능할 수 있습니다: 사용자 또는 어떤 프로세스가 변경할 수 있고, 그 경우에는 뷰가 자동으로 재평가됩니다.
- 그러나 대부분의 신호는 본질적으로 불변할 수 있습니다; 즉, 변경되지 않을 초기값을 가집니다. 이러한 신호는 입력(다른 컴포넌트나 라우터로부터의 입력) 및 observable을 기반으로 한 toSignal()에서 나오는 신호입니다.
- 다른 신호는 computed() 함수를 사용하여 이러한 신호에서 파생될 수 있습니다. 이 경우 초기값을 변경하지 않고 변경된 것만 재평가합니다.

# 🕵️ 옵저버블

- RxJs의 유형, 함수 및 연산자는 API에 대한 일반적인 액세스와 같은 비동기 프로세스를 처리하기 위한 가장 다목적 도구로 남아 있습니다.
- 특히 pipes()에서 채널화할 수 있는 복잡한 연산자가 필요한 경우에 유용합니다.

# 🌉 상호 운용

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

- API에 접근하기 위해 observables를 사용하고 데이터를 보여주기 위해 시그널을 사용하기 때문에 상호작용하고 변환할 필요가 있을 거에요.
- Angular에는 하나를 다른 것으로 변환하기 위한 내장 함수가 있어요.
- toObservable(sourceSignal) 함수는 소스 시그널의 상태가 변경될 때 새로운 observable 값을 방출합니다.
- toSignal(sourceObservable$) 함수는 구독하고, 각 방출마다 시그널에 값을 할당하고 observable이 완료되면 구독을 취소합니다.

# 😈 문제점

- 변수나 속성을 선언할 때 toSignal(sourceObservable$) 함수를 사용하며, 불변의 시그널 유형의 인스턴스를 반환합니다.
- 즉, 한 번만 실행되어 시그널의 인스턴스를 만들고 이를 변형할 수 있지만 읽을 수만 있어요.
- 초기 선언에서 sourceObservable$ 인수가 알려진 데이터를 표시하는 데 이상적입니다. 예:

```js
dataSignal = toSignal(this.http.get(this.url), { initialValue: [] });
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

- 그러나 시간이 지남에 따라 변하는 인수가 있다면 sourceObservable$를 쉽게 결정할 수 없습니다. URL이나 페이로드 데이터가 동적일 수 있습니다.
- 사용자, 라우터 매개변수, 기타 프로세스로부터 변수 필터를 사용하여 API에 접근할 때 자주 발생합니다.

```js
dataSignal = toSignal(this.http.get(this.url + this.someVariable), { initialValue: [] });
```

# 🔧 해결책

- 제다이들을 위한 1차 RxJS 연산자가 있는 파이프.
- 아이디어는 sourceObservable$가 변경될 수 있고, 이를 위해 인수가 observable하게 변경될 수 있어야한다는 것입니다... 그를 위해 switchMap, concatMap, forkJoin과 같은 연산자가 사용됩니다.
- 최종 구문이 약간 번거로울 수 있지만, 일관된 패턴을 따르며 이 복잡성을 숨기는 함수로 쉽게 추상화될 수 있습니다.

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
someVariable$ = new Subject(); // 또는 toObservable(), RxJs의 다른 소스...
dataSignal = toSignal(this.someVariable$.pipe(switchMap((someVariable) => this.http.get(this.url + someVariable))), {
  initialValue: [],
});
```

# 💻 코드

- 실제 사용 예시로 이 코드를 따라할 수 있습니다:
- productId가 언제든지 변경될 수 있는 페이지.
- 우리가 관심 있는 것은 API에서 오는 Product 객체입니다.
- RxJs 구독에 대한 걱정 없이 그 객체를 뷰로 신호로 반환하고자 합니다.

## [코드 예시]

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

https://github.com/AlbertoBasalo/ActionBuy/blob/master/src/app/routes/buy/buy.store.ts

# 🔮 미래

- 이러한 기술은 매우 최근에 나왔으며, 시그널 및 옵저버블의 동기/비동기 임피던스를 해결하는 새로운 방법이 나타날 수 있습니다.

Alberto Basalo
