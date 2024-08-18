---
title: "앵귤러에서 lazy loading 적용하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-18 10:57
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Did You Know Angular Has 3 Lazy Loading Levels"
link: "https://medium.com/javascript-in-plain-english/did-you-know-angular-has-3-lazy-loading-levels-da2877f813ad"
isUpdated: true
updatedAt: 1723951131645
---

## 안녕하세요 👋

Angular는 매일 더욱 강력하고 인기 있는 프레임워크가 되고 있습니다. 이 팁 기사에서는 Angular 프레임워크의 특별 기능 중 하나인 세 가지 지연 로딩 수준을 공유하고 싶습니다.

자세히 살펴보겠습니다!

# 📦 모듈 레벨

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

이것은 NgModule에서 loadChildren과 함께 사용하는 전통적인 수준입니다.

예시:

```js
const routes: Routes = [
  {
    path: "a-route",
    loadChildren: () => import("./path-to/feature.module").then((m) => m.FeatureModule),
  },
];
```

# 🧩 컴포넌트 레벨

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

최신 Angular 버전(15부터)의 독립 기능 덕분에 Angular 앱의 성능을 더욱 향상시킬 수 있는 컴포넌트를 지연 로드할 수 있습니다.

예시:

```js
const routes: Routes = [
  {
    path: "a-route",
    loadComponent: () => import("./path-to/a-component.component").then((m) => m.AComponent),
  },
];
```

독립 기능에 관심이 있다면, 이 아티클을 읽어보시기를 강력히 추천합니다:

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

# `/` 템플릿 레벨

컴포넌트 템플릿 내에서의 지연 로딩은 Angular 17에서 도입된 deferrable views 기능 덕분에 가능해졌습니다. 이 강력한 기능을 통해 컴포넌트를 템플릿 내에서 지연 로딩하여 앱을 최적화할 수 있으며, 특히 대규모 컴포넌트에 유용합니다.

예시:

```js
@Component({
  selector: "app",
  standalone: true,
  template: `
    <h2>즉시 표시될 일부 내용...</h2>
    @defer (상호작용 시; 화면에 prefetch) {
    <a-large-component />
    }
  `,
})
export class AppComponent {}
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

위대한 기능에 대해 더 알아보세요:

오늘의 빠른 팁이 도움이 되었기를 바랍니다. 다음 팁을 기대해 주세요 🙋

질문이나 피드백이 있으시면 답글을 남기거나 LinkedIn을 통해 연락해 주세요. 언제든지 귀 기울일게요!

커피 한 잔 해주고 싶으신가요? ☕️

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

# 쉽게 설명한 영어 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 글쓴이를 추천하고 팔로우하세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 즐기세요
