---
title: "Nextjs 14 앱 라우터에 타입 안정성을 갖춘 GraphQL 구현 방법"
description: ""
coverImage: "/assets/img/2024-08-19-ImplementingGraphQLwithTypeSafetyinNextjs14AppRouter_0.png"
date: 2024-08-19 02:06
ogImage: 
  url: /assets/img/2024-08-19-ImplementingGraphQLwithTypeSafetyinNextjs14AppRouter_0.png
tag: Tech
originalTitle: "Implementing GraphQL with Type Safety in Nextjs 14 App Router"
link: "https://medium.com/@sagardhami2001/implementing-graphql-with-type-safety-in-next-js-14-app-router-0dd0a1259327"
isUpdated: true
updatedAt: 1724033342881
---


최근에 저는 Next.js 프로젝트에서 Strapi의 GraphQL API를 Next.js 14 앱에 통합해 보았어요. Next.js 14 앱 라우터에 GraphQL을 통합하는 데 도움이 될만한 명확한 공식 문서가 없어서 좀 답답했지만, 몇 가지 통합 방법을 연구했어요.

Next.js 13 이후부터 앱 라우터의 복잡성은 최고점에 도달했는데, Next.js는 기본적으로 모든 컴포넌트를 서버 컴포넌트로 선언합니다. 이는 서버 컴포넌트에서 context, useEffect, useRef 또는 useState를 사용할 수 없다는 것을 의미해요. 하지만 Apollo Client는 context의 원리에 의존합니다.

```js
import React from 'react';
import * as ReactDOM from 'react-dom/client';
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';
import App from './App';

const client = new ApolloClient({
  uri: process.env.API,
  cache: new InMemoryCache(),
});

// React 18+에서 지원됩니다
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
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

이거, Next.js 서버 컴포넌트에서는 실제로 불가능해요. 에러가 발생하게 될 거에요. Next.js 13 앱 라우터로 데이터 가져오기가 완전히 바뀌었고, 캐싱 및 다시 유효화는 상당히 혼란스러울 수 있어요. 이러한 도전에 대처하기 위해 다양한 옵션을 탐험하다가 잘 작동하는 하나를 찾았어요.

그러니 시작해볼까요? create-next-app을 사용하여 새로운 Next.js 앱을 만들어봅시다.

```js
npx create-next-app@latest
```

앱을 만든 후에는 다음 패키지들을 설치해야 해요:

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
npm install @apollo/client @apollo/experimental-nextjs-app-support graphql
```

이러한 패키지를 설치한 후에 GraphQL 구성을 만들어야 합니다.

```js
// config/api.ts
import { HttpLink, InMemoryCache, ApolloClient } from "@apollo/client";
import { registerApolloClient } from "@apollo/experimental-nextjs-app-support/rsc";

export const { getClient } = registerApolloClient(() => {
  return new ApolloClient({
    cache: new InMemoryCache(),
    link: new HttpLink({
      uri: process.env.API,
    }),
  });
});
```

@apollo/experimental-nextjs-app-support 패키지는 실험적이며 나중에 제거될 수 있지만 현재 잘 작동하고 있습니다.


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

<img src="/assets/img/2024-08-19-ImplementingGraphQLwithTypeSafetyinNextjs14AppRouter_1.png" />

지금은 Apollo가 제공하는 더미 GraphQL API를 사용해 봅시다.

```js
//.env
API = "https://countries.trevorblades.com/graphql"
```

이제 다음과 같은 방법으로 서버 구성 요소 중에서 getClient를 직접 사용할 수 있습니다.

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
// app/page.tsx

import { getClient } from "@/config/api";
import { gql } from "@apollo/client";


export default async function Home() {
  const { data } = await getClient().query({
    query: CountriesQuery,
    context: {
      fetchOptions: {
        next: { revlidte: 10 },
      },
    },
  });

  return (
    <main className="">
      {data?.countries?.map((country: any, index: number) => {
        return (
          <div key={index} className="border-white border-b-2">
            <ul>
              <li>{country?.name}</li>
              <li>{country?.code}</li>
            </ul>
          </div>
        );
      })}
    </main>
  );
}
```

이 코드의 가장 좋은 점은 Next.js 앱 라우터에서 REST API 데이터 가져오기와 같은 방식으로 캐시를 다시 유효화할 수 있다는 점입니다.

JavaScript 사용자에게는 충분한 정보일 것 같지만 TypeScript 사용자에게 실제 도전이 시작됩니다! 😂 'Type Safe GraphQL'을 알아 차렸다면 대부분의 TypeScript 사용자가 graphql-codegen을 사용하여 유형을 생성한다는 것을 알고 계실 것입니다. GraphQL Code Generator는 TypeScript 유형, 쿼리, 리졸버 및 GraphQL 스키마에서 데이터를 가져오기 위한 사용자 정의 React 훅을 자동화합니다. 이것이 좋은 옵션인 동시에 서버 구성 요소에는 이상적이지 않습니다.

최근 gql.tada에 대해 듣고 연구해 보았습니다. GraphQL에서 유형 안전성을 보장하는 놀라운 도구로 인상적으로 발견했습니다. 현재 버전 1 정도인데, 팀은 계속해서 더 효율적으로 만들고 있습니다.


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

gql.tada를 시작하려면 다음 명령을 사용하여 설치해야 합니다:

```js
npm install gql.tada
```

설치 후에는 TypeScrpt 플러그인을 tsconfig.json에 추가하여 TypeScript 언어 서버와 함께 설정해야 합니다. gql.tada/ts-plugin을 설정하면 TypeScript가 IDE나 편집기에서 파일을 분석할 때 "TypeScript Language Service Plugin"이 시작됩니다. 이는 GraphQL에 대한 편집기 힌트인 진단, 자동완성 및 타입 호버를 제공합니다.

```js
    "plugins": [
      {
        "name": "next"
      },
      {
        "name": "gql.tada/ts-plugin",
        "schema": "https://countries.trevorblades.com/graphql",
        "tadaOutputLocation": "./graphql-env.d.ts"
      }
    ],
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

스키마를 구성하려면 여러 옵션이 있어요. 스키마는 GraphQL SDL 형식의 스키마 정의가 포함된 .graphql 파일이거나 스키마의 내략 쿼리 데이터를 포함한 .json 파일로 제공할 수 있어요. 자세한 내용은 gql.tada 공식 문서를 참조해주세요.

대부분의 개발 시나리오에서는 GraphQL 서버에서 타입을 생성해야 해요. 스키마를 설정한 후에는 tadaOutputLocation을 지정해야 해요. 이 옵션은 TypeScript 타입 시스템 내에서 GraphQL 타입을 추론하는 데 사용하는 typings 파일의 출력 경로를 정의합니다. tadaOutputLocation은 제공하는 파일 경로에 따라 두 가지 형식을 지원해요: .d.ts와 .ts. 성능 향상을 위해 .d.ts를 사용하는 것을 문서에서 제안하고 있으니, 우선은 .d.ts를 사용하도록 하죠.

이제 모든 타입이 포함된 출력 파일을 생성해야 해요. 이를 위해 다음 CLI 명령어를 사용하세요:

```js
npx gql.tada generate output
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

그러나 처음에 .d.ts 파일을 생성할 수 없었기 때문에 다음 명령을 사용하여 장치 전역으로 gql.tada를 설치해야 했습니다.

```js
npm i -g gql.tada
```

이제 다음 명령을 사용하여 .d.ts 파일을 생성하세요.

```js
gql.tada generate output
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

gql.tada를 전역으로 설치한 후 .d.ts 파일을 생성할 수 있었는데, 해당 파일은 다음과 유사한 내용을 가지고 있습니다:

```js
declare const introspection: {
  __schema: { /*...*/ };
};

import * as gqlTada from 'gql.tada';

declare module 'gql.tada' {
  interface setupSchema {
    introspection: typeof introspection;
  }
}
```

설정이 완료되었으므로, VSCode가 기본적으로 워크스페이스의 TypeScript 설치를 로드하지 않고 대신 전역 설치를 사용할 수 있습니다. 이 문제를 해결하기 위해 VSCode의 settings.json 구성에 다음 두 줄을 추가해주세요:

```js
{
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true
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

이제 모두 설정이 완료되었습니다! 페이지.tsx에서 필요한 변경 사항을 살펴봅시다.

@gql/client에서 gql.tada로 쿼리 가져오기를 업데이트해야 합니다. 최종 코드는 다음과 같아야 합니다:

```js
//page.tsx
import { getClient } from "@/config/api";
import { graphql } from "gql.tada";

const CountriesQuery = graphql(`
  query Countries {
    countries {
      name
      capital
      code
      continent {
        code
        name
      }
      currency
    }
  }
`);


export default async function Home() {
  const { data } = await getClient().query({
    query: CountriesQuery,
    context: {
      fetchOptions: {
        next: { revlidte: 10 },
      },
    },
  });

  return (
    <main className="">
      {data?.countries?.map((country: any, index: number) => {
        return (
          <div key={index} className="border-white border-b-2">
            <ul>
              <li>{country?.name}</li>
              <li>{country?.code}</li>
            </ul>
          </div>
        );
      })}
    </main>
  );
}
```

이 변경 사항으로 코드가 GraphQL 응답에서의 타입 안정성을 활용할 수 있게 되었습니다. 이 방법이 문제를 해결하고 개발 경험을 향상시켜 주기를 바랍니다. 작동 상태를 알려주시면 감사하겠습니다!

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

# 참고 자료

https://www.npmjs.com/package/@apollo/experimental-nextjs-app-support