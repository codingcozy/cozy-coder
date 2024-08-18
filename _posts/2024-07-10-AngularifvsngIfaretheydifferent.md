---
title: "Angular if와 ngIf, 둘의 차이점은 무엇인가요"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:55
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Angular @if vs ngIf, are they different?"
link: "https://medium.com/@fixitblog/solved-angular-if-vs-ngif-are-they-different-5d4f4081c875"
isUpdated: true
---

새 @if를 사용하는 것과 전통적인 ngIf 지시문을 사용하는 것 중 어떤 것이 더 유익한가요?

\*ngIf 버전:

```js
class="lang-js prettyprint-override">@Component({
  standalone: true,
  selector: 'scrollbars',
  imports: [NgIf, ScrollbarX, ScrollbarY],
  template: `
    <scrollbar-x *ngIf="horizontalUsed()"/>
    <scrollbar-y *ngIf="verticalUsed()"/>
  `
})
```

@if 버전:

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
@Component({
  standalone: true,
  selector: 'scrollbars',
  imports: [ScrollbarX, ScrollbarY],
  template: `
    @if (verticalUsed()) {
      <scrollbar-y/>
    }
    @if (horizontalUsed()) {
      <scrollbar-x/>
    }
  `
})
```

문법 차이 외에도 @if가 작동하기 위해 NgIf나 CommonModule을 가져오지 않아도 된다는 점을 알았어요. 참 좋네요!

혹시 이것이 실제로는 어떻게 작동하는지 설명해주시는 분이 있다면 감사하겠어요!

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

기능적으로 맞아요, @if 블록이 ngIf 지시문의 사용을 대체합니다.

하지만, 새로운 제어 흐름 블록(@for/@if/@switch)을 사용하는 것에는 몇 가지 기술적 이점이 있습니다.

- 독립적인 컴포넌트를 빌드할 때 지시문이나 CommonModule을 가져오지 않아도 됩니다.
- 지시문을 없애면 주 번들에서 몇 KB를 더 줄일 수 있습니다.
- 더 적은 템플릿 지시문이 필요하기 때문에 일부 성능 향상이 예상됩니다. 이에 대한 자세한 내용은 이 기사를 참고하세요.

답변자: Matthieu Riegler

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

위 답변은 Willingham(FixIt 자원봉사자)님이 확인했습니다.

이 답변은 stackoverflow에서 수집되었으며 cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0으로 라이선스가 부여됩니다.
