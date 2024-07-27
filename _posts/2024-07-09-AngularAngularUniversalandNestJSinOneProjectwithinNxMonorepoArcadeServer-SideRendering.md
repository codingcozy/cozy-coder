---
title: "Nx 모노레포에서 하나의 프로젝트로 Angular, Angular Universal, NestJS 사용 서버사이드 렌더링 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-AngularAngularUniversalandNestJSinOneProjectwithinNxMonorepoArcadeServer-SideRendering_0.png"
date: 2024-07-09 10:27
ogImage: 
  url: /assets/img/2024-07-09-AngularAngularUniversalandNestJSinOneProjectwithinNxMonorepoArcadeServer-SideRendering_0.png
tag: Tech
originalTitle: "Angular, Angular Universal and NestJS in One Project within Nx Monorepo: Arcade Server-Side Rendering"
link: "https://medium.com/@mohamedali.mnassar/angular-angular-universal-and-nestjs-in-one-project-within-nx-monorepo-arcade-ssr-59d6a7280f86"
---


앞으로 SPAs(Server-side rendering가 일반적이었던 시대에는 php, java 등을 사용해 일반적인 방식으로 작동했습니다. HTML, CSS, JS는 백엔드에서 생성되어 브라우저가 렌더링하고 일부 사용자 상호작용을 개선하는 데 사용되었습니다. 이제 사람들은 그런 설정으로 돌아가려고 하지만 SPA용으로 구축된 도구를 사용하면 효율적이지 않습니다.

이제 NestJS를 더 자세히 살펴보면 JavaEE 방식과 매우 유사한 MVC 섹션이 있다는 것을 알 수 있습니다. 여전히 SSR로 간주됩니다. 현대적인 프레임워크에서 볼 수 있는 SSR과는 다릅니다. 우리는 Angular이 템플릿 엔진이 되고 NestJS가 Java의 역할을 하는 구식 SSR로 돌아갈 수도 있습니다.

이 블로그는 Angular + Angular Universal 및 NestJS를 Nx Monorepo에서 통합하며 이 접근 방식을 시도하는 것을 탐구합니다.

이 블로그에서 사용하는 각 프레임워크 또는 도구에 대한 간단한 소개입니다.

<div class="content-ad"></div>

# Angular

Angular은 구글에서 개발 및 유지보수되는 인기 있는 오픈 소스 웹 애플리케이션 프레임워크입니다. 성능과 유지보수성에 중점을 둔 동적 단일 페이지 애플리케이션(SPA)을 구축하기 위해 설계되었습니다. Angular은 포괄적인 도구와 기능을 제공합니다.

# Angular Universal

Angular Universal은 Angular의 기능을 확장하여 Angular 애플리케이션의 서버 측 렌더링(SSR)을 가능하게 하는 기술입니다. SSR은 초기 HTML을 클라이언트가 아닌 서버에서 렌더링함으로써 웹 애플리케이션의 성능과 SEO를 향상시킵니다. Angular Universal의 주요 장점은:

<div class="content-ad"></div>

- 향상된 성능: 초기 페이지 로드가 빨라지고 사용자가 더 빠른 성능을 경험할 수 있습니다.
- 향상된 SEO: 완전히 렌더링된 HTML 콘텐츠는 검색 엔진 크롤러에 더 쉽게 접근됩니다.
- 더 나은 사용자 경험: 상호 작용 시간을 단축하여 더 부드러운 사용자 경험을 제공합니다.

# NestJS

NestJS는 효율적이고 확장 가능한 서버 측 애플리케이션을 구축하기 위한 혁신적인 Node.js 프레임워크입니다. Angular에 영향을 받은 NestJS는 TypeScript를 활용하며 모듈화 아키텍처를 포용하여 풀 스택 개발에 적합합니다. NestJS의 주요 기능은 다음과 같습니다:

- 모듈화 아키텍처: 더 나은 구성과 확장성을 위한 모듈 구조를 권장합니다.
- 의존성 주입: 의존성 관리를 간소화하고 테스트 가능성을 향상시킵니다.
- 강력한 CLI: 코드 생성 및 관리를 위한 견고한 명령줄 인터페이스를 제공합니다.
- 넓은 호환성: GraphQL, WebSockets 등 다양한 라이브러리 및 프레임워크와 원활하게 작동합니다.

<div class="content-ad"></div>

# Nx Monorepo

Nx는 모노 레포(Monorepo)용 고급 확장 가능한 개발 도구 모음으로, 작업 공간을 효율적으로 관리하는 데 도움을 주는 도구입니다. 모노 레포는 주로 여러 애플리케이션과 라이브러리를 포함한 여러 프로젝트를 보유한 버전 관리 코드 저장소입니다. Nx는 이러한 작업 공간을 효율적으로 관리하기 위한 여러 가지 이점을 제공합니다:

- 통합 개발 환경: 공유 코드베이스로 여러 프로젝트 개발을 용이하게 합니다.
- 코드 공유와 재사용: 코드 재사용을 촉진하고 프로젝트 간 중복을 줄입니다.
- 고급 도구: 코드 생성, 테스트 및 빌드를 위한 강력한 도구를 제공하여 개발자 생산성을 향상시킵니다.
- 일관된 코드 품질: 전체 모노 레포 내에서 일관된 코드 품질과 표준을 보장합니다.

다음 섹션에서는 이러한 기술들을 하나로 통합하여 조화로운 프로젝트로 통합하는 설정 프로세스에 대해 자세히 살펴볼 것입니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-AngularAngularUniversalandNestJSinOneProjectwithinNxMonorepoArcadeServer-SideRendering_0.png)

참고: 이 글은 글쓰기를 개선할 수 있지만, 더 나은 사용성을 위해 개선할 수 있다면 언제든 언급해 주세요. 

# 0. Nx 워크스페이스 생성

이미 동일 저장소에서 프로젝트를 관리하기 위해 Nx를 사용 중이지 않은 경우, 고려해 보시기를 권장합니다. 그러나 이 블로그는 학습 목적으로 작성되었으므로 언급해 두겠습니다.

<div class="content-ad"></div>

Nx를 시작하려면 공식 문서에 제공된 지침을 따르고 마음에 드는 기술 스택을 사용하여 모노레포를 생성할 수 있습니다.

참고: 컴퓨터에 nodejs와 npm이 설치되어 있어야 합니다.

```js
npx create-nx-workspace@latest
```

이 명령어를 사용하여 Nx 워크스페이스를 생성하고, 그 후에 유용하게 활용할 수 있도록 프로세스를 따르면 됩니다.

<div class="content-ad"></div>

# 1. Nx 내부에서 Angular Microfrontend 생성하기

만약 우리가 애플리케이션을 website-nest라고 부르고 싶다면, 다음 명령어를 사용하여 선호하는 스타일링 유형(예: css, scss...), 테스팅 환경(예: cypress) 및 번들러(예: webpack)를 선택한 후에 Nx로 독립 실행형이 아닌 Angular 애플리케이션을 성공적으로 생성할 수 있습니다.

```js
npx nx g @nx/angular:application website-nest-mfe --directory website-nest/mfe --standalone false
```

<div class="content-ad"></div>

주의: 현재의 에세이는 NgModule을 사용하는 것을 원하시는 것으로 가정합니다 (즉, 독립적이지 않은 앱 구성 요소와 함께).

## 2. Angular Universal 통합

```js
npx nx g @nx/angular:setup-ssr --project=website-nest-mfe
```

여기서 우리는 Angular Universal을 추가하여 Angular Microfront에 ssr 기능을 사용하고 있습니다.

<div class="content-ad"></div>

미크로프론트엔드의 프로젝트 이름은 관련된 project.json 안에서 찾을 수 있습니다.

## 3. Angular에 NestJS 추가하기 (동일한 호스트)

이제 Angular에 NestJS를 추가해서 협동할 수 있도록 만드는 것은 수동으로 설정을 해주어야 해서 다소 복잡할 수 있습니다.

먼저, mfe 디렉토리 내부에 새 디렉토리를 생성하고 server라는 이름을 지어주세요.
그런 다음, mfe/server 내부에 main.ts 파일을 다음 코드와 함께 생성해주세요.

<div class="content-ad"></div>


/**
 * 이것은 아직 제품 서버가 아닙니다!
 * 시작하는 데 필요한 최소한의 백엔드만 있는 것입니다.
 */


import { Logger } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';


async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const globalPrefix = '';
  app.setGlobalPrefix(globalPrefix);
  const port = process.env['PORT'] || 3000;
  await app.listen(port);
  Logger.log(
    `🚀 Application is running on: http://localhost:${port}/${globalPrefix}`
  );
}


// 웹팩은 'require'를 '__webpack_require__'로 대체합니다.
// '__non_webpack_require__'는 Node 'require'에 대한 프록시입니다.
// 아래 코드는 번들을 요구하지 않을 때에만 서버가 실행되도록 보장하기 위한 것입니다.
declare const __non_webpack_require__: NodeRequire;
const mainModule = __non_webpack_require__.main;
const moduleFilename = (mainModule && mainModule.filename) || '';
if (moduleFilename === __filename || moduleFilename.includes('iisnode')) {
  bootstrap().catch((err) => console.error(err));
}


이 파일은 NestJS 애플리케이션을 빌드하고 express를 사용하여 Angular의 index.html을 렌더링하는 역할을 할 것입니다.
그러나 이 작업을 수행하려면 서버 디렉토리에 다른 파일을 추가해야 합니다. 그것이 바로 app.module.ts입니다.

mfe/server 내부의 기본 app.module.ts nestjs 파일은 다음과 같아야 합니다:

```js
import { Module } from '@nestjs/common';
import { AngularUniversalModule } from '@nestjs/ng-universal';
import { join } from 'path';
import { AppServerModule } from '../src/main.server';
// import { AppController } from './app.controller';


@Module({
  imports: [
    AngularUniversalModule.forRoot({
      bootstrap: AppServerModule,
      viewsPath: join(process.cwd(), '<dist-browser-folder>'),
    }),
  ],
  // controllers: [AppController],
})
export class AppModule {}
```  

<div class="content-ad"></div>

mfe가 NestJS 코드를 해석할 수 있어야 하므로 tsconfig를 업데이트해야 합니다:
mfe/tsconfig.app.json

```json
{
  ….
  "include": ["src/**/*.d.ts", "server/**/*.ts"],
  "exclude": [
    "jest.config.ts",
    "src/**/*.test.ts",
    "src/**/*.spec.ts",
    "server/**/*.spec.ts"
  ]
}
```

이제 Angular Universal의 mfe/server.ts도 업데이트하여 NestJS를 컴파일하도록 순서를 정해야 합니다:

```javascript
import 'zone.js/node';

import './server/main';

export * from './src/main.server';
```

<div class="content-ad"></div>

다음으로, mfe/src/main.server.ts와 mfe/src/main.ts를 업데이트해야 합니다.
#mfe/src/main.ts

```js
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
document.addEventListener('DOMContentLoaded', () => {
  platformBrowserDynamic()
    .bootstrapModule(AppModule)
    .catch((err) => console.error(err));
});
```

#mfe/src/main.server.ts

```js
export { AppServerModule } from './app/app.server.module';
export { renderModule, renderApplication } from '@angular/platform-server';
```

<div class="content-ad"></div>

# 4. 필요한 종속 항목 설치하기 (Nest, Angular)

```js
npm i @nestjs/websockets @nestjs/microservices @nestjs/ng-universal @nestjs/common @nestjs/core  class-validator kafkajs mqtt nats ioredis amqplib amqp-connection-manager @nestjs/platform-socket.io --legacy-peer-deps 

npm i class-transformer @nestjs/platform-express @grpc/grpc-js @grpc/proto-loader @grpc/grpc-js @grpc/proto-loader --legacy-peer-deps
```

# 5. 코드 빌드 및 배포

우리 블로그를 신중히 따라왔다면 문제없이 적용되어 작동할 것입니다.
애플리케이션을 빌드하기 위해

<div class="content-ad"></div>

```js
npx nx run website-nest-mfe:server
```

애플리케이션을 실행하려면

```js
node dist/website-nest/mfe/server/main.js
```

개발 목적으로는 이 명령어를 사용하세요:

<div class="content-ad"></div>

```js
npx nx run website-nest-mfe:serve-ssr
```

# 6. 개발 및 유지보수하기 (Nest, Angular)

nestjs에서 새로운 컨트롤러, 미들웨어, 모듈을 만들고 (server 디렉토리) Angular에서 새로운 컴포넌트, 서비스, 모델을 만들어주세요 (src 디렉토리)

진행하면 됩니다 :)

<div class="content-ad"></div>

# 주목해 주셔서 감사합니다

https://www.linkedin.com/in/mohamed-ali-mnassar/
https://www.facebook.com/dali.mnassar