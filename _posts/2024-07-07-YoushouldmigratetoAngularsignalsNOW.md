---
title: "지금 Angular Signals로 마이그레이션 해야 하는 이유"
description: ""
coverImage: "/assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_0.png"
date: 2024-07-07 19:56
ogImage:
  url: /assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_0.png
tag: Tech
originalTitle: "You should migrate to Angular signals NOW"
link: "https://medium.com/@hmidihamdi7/you-should-migrate-to-angular-signals-now-c6a74a924017"
isUpdated: true
---

![Signals API](/assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_0.png)

Starting from Angular 16, the Angular Team introduced Signals APIs to kick off their project of re-designing the reactivity system. Let's delve into Signals API and understand why we should migrate to them as soon as possible.

Before we continue, I would like to share my YouTube video about signals, including a small demo: [Watch here](https://youtu.be/T0-e3H6r0rc).

According to angular.dev documentation

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

정확히 말씀하신 것처럼, 변경 사항에 관심 있는 소비자들에게 신호를 보내는 값을 감싸는 래퍼일 뿐입니다. Angular 16에서 미리보기로 도입된 Signals가 Angular 17.2에서 안정화되었습니다.

Signals에 대해 더 설명하기 전에, Reactivity에 대해 설명하고 Angular에서 변경 감지가 작동하는 방식에 대해 개요를 제공하고 싶습니다.

# Reactivity

Angular 팀에 따르면, Reactivity는 변경의 전파를 표현하는 선언적인 방법입니다. 대학의 입학률을 계산하는 예시로 스프레드시트를 사용하여 설명해 보겠습니다:

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

![image](https://miro.medium.com/v2/resize:fit:756/1*-AERgGgF9qwc_JqcQ6FOrw.gif)

수락률은 계산된 셀로, 지원자 수와 합격자 수에 따라 달라집니다. 두 셀 중 하나를 변경하면 수락률 셀도 변경되어야 합니다. 이것이 스프레드시트 도구에서의 반응성입니다.

웹에서도 마찬가지로, 반응성은 의존성 추적을 통해 변경 전파를 가능하게 하는 프레임워크를 활성화합니다.

이것은 간단한 예시지만, 복잡한 웹 애플리케이션에서 여러 값, 상태 및 이벤트를 전파하고 변경사항을 감지한 후 반응적으로 UI를 업데이트해야 하는 경우가 있습니다.

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

애플리케이션에서 값과 상태를 종속 관계를 가진 노드 그래프로 생각할 수 있어요:

![image](https://miro.medium.com/v2/resize:fit:1400/1*uj43tgjmo61A1lP6hpFDzg.gif)

node1의 값이 변경되면 node2가 이 변경을 감지해야하고, 그 후에는 node3 및 node4가 동작해야 합니다. 그냥 이 두 노드들이 종속된 노드에 의존하지 않기 때문에 영향을 받지 않게 될 거에요.

이러한 변경의 흐름은 우리 애플리케이션의 성능에 매우 중요해요.

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

# Angular 변경 감지 메커니즘

## Zone.js

Angular는 변경 감지 메커니즘을 수행하기 위해 zone.js 라이브러리를 사용합니다. 자세히 말하면, 이는 브라우저 API에 패치를 추가하는 것을 의미합니다. 시작할 때 Angular가 addEventListener 브라우저 함수에 패치를 적용하여 변경 감지를 실행하게 되며, 변경이 있으면 페이지 부분을 다시 렌더링합니다.

```js
// 이것은 addEventListener의 새 버전입니다
function addEventListener(eventName, callback) {
     // 실제 addEventListener를 호출합니다
     callRealAddEventListener(eventName, function() {
        // 먼저 원래의 콜백을 호출합니다
        callback(...);
        // 그리고 Angular 특정 기능을 실행합니다
        var changed = angular.runChangeDetection();
         if (changed) {
             angular.reRenderUIPart();
         }
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

zone.js는 많은 API를 수정하여 Angular 변경 감지를 투명하게 트리거합니다. 예를 들어:

- 모든 브라우저 이벤트 (클릭, 마우스오버, 키다운 등)
- 타이머
- Ajax HTTP 요청

## 변경 감지 트리

애플리케이션 시작시, Angular는 각 컴포넌트에 대한 연관된 변경 감지기를 생성합니다:

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

마크다운 형식으로 변경해주세요.

![이미지](/assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_1.png)

## 변경 감지 전략

Angular에서는 두 가지 변경 감지 전략이 있습니다: Default와 OnPush입니다. 이러한 전략은 Angular에게 컴포넌트 트리 변경을 어떻게 확인하고 UI를 다시 렌더링할지 정의합니다.

기본 변경 감지

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

Angular에서 사용하는 기본 전략은 'ChangeDetectionStrategy.Default'입니다. 이 전략에서 Angular은 각 변경 감지 주기마다 모든 응용프로그램 컴포넌트의 변경을 확인합니다.

![이미지](https://miro.medium.com/v2/resize:fit:1200/1*z3NR45ds3DBBytzlNn_DGA.gif)

이 전략은 응용프로그램에 많은 컴포넌트가 있을 때 성능 문제가 발생할 수 있습니다.

OnPush 변경 감지

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

이 전략에서 Angular은 체크 과정을 최적화하려고 노력합니다. 그래서 체크는 다음의 경우에만 트리거됩니다:

- @Input의 참조가 변경될 때 ("===" 연산자로 비교);
- 이벤트가 컴포넌트나 그의 자식 중 하나에 의해 트리거될 때 (@Outputs나 @HostListeners로);
- "async" 파이프가 Observable로부터 새로운 값을 받을 때;
- 변경 감지가 수동으로 트리거될 때;

<img src="https://miro.medium.com/v2/resize:fit:1200/1*6cFRoSYEhaJ3xpKLXxkBmw.gif" />

## 신호(signals)가 성능 문제를 해결할 것입니다

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

두 가지 실제 변경 감지 전략은 성능 문제 때문에 항상 비판을 받습니다. 그래서 우리는 사용 사례에 따라 Default 또는 OnPush를 사용해야 합니다. Angular 팀은 다른 프레임워크 개발에서 zone.js의 사용으로 더 이상 경쟁할 수 없다는 것을 알고 있습니다. 그래서 Angular 반응성을 신호 없이하기 위해 노력하고 있습니다.

이 글의 맨 위에서 신호를 정의했듯이, 신호의 특이성은 값의 업데이트에 대해 Angular에 알린다는 것입니다. 이 알림은 Angular이 영향을 받은 UI만 다시 렌더링할 수 있도록 도와줍니다. 그래서 이제 변경된 내용을 확인하고 다시 렌더링할 컴포넌트 트리 전체를 확인할 필요가 없어졌습니다.

신호는 Angular의 새로운 변경 감지 시스템의 기초입니다. 그래서 Angular 팀은 개발자 미리보기로 V16에서 소개하고 V17.2에서 완전히 안정화했습니다.

성능 향상뿐만 아니라, 신호를 사용하면 Rxjs나 Observables을 사용하지 않고도 간단하고 읽기 쉽고 반응적인 코드를 얻을 수 있습니다.

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

## 시그널 사용의 장점

<img src="/assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_2.png" />

시그널을 사용하는 주요 이점은:

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

- zone.js 라이브러리가 필요하지 않아요.
- 코드가 더 간단하고 읽기 쉬워요.
- 변경 감지 프로세스에서 성능이 우수해요.
- 어플리케이션을 쉽게 테스트할 수 있어요.

## 신호의 종류

Angular은 세 가지 유형의 신호를 소개했어요:

- 쓰기 가능한 신호: .set() 또는 .update() 메서드를 사용하여 업데이트할 수 있는 신호들이에요.

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
// 새로운 시그널 정의하기
user_rating = signal < number > 0;

this.user_rating.set(5);
// 또는
this.user_rating.update((val) => val - 2);

console.log("Our User rating is :", this.user_rating());
// 예상 결과: 3
```

- Computed Signals: 다른 시그널에 의존하며, 해당 값은 종속 시그널이 변경될 때 다시 계산됩니다.

```js
// 등급 유형 (별 또는 엄지)에 대한 두 라디오 상자를 위한 시그널 정의
rating_type = signal < string > "STARS";

// 빈 이미지를 반환하는 계산된 시그널
image_empty_computed = computed(() => {
  return this.rating_type() === "STARS" ? "star_empty.png" : "thumb_empty.png";
});

// 채워진 이미지를 반환하는 계산된 시그널
image_filled_computed = computed(() => {
  return this.rating_type() === "STARS" ? "star_filled.png" : "thumb_filled.png";
});

console.log(this.image_empty_computed()); // 출력: star_empty.png
console.log(this.image_filled_computed()); // 출력: star_filled.png
```

- Effects: 효과는 응용 프로그램의 하나 이상의 시그널이 변경될 때 트리거되는 프로세스입니다.

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
// 새로운 신호를 정의하는 중입니다
user_rating = signal < number > 0;

this.user_rating.set(5);

// 변경 사항을 캡처하는 효과를 정의합니다
effect(() => {
  console.log("user rating has changed to : ", this.user_rating());
});

this.user_rating.update((val) => val - 2);
```

## Angular Team 로드맵

앞서 말한 대로, Angular 팀은 V16에서 개발자 미리보기로 신호 API를 출시하여 새로운 반응성 시스템의 기반이 되었고, V17.2에서 안정화되었습니다. V18에서는 성능 향상을 위해 zone을 제거할 준비를 지원하도록 개발자들에게 알리기 위해 개발자 미리보기로 zone 비활성화 기능을 추가했습니다.

이것이 Angular 팀의 로드맵입니다.

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

![이미지](/assets/img/2024-07-07-YoushouldmigratetoAngularsignalsNOW_3.png)

평가 애플리케이션의 완전한 예제를 이미 구현했습니다. 이 저장소에서 소스 코드를 확인할 수 있어요.

이 기사의 끝까지 읽어주셔서 감사합니다! 떠나시기 전에:

- 반드시 좋아요를 눌러주시고 작성자를 팔로우해주세요 🎉
- 제 소식을 팔로우해주세요: X | LinkedIn | YouTube
