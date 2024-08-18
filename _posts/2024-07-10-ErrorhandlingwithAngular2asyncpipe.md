---
title: "Angular2 async pipe를 사용하여 에러 처리하는 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:57
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Error handling with Angular2 async pipe"
link: "https://medium.com/@fixitblog/solved-error-handling-with-angular2-async-pipe-bc48e674272a"
isUpdated: true
---

저는 Angular2의 async 파이프를 사용하여 값을 DOM에 스트리밍하는 중입니다. 여기에 간단한 예제가 있습니다:

```js
const stream = Observable.interval(1000)
  .take(5)
  .map(n => { if (n === 3) throw "ERROR"; return n; });

<div *ngFor="let num of stream | async">
  {{num}}
</div>

<div id="error"></div>
```

저는 1부터 5까지의 시퀀스를 표시하려고 합니다. 하지만 에러 아이템(3)의 경우, 어떻게 하면 #error div에 에러 메시지를 표시할 수 있을까요?

이를 위해서 두 가지가 필요한 것 같습니다: 첫째, Angular async 파이프가 에러를 처리할 수 있는 기능이 필요합니다. 그러나 이를 수행하는 기능을 찾을 수 없습니다. 소스 코드를 살펴본 결과, 이 파이프는 JS 예외를 throw 하는 것으로 보여 친근하지 않아 보입니다.

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

두 번째로 에러가 발생한 후 일련의 순서를 다시 시작하거나 계속할 수 있는 능력이 있습니다. catch와 onErrorResumeNext 등에 대해 읽어봤는데, 모두 에러가 발생할 경우에 다른 시퀀스로 전환됩니다. 이렇게 하면 단순히 일련의 숫자를 넣고 싶은 스트림 생성 로직이 복잡해집니다. 한 번 에러가 발생하면 게임이 끝나고 observable은 완료되어 다른 observable로만 "재시작"될 수 있다는 느낌을 받습니다. 아직 옵저버블을 배우고 있지만, 실제로 그런가요?

그래서 제 질문은 두 가지입니다:

- Angular2의 async 파이프가 에러를 적절하게 처리할 수 있을까요?
- 옵저버블은 에러 발생 후 간단히 계속할 수 있는 방법이 있나요?

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

네, 오류 처리 연산자와 오류 발생 후에 작업을 수행할 수 있는 능력에 대해 올바르게 이야기하셨어요...

나는 오류를 잡고 어떤 일을 수행하기 위해 catch 연산자를 활용할 거에요:

```js
const stream = Observable.interval(1000)
  .take(5)
  .map(n => {
    if (n === 3) {
      throw Observable.throw(n);
    }
    return n;
  })
  .catch(err => {
    this.error = error;
    (...)
  });
```

그리고 템플릿에서:

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
<div>{error}</div>
```

초기 observable을 계속해서 진행하려면, 오류가 발생하는 지점부터 새로운 observable을 생성해야 합니다:

```js
createObservable(i) {
  return Observable.interval(1000)
    .range(i + 1, 5 - i)
    .take(5 - i);
}
```

그리고 catch 콜백에서 이용하세요:

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
.catch(err => {
    this.error = error;
    return this.createObservable(err);
  });
```

다음 두 가지 질문이 유용할 수 있습니다:

- RxJS5에서 resumeOnError (또는 유사한 것)은 어떻게 사용하나요?
- RxJS에서 Ajax 오류 후 계속 수신하는 방법 (최신 답변)

Thierry Templier의 답변입니다.

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

답변 확인자: Clifford M. (수정 도와주는 자원봉사자)

이 답변은 stackoverflow에서 수집되었으며 cc by-sa 2.5, cc by-sa 3.0, cc by-sa 4.0 라이선스에 따라 사용이 허가됩니다.
