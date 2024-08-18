---
title: "Angular Signals 최적의 사용 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:43
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Angular Signals: Best Practices"
link: "https://medium.com/@eugeniyoz/angular-signals-best-practices-9ac837ab1cec"
isUpdated: true
---

이 문서에서 Angular Signals와 함께 일년 가까이 사용해본 경험을 공유하겠습니다.

## Signals를 언제 사용해야 할까요?

- 템플릿에서;
- 상태 관리에;
- 시간 측면이 없는 값의 변경에 반응해야 할 때.

Angular 템플릿에서 Signals은 Observables보다 우수합니다: 파이프 없이 Change Detection을 스케줄링하며, 그리치가 발생하지 않습니다. 한 신호를 여러 번 읽어도 성능 면에서 "무료"이며, 읽은 값은 항상 동일합니다. 간단히 설명하기 쉽지 않은 다른 이유들도 있지만, 이미 충분히 이유가 되어 새로운 템플릿 내의 모든 (변할 수 있는) 변수는 Signal이어야 한다는 규칙을 만들어야 합니다.

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

템플릿 이외에도 시그널은 반응성을 위해 사용될 수 있지만, 시간 측면이 없이라고 언급했던 대로입니다.

저는 한 번 트위터에 관한 게시물을 작성한 적이 있습니다. 이제 그것을 여기에 업데이트하고 개선해서 올리겠습니다.

앵귤러에서 반응성 변수를 생성하는 두 가지 방법이 있습니다: 옵저버블(Observables)과 시그널(Signals). 변수가 어떻게 반응해야 하는지 말로 설명하면 무엇을 사용해야 하는지 알 수 있습니다.

변수의 역할이 조건으로 설명될 수 있다면, 시그널을 사용해야 합니다:

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

- "만약 이 변수가 이 값이면, 이 목록을 표시하세요."
- "만약 이 변수가 이 값이면, 이 버튼은 비활성화됩니다."

변수 역할에 시간과 관련된 단어가 포함되어 있다면 Observable이 필요합니다:

- "커서가 이동할 때..."
- "파일 업로드 이벤트를 기다렸다가..."
- "이 이벤트가 발생할 때마다 이 작업 수행..."
- "이 이벤트까지..."
- "N초 동안 무시하세요..."
- "이 요청 이후에..."

시그널에는 시간 축이 없으며 값을 지연시키지 못합니다. 항상 값을 가지고 있으며 소비자는 항상 해당 값을 읽을 수 있어야 합니다.

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

신호의 사용자들은 computed(), effect(), 그리고 템플릿을 사용해 신호를 볼 때마다 모든 새 값이 읽힐 것을 보장하지 않습니다. 업데이트된 신호는 발생한 즉시가 아닌 언젠가는 읽힐 것입니다. 사용자들은 새 값을 언제 읽을지 스케줄링 메커니즘을 사용해 결정합니다. 이는 다음 작업 중에, 다음 변경 감지 주기 동안 또는 다른 시간에 이루어질 수 있습니다. 소비자에게 달려있습니다.

## computed()는 언제 사용해야 할까요?

언제든지요!

computed()는 Angular Signals에서 가장 좋은 것으로, 믿을 만하고 안전하게 사용할 수 있는 것입니다. computed()를 사용하면 코드를 더 선언적으로 만들어서 코드를 더욱 편리하게 이해할 수 있습니다 (이에 대해 더 읽기 위해서는 이 글을 읽어보세요).

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

computed()에 대한 사용 규칙은 두 가지만 있습니다:

- computed() 내부를 수정하지 마십시오. 새 결과를 계산하는 데 사용되어야 합니다. DOM을 수정하지 마세요, this를 사용하여 변수를 변경하지 마세요, 그리고 그럴 수 있는 함수를 호출하지 마세요. 값들을 Observables에 push하지 마세요 - 이로 인해 의도하지 않은 반응적인 컨텍스트 전파가 발생합니다 (아래 effect()에 대해 설명되어 있습니다). computed()에 부수 효과가 있으면 안 되며 순수한 함수여야 합니다.
- computed() 내부에서 비동기 호출을 하지 마십시오. 이 함수는 Signals의 수정을 허용하지 않습니다(이는 매우 유용하지만 비동기 코드를 추적할 수 없습니다). 게다가, Angular Signals은 엄격하게 동기적이므로 computed()에서 비동기 코드를 사용하려면 잘못된 방법을 사용하고 있습니다. 따라서 setTimeout(), Promises, 다른 비동기적인 작업은 하지 마십시오.

## effect()를 언제 사용해야 할까요?

Angular 문서는 effect()가 거의 필요 없을 것이라고 말하고 사용을 권장하지 않습니다(만약 문서가 편집된다면).

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

그 정보는 맞아요: 당신의 코드가 선언적이라면 effect()를 거의 필요로하지 않을 거에요 ;)

당신의 코드가 명령형이라면, 더 자주 effect()를 필요로 할 거에요. 명령형 부분이 없는 코드는 없지만, 우리 모두는 우리의 코드를 가능한 한 선언적으로 만들려고 노력해야 하기 때문에 effect()를 최소한으로 사용해야 합니다.

Angular 문서에서 언급한 위험뿐만 아니라, 다른 것 하나가 꽤 골치아플 수 있어요: effects는 반응적인 컨텍스트에서 실행되는데, effect에서 호출하는 모든 코드는 반응적인 컨텍스트에서 실행될 거에요. 만약 그 코드가 신호를 읽는다면, 해당 신호들이 당신의 effect의 종속성으로 추가될 거에요. 이를 위해 Alex Rickabaugh이(가) 자세히 설명해줘요.

아직도 effect()를 사용하도록 권장하고 싶진 않지만, 가능한 안전하게 사용하는 방법에 대한 조언은 해드리겠습니다:

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

- effect() 함수에 제공하는 기능은 최대한 작게 유지해야 합니다. 이렇게 하면 읽기가 더 쉬워지고 오류 행동을 찾기 쉬워집니다.
- 신호를 먼저 읽고 나머지 효과를 untracked()로 래핑하세요:

```js
effect(() => {
  // 우리가 필요로하는 신호를 읽기
  const a = this.a();
  const b = this.b();

  untracked(() => {
    // 여기에 나머지 코드가 있습니다 - 이 코드는 위에서 읽은 신호를 수정해서는 안 됩니다!
    if (a > b) {
      document.title = "Ok";
    }
  });
});
```

untracked()가 도움이 되는 경우에 대한 자세한 정보는 이 문서에서 확인할 수 있습니다.

## 신호와 Observables 혼합 사용

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

…괜찮아요!

당신의 코드는 신호(Signals)와 Observable을 가지게 될 거예요. 최소한 Signals는 모든 종류의 반응성에 사용될 수 없기 때문에 (자세한 설명은 위에서 볼 수 있어요.) 문제가 되지 않아요, 완전히 괜찮아요.

만약 computed()에서 Observable에서 값을 필요로 한다면, toSignal()을 사용하여 Signal을 생성하세요 (computed() 외부에서).

Observable의 pipe()에서 Signal을 읽어야 한다면, 두 가지 방법이 있어요:

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

- 만약 Observable에서 Signal의 변화에 반응해야 한다면 Signal을 Observable로 변환하고 일부 join 연산자를 사용하여 추가해야 합니다.
- Signal의 현재 값만 필요하고 해당 변경에 반응할 필요가 없다면 operators나 subscribe()에서 Signal을 직접 읽을 수 있습니다. Observable은 반응적인 문맥이 아니므로 여기서 untracked()이 필요하지 않습니다.
