---
title: "Nextjs에 Eslint 9 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-Eslint9NextjsSetupGuide_0.png"
date: 2024-08-03 18:33
ogImage:
  url: /assets/img/2024-08-03-Eslint9NextjsSetupGuide_0.png
tag: Tech
originalTitle: "Eslint 9 , Nextjs  Setup Guide"
link: "https://medium.com/linotte-technology/eslint-9-next-js-935c2b6d0371"
isUpdated: true
---

![이미지](/assets/img/2024-08-03-Eslint9NextjsSetupGuide_0.png)

# 공통 분모 - 현대 프론트 엔드 스택

새 프로젝트를 시작하거나 기존 프로젝트를 업그레이드할 때는 현재 트렌드를 확인하여 커뮤니티에서 가장 많이 지원하는 기술을 선택하는 것이 좋은 습관입니다.

웹 개발자로서, 가장 인기 있는 프론트엔드 프레임워크인 React, 가장 인기 있는 메타 프레임워크인 Next.js 및 가장 인기 있는 린터인 ESLint을 선택할 가능성이 높습니다. 실제로 React.dev는 Next.js를 권장하며 (프로덕션 등급 및 최신 기능 모두를 위해) Next.js에는 통합된 ESLint가 함께 제공됩니다.

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
$ npx create-next-app@latest

성공했습니다! ./my-app에 my-app가 생성되었습니다
```

```js
$ cd my-app && npm outdated
Package       Current    Wanted  Latest  Location                  Depended by
@types/node  20.14.14  20.14.14  22.0.3  node_modules/@types/node  my-app
eslint         8.57.0    8.57.0   9.8.0  node_modules/eslint       my-app
```

# 문제 설명 — Flat 구성 변경

2024년 4월에 ESLint는 새로운 주 버전인 v9.0.0을 출시했습니다. Flat 구성이 이제 기본 구성으로 설정되어 ESLint 사용자, 플러그인 개발자 및 통합자에게 큰 구조적 변경사항입니다. 더 나아지는 거죠?

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

![이미지](/assets/img/2024-08-03-Eslint9NextjsSetupGuide_1.png)

평평한 구성은 ESLint를 더 쉽게 접근할 수 있게 하며 명시적 종속성 로딩, 평평한 계단식 규칙, 통합된 형식 등을 통해 예상했던 현대적인 자바스크립트 개발자 경험을 제공합니다. 그러나 이주 과정에서 발생하는 고통 점들은 환영받기 어렵습니다. 이 이주 가이드에 나열된 30개 이상의 급격한 변화가 있습니다. 그리고 위에서 보여주었듯이, Next.js를 선택한 경우 호환성 문제로 ESLint의 이전 버전에 갇힐 수도 있습니다 : Next.js Pull Request #64141을 참조하세요.

# 해결 방안으로 가는 길 — Next.js Opt-out 및 ESLint 이주 도구

우리는 초기 상황을 제시했습니다. 다행스럽게도, ESLint 팀은 여러 이주 도구를 제공하는 데 열심히 노력해 왔습니다. 그러나 가장 간단한 Next.js 프로젝트조차 ESLint 9를 사용하도록 이주하는 것은 많은 문제를 야기합니다. 우리는 해결 방안으로 가는 길을 시연하려 합니다. 계속해서 우리의 my-app을 사용할 것입니다.

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

## 저는 ESLint 9을 설치했어요

```js
npm install --save-dev eslint@latest
```

몇 가지 경고가 나왔지만 잘 작동하는 것 같아요. 그런데 확실하게 하기 위해 npm install을 실행해 볼까요?

```js
npm error code ERESOLVE
npm error ERESOLVE could not resolve
...
npm error Conflicting peer dependency: eslint@8.57.0
...
npm error Fix the upstream dependency conflict, or retry
npm error this command with --force or --legacy-peer-deps
npm error to accept an incorrect (and potentially broken) dependency resolution.
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

--force를 사용하여 npm install을 계속 진행하면 미래에 더 많은 문제를 발생시킬 수 있습니다. 대신, 우리는 의존성 간에 ESLint 특정 버전을 강제해야 합니다.

## II ESLint 버전 재정의

package.json 파일에서 의존성이 사용하는 ESLint 버전을 재정의할 수 있습니다. 이를 통해 위에서 본 충돌을 제거할 수 있습니다.

```js
 {
   ...
   "devDependencies": {
     ...
     "eslint": "^9.8.0",
     ...
   },
+  "overrides": {
+    "eslint": "^9.8.0"
+  }
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

좋아요, 문제를 해결하는 데 이것이 작동하는지 확인하기 위해 npm install을 실행합시다.

```js
최신 상태, 506ms 내에 352개의 패키지를 감사함

135개의 패키지가 자금 지원을 찾고 있습니다
  세부 정보는 `npm fund`를 실행하십시오

0개의 취약점을 발견함
```

ESLint 9가 제대로 설치되었는데, 린터를 실행할 때 npm run lint를 실행하면 다음과 같은 오류가 발생합니다:

```js
유효하지 않은 옵션:
- 알 수 없는 옵션: useEslintrc, extensions, resolvePluginsRelativeTo, rulePaths, ignorePath, reportUnusedDisableDirectives
- 'extensions'가 제거되었습니다.
- 'resolvePluginsRelativeTo'가 제거되었습니다.
- 'ignorePath'가 제거되었습니다.
- 'rulePaths'가 제거되었습니다. 플러그인을 사용하여 규칙을 정의하십시오.
- 'reportUnusedDisableDirectives'가 제거되었습니다. 대신 'overrideConfig.linterOptions.reportUnusedDisableDirectives' 옵션을 사용하십시오.
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

이것은 Next.js 통합 ESLint 구성을 사용할 때 next 링크로 인해 발생합니다 (출처).

## III Next.js 통합 ESLint 비활성화

`next.config.mjs` 파일에 다음 라인을 사용하여 통합 ESLint를 쉽게 비활성화할 수 있습니다.

```js
 /** @type {import('next').NextConfig} */
 const nextConfig = {
+  eslint: {
+    ignoreDuringBuilds: true,
+  },
 };

 export default nextConfig;
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

패키지.json에서 자체 린트 규칙을 선언해야 합니다. 이 규칙은 빌드 규칙에서 사용될 것입니다.

```js
{
 ...
   "scripts": {
     "dev": "next dev",
-    "build": "next build",
+    "build": "npm run lint && next build",
     "start": "next start",
-    "lint": "next lint"
+    "lint": "eslint ."
   },
 ...
 }
```

한 번 더 npm run lint를 시도해 봅시다...

```js
이런! 뭔가 잘못됐어요! :(

ESLint: 9.8.0

ESLint가 eslint.config.(js|mjs|cjs) 파일을 찾을 수 없습니다.

ESLint v9.0.0부터 기본 설정 파일이 이제 eslint.config.js로 변경되었습니다.
만약 .eslintrc.* 파일을 사용하고 있다면, 이전 형식에서
새로운 형식으로 설정 파일을 업데이트하려면 이주 안내서를 따라주세요:

https://eslint.org/docs/latest/use/configure/migration-guide

이주 안내서를 따라도 문제가 계속되면 아래 링크로 방문하여
팀과 채팅해주세요:
https://eslint.org/chat/help
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

잘하고 있어요! 🙌 이제 새로운 flat config 파일을 만들어볼 수 있는 것 같아요.

## IV .eslintrc.json에서 eslint.config.mjs로 이주

ESLint에서 제공하는 eslint configuration migrator를 사용하여 새로운 flat config 파일을 만들어 봅시다.

```js
npx @eslint/migrate-config .eslintrc.json
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

아래 Markdown 형식으로 요구된 표 태그를 변경해주세요.

You might be prompted to install new eslint packages, let’s do that.

```js
npm install @eslint/js @eslint/eslintrc -D
```

If you followed step II this should work without any issue and you should get the following eslint.config.mjs file:

```js
import path from "node:path";
import { fileURLToPath } from "node:url";
import js from "@eslint/js";
import { FlatCompat } from "@eslint/eslintrc";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
const compat = new FlatCompat({
  baseDirectory: __dirname,
  recommendedConfig: js.configs.recommended,
  allConfig: js.configs.all,
});
export default [...compat.extends("next/core-web-vitals")];
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

한 번 더 npm run lint를 실행해 봐요!

```js
앗싸! 뭔가 잘못됐어요! :(

ESLint: 9.8.0

ESLint가 "eslint-plugin-react-hooks" 플러그인을 찾지 못했어요.

("/private/tmp/my-app" 디렉터리에서 Node 모듈로 로드될 때 "eslint-plugin-react-hooks" 패키지를 찾을 수 없었습니다.)

플러그인이 제대로 설치되지 않았을 가능성이 높아요. 아래 명령을 실행하여 다시 설치해 보세요:

    npm install eslint-plugin-react-hooks@latest --save-dev

"eslint-plugin-react-hooks" 플러그인은 " » eslint-config-next/core-web-vitals » /private/tmp/my-app/node_modules/eslint-config-next/index.js"의 구성 파일에서 참조되었어요.

문제를 해결하지 못한다면 https://eslint.org/docs/latest/use/troubleshooting을 참고해 주세요.
```

이번에는 필요한 의존성이 빠졌어요. ESLint가 플러그인 및 기타 종속성을 명시적으로 처리하는 방식이 변경되었기 때문이에요. 지금까지 이러한 것들은 Next.js에서 암시적으로 제공되었지만, 이제는 오류 로그에 설명된 대로 설치해야 해요.

## 누락된 ESLint 의존성 설치하기

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
npm install eslint-plugin-react-hooks@latest --save-dev
```

설치는 잘 진행되었지만, 다시 한 번 좋은 진전 🙌 단계를 거치면서 npm run lint를 실행할 때 새로운 종류의 문제가 발생했습니다.

```js
어이쿠! 뭔가 잘못되었어요! :(

ESLint: 9.8.0

TypeError: context.getAncestors is not a function
/private/tmp/my-app/src/app/page.tsx에서 린팅 중 오류 발생:4
규칙: "@next/next/no-duplicate-head"
    at ...
```

ESLint 9의 호환성 문제로 인해 새로운 문제가 발생했습니다. 특히, getAncestors context와 소스 코드 변경과 관련된 문제입니다. 이러한 문제는 상류에서 수정되지 않을 것이며, 대신 ESLint의 호환성 유틸리티를 사용할 것입니다.

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

## ESLint 호환 유틸리티 사용하기

우선, 호환 유틸리티를 설치해 봅시다.

```js
npm install @eslint/compat -D
```

이제 우리는 fixupPluginRules 함수에 액세스할 수 있게 되었습니다. 이전 @next/next/no-duplicate-head에서 이슈를 일으킨 플러그인 @next/next를 우리가 이전한 eslint.config.mjs 파일에서 찾게 될 것입니다. 아래 새로운 eslint.config.mjs 파일을 확인해 주세요:

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
import path from "node:path";
import { fileURLToPath } from "node:url";
import js from "@eslint/js";
import { FlatCompat } from "@eslint/eslintrc";
import { fixupPluginRules } from "@eslint/compat";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
const compat = new FlatCompat({
  baseDirectory: __dirname,
  recommendedConfig: js.configs.recommended,
  allConfig: js.configs.all,
});

const compatConfig = [...compat.extends("next/core-web-vitals")];

export default compatConfig.map((entry) => {
  const plugins = entry.plugins;
  for (const key in plugins) {
    if (plugins.hasOwnProperty(key) && key === "@next/next") {
      plugins[key] = fixupPluginRules(plugins[key]);
    }
  }
  return entry;
});
```

알았어요, npm run lint 명령어로 다시 테스트해봅시다! 이번에는 잘 동작해야 할 거에요 🎉
코드에서 린트 오류를 발생시킬 수 있는지 확인해야 해요; 예를 들어 컴포넌트 함수 이름을 삭제해볼 수 있어요.

## VII 구성 파일 정리

이제 잘 작동하는 구성 파일이 있으니, 정리하고 새로운 플러그인을 쉽게 추가하여 일반적인 문제를 수정할 수 있도록 리팩터링해봅시다.

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
// @ts-check
import path from "node:path";
import { fileURLToPath } from "node:url";
import js from "@eslint/js";
import { FlatCompat } from "@eslint/eslintrc";
import { fixupPluginRules } from "@eslint/compat";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
const compat = new FlatCompat({
  baseDirectory: __dirname,
  recommendedConfig: js.configs.recommended,
  allConfig: js.configs.all,
});

const pluginsToPatch = [
  "@next/next",
  // 다른 수정해야 할 플러그인들, 예시:
  // "react-hooks",
];

const compatConfig = [...compat.extends("next/core-web-vitals")];

const patchedConfig = compatConfig.map((entry) => {
  const plugins = entry.plugins;
  for (const key in plugins) {
    if (plugins.hasOwnProperty(key) && pluginsToPatch.includes(key)) {
      plugins[key] = fixupPluginRules(plugins[key]);
    }
  }
  return entry;
});

const config = [...patchedConfig, { ignores: [".next/*"] }];

export default config;
```

# 마무리 글

이 문제를 해결하는 것은 흥미로웠고, 새로운 문제를 해결하기 위해 훈련된 AI의 한계를 보여줍니다. Linotte에서는 AI에 대해 강력한 Linting 및 정적 분석 전략이 AI 코드 환각을 찾는 데 가장 중요한 안전망 중 하나라고 믿기 때문에 여러분이 그 목표를 달성하는 데 도울 수 있어 기쁩니다!

<img src="/assets/img/2024-08-03-Eslint9NextjsSetupGuide_2.png" />

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

ESLint 팀과 Next.js 팀에게 특별한 감사를 전합니다! 두 팀의 노력 덕분에 새로운 최신 도구를 활성화할 수 있는 충분한 유틸리티, 옵션 및 설정이 제공되었습니다!
