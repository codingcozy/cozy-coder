---
title: "Signal 제로에서부터 개념과 구현하기"
description: ""
coverImage: "/assets/img/2024-08-19-SignalConceptandImplementationFromScratch_0.png"
date: 2024-08-19 02:09
ogImage: 
  url: /assets/img/2024-08-19-SignalConceptandImplementationFromScratch_0.png
tag: Tech
originalTitle: "Signal Concept and Implementation From Scratch"
link: "https://medium.com/@sib.sustswe/signal-concept-and-implementation-from-scratch-0515b2a7b99a"
isUpdated: false
---



![사진](/assets/img/2024-08-19-SignalConceptandImplementationFromScratch_0.png)

자바스크립트/타입스크립트 세계에서 작업 중이라면 신호에 대해 이미 들어봤거나 알고 있을 것입니다. 요즘에는 리액트를 제외한 모든 현대 프레임워크가 신호를 사용하고 있습니다. 심지어 앵귤러 팀도 신호를 변경 감지 메커니즘으로 채택하고 있습니다. 그들은 예전에는 변경 감지를 위해 몽키 패칭을 했지만 지금은 존리스(change detection)로 향하고 있습니다. 모든 새로운 프레임워크들은 신호 개념을 중심으로 설계되었습니다.

그래서 신호란 무엇인가요? 왜 그렇게 특별한가요? 프로그래밍 세계에서 새로운 개념인가요?

신호는 반응형 프로그래밍의 한 방법입니다. 사실 신호는 새로운 것이 아니라 옵서버 패턴(observer pattern)의 구현입니다. 옵서버 디자인 패턴에 대해 알고 계신다면 이 패턴에는 옵서버가 여럿인 옵저버블이 있습니다. 옵저버들은 옵저버블에 구독하고 옵저버블의 상태가 변경될 때마다 이를 관찰하고 있는 모든 옵저버들에게 변경 사항을 알리게 되는 것입니다.


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

시그널 개념에서는 두 가지 주요 구성 요소가 있습니다. 하나는 시그널이고 다른 하나는 이펙트입니다. 우리는 관측 가능한 것을 시그널로, 관찰자를 이펙트로 부릅니다. 그러나 여기에는 약간의 특이사항이 있습니다. 관찰자 패턴에서 시그널에 수동으로 구독할 필요가 없습니다. 대신 시그널 내에서 의존하는 이펙트를 자동으로 알게 될 것입니다. 이를 달성하기 위해 시그널에서 일반 값 대신 getter 및 setter 함수를 반환하므로 getter가 호출될 때마다 현재 실행 중인 이펙트에 대해 알 수 있습니다.

따라서 이제 시그널 개념에 대해 알았으니 구현을 살펴보겠습니다:

```js
let currentEffect = null;

const signal = (value) => {
  const observers = [];
  let _value = value;

  return {
    get() {
      if (currentEffect) {
        observers.push(currentEffect);
      }
      return _value;
    },
    set(newValue) {
      _value = newValue;
      observers.forEach((observer) => observer());
    },
  };
};

const effect = (callback) => {

  currentEffect = callback;   
  // 콜백은 전역적으로 저장됨, 이를 통해 콜백 내에서 사용되는 getter에게 
  // 변경 시 실행해야 하는 이펙트를 알려줍니다.

  callback();
  currentEffect = null;
};
```

이 두 가지 구성 요소를 사용하여 대부분의 프레임워크에서 사용되는 계산된 값이라고 불리는 또 다른 구성 요소를 만들 수 있습니다.

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
const computed = (callback) => {
  const computedSignal = signal(callback());
  effect(() => {
    computedSignal.set(callback()); 
    // whenever any signal used in the callback will change, this effect 
    // will run again and the computed value will be updated.
  });
  return { get: computedSignal.get };
};
```

사용 방법:

```js
const a = signal(1);
const b = computed(() => {
  return a.get() * 2;
});

effect(() => {
  console.log(a.get(), b.get());
});

a.set(a.get() * 2);
```

여기서 a가 새 값으로 설정되면 b가 자동으로 업데이트되고 이펙트도 다시 실행됩니다. 따라서 이를 통해 리액티브 프로그래밍을 구현했습니다.

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

요런 신호에 대한 간단한 설명과 구현이었어요. 현대적인 프레임워크는 더 많은 경우를 다루겠죠. 하지만 우리의 기본적인 이해를 위해서는 이 구현이 충분합니다.

코딩을 즐기세요!

#javascript #signal #typescript #frontend