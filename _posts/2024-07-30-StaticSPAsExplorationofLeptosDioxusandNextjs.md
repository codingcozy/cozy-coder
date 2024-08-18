---
title: "정적 SPA Leptos, Dioxus, Nextjs 비교 탐구"
description: ""
coverImage: "/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_0.png"
date: 2024-07-30 17:21
ogImage:
  url: /assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_0.png
tag: Tech
originalTitle: "Static SPAs Exploration of Leptos, Dioxus, and Nextjs"
link: "https://medium.com/@codethoughts/static-spas-exploration-of-leptos-dioxus-and-next-js-da2f00ae8f61"
isUpdated: true
---

내가 좋아하는 프론트엔드 배포 방법 중 하나는 모든 경로를 정적으로 미리 생성한 다음 각 경로가 상호 작용에 필요로 하는 종속성을 로드하도록 하는 것입니다.

- 로드 시간이 빠릅니다.
- 사용자는 필요한 것만 다운로드합니다 (느린 또는 모바일 네트워크에 적합).
- SEO는 많은 경우에 좋고 다른 경우에 괜찮습니다.
- 여전히 CDN + 정적 자산이 제일 최적인 방법으로 배포될 수 있습니다.

인프라 확장 가능성, 유지 관리, 견고함 및 비용에 대한 관심이 많습니다. SSR은 각각을 단순히 정적 파일을 제공하는 것과 비교했을 때 많이 악화시킵니다. 또한 API를 프론트엔드 코드에 혼합하는 것(즉, 서버 액션)은 팀 규모를 확장할 때 도움이 되지 않을 것이라고 믿습니다. 하지만 이는 다른 날의 주제입니다.

이 게시물에서는 이 구성을 어떻게 설정하고 몇 가지 다른 프레임워크를 비교할 수 있는지 알아보겠습니다. 자세한 내용에 들어가기 전에 전반적인 결과부터 시작해 보겠습니다.

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

<img src="/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_0.png" />

\*: WASM에서 청킹 지원은 일반적으로 부족하지만, 최근에 이에 대한 발전이 있었습니다 (wasm-bindgen#3939 참조)

\*\*: Dioxus 지원은 변동이 심해 정적 생성을 지원하는 방식이 계속 바뀌고 개선 중이며, 현재 문서와 예제가 작동하지 않는 상태입니다 (dioxus#2587로 추적됨)

아직 "정적 SPA"가 정확히 무엇인지 애매하다면, Next.js를 사용하여 예제를 설정해보겠습니다. 이는 Next.js에서 기본적으로 지원하는 기능입니다. 이 용어는 약간 복잡하며, React는 여전히 렌더링이 컴파일 시에만 발생하는 경우에도 여전히 SSR로 표시할 수 있다고 생각합니다. Next.js에서는 이를 페이지 라우터에서 SSG로 부르고 있으며, 최신 앱 라우터에서는 혼란스러운 역할을 하는 이를 정적 Export로 변경하고 있습니다.

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

각 프레임워크에서 이를 어떻게 달성하는지 비교해 보겠습니다:

- Next.js SSG
  ∘ 기본 Next.js 프로젝트 확장
  ∘ SSG 구성
  ∘ JavaScript 비활성화 상태에서 테스트
  ∘ 선택적 구성
  ∘ 추가 읽을거리
- Leptos SSG
  ∘ 기본 Leptos 프로젝트 확장
  ∘ SSG 구성
  ∘ JavaScript 비활성화 상태에서 테스트
- Dioxus SSG
  ∘ 기본 Dioxus 프로젝트 확장
  ∘ SSG 구성
  ∘ JavaScript 비활성화 상태에서 테스트

[¹]: 적어도 그것이 관심사일 때이고, 앱이 이미 인증 뒤에 있지 않은 경우이며, 이 경우에는 SEO 측면이 전혀 중요하지 않습니다.

[²]: 자산을 생성하고 제공할 수있는 서버가 실행되어야 하는 SSR (서버 측 렌더링)와 달리

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

# Next.js SSG

안녕하세요! Next.js 15 릴리스 후보를 사용하여 새 Next.js 프로젝트를 설정해 보려고 해요. 이를 통해 React와 Next.js의 최신 기술 발전을 테스트할 수 있어요.

기본 값 대부분을 수락할 거에요:

```js
$ bunx create-next-app@rc --turbo
✔ 프로젝트 이름은 무엇인가요? … next-example
✔ TypeScript를 사용하시겠습니까? … 예
✔ ESLint를 사용하시겠습니까? … 예
✔ Tailwind CSS를 사용하시겠습니까? … 예
✔ `src/` 디렉토리를 사용하시겠습니까? … 아니요
✔ App Router를 사용하시겠습니까? (추천) … 예
✔ next dev에 Turbopack를 사용하시겠습니까? … 예
✔ 기본 import alias(@/*)를 사용자 정의하시겠습니까? … 아니요
./next-example에 새 Next.js 앱을 생성 중이에요.
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

만약 next-example 폴더로 이동한 다음 bun run dev를 실행하면 귀여운 작은 시작 페이지를 얻을 수 있어요:

![StaticSPAsExplorationofLeptosDioxusandNextjs_1](/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_1.png)

## 기본 Next.js 프로젝트 확장

나중에 생성된 것을 실제로 볼 수 있게 하려면, 단순한 시작 페이지 이상의 페이지가 필요해요. 몇 가지 테스트 콘텐츠가 있는 새 페이지를 추가해 보죠.

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

두 개의 페이지를 설정할 거에요. 메인 페이지의 스타일을 재사용하여, 첫 번째 페이지는 next-example/app/sub-page/page.tsx에 설정할 거에요 (누락된 sub-page 디렉토리를 생성해주세요):

```js
"use client";

import { useState } from "react";

export default function SubPage() {
  const [counter, setCounter] = useState(0);
  return (
    // 스타일 재사용.
    <div className="font-sans grid grid-rows-[20px_1fr_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20">
      <main className="flex flex-col gap-8 row-start-2 items-center sm:items-start">
        {/* 제 컨텐츠 */}
        <h1>SubPage</h1>
        <p>이곳은 상호작용이 있는 서브페이지입니다: {counter}</p>
        <button onClick={() => setCounter((prev) => prev + 1)}>증가</button>
      </main>
    </div>
  );
}
```

그리고 다른 비슷한 페이지는 next-example/app/another-page/page.tsx에 설정할 거에요 (누락된 another-page 디렉토리를 생성해주세요):

```js
"use client";

import { useState, useEffect } from "react";

export default function AnotherPage() {
  const [windowHeight, setWindowHeight] = (useState < number) | (undefined > undefined);
  // 브라우저 API를 호출하기 전에 window 객체가 사용 가능한지 확인해요.
  useEffect(() => {
    setWindowHeight(window.innerHeight);
  }, []);

  return (
    // 스타일 재사용.
    <div className="font-sans grid grid-rows-[20px_1fr_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20">
      <main className="flex flex-col gap-8 row-start-2 items-center sm:items-start">
        {/* 제 컨텐츠 */}
        <h1>Another Page</h1>
        <p>
          브라우저 API를 호출하는 다른 페이지에요
          {windowHeight ? `: ${windowHeight}` : ""}
        </p>
      </main>
    </div>
  );
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

이 두 예제 모두 클라이언트 측에서 발생하는 작업들을 하고 있기 때문에, 우리는 상단에 "use client" pragma를 추가해야 합니다.

마지막으로, 이러한 페이지들을 우리의 앱 진입점에서 다음 예제/app/page.tsx로부터 링크할 것입니다.

```js
import Image from "next/image";
import Link from "next/link"; // 이 부분을 추가하시면 됩니다.

export default function Home() {
  return (
    <div className="font-sans grid grid-rows-[20px_1fr_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20">
      <main className="flex flex-col gap-8 row-start-2 items-center sm:items-start">
        <Image
          className="dark:invert"
          src="/next.svg"
          alt="Next.js 로고"
          width={180}
          height={38}
          priority
        />
        <Link href="/sub-page">서브 페이지</Link> {/* <-- 이 부분을 추가하시면 됩니다. */}
        <Link href="/another-page">다른 페이지</Link> {/* <-- 이 부분을 추가하시면 됩니다. */}
        {/* 나머지 내용을 유지합니다 */}
```

이제 개발 서버를 실행하여 모든 것이 잘 작동하는지 확인할 수 있습니다. 다음 예제 디렉토리에서 `npm run dev`를 실행하면 됩니다.

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

이제 몇 페이지를 렌더링할 예정이니, 상호작용성과 브라우저 API를 사용해 어떻게 할 지 살펴봅시다.

## SSG 구성

첫 번째 변경 사항은 빌드 출력을 Next.js가 App Router에서 SSG라고 부르는 export로 변경하는 것입니다.

next-example/next.config.mjs 파일을 열고 다음 라인을 추가해주세요:

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
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export", // <-- 이 줄이 여기 있습니다.
  // 각 route마다 index.html 파일을 생성합니다.
  trailingSlash: true, // <-- 이 줄이 여기 있습니다.
};

export default nextConfig;
```

trailingSlash 구성은 배포 방법에 따라 다를 수 있지만, 기본적으로 간단한 정적 파일 서버가 경로를 재작성하는 논리 없이 파일을 제공할 수 있도록합니다.

이제 next-example 디렉토리에서 bun run build를 실행하여 생성된 빌드 자산을 검사할 수 있습니다.

```js
$ bun run build
...
Route (app)                              Size     First Load JS
┌ ○ /                                    13.3 kB         102 kB
├ ○ /_not-found                          900 B          89.8 kB
├ ○ /another-page                        882 B          89.8 kB
└ ○ /sub-page                            872 B          89.8 kB
+ First Load JS shared by all            88.9 kB
  ├ chunks/180-42348583fa4569ac.js       35.6 kB
  ├ chunks/4bd1b696-a08a63850fcad1d6.js  51.4 kB
  └ other shared chunks (total)          1.86 kB
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

out/ 폴더에 있는 Next.js에서 생성된 내용을 확인해보세요:

```js
out
├── 404
│   └── index.html
├── 404.html
├── _next
│   ├── K46Mb3V6rvnSZkb3S0DFD
│   └── static
│       ├── K46Mb3V6rvnSZkb3S0DFD
│       │   ├── _buildManifest.js
│       │   └── _ssgManifest.js
│       ├── chunks
│       │   ├── 182-5aa8ba6aa9ca46c7.js
│       │   ├── 4bd1b696-3f85179bbee9de79.js
│       │   ├── 932-e409b1b1d42740a9.js
│       │   ├── app
│       │   │   ├── _not-found
│       │   │   │   └── page-1477ca64449fa6ca.js
│       │   │   ├── another-page
│       │   │   │   └── page-aa4b7b15eb983969.js
│       │   │   ├── layout-94b4bb4f7b4ed0fc.js
│       │   │   ├── page-18c8594afae77168.js
│       │   │   └── sub-page
│       │   │       └── page-b5ac62c0a67a677a.js
│       │   ├── framework-d2f4bc65ced8d4a1.js
│       │   ├── main-286537da132b2fda.js
│       │   ├── main-app-5fa6097c9a9b5d34.js
│       │   ├── pages
│       │   │   ├── _app-daa5cb8560567b4d.js
│       │   │   └── _error-4cacf33de97c3163.js
│       │   ├── polyfills-78c92fac7aa8fdd8.js
│       │   └── webpack-d22356f1f4db00c4.js
│       ├── css
│       │   └── ba86f67683758cf0.css
│       └── media
│           ├── 4473ecc91f70f139-s.p.woff
│           └── 463dafcda517f24f-s.p.woff
├── another-page
│   ├── index.html
│   └── index.txt
├── favicon.ico
├── file-text.svg
├── globe.svg
├── index.html
├── index.txt
├── next.svg
├── sub-page
│   ├── index.html
│   └── index.txt
├── vercel.svg
└── window.svg
```

여기서 흥미로운 파일들은:

- out/index.html: 초기 페이지에 대한 생성된 HTML
- out/sub-page/index.html: 하위 페이지에 대한 생성된 HTML
- out/\_next/static/chunks/app/sub-page/page-b5ac62c0a67a677a.js: 하위 페이지에 특화된 JavaScript 파일이며 해당 페이지에서만 로드됩니다.
- out/another-page/index.html: 다른 페이지에 대한 생성된 HTML
- out/\_next/static/chunks/app/another-page/page-aa4b7b15eb983969.js: 다른 페이지에 특화된 JavaScript 파일

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

JavaScript를 더 작은 파일 로 나누면 좋아요. 일부는 공유되고 일부는 특정 페이지로, 그렇게 함으로써 사용자들에게 최소한으로 필요한 JavaScript 만로드하게 되어 페이지와 상호 작용할 수 있게 해줘요.

## JavaScript 비활성화로 테스트하기

"Chrome DevTools"을 열고 CMD + Shift + P를 누르고 "JavaScript 비활성화"를 입력하여 Chrome에서 JavaScript가 비활성화된 상태로 사이트를 테스트할 수 있어요.

![이미지](/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_2.png)

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

우리는 그 다음에 cd next-example/out && bunx simplehttpserver로 정적 파일을 제공할 수 있습니다. 몇 가지 비교를 해보겠습니다. JavaScript를 실행했을 때와 JavaScript를 비활성화했을 때의 결과를 확인해 보겠습니다.

먼저, 시작 페이지입니다. 링크가 모두 작동하고 기능에 다른 변화가 없기 때문에 완전히 동일하게 보입니다. 여기에서는 아무 상호작용이 없어서 변화가 없습니다.

우리의 하위 페이지는 외관상 동일하지만 JavaScript를 비활성화한 상태에서 Increment 버튼과 상호작용을 시도하면 작동하지 않음을 확인할 수 있습니다.

우리는 또 다른 곳에서 약간의 차이를 발견하는데, JavaScript를 비활성화했을 때 브라우저 높이를 가져 오는 useEffect가 호출되지 않습니다. 이것은 놀라운 것이 아닙니다.

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

위의 것은 정확히 이 방법의 혜택을 보여줍니다: 비용을 지불하지 않고도 SSR의 대부분의 이점을 누릴 수 있습니다. SEO 크롤러는 동적 부분을 제외하고 대부분의 콘텐츠를 읽을 수 있을 것입니다.

정적 익스포트에서 할 수 있는 일이 훨씬 더 많습니다. 예를 들어, 각 언어마다 모든 i18n 루트를 사전 생성하는 동적 루트를 생성할 수 있습니다.

이 게시물에서 사용하는 Next.js 설정 예제가 있습니다:

## 선택 구성

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

Next.js 웹사이트를 사용자에게 더 나은 경험을 제공하기 위해 몇 가지 추천드리고 싶어요.

먼저 React 컴파일러의 실험적인 지원을 활성화하는 것을 추천드립니다. 아래 명령어로 설치해주세요:

```js
$ bun install --dev babel-plugin-react-compiler
```

다음으로 next-example/next.config.mjs 파일을 업데이트해주세요.

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
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
  // 출력물에 각 경로에 대한 index.html 파일을 생성합니다.
  trailingSlash: true,
  // 선택 사항: 실험적인 React Compiler 지원 활성화
  experimental: {
    reactCompiler: true, // <-- 여기에 이 줄을 추가해주세요
  },
};

export default nextConfig;
```

## 추가 정보

지원되는 기능 및 지원되지 않는 기능에 대한 주의 사항을 읽어보는 것이 좋습니다. 몇 가지 스니펫을 확인해보세요:

그리고:

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

## Leptos SSG

이제 우리가 목표가 무엇이며 왜 이를 달성하려고 하는지 명확한 구체적인 아이디어를 갖게 되었으니, Leptos와 WASM을 사용하여 유사한 설정을 만들어 볼 수 있는지 확인해 봅시다. 불행하게도 정적 경로가 아직 지원되지 않으므로 새로운 0.7 알파 테스트를 할 수는 없습니다.

그러나 제가 수정한 하위 경로를 올바르게 생성하는 문제(leptos#2667)가 통합될 때까지 해당 기능이 포함된 브랜치를 사용해야 합니다. 그것이 병합되고 새 릴리스가 이뤄질 때까지 기다리는 것을 추천합니다.

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

이미 cargo-leptos와 cargo-generate가 없는 경우, 먼저 설정해보겠습니다. 그렇게 해서 leptos-rs/start-axum 스타터 템플릿을 사용할 수 있게 됩니다:

```js
$ cargo binstall cargo-generate # 또는: cargo install cargo-generate
$ cargo binstall cargo-leptos # 또는: cargo install cargo-leptos --locked
```

이제 leptos-example이라는 이름으로 새로운 Leptos 프로젝트를 시작할 수 있습니다:

```js
$ cargo leptos new --git leptos-rs/start-axum
🤷   프로젝트 이름: leptos-example
🤷   Nightly 기능 사용? · 예
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

"leptos-example" 폴더로 이동하여 러스트에서 런타임을 설치하는 것으로 프로젝트 설정을 마무리할 거에요. 컴파일러 타겟을 추가해봅시다:

```js
$ rustup toolchain install nightly --allow-downgrade
$ rustup target add wasm32-unknown-unknown
```

마지막으로, 생성된 파일에 해시를 적용하고 싶어요. "leptos-example/Cargo.toml" 파일을 수정해주세요:

```js
# ...기존 [package.metadata.leptos]에 아래 내용 추가
[package.metadata.leptos]
# 출력된 css, js 및 wasm 파일에 관련 파일 해시 추가
#
# 선택 사항: 기본값은 false입니다. LEPTOS_HASH_FILES=false 환경 변수로 설정할 수도 있어요.
hash-files = true
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

만약 cd leptos-example을 실행한 후 cargo leptos watch를 실행하면 귀여운 작은 시작 페이지를 볼 수 있어요:

![이미지](/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_3.png)

기본 흰 배경에서 눈이 아플 수 있으니까 default leptos-example/style/main.scss를 다음과 같이 업데이트할 수 있어요:

```js
body {
    font-family: sans-serif;
    text-align: center;
    background: #0a0a0a;
    color: #ededed;
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

## 기본 Leptos 프로젝트 확장

Next.js와 마찬가지로 나중에 생성된 흥미로운 내용을 실제로 볼 수 있기 위해서는 시작 페이지 하나 이상의 추가 페이지가 있어야 합니다. 몇 가지 테스트 콘텐츠로 새 페이지를 추가해 보겠습니다.

두 개의 페이지를 설정해 보겠습니다. 첫 번째 페이지는 leptos-example/src/subpage.rs에 있으며 동등한 Next.js 페이지를 모방합니다:

```js
use leptos::*;

#[component]
pub fn SubPage() -> impl IntoView {
    // 버튼을 업데이트하는 반응적 값 생성
    let (count, set_count) = create_signal(0);
    let on_click = move |_| set_count.update(|count| *count += 1);

    view! {
        <h1>"SubPage"</h1>
        <p>"인터랙션을 포함한 서브페이지: " {count}</p>
        <button on:click=on_click>"증가"</button>
    }
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

비슷하게, Next.js 페이지와 유사한 leptos-example/src/anotherpage.rs에 설정할 겁니다:

```js
use leptos::*;
use leptos_dom::window;

#[component]
pub fn AnotherPage() -> impl IntoView {
    let (window_height, set_window_height) = create_signal::<Option<f64>>(None);
    let window_text = move || {
        window_height()
            .map(|w| format!(": {}", w))
            .unwrap_or("".to_owned())
    };

    // 브라우저 API를 호출하기 전에 window 객체를 사용할 수 있도록 보장합니다.
    create_effect(move |_| {
        set_window_height(window().inner_height().ok().map(|w| w.as_f64().unwrap()));
    });

    view! {
        <h1>"Another Page"</h1>
        <p>"This is a another page calling browser APIs" {window_text}</p>
    }
}
```

또한 프로젝트에서 이러한 페이지에 액세스할 수 있도록 leptos-example/src/lib.rs를 업데이트해야 합니다:

```js
pub mod app;
pub mod error_template;
#[cfg(feature = "ssr")]
pub mod fileserv;
pub mod subpage; // 이 줄 추가
pub mod anotherpage; // 이 줄 추가

// ...파일의 나머지 부분은 그대로 유지됩니다
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

그리고 마지막으로, **leptos-example/src/app.rs** 파일을 편집하여 이러한 페이지들을 링크를 가진 Routes에 추가하십시오. 먼저 이러한 페이지를 가져와주세요.

```rust
use crate::error_template::{AppError, ErrorTemplate};
use leptos::*;
use leptos_meta::*;
use leptos_router::*;
use crate::subpage::SubPage; // 이 줄 추가
use crate::anotherpage::AnotherPage; // 이 줄 추가
// ...
```

그리고 App 컴포넌트에서 Routes를 확장하십시오.

```js
                // ...App 컴포넌트에 라우트 추가.
                <Routes>
                    <Route path="" view=HomePage/>
                    <Route path="/sub-page" view=SubPage/> // 이 줄 추가
                    <Route path="/another-page" view=AnotherPage/> // 이 줄 추가
                </Routes>
                // ...
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

마지막으로 HomePage 컴포넌트를 확장하여 다음 링크를 추가해 보겠습니다:

```js
#[component]
fn HomePage() -> impl IntoView {
    // 버튼을 업데이트하기 위한 반응형 값 생성
    let (count, set_count) = create_signal(0);
    let on_click = move |_| set_count.update(|count| *count += 1);

    view! {
        <h1>"Leptos에 오신 것을 환영합니다!"</h1>
        <button on:click=on_click>"클릭해주세요: " {count}</button>
        [서브 페이지로 이동](/sub-page) // 이 줄을 추가합니다
        [다른 페이지로 이동](/another-page) // 이 줄을 추가합니다
    }
}
```

이제 leptos-example 디렉토리에서 cargo leptos watch를 사용하여 개발 서버를 실행하여 모든 것이 제대로 작동하는지 확인할 수 있습니다:

렌더링할 몇 페이지와 상호 작용 및 브라우저 API를 사용 중이므로 이를 어떻게 처리할지 살펴보겠습니다.

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

## SSG 설정하기

Leptos에서 정적 페이지 생성은 조금 문서화가 부족하지만, Greg의 댓글과 0.5 릴리스 노트를 따라가면 여전히 잘 해낼 수 있어요. 정적 사이트 생성이 어디서 발표되었는지에 대한 내용을 볼 수 있어요.

어떠한 변경을 하기 전에, leptos#2667이 병합될 때까지는 우리 PR을 가리키는 leptos 의존성을 변경해야 합니다:

```js
[package]
# ...

[dependencies]
# ...
leptos = { git = "https://github.com/leptos-rs/leptos", rev = "refs/pull/2667/head", features = ["nightly"] }
leptos_axum = { git = "https://github.com/leptos-rs/leptos", rev = "refs/pull/2667/head", optional = true }
leptos_meta = { git = "https://github.com/leptos-rs/leptos", rev = "refs/pull/2667/head", features = ["nightly"] }
leptos_router = { git = "https://github.com/leptos-rs/leptos", rev = "refs/pull/2667/head", features = ["nightly"] }
# ...나머지 부분은 그대로 유지됩니다
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

이제 변경 사항을 실제로 적용할 준비가 되었습니다. 첫 번째로 할 일은 Routes를 StaticRoutes로 변경하는 것입니다. leptos-example/src/app.rs 파일을 편집하여 Routes를 다음과 같이 바꿔주세요:

```js
                // ...App 컴포넌트 내의 라우트를 변경합니다.
                <Routes>
                    <StaticRoute
                        mode=StaticMode::Upfront
                        path=""
                        view=HomePage
                        static_params=move || Box::pin(async move { StaticParamsMap::default() })
                    /> // 이 부분이 중요합니다.
                    <StaticRoute
                        mode=StaticMode::Upfront
                        path="/sub-page/"
                        trailing_slash=TrailingSlash::Exact
                        view=SubPage
                        static_params=move || Box::pin(async move { StaticParamsMap::default() })
                    /> // 이 부분이 중요합니다.
                    <StaticRoute
                        mode=StaticMode::Upfront
                        path="/another-page/"
                        trailing_slash=TrailingSlash::Exact
                        view=AnotherPage
                        static_params=move || Box::pin(async move { StaticParamsMap::default() })
                    /> // 이 부분이 중요합니다.
                </Routes>
                // ...
```

trailing_slash=TrailingSlash::Exact 및 라우트의 /는 Leptos가 파일을 올바른 위치에 원하는 대로 빌드할 수 있도록 중요합니다.

또한, 이러한 파일에 대한 출력을 생성하도록 Leptos에게 알려야 합니다. 이를 위해 leptos-example/src/main.rs에 일부 추가적인 import를 업데이트하여 build_static_routes 함수를 호출할 수 있도록 해야 합니다.

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
#[cfg(feature = "ssr")]
#[tokio::main]
async fn main() {
    // ...update our imports with generate_route_list_with_ssg and build_static_routes
    use leptos_axum::{generate_route_list_with_ssg, build_static_routes, LeptosRoutes};
    // ...replace the existing let routes ... with this
    let (routes, static_data_map) = generate_route_list_with_ssg(App); // This line here
    build_static_routes(&leptos_options, App, &routes, static_data_map).await; // This line here
    // ...the rest of the file remains the same
}
```

이렇게하면 entry 함수가 실행될 때마다 정적 파일이 생성됩니다.

LEPTOS_HASH_FILES=true cargo leptos build --release을 실행하여 생성된 build 자산을 확인할 수 있습니다. 그런 다음 leptos-example 디렉터리에서 LEPTOS_HASH_FILES=true ./target/release/leptos-example을 실행하여 이진 파일을 실행하세요 (수동으로 Ctrl-C로 종료해야 할 수 있습니다).

```js
$ LEPTOS_HASH_FILES=true cargo leptos build --release
$ LEPTOS_HASH_FILES=true ./target/release/leptos-example
building static route: /sub-page
listening on <http://127.0.0.1:3000>
building static route: /another-page
building static route:
# .. 라우트를 빌드하는 것을 본 후 프로세스를 종료하세요
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

저희가 생성한 Next.js의 결과물을 target/site/ 폴더에서 살펴보겠습니다:

```js
$ tree target/site
target/site
├── another-page
│   └── index.html
├── favicon.ico
├── index.html
├── pkg
│   ├── leptos-example.8RfUbHBiJMoruMHObx2F7Q.css
│   ├── leptos-example.XcFzle8Cx3F6iOeNDhvIAw.js
│   └── leptos-example.p9VEDheNIWNS98tonwfhvw.wasm
└── sub-page
    └── index.html
```

## JavaScript 비활성화 시 테스트

JavaScript를 비활성화하여 Chrome에서 사이트를 테스트할 때는 Chrome DevTools를 열고 CMD + Shift + P를 눌러 "Disable JavaScript"라고 입력합니다.

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

<img src="/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_4.png" />

저희는 그런 다음에 cd leptos-example/target/site && bunx simplehttpserver를 통해 정적 파일을 제공할 수 있습니다. 자바스크립트로 실행하거나 자바스크립트 비활성화 상태로 실행하면 비교를 할 수 있습니다.

먼저, 시작 페이지입니다. 모든 링크가 작동하고 다른 변경 사항이 없는 상태로 정확히 똑같이 보이며 대화형 요소가 없기 때문에 기능적으로 동일합니다.

저희 하위 페이지는 정확히 같이 보일 것이지만, 자바스크립트가 비활성화 상태일 때 증분 버튼과 상호 작용을 시도하면 당연히 작동하지 않을 것입니다.

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

우리는 또 하나의 차이를 발견했어요. 특히, JavaScript가 비활성화되면 브라우저 높이 값을 가져오기 위해 create_effect를 호출하지 않을 거예요. 여기서는 놀라울 게 없네요:

Next.js와 매우 유사한 결과!

# Dioxus SSG

Rust 기반 프레임워크인 Dioxus를 살펴보며 SSG가 지원되고 어떻게 작동하는지 알아봐요.

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

Dioxus CLI를 설정하는 방법부터 시작해보겠습니다:

```js
$ cargo binstall dioxus-cli # 또는: cargo install dioxus-cli
```

그런 다음 dx CLI를 사용하여 새 프로젝트를 만들 수 있습니다. 이 프로젝트는 서버에서 HTML 텍스트를 렌더링하고 클라이언트에서 해당 텍스트를 활성화하는 전체 스택 프로젝트로 설정될 것입니다[⁴]:

```js
$ dx new
✔ 🤷   펼쳐질 하위 템플릿을 선택하세요: · Fullstack
  🤷   프로젝트 이름: dioxus-example
✔ 🤷   응용 프로그램이 Dioxus 라우터를 사용해야 하나요? · true
✔ 🤷   어떻게 CSS를 생성하고 싶나요? · Tailwind
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

만약 TailwindCSS를 선택했다면 기본 스타일이 없을 거에요. dioxus-example/input.css를 개선해보죠. 여기서 Tailwind 스타일이 선택될 거에요. 아래와 같이 보일 거에요:

```js
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-black text-white text-center justify-center mt-16;
  a {
    @apply underline mx-4;
  }
  button {
    @apply bg-neutral-700 p-2 m-4;
  }
}
```

그리고 dioxus-example 디렉터리에서 bunx tailwindcss -i ./input.css -o ./assets/tailwind.css를 실행해요. 이렇게 하면 스타일이 dioxus-example/assets/tailwind.css로 컴파일될 거에요.

만약 TailwindCSS를 선택하지 않았다면 사용했을 대체 dioxus-example/assets/main.css 파일을 안전하게 삭제할 수 있어요.

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

마침내, 나중에 필요한 웹-sys 패키지를 dioxus-example/Cargo.toml의 종속성에 추가할 것입니다:

```js
[package]
# ...

[dependencies]
# 웹 브라우저 API와 상호 작용하기 위한 웹-sys 크레이트 추가
web-sys = { version = "0.3", features = ["Window"] }
# ... 파일의 나머지 부분은 그대로 유지
```

이제 dioxus-example로 이동하여 dx serve --platform fullstack을 실행하면 멋진 시작 페이지가 표시됩니다:

<img src="/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_5.png" />

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

[⁴]: https://dioxuslabs.com/learn/0.5/getting_started#create-a-new-project

## 기본 Dioxus 프로젝트 확장하기

한 번 더, 실제로 흥미로운 것이 생성되는 것을 보려면, 하나의 시작 페이지만 있는 것보다 더 많은 페이지가 필요합니다[⁵]. 몇 가지 테스트 콘텐츠가 포함된 새로운 페이지를 추가해 봅시다.

두 페이지를 설정할 것이며, 첫 번째 페이지는 dioxus-example/src/subpage.rs에 설정됩니다. 이는 해당 Next.js 페이지와 유사합니다:

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

```rust
use dioxus::prelude::*;

#[component]
pub fn SubPage() -> Element {
    // 버튼을 업데이트하는 반응 값 생성
    let mut count = use_signal(|| 0);

    rsx! {
        h1 { "SubPage" }
        p { "이곳은 상호 작용이 가능한 하위 페이지입니다: {count}" }
        button { onclick: move |_| count += 1, "증가" }
    }
}
```

동일한 방법으로, 다음과 같은 Next.js 페이지와 유사한 dioxus-example/src/anotherpage.rs를 설정합니다:

```rust
use dioxus::prelude::*;
use web_sys::window;

#[component]
pub fn AnotherPage() -> Element {
    let mut window_height = use_signal::<Option<f64>>(|| None);
    let window_text = use_memo(move || {
        window_height()
            .map(|w| format!(": {}", w))
            .unwrap_or("".to_owned())
    });

    // 브라우저 API를 호출하기 전에 윈도우 객체를 사용할 수 있도록합니다.
    use_effect(move || {
        let window = window();
        if let Some(w) = window {
            window_height.set(w.inner_height().ok().map(|w| w.as_f64().unwrap()));
        }
    });
    rsx! {
        h1 { "다른 페이지" }
        p { "이곳은 브라우저 API를 호출하는 다른 페이지 입니다{window_text}" }
    }
}
```

이제 dioxus-example/src/main.rs에서 이러한 페이지를 App에 통합 할 수 있습니다. 먼저, 새로운 페이지를 포함하도록 Route enum을 변경하고 기본 블로그 페이지를 삭제하겠습니다:

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
// ...
#[파생(Clone, Routable, Debug, PartialEq, serde::Serialize, serde::Deserialize)]
enum Route {
    #[route("/")]
    홈 {},
    #[route("/sub-page")] // 이 부분이 여기에 있습니다
    서브페이지 {}, // 이 부분이 여기에 있습니다
    #[route("/another-page")] // 이 부분이 여기에 있습니다
    다른페이지 {}, // 이 부분이 여기에 있습니다
}
// ...
```

다음으로 Home 구성 요소를 업데이트하여 페이지로 이동할 수 있도록 링크를 추가하고, 블로그 페이지로 이동하는 링크와 Blog 구성 요소를 제거합니다:

```js
#[component]
fn Home() -> Element {
    let mut count = use_signal(|| 0);
    let mut text = use_signal(|| String::from("..."));

    rsx! {
        Link { to: Route::SubPage {}, "서브 페이지" } // 이 부분이 여기에 있습니다
        Link { to: Route::AnotherPage {}, "다른 페이지" } // 이 부분이 여기에 있습니다
        div {
            h1 { "하이파이브 카운터: {count}" }
            button { onclick: move |_| count += 1, "위로!" }
            button { onclick: move |_| count -= 1, "아래로!" }
            button {
                onclick: move |_| async move {
                    if let Ok(data) = get_server_data().await {
                        tracing::info!("클라이언트가 수신함: {}", data);
                        text.set(data.clone());
                        post_server_data(data).await.unwrap();
                    }
                },
                "서버 데이터 가져오기"
            }
            p { "서버 데이터: {text}"}
        }
    }
}
```

우리는 dioxus-example 디렉토리에서 `dx serve --platform fullstack` 명령을 사용하여 개발 서버를 실행하여 모든 것이 제대로 작동하는지 확인할 수 있습니다:

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

이제 몇 개의 페이지를 렌더링할 예정이니, 상호 작용과 브라우저 API를 사용해서 어떻게 할 지 살펴보겠습니다.

[⁵]: Dioxus는 기본적으로 하위 경로를 추가하지만, 비교를 동등하게 하기 위해 무시할 것입니다.

## SSG 구성

새로운 Dioxus 릴리스가 나올 때까지, 최신 기능을 얻으려면 git에서 CLI를 설치해야 합니다:

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
cargo install --git <https://github.com/DioxusLabs/dioxus> dioxus-cli
```

우리는 또한 의존성을 업데이트하여 정적 사이트 생성이 포함된 지정 커밋 245003a5d430ab8e368094cd32208178183fc24e에서 dioxus를 가리키도록 할 것입니다. 이와 함께 fullstack 기능을 static-generation 기능으로 대체할 것입니다:

```js
[package]
# ...

[dependencies]
# 지정 커밋 245003a5d430ab8e368094cd32208178183fc24e를 사용하도록 dixous를 업데이트합니다
dioxus = { git = "<https://github.com/DioxusLabs/dioxus>", rev = "245003a5d430ab8e368094cd32208178183fc24e", features = [
  "static-generation",
  "router",
] }
# ...파일의 나머지 부분은 동일함
```

이제 기본 main 함수를 매우 간단한 것으로 교체하고 default post_server_data 및 get_server_data 함수를 삭제할 수 있습니다. 이제 dioxus-example/src/main.rs 파일은 다음과 같아야합니다:

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

```rust
#![allow(non_snake_case)]
use dioxus::prelude::*;

mod subpage;
use subpage::SubPage;
mod anotherpage;
use anotherpage::AnotherPage;

#[derive(Clone, Routable, Debug, PartialEq, serde::Serialize, serde::Deserialize)]
enum Route {
    #[route("/")]
    Home {},
    #[route("/sub-page")]
    SubPage {},
    #[route("/another-page")]
    AnotherPage {},
}

// 모든 경로를 생성하고 정적 경로로 출력합니다.
fn main() {
    launch(App);
}

fn App() -> Element {
    rsx! {
        Router::<Route> {}
    }
}

#[component]
fn Home() -> Element {
    let mut count = use_signal(|| 0);
    rsx! {
        Link { to: Route::SubPage {}, "Sub page" }
        Link { to: Route::AnotherPage {}, "Another page" }
        div {
            h1 { "High-Five counter: {count}" }
            button { onclick: move |_| count += 1, "Up high!" }
            button { onclick: move |_| count -= 1, "Down low!" }
        }
    }
}
```

Dioxus는 활성화된 특징 플래그에 따라 호출할 launch 함수를 알고 있습니다. 우리 경우에는 정적 생성입니다.

마지막으로, 생성된 tailwind.css 파일의 기본 경로를 조정하여 하위 경로가 아닌 루트에서 정확히로 로드되도록 해야합니다. dioxus-example/Dioxus.toml에서 style 설정을 조정하세요:

```rust
# ...
[web.resource]
style = ["/tailwind.css"]
# ...파일의 나머지 부분은 그대로 유지됩니다
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

이제 dx build --platform fullstack --release를 실행하여 생성된 빌드 자산을 검사할 수 있습니다. 그런 다음 dioxus-example 디렉토리에서 바이너리를 실행해보세요:

```js
$ dx build --platform fullstack --release && ./dist/dioxus-example
```

이제 Next.js가 static/ 폴더에 생성한 것을 확인해봅시다 (dist/ 폴더가 아니라 static/ 폴더입니다):

```js
$ tree static
static
├── __assets_head.html
├── another-page
│   └── index.html
├── assets
│   └── dioxus
│       ├── dioxus-example.js
│       ├── dioxus-example_bg.wasm
│       └── snippets
│           ├── dioxus-interpreter-js-7c1300c6684e1811
│           │   ├── inline0.js
│           │   └── src
│           │       └── js
│           │           └── common.js
│           ├── dioxus-interpreter-js-9ac3b5e174d5b843
│           │   ├── inline0.js
│           │   └── src
│           │       └── js
│           │           ├── common.js
│           │           └── eval.js
│           ├── dioxus-web-84af743b887ebc54
│           │   ├── inline0.js
│           │   ├── inline1.js
│           │   └── src
│           │       └── eval.js
│           └── dioxus-web-90b865b1369c74f4
│               ├── inline0.js
│               └── inline1.js
├── dioxus-example
├── favicon.ico
├── header.svg
├── index.html
├── sub-page
│   └── index.html
└── tailwind.css
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

## 자바스크립트를 비활성화하여 테스트 중

지난과 마찬가지로, Chrome에서 사이트를 테스트하기 위해 Chrome DevTools을 열고 CMD + Shift + P를 눌러 "자바스크립트 비활성화"라고 입력합니다.

![이미지](/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_6.png)

그런 다음 cd dioxus-example/static으로 이동하여 bunx simplehttpserver를 통해 정적 파일을 제공할 수 있습니다. 자바스크립트 실행 여부에 따른 비교를 해보겠습니다.

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

먼저, 다음은 우리의 시작 페이지입니다. 모든 링크가 작동하고 다른 기능에 변경사항이 없어야 합니다. 인터랙티브한 기능이 없었기 때문에 완전히 동일하게 보여야 합니다.

우리의 하위 페이지는 완전히 동일하게 보일 것이지만 JavaScript가 비활성화된 상태에서 Increment 버튼과 상호작용하려고 하면 당연히 작동하지 않을 것입니다.

우리는 다른 요소에서 작은 차이를 발견했습니다. 특히 JavaScript가 비활성화된 상태에서 use_effect를 호출하여 브라우저 높이를 얻지 않을 것입니다. 여기서 놀라울 게 없습니다.

# 결론

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

Next.js는 확실히 가장 원활한 경로와 SSG에 대한 가장 좋은 지원을 제공했지만, Rust 기반의 프레임워크는 Rust 생태계에 남아있는 것에 헌신한다면 유효한 방법을 보여줍니다.

개인적으로 Leptos를 설정하고 사용하기 더 쉬웠지만, 두 프레임워크 모두 매우 빠르게 발전 중이므로 조금은 개인적인 선호도에 따라 다를 수 있습니다.

게시물 초반에 언급한 대로, 최종 결과물은 다음과 같았습니다:

![이미지](/assets/img/2024-07-30-StaticSPAsExplorationofLeptosDioxusandNextjs_7.png)

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

- : WASM에서 청킹 지원이 대체적으로 떨어지지만, 최근에는 그 방향으로 나아가고 있습니다 (wasm-bindgen#3939 참조)

\*\* : Dioxus 지원은 현재 변화의 상태에 있으며, 정적 생성을 지원하는 방법이 계속 개선되고 있어서 현재 문서와 예제가 손상되었습니다 (dioxus#2587로 추적 중)
