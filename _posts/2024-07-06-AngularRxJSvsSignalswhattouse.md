---
title: "Angular RxJS와 Signals 비교, 무엇을 사용해야 할까"
description: ""
coverImage: "/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_0.png"
date: 2024-07-06 03:08
ogImage:
  url: /assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_0.png
tag: Tech
originalTitle: "Angular: RxJS vs Signals, what to use?"
link: "https://medium.com/@IgorPak-dev/angular-rxjs-vs-signals-what-to-use-17f2655b7e9c"
---

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_0.png)

17번째와 18번째 Angular 릴리스 이후로, 더 많은 개발자들이 Angular 세계에서 새로운 반응형 컨셉인 Signals를 시도하고 채택해왔습니다. 상황은 더욱 긴장되었는데, Angular 팀이 Angular 기반의 신호를 새로운 JavaScript 네이티브 기능으로 포함시키기 위한 새로운 이니셔티브를 시작했다는 보도가 있었습니다. 나는 이러한 미친 듯한 부르짖음을 볼 수 있었습니다: "우리는 RxJS가 필요하지 않아요. 우리의 코드에서 그것을 떨어뜨리자." 이것은 매우 나쁜 아이디어일 수 있으며, 당신에게 많은 시간/돈과 시스템 복잡성 증가를 야기할 수 있습니다. Angular 팀은 강력히 당신에게 상태와 데이터 저장을 위해 Signals를 사용하고, 이벤트 및 복잡한 논리를 처리하는 데는 RxJS를 사용하도록 권장합니다. 두 가지 일상 예제를 확인하고 각 경우에 어떤 것을 사용하는 것이 더 좋은지 알아보겠습니다.

## CASE#1: 간단한 값 표시하기.

가장 먼저, 가장 간단한 경우부터 시작해봅시다. 당신은 특정 값으로 초기화해야 하는 덤 (표현) 컴포넌트를 가지고 있습니다. 무엇을 선택해야 할까요? 답은: 달려 있습니다.

<div class="content-ad"></div>

- 미래에 계속해서 동일한 입력 필드를 사용해야 하는 경우 일반 @Input을 사용하세요 (더 나아가 한 번만 바인딩하기 위해 @Attribute를 사용할 수도 있습니다).
- 정보가 자주 변경되고 템플릿에서 렌더링해야 한다면, signal 입력이 가장 좋은 선택입니다. 변화에 적응하는 것 뿐만 아니라 효율적으로 실행할 수 있게 해주어 나중에는 세부적인 신호 기반의 영역이 제거된 변경 검출을 활용할 수 있습니다.

## 조사: "각 값이 중요한" 문제.

신호에 대해 알아야 할 특정한 사항이 하나 있습니다 (사실, 이것이 이들을 이벤트 처리나 복잡한 데이터 처리에 사용해서는 안 되는 이유입니다) - 특정 조건에서, 귀하의 효과/계산은 일부 값들을 건너뛸 수 있습니다. 아래 코드를 확인해 보세요:

\`\`\`js
@Component({
standalone: true,
selector: 'app-root',
template: \`\`,
})
export class AppComponent implements OnInit {
public testSignal = signal('test-signal');

constructor() {
const computedSignal = computed(() => {
console.log('COMPUTED is being computed...');
return 'computed ' + this.testSignal();
});

    effect(() => {
      console.log('EFFECT is being executed...');
      console.log('computed value: ' + computedSignal());
    });

}

ngOnInit(): void {
this.testSignal.set('test-signal-2');
this.testSignal.set('test-signal-3');
this.testSignal.set('test-signal-4');

    Promise.resolve().then(() => {
      this.testSignal.set('test-signal-in-promise');
    });

    setTimeout(() => {
      this.testSignal.set('test-signal-in-timeout');
    }, 0);

}
}
\`\`\`

<div class="content-ad"></div>

여기에는 신호, 해당 신호를 기반으로 계산된 신호, 그 효과가 그대로 반영된 mid-value를 등록하기 위한 것이 있습니다. 코드를 처음 보면 아마 이런 식으로 보일 것 같습니다:

```js
...
computed value: computed test-signal-2
...
computed value: computed test-signal-3
...
computed value: computed test-signal-4
...
computed value: computed test-signal-in-promise
...
computed value: computed test-signal-in-timeout
```

그리고 이는 상당히 논리적으로 보입니다. 적어도 Observables를 사용했을 때의 것과 비교해보면요. 그러나 실제 상황은 훨씬 더 흥미로운 것 같습니다:

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_1.png)

<div class="content-ad"></div>

- 네, 그것은 전체 로그입니다. 맞아요, 맞아요. 우리는 완전히 test-signal-2, test-signal-3, 그리고 test-signal-4를 잃었어요. 이것은 정말 이상해요. 적어도 test-signal-4는 남아 있어야 했는데. 아니면 그렇지 않았어요?
- 이 로그에서 두 번째로 알 수 있는 것은, 첫 번째로 EFFECT가 먼저 발생했고, 두 번째로 COMPUTED가 먼저 발생했다는 거예요. 놀랍죠, 놀랍죠...

무슨 일이 일어나고 있는지 이해하려고 해봅시다. 먼저, 어디서 그리고 어떻게 몇 가지 값을 놓쳤는지 알아보죠. 먼저 set 함수를 확인해봅시다. 제 생각에는 괜찮아 보여요. 새 값이 설정되었고, 노드(신호) 버전이 업데이트되었어요. 지금까지 이상하게 보이지 않아요...

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_2.png)

...앗, 업데이트할 대상이 없다고요? 뭐라고요?

<div class="content-ad"></div>

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_3.png)

몇 차례의 반복 끝에 ngOnInit는 드디어 끝났습니다. 이제 JavaScript가 다른 작업을 처리할 수 있게 되었고 ... 우리는 마침내 효과를 발동할 수 있습니다! 여기서 우리는 퍼즐의 첫 부분에 대한 킷값을 가져옵니다. 효과 함수에는 실행 지연이 있으므로 효과 콜백이 호출될 때에는 이미 testSignal의 값이 test-signal-in-promise로 설정됩니다.

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_4.png)

![](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_5.png)

<div class="content-ad"></div>

효과 함수가 시작되면 시그널 그래프가 업데이트되며, 타임아웃 콜백에서 시그널을 설정한 다음엔 모든 것이 예상대로 작동합니다:

![그림 링크](/assets/img/2024-07-06-AngularRxJSvsSignalswhattouse_6.png)

하지만 주의하세요! 전부 시작된 것만이 중요한 게 아닙니다; 효과/계산된 시그널을 이미 시작한 경우에도 자바스크립트는 여전히 단일 쓰레드 언어임을 잊지 마세요. 따라서 한 함수 프레임에서 여러 값들을 설정하면 시그널 그래프는 모두 등록하지만 막바지 값에만 반응합니다.

## CASE#2: 값의 스트림.

<div class="content-ad"></div>

- 모든 값을 세어야 하는 경우에는 옵저버블 및 RxJS의 모든 잠재력을 활용하는 것을 강력히 권합니다.
- 중간값에 관심이 없다면, 시그널은 적합한 대안이 될 수 있습니다. 그러나 특히 초기화 단계에서 잠재적인 문제를 예방하기 위해 조심하는 것이 중요합니다.
- 위의 상황이 있지만 복잡한 로직이 있다면, 저라면 시그널 대신 RxJS를 선택할 것입니다. (스위치맵 등의 일반적인 RxJS 케이스에 대한 대체 방법을 다음 기사에서 소개할 예정입니다)

## 결과.

시그널 반응성은 모든 문제와 개발 사례의 플라시보가 아닙니다. 시그널에도 장점이 있습니다. 많은 경우에 유용하며, 미래의 시그널 기반 변경 감지는 시그널 사용을 더욱 매력적으로 만듭니다. 그러나 시그널은 학습 곡선이 있습니다 (RxJS만큼 가파르진 않습니다). 어떤 경우에는 시그널이 당신을 물어뜯을 수도 있으며, 무슨 일이 일어나고 있는지 이해하려고 많은 시간을 들이거나 코드를 시그널 아키텍처에 맞게 변경해야 하는 경우가 발생할 수 있습니다.
그러므로 엔지니어로서 여러 도구를 모두 활용하고 언제, 어떻게 적용해야 하는지 알아내는 것이 중요합니다.
