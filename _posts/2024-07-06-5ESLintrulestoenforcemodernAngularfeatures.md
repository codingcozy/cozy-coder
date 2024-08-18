---
title: "Angular 모던 기능을 강화하는 5가지 ESLint 규칙"
description: ""
coverImage: "/assets/img/2024-07-06-5ESLintrulestoenforcemodernAngularfeatures_0.png"
date: 2024-07-06 03:11
ogImage:
  url: /assets/img/2024-07-06-5ESLintrulestoenforcemodernAngularfeatures_0.png
tag: Tech
originalTitle: "5 ESLint rules to enforce modern Angular features"
link: "https://medium.com/javascript-everyday/5-eslint-rules-to-enforce-modern-angular-features-c3f6e66d7c9e"
isUpdated: true
---

/assets/img/2024-07-06-5ESLintrulestoenforcemodernAngularfeatures_0.png

Angular은 최근 새로운 많은 기능을 소개했는데, 살펴볼 가치가 있습니다. 초기 단계는 이러한 추가 기능에 익숙해지는 것이며, 프로젝트에 통합하기 전에 이를 공부하는 것입니다.

하지만 이러한 기능을 모두 외우는 것은 어려울 수 있습니다, 특히 많은 기여자들이 있는 대규모 코드베이스에서 작업할 때입니다. 그래서 새로운 기능을 사용하도록 강제할 수 있는 신뢰할 수 있는 도구인 ESLint 같은 도구를 사용하는 것이 유익합니다.

본 블로그 포스트에서는 angular-eslint 패키지에서 여러분의 Angular 애플리케이션에 활성화할 고려해야 할 다섯 가지 ESLint 규칙에 대해 논의할 것입니다.

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

자가 닫히는 컴포넌트 태그

무언가 간단하면서 유용한 것부터 시작해봐요. 크게 혁신적이지는 않지만 가독성을 향상시키고 개발자 경험을 향상시킬 수 있어요.

다음 규칙은 이 패턴을 강제합니다:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.html"],
    extends: [
      ...angular.configs.templateRecommended,
      ...angular.configs.templateAccessibility,
    ],
    rules: {
      "@angular-eslint/template/prefer-self-closing-tags": ["warn|error"],
    },
  },
  ...
);
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

게다가, 이 규칙은 자동 수정을 지원합니다. 이를 통해 ng lint --fix를 실행하여 쉽게 self-closing 태그로 전환할 수 있습니다.

독립형 아키텍처

많은 개발자들이 이미 NgModule 없는 세계에 익숙해졌습니다. 그러나 다음 명령어를 사용하여 이를 강제화할 수 있습니다:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.ts"],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
    ],
    processor: angular.processInlineTemplates,
    rules: {
      "@angular-eslint/prefer-standalone": ["warn|error"],
    },
  },
  ...
);
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

안전한 쪽이 나중에 후회하는 것보다 나아요.

룰이 자동으로 수정을 지원하지만 스탠드얼론 모드로 전환하면 추가 작업이 필요할 수 있어요.

angular.json (nx.json) 파일에서 기본값을 설정할 수 있어요. 그러나 Angular CLI를 피할 수 있다는 것을 감안하면 ESLint 규칙은 보조 방어선 역할을 해요.

이미지 최적화

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

앵귤러에서는 이미지 자산과 관련된 일반적인 문제를 해결하기 위해 NgOptimizedImage 지시문을 도입했어요. 이 지시문은 Largest Contentful Paint (LCP)와 Cumulative Layout Shift (CLS)와 같은 핵심 웹 핵심 지표를 개선하는 데 초점을 맞추면서 전반적인 모범 사례를 준수하도록 설계되었어요. 지시문을 사용하는 것이 원시 src 속성보다 나은 선택임은 의심할 여지가 없어요.

다음 ESLint 규칙이 이를 다루고 있어요:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.html"],
    extends: [
      ...angular.configs.templateRecommended,
      ...angular.configs.templateAccessibility,
    ],
    rules: {
      "@angular-eslint/template/prefer-ngsrc": ["warn|error"],
    },
  },
  ...
);
```

내장된 제어 흐름 구문

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

유명한 NgFor 구조 지시문은 이제 더 이상 구성 요소의 템플릿에서 항목 집합을 반복하는 데 권장되지 않습니다. for 루프를 포함한 내장 제어 흐름 구문이 이제 선호됩니다. 이 대안은 더 직관적인 구문뿐만 아니라 중요한 성능 향상도 제공합니다.

다음 규칙은 새로운 솔루션의 강제 적용을 가능하게 합니다:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.html"],
    extends: [
      ...angular.configs.templateRecommended,
      ...angular.configs.templateAccessibility,
    ],
    rules: {
      "@angular-eslint/template/prefer-control-flow": ["warn|error"],
    },
  },
  ...
);
```

zoneless change detection

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

Angular 18은 zoneless 변경 감지를 실험적으로 지원하기 시작했습니다. 제 이전 블로그 게시물에서 언급했듯이, 코드가 OnPush 변경 감지 전략과 호환되는 경우, 이후에 출시될 zoneless 미래에 대비하여 준비가 되어 있습니다.

그러므로 다음 규칙을 사용하여 OnPush 변경 감지 전략을 적용하는 것이 유익합니다:

```js
// @ts-check
const eslint = require("@eslint/js");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = tseslint.config(
  {
    files: ["**/*.ts"],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
    ],
    processor: angular.processInlineTemplates,
    rules: {
      "@angular-eslint/prefer-on-push-component-change-detection": ["warn|error"],
    },
  },
  ...
);
```

게다가, 이를 angular.json (또는 nx.json) 파일의 기본값으로 설정할 수 있습니다.

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

요약하자면, Angular-eslint 패키지에서 가져온 다섯 가지 ESLint 규칙은 Angular 애플리케이션의 개발을 크게 향상시킬 수 있습니다. 이 규칙들은 최상의 실천 방법을 시행하고, 특히 다수의 기여자가 있는 대규모 코드베이스에서 코드 일관성을 유지하는 데 도움이 됩니다.

제 블로그 포스트가 마음에 드셨기를 바라며, 읽어 주셔서 감사합니다! 🙂
