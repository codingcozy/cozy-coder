---
title: "Angular 2 시그널 사용법"
description: ""
coverImage: "/assets/img/2024-07-10-Angukar2signals_0.png"
date: 2024-07-10 00:59
ogImage:
  url: /assets/img/2024-07-10-Angukar2signals_0.png
tag: Tech
originalTitle: "Angukar 2 : signals"
link: "https://medium.com/@ireneamedji/angukar-2-signals-7dbfae5ddd1b"
isUpdated: true
---

😪 네, 알아요. 마지막 게시물 이후에는 매주 일요일마다 올려서 앵귈러 학습을 더 진행해야 할 텐데 하지 못했어요. 그 이유는요? 집에서 인터넷 연결이 안 되고, 면허를 인증하기 위해 해야 할 학교 프로젝트가 너무 많았고, 몸이 지치면서 너무나도 피곤했으며... 그리고 너무 많은 디레스지가 있었어요.

지금까지 그냥 버티며 내가 계획한대로 진행하려고 해요. 적어도 학교는 끝났고, 스스로를 지지해 와야 하니까요. 프로젝트 관리 (곧 구글 인증을 받을 거예요)와 데이터 관리 과목을 수강 중이지만, 내가 되고 싶은 GDE가 되는 것을 막아서는 것은 아니에요.

프로젝트 관리 과목에 대한 요약도 이곳에 곧 올릴 거예요.
하지만 물론, 매주 일요일은 #앵귈카의 게시를 위해 예약돼 있어요.

그럼, 출발합시다!!! 🚀

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

![이미지](/assets/img/2024-07-10-Angukar2signals_0.png)

오늘은 시그널에 대해 이야기할 거예요! 전에 이 용어를 들었을 때, 코드에서 알림이나 오류가 있을 경우 코드에서 제안된 수정 사항을 의미하는 줄 알았는데, 전혀 틀렸어요 😑!

이제 이해했어요. 시그널이란 Angular 16에서 소개된 새로운 반응형 메커니즘이고, 변경 감지를 최적화하고 전체 애플리케이션 성능을 향상시키기 위한 것이에요. 데이터 변경을 효율적으로 관리하고 업데이트에 관심이 있는 구성 요소에게 알리는 가벼운 방법을 제공하며, 보다 세분화되고 고성능의 변경 감지 전략으로 이끄는 것이죠. 또한 @observable과 같은 프로토콜을 거치지 않고도 코드에 반응성을 추가할 수 있어요.

이렇게 생각해 봅시다:

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

먼저, 표 태그를 Markdown 형식으로 변경합니다.

먼저 표를 만든 것을 상상해보세요. 긴 탑을 만들었어요. 이제 맨 위에 새로운 블록을 추가하고 싶다고 해봅시다. 지난에는 새로운 블록을 추가하기 전에 아래에 있는 각 블록이 여전히 안정적인지 확인해야 했어요. 특히 탑이 아주 높은 경우에는 시간이 오래 걸릴 수도 있죠.

Angular 시그널은 건설 블록 세계에서 특별한 도우미 로봇이 있는 것과 같아요. 새로운 블록을 추가하면 로봇이 즉시 영향을 받는 블록만 확인하고 빠르게 타워가 여전히 안정적인지 확인해요. 이렇게 하면 시간과 노력을 많이 절약할 수 있으면서도 항상 탑이 단단함을 보장해줘요.

맞죠? 🤗

변경 감지는 두 가지 방법으로 수행됩니다:

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

기본적으로, 애플리케이션 요소와 상호 작용할 때마다 Angular는 모든 구성 요소를 통과하고 각각의 구성 요소에 대해 수정 사항이 있는지 확인합니다. 수정 사항이 있으면 새 값으로 구성 요소를 업데이트하여 표시합니다.

OnPush 방식은 Angular에서 수행하는 확인 작업의 수를 제한합니다. 근데 왜 제한하는 거지 😶? 나도 똑같은 질문을 했어요! 이 방법의 아이디어는 특정 입력으로 전달된 데이터 참조가 변경되지 않는다면, 구성 요소도 변경할 이유가 없다는 것입니다. 알겠죠? 그러니 예전 값과 새 값 비교를 하기 전에 확인 작업을 할 필요가 없습니다.

## 좋아요! 코드 예제 및 설명:

여기 Angular 코드 예제가 있어요. 이 코드는 버튼 클릭 수를 관리하기 위해 신호(signal)를 사용합니다:

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
// Angular 시그널 import 중
import { Component, signal } from "@angular/core";
```

```js
@Component({
  selector: 'app-signal-example',
  template: `
    <button (click)="onClick()">Click me</button>
```

```js
/* 이 HTML 요소는 클릭 이벤트를 가진 버튼을 나타냅니다. 버튼을 클릭하면 컴포넌트의 onClick() 메소드가 호출됩니다. */
<p>클릭 수: { clickCount }</p>
/* 이 HTML 단락은 현재 클릭 수를 표시하며, 클릭 수 값과 연결되어 있습니다. { clickCount } 구문은 시그널 값을 템플릿에 삽입하는 Angular 디렉티브입니다. */  `,
})
/* 이 데코레이터는 SignalExampleComponent라는 Angular 컴포넌트를 정의합니다. 또한 컴포넌트의 CSS 선택자 (app-signal-example)와 컴포넌트의 HTML 템플릿을 지정합니다. */
export class SignalExampleComponent {
  clickCount = signal(0);
/* 이 줄은 clickCount라는 시그널을 만들고 초기값을 0으로 지정합니다. 이 시그널은 버튼 클릭 수를 저장하고 추적하는 데 사용됩니다. */
  onClick() {
    this.clickCount.update((currentValue) => currentValue + 1); // 클릭 수 증가
  }
}
/* 버튼을 클릭하면 onClick() 메소드가 호출됩니다. 이 메소드는 clickCount 시그널의 update() 함수를 사용하여 현재값을 1 증가시킵니다. update() 함수는 인수로 콜백 함수를 취하며, 이 함수는 시그널의 현재값을 받습니다. 콜백 함수는 이 경우에는 현재값에 1을 더한 새로운 값을 반환합니다. */
```

여기까지입니다. 다음 주에는 Angular에서 새로운 엘리먼트를 만나러 와요!

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

# 이런 코드 # 개선
