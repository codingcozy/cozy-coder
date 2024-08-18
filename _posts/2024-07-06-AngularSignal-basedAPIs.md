---
title: "Angular 시그널 기반 API 사용법"
description: ""
coverImage: "/assets/img/2024-07-06-AngularSignal-basedAPIs_0.png"
date: 2024-07-06 03:07
ogImage:
  url: /assets/img/2024-07-06-AngularSignal-basedAPIs_0.png
tag: Tech
originalTitle: "Angular Signal-based APIs"
link: "https://medium.com/@tayuelo26/angular-signal-based-apis-c731bc27b9b5"
isUpdated: true
---

![](/assets/img/2024-07-06-AngularSignal-basedAPIs_0.png)

이 게시물은 Signal 기반 API에 대해 기본적이거나 기초적인 정보를 기억해야 할 때 찾아볼 곳입니다. 나는 이 게시물을 나를 위해 읽기 쉽고 흥미롭고 유용하게 쓸 것이며, 그렇게 함으로써 아마 당신도 이 게시물을 유용하게 여기게 될 것입니다.

그럼, 시작해봅시다.

- Signal 기반 API가 무엇인가요?
- 왜 사용해야 하나요?
- input API
- model API

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

시그널 기반 API란 무엇인가요?

시그널 패턴을 따르는 API입니다. 이들은 Signal 유형의 기능을 확장하는 API들로, 입력 및 출력에서 반응성을 가질 수 있는 기회를 제공합니다. 시그널 기반 입력을 통해, 새로운 변경 감지 메커니즘을 활용하여 애플리케이션의 성능을 향상시킬 수 있습니다. 이를 통해 변경 사항을 확인해야 하는 구성 요소의 양을 줄이는 방식입니다.

또한, 이제 양방향 바인딩을 수행하는 새로운 방법이 제공됩니다. 나중에 이에 대해 더 이야기하겠습니다.

왜 시그널 기반 API를 사용해야 하나요?

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

시그널 입력은 데코레이터 @Input()에 대안으로 반응적입니다.

시그널 입력이 제공하는 몇 가지 이점이 있습니다:

[문서에서 발췌]

- 변환은 허용된 입력 값과 일치하는지 자동으로 확인됩니다.
- 템플릿에서 사용할 때, 시그널 입력은 OnPush 컴포넌트를 자동으로 더티하게 표시합니다.
- 값이 입력이 변경될 때마다 손쉽게 유도될 수 있습니다.
- ngOnChanges 또는 세터 대신 효과를 사용하여 입력을 더 쉽게 모니터링할 수 있습니다.

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

API 입력

새롭게 추가된 입력 기능은 컴포넌트의 입력을 선언할 수 있게 해줍니다. 그러나 값이 아닌 값을 포함하는 시그널을 받습니다. 이 값은 부모 컴포넌트로부터 바인딩되며 읽기 전용입니다.

```js
@Component({
  selector: 'app-card',
  template: `{ age() }`
})
export class CardComponent {
  public age = input(0);
}
```

템플릿에서 시그널의 getter를 호출해야 하는 것에 주목해야 합니다. 이는 시그널 값에 액세스하고 시그널의 소비자를 등록하는 데 필요합니다. 이제 시그널이 변경될 때마다 Angular는 이 컴포넌트를 RefreshView 플래그로 뷰 갱신을 위해 표시합니다. 이 기능은 markViewForRefresh 함수에 의해 내부적으로 수행됩니다.

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

입력 값을 필수로 만들려면, API에서 노출된 required 함수를 사용하면 됩니다.

```js
public age = input.required(0);
```

또한 값에 변환 함수를 적용할 수도 있습니다.

```js
public age = input.required({
  transform: (v: string | number) => `${v}`,
  alias: 'customerAge'
});

click() {
  console.log(this.age()); // 항상 문자열을 반환합니다
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

위의 내용을 한국어로 번역해 드리겠습니다.

아마도 이미 알고 있겠지만, 우리는 컴포넌트를 사용하는 사용자들이 사용할 수 있도록 입력에 별칭을 설정할 수도 있습니다.

```js
<app-card [customerAge]="0"></app-card>
```

예상대로이 신호 입력은 계산 함수를 사용하여 입력에서 값을 파생할 수 있도록 합니다.

```js
age = input(0);
dogYears = computed(() => this.age() * 7);
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

computed를 사용할 때, 값들은 메모이제이션됩니다. 즉, 신호 age가 읽힐 때마다 메모이즈된 값을 반환하며, 다시 계산하지 않습니다. 다시 계산은 age 신호의 값이 변경될 때에만 발생합니다.

모델 API

새로운 모델 함수는 해당 컴포넌트/지시자에서 입력/출력 쌍으로 노출된 쓰기 가능한 신호를 선언합니다.

다음과 같이 모델 함수를 사용하여 모델 입력을 만들 수 있습니다.

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
// 모델 입력
selected = model(false);

// 일반 입력
checked = input(false);
```

두 입력 모두 속성에 값을 바인딩할 수 있습니다. 그러나 모델 입력의 경우 컴포넌트 작성자가 속성에 값을 쓸 수 있습니다.

```js
selected = model(false);
checked = input(false);

toggle() {
  // 모델 입력은 쓰기 가능한 시그널입니다
  this.selected.set(!this.selected());

  // 일반 입력은 읽기 전용입니다
  this.checked.set(!this.checked()); // 오류, 프로퍼티 'set'이 존재하지 않음
}
```

컴포넌트가 모델 입력에 새 값을 작성하면 Angular는 해당 입력에 값을 바인딩 중인 컴포넌트로 새 값이 전파됩니다. 이를 양방향 바인딩이라고 합니다.

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

신호를 사용한 양방향 바인딩

우리는 모델 입력값에 쓰기 가능한 신호를 전달할 수 있습니다.

```js
@Component({
  template: '<custom-checkbox [(selected)]="isActive" />',
})
export class UserProfile {
  protected isActive = signal(false);
}
```

우리의 예제에서 CustomCheckbox 컴포넌트는 선택된 모델 입력값에 값을 쓸 수 있으며, 이 값은 다시 UserProfile의 isActive 신호로 전파됩니다. 이 바인딩은 selected와 isActive의 값을 동기화 상태로 유지합니다.

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

암시적 변화 이벤트

Angular는 모델 입력을 선언할 때 모델의 변경 이벤트를 자동으로 생성합니다. 출력의 이름은 모델 입력의 이름 뒤에 "Change"가 붙은 것입니다.

```js
// 이는 "selectedChange"라는 출력을 생성합니다.
// 템플릿에서는 (selectedChange)="handler()"를 사용하여 구독할 수 있습니다.
selected = model(false);
```

Angular는 구성 요소 작성자가 모델에 새 값을 쓸 때마다 변경 이벤트를 발생시킵니다.

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

모델 입력 부분은 입력 변환을 지원하지 않습니다. 모델 입력을 필수로 표시하거나 표준 입력과 마찬가지로 별칭을 제공할 수 있습니다.

주요 차이점:

- model()은 입력과 출력을 모두 정의합니다. 사용자가 입력만 사용하거나 출력만 사용하거나 둘 다 사용할지는 사용자에게 달려 있습니다.
- ModelSignal은 WritableSignal이며, InputSignal은 읽기 전용이며 템플릿을 통해서만 변경할 수 있습니다.
- 모델 입력은 입력 변환을 지원하지 않습니다.

여기서 마치겠습니다. Angular에서 제공하는 새로운 메커니즘을 이해할 때 이 게시물이 유용하다면 좋겠습니다. 피드백이 있으시면 자유롭게 아래에 댓글을 남겨주세요.

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

읽어 주셔서 감사합니다,

알자디스
