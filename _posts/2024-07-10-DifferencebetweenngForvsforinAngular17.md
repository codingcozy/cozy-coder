---
title: "Angular 17에서 ngFor와 for의 차이점"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:51
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Difference between *ngFor vs @for in Angular 17"
link: "https://medium.com/@fixitblog/solved-difference-between-ngfor-vs-for-in-angular-17-550e9ca4b199"
isUpdated: true
---

Angular 17으로 몇 개의 Angular 프로젝트를 업그레이드 및 이전하려고 노력 중입니다. 또한 해당 버전의 주요 업그레이드 일환으로 스탠드얼론 컴포넌트를 학습 중입니다.

주목한 한 가지는 문서에서 이제 @를 사용한다는 것이에요.

```js
@for (car of cars; track car) {
   <option [value]="car.value">{car.viewValue}</option>
}
```

\*ngFor 대신에요.

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
<option *ngFor="let car of cars" [value]="car.value">{{car.viewValue}}</option>
```

정말 솔직히 \*ngFor를 여전히 좋아하고 있어요.

이 둘 사이에 어떤 차이가 있는 건가요? 아니면 @for이 새로운 *ngFor인가요? 만약 그게 사실이라면, 왜 그렇게 한 건가요? *ngFor가 훨씬 간단하고 한 줄로 작성될 수 있어요.

# 해결책

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

Angular 17.0에서 소개된 새로운 제어 흐름 구문 중 하나인 @for 블록입니다.

제어 흐름 블록(@if, @for, @switch)은 ngIf, ngFor 및 ngSwitch와 같은 3가지 구조 지시어를 대체하기 위해 도입되었습니다.

@for 블록의 경우, 기능적으로 동일한 목적을 수행하면서 몇 가지 장점이 있습니다:

- 독립형 구성 요소에 지시어를 가져올 필요가 없습니다.
- 최종 번들에서 코드를 조금 더 생성합니다.
- 키를 직접 전달하여 track by의 DX를 개선합니다.
- 트랙 기능에 대해 개발자들이 더 의식적이 되도록 만들어 필요합니다.
- @empty 블록을 사용하여 빈 for-루프에 대한 좋은 DX를 제공합니다.

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

위의 텍스트를 한국어로 친근하게 번역해 드리겠습니다.

응답자 — Matthieu Riegler

확인자 — Willingham (FixIt 자원봉사자)

이 응답은 stackoverflow에서 수집되었으며 cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0 라이선스에 따라 사용이 허가됩니다.
