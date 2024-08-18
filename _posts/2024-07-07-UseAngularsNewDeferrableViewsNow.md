---
title: "Angular의 새로운 Deferrable Views 사용 방법 바로 알아보기"
description: ""
coverImage: "/assets/img/2024-07-07-UseAngularsNewDeferrableViewsNow_0.png"
date: 2024-07-07 19:58
ogImage:
  url: /assets/img/2024-07-07-UseAngularsNewDeferrableViewsNow_0.png
tag: Tech
originalTitle: "Use Angular’s New Deferrable Views Now"
link: "https://medium.com/angular-blog/use-angulars-new-deferrable-views-now-20ca9f64f1e5"
isUpdated: true
---

<img src="/assets/img/2024-07-07-UseAngularsNewDeferrableViewsNow_0.png" />

앵귤러 커뮤니티에서는 컴포넌트, 디렉티브 및 파이프를 지연 로드하는 직관적이고 내장된 방법을 요청해 왔습니다. 앵귤러 v17에서, 우리는 Deferrable Views를 만들었습니다. 이 새로운 기능은 의존성을 지연 로드하는 프로세스를 상당히 직관적으로 만들어줍니다. 새로운 컨트롤 플로우 구문과 함께 이러한 Deferrable Views 기능을 만들었는데, 이 구문은 동일한 "@-구문"을 사용합니다. 우리의 목표는 새로운 구문을 컴포넌트 템플릿에서 사용할 때 직관적이고 익숙하게 만드는 것입니다.

Deferrable views를 사용하면 초기 번들 크기를 최적화하고 애플리케이션의 초기 로드를 개선할 수 있습니다. 나중에 필요할 때까지 템플릿 일부를 지연 로드하는 것을 선언적으로 허용함으로써 이점을 취할 수 있습니다. 예를 들어, 폴드 아래에 있는 큰 컴포넌트를 지연 로드하고 뷰포트에 진입하면 로드되도록 원할 수 있습니다. 또는 클릭 상호작용에 따라 컴포넌트를 로드하고 싶을 수도 있습니다. 앵귤러 v17에서 deferrable views를 사용하여 이러한 경우와 더 많은 경우를 템플릿에서 네이티브로 처리할 수 있습니다. 이는 컴포넌트 코드가 애플리케이션이 편리하게 로드되는 시점에 로드되는 별도의 번들에 있음을 의미합니다.

지연 로드된 라우트는 이전과 같이 작동합니다. 라우트 수준에서의 지연 로딩은 라우트별로 애플리케이션 청크를 분할하는 훌륭한 방법을 제공합니다. 라우트를 지연 로드하여 애플리케이션을 위해 청크 최적화를 극대화하는 방법을 deferrable views와 결합할 수 있습니다. 이것이 최적화된 앱을 위한 최적의 방법입니다.

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

# 빠른 하이라이트

우리는 선언적 경험을 제공하는 지연 로드 가능한 뷰에 흥분하고 있습니다. 개발자들이 템플릿에서 명확한 의도를 표현할 수 있도록 합니다. 사용자가 지루한 동적 임포트 관리, 교차 옵저버 사용 또는 이벤트 핸들러 추가할 필요가 없습니다. 지연 로드 가능한 뷰에는 이 모든 기능이 이미 내장되어 있고 사용자를 위해 관리됩니다. 우리는 독립적으로 또는 함께 사용할 수 있는 일련의 통합된 트리거를 만들었으며, 사용자 정의 조건과 결합하여 언제 어떻게 지연 로딩을 수행할지 정확히 지정할 수 있습니다. 다음은 예제입니다:

```js
<blog-post-cmp />
@defer (on viewport) {
  <comments />
} @placeholder {
  <img src=”placeholder.png” alt=”placeholder” />
}
```

이 예제에서는 내장된 플레이스홀더 블록을 사용하여 지연 로드가 발생하기 전에 표시되는 내용을 사용자 정의합니다. 로딩 및 에러 블록을 사용하여 표시되는 내용을 더욱 자유롭게 사용자 정의할 수 있으며, 응용 프로그램에서 최상의 사용자 경험을 제공할 수 있는 유연성을 제공합니다.

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

# 미리 가져오기

보류 중인 뷰는 게으른 로딩에 그치지 않습니다. 미리 가져오기를 네이티브로 지원하기 때문에, 리소스가 단순히 미뤘을 때보다 더 빨리 이용할 수 있습니다. 미리 가져오기는 모든 동일한 트리거와 사용자 정의 조건을 지원하며 애플리케이션의 보류 중인 로딩 경험을 완전히 사용자화할 수 있게 해줍니다. 다음 예제를 고려해보세요:

```js
<blog-post-cmp />
@defer (on viewport; prefetch on idle) {
  <comments />
} @placeholder {
  <img src=”placeholder.png” alt=”placeholder” />
}
```

댓글 컴포넌트는 뷰포트에 진입하는 즉시 렌더링됩니다. 먼저 가져오기를 기다리지 않습니다. 멋지지 않나요? 우리는 그렇게 생각합니다.

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

# 불필요한 코드 줄이기

데퍼러블 뷰를 사용할 때 종속성을 관리할 필요가 없습니다. 모든 종속성 관리 작업은 뒷단에서 처리됩니다. @defer 블록을 작성하고 해당 블록 내에서 보통처럼 컴포넌트를 사용하면 됩니다. Angular는 종속성을 자동으로 추출하고 동적 임포트를 생성해줍니다. 이는 애플리케이션에서 지연 로딩이 작동하도록 하는데 더 이상 boilerplate를 쓸 필요가 없다는 뜻입니다.

# 미래를 내다보며

데퍼러블 뷰는 우리가 발전시킬 수 있는 견고한 기반이 됩니다. 우리가 탐험 중인 첫 번째 과제는 defer 블록을 컴포넌트 비동기 초기화를 기다릴 수 있도록 가르치는 방법입니다. 커뮤니티가 표현한 관심사는 데퍼러블 뷰를 데이터 가져오기 이야기와 더 통합시킬 수 있는 방법입니다. 구체적으로 말하면 defer 블록이 데이터 가져오기가 완료됐을 때 트리거되게 하는 것입니다. 우리는 이 문제에 대한 최상의 해결책을 현재 논의 중입니다. 추가로, 데퍼러블 뷰는 점진적 유수화에 좋은 기반 요소입니다.

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

데펀더블 뷰는 현재 개발자 미리 보기 상태에 있습니다. 개발자들이 이 새로운 기능을 채택함에 따라 피드백을 수집하고 향후 릴리스에서 개발자 미리 보기를 종료할 계획으로 지속적으로 개선할 것입니다.

더 자세히 알고 싶다면, angular.dev의 데펀더블 뷰 가이드를 확인하고 브라우저에서 바로 학습할 수 있는 튜토리얼을 방문해보세요.

Angular 팀에서 Andrew Kushnir가 이 블로그 포스트를 공동 저술했습니다.
