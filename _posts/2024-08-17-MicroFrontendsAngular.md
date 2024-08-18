---
title: "Angular로 마이크로 프론트엔드  구현하는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-MicroFrontendsAngular_0.png"
date: 2024-08-17 00:53
ogImage:
  url: /assets/img/2024-08-17-MicroFrontendsAngular_0.png
tag: Tech
originalTitle: "Micro Frontends Angular"
link: "https://medium.com/stackademic/micro-frontends-angular-cb0ce751dbdd"
isUpdated: true
updatedAt: 1723863754582
---

마이크로 프론트엔드는 대규모 애플리케이션을 작은 독립적인 애플리케이션으로 분해하는 아키텍처 스타일입니다. 각 부분은 개별적으로 개발, 배포 및 유지 관리됩니다. 이 안내서에서는 Angular를 사용하여 마이크로 프론트엔드 아키텍처를 생성하고 호스트 응용 프로그램이 공유 구성 요소를 사용하여 두 원격 애플리케이션을 통합하는 방법을 보여줍니다.

![MicroFrontendsAngular](/assets/img/2024-08-17-MicroFrontendsAngular_0.png)

# 단계별 구현

# 1. 워크스페이스 및 애플리케이션 생성하기

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

- 새 워크스페이스를 생성하세요:

```js
ng new angular-mfe-example --create-application=false
cd angular-mfe-example
```

2. 호스트 애플리케이션을 생성하세요:

```js
ng generate application host --standalone
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

3. 원격 애플리케이션을 생성하세요:

```js
ng generate application remoteapp1 --standalone
ng generate application remoteapp2 --standalone
```

## 2. 모듈 연맹 플러그인 추가

- 호스트 애플리케이션에 모듈 연맹 추가하기:

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
ng add @angular-architects/module-federation --project host --port 4200
```

2. 원격 애플리케이션에 모듈 연맹 추가하기:

```js
ng add @angular-architects/module-federation --project remoteapp1 --port 5001
ng add @angular-architects/module-federation --project remoteapp2 --port 5002
```

# 3. 독립 구성요소 및 공유 구성요소 설정하기

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

- 각 원격 앱을 위해 독립적인 컴포넌트를 생성해보세요:

```js
cd projects/remoteapp1
ng generate component admin --standalone

cd ../remoteapp2
ng generate component user --standalone
```

2. 호스트 애플리케이션에서 공유 컴포넌트를 생성합니다:

```js
cd ../host
ng generate component shared-button --standalone
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

3. 공유 컴포넌트를 내보내기:

프로젝트/host/src/app/shared-button/shared-button.component.ts:

```js
import { Component } from "@angular/core";

@Component({
  selector: "app-shared-button",
  template: `<button>공통 버튼</button>`,
  standalone: true,
})
export class SharedButtonComponent {}
```

# 4. 모듈 연합을 위해 웹팩 구성하기

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

- 원격 애플리케이션에서 스탠드얼론 컴포넌트 노출하기 (webpack.config.js):

원격앱1:

```js
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  output: {
    publicPath: "http://localhost:5001/",
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "remoteapp1",
      filename: "remoteEntry.js",
      exposes: {
        "./Admin": "./src/app/admin/admin.component.ts",
      },
      shared: {
        "@angular/core": { singleton: true },
        "@angular/common": { singleton: true },
        "@angular/router": { singleton: true },
        "./SharedButton": "host@http://localhost:4200/remoteEntry.js",
      },
    }),
  ],
};
```

원격앱2:

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
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  output: {
    publicPath: "http://localhost:5002/",
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "remoteapp2",
      filename: "remoteEntry.js",
      exposes: {
        "./User": "./src/app/user/user.component.ts",
      },
      shared: {
        "@angular/core": { singleton: true },
        "@angular/common": { singleton: true },
        "@angular/router": { singleton: true },
        "./SharedButton": "host@http://localhost:4200/remoteEntry.js",
      },
    }),
  ],
};
```

2. 호스트의 webpack.config.js에서 원격 구성 요소를 로드합니다:

```js
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  output: {
    publicPath: "http://localhost:4200/",
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "host",
      remotes: {
        remoteapp1: "remoteapp1@http://localhost:5001/remoteEntry.js",
        remoteapp2: "remoteapp2@http://localhost:5002/remoteEntry.js",
      },
      shared: {
        "@angular/core": { singleton: true },
        "@angular/common": { singleton: true },
        "@angular/router": { singleton: true },
      },
    }),
  ],
};
```

# 5. 공유 구성 요소 통합하기

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

- 원격 애플리케이션에서 공용 구성 요소 사용하기:

remoteapp1/src/app/admin/admin.component.ts 파일 내용:

```js
import { Component } from "@angular/core";
import { SharedButtonComponent } from "host/SharedButton";

@Component({
  selector: "app-admin",
  template: `<h1>원격 앱 1 관리자</h1>
    <app-shared-button></app-shared-button>`,
  standalone: true,
  imports: [SharedButtonComponent],
})
export class AdminComponent {}
```

remoteapp2/src/app/user/user.component.ts 파일 내용:

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
import { Component } from "@angular/core";
import { SharedButtonComponent } from "host/SharedButton";

@Component({
  selector: "app-user",
  template: `<h1>Remote App 2 User</h1>
    <app-shared-button></app-shared-button>`,
  standalone: true,
  imports: [SharedButtonComponent],
})
export class UserComponent {}
```

2. 호스트 애플리케이션에서 라우트 구성 (app-routing.module.ts):

```js
import { Routes } from "@angular/router";

const routes: Routes = [
  {
    path: "admin",
    loadComponent: () => import("remoteapp1/Admin").then((m) => m.AdminComponent),
  },
  {
    path: "user",
    loadComponent: () => import("remoteapp2/User").then((m) => m.UserComponent),
  },
  { path: "", redirectTo: "admin", pathMatch: "full" },
];
```

3. 호스트 애플리케이션의 decl.d.ts 파일에 모듈 선언을 추가하세요.

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

`projects/host/src/app`로 이동하고 decl.d.ts라는 파일을 만들어주세요. 파일이 없다면요:

```js
declare module 'remoteapp1/Admin';
declare module 'remoteapp2/User';
declare module 'host/SharedButton';
```

# 6. 모든 애플리케이션 실행

- 호스트 애플리케이션 실행하기:

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
cd projects/host
ng serve
```

2. 원격 애플리케이션 실행:

```js
cd ../remoteapp1
ng serve

cd ../remoteapp2
ng serve
```

위 단계를 따르면 Angular를 사용하여 스탠드얼론 컴포넌트와 공유 컴포넌트를 사용하는 작동하는 마이크로 프론트엔드 설정을 갖게됩니다. 이 아키텍처를 통해 애플리케이션의 다른 부분을 독립적으로 개발하고 배포할 수 있어 더 큰 유연성과 확장성을 제공할 수 있습니다.

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

감사합니다!  
LinkedIn: Vishalini Sharma

# Stackademic 🎓

텍스트를 끝까지 읽어주셔서 감사합니다. 떠나시기 전에:

- 작가에게 박수를 보내고 팔로우해주시는 것을 고려해주세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문: In Plain English | CoFeed | Differ
- 더 많은 콘텐츠는 Stackademic.com에서 확인해주세요
