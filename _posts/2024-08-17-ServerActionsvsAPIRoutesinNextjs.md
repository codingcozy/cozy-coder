---
title: "Nextjs에서 Server Actions와 API Routes 비교"
description: ""
coverImage: "/assets/img/2024-08-17-ServerActionsvsAPIRoutesinNextjs_0.png"
date: 2024-08-17 00:49
ogImage: 
  url: /assets/img/2024-08-17-ServerActionsvsAPIRoutesinNextjs_0.png
tag: Tech
originalTitle: "Server Actions vs API Routes in Nextjs"
link: "https://medium.com/gitconnected/server-actions-vs-api-routes-in-next-js-b6ac7247a86c"
isUpdated: false
---


<img src="/assets/img/2024-08-17-ServerActionsvsAPIRoutesinNextjs_0.png" />

Next.js에서 Server Actions 및 API Routes는 데이터를 보내거나 변이를 실행하여 웹 서버와 통신하는 데 사용되는 방법입니다.

오늘은 Server Actions와 API Routes 간의 주요 차이를 표 형식으로 강조하고, 각각에 대한 예제와 설명을 제공할 것입니다.

그럼 이제... 바로 시작해 봅시다!

<div class="content-ad"></div>

# API 라우트

API 라우트는 Next.js에서 데이터를 가져오고 보내는 전통적인 방법이며, Next.js 앱 라우터가 출시되기 전부터 사용되어 왔습니다.

다음은 MongoDB에서 데이터를 가져와 프론트엔드로 반환하는 기본적인 API 라우트의 예시입니다:

app/api/items/route.ts

<div class="content-ad"></div>

```js
export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  })
  const data = await res.json()

  return Response.json({ data })
}
```

중요 사항:

- API 경로의 네이밍 규칙은 HTTP 메소드를 대문자로 사용하는 것입니다. 예를 들어 GET, POST.
- 이 API에 액세스하려면 example.com/api/items로 GET 요청을 보내야 합니다. 이를 통해 GET 핸들러의 코드가 실행됩니다.
- API 경로는 페이지 파일과 충돌할 수 있습니다. 예를 들어 app/api/items 폴더 안에 page.tsx 파일을 정의하면 동일한 디렉토리의 route.ts 파일과 충돌할 수 있습니다.

또 다른 중요한 점은 웹훅이 API 경로와 아주 잘 작동한다는 것입니다.

<div class="content-ad"></div>

웹훅은 특정 이벤트가 발생할 때 다른 앱으로 실시간 데이터를 전송할 수 있는 특별한 API 루트입니다 (스트라이프 웹훅과 같은 것을 생각해보세요).

여기 실제 Stripe 웹훅의 예시가 있습니다. 이 웹훅은 고객 구매 후 트랜잭션 데이터를 저장합니다:

```js
import { headers } from "next/headers"
import { db } from "@/db"
import { transactions } from "@/db/schema"
import { env } from "@/env.mjs"
import Stripe from "stripe"
import { stripe } from "@/lib/stripe"

export async function POST(req: Request) {
  const body = await req.text()
  const signature = headers().get("Stripe-Signature") as string

  let event: Stripe.Event
  try {
    event = stripe.webhooks.constructEvent(
      body,
      signature,
      process.env.NODE_ENV === "development"
        ? env.STRIPE_WEBHOOK_TEST_SECRET
        : env.STRIPE_WEBHOOK_SECRET
    )
  } catch (error: any) {
    return new Response(`Webhook Error: ${error.message}`, { status: 400 })
  }

  const session = event.data.object as Stripe.Checkout.Session

  if (event.type === "checkout.session.completed") {
    const { data } = await stripe.checkout.sessions.listLineItems(session.id)

    const { name: productName } = await stripe.products.retrieve(
      data[0].price!.product.toString()
    )
    const priceId = data[0].price!.id
    const userId = session.metadata!.userId
    const paymentIntent = session.payment_intent!.toString()

    await db.insert(transactions).values({
      paymentIntent,
      priceId,
      userId,
      productName,
    })
  }
  return new Response(null, { status: 200 })
}
```

API 루트가 웹훅에 사용되는 이유는 노출된 엔드포인트로 설정되어 쉽게 트리거할 수 있기 때문입니다.

<div class="content-ad"></div>

서버 작업에는 미리 정의된 엔드포인트 URL이 없어서 이 방법으로 액세스할 수 없어요.

## 서버 작업

서버 작업은 Next.js 13에서 App 라우터의 출시와 함께 소개되었습니다.

서버 작업의 주요 목표는 데이터 변이를 쉽게 만들고 프론트엔드와 백엔드 개발 간의 갭을 줄이는 것입니다.

<div class="content-ad"></div>

다음은 Resend 마법 링크를 사용하여 사용자를 인증하는 서버 액션의 예시입니다:

```js
"use server" // 이것은 이 파일이 서버 액션을 포함하고 있음을 Next.js에 알려줍니다.

// 시간당 매직 링크 요청을 2회로 제한합니다.
const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(2, "60 m"),
})

interface AuthResend {
  redirectTo: string
  email: string
}

export async function authenticateResend({
  redirectTo,
  email,
}: AuthResend): Promise<ActionResponse> {
  // IP별로 인증 요청 비율 제한하는 코드입니다.
  // 이를 통해 이메일 스팸을 방지할 수 있습니다.
  const ip = headers().get("x-forwarded-for") ?? ""
  const { success } = await ratelimit.limit(ip)

  if (!success) {
    return {
      ok: false,
      code: 429,
      error: "에러, 나중에 다시 시도해주세요",
    }
  }
  // Resend를 사용하여 로그인하면 사용자의 이메일 수신함에서 클릭할 수 있는 마법 링크가 생성됩니다.
  await signIn("resend", {
    email,
    redirect: false,
    callbackUrl: redirectTo === "/login" ? "/dashboard" : redirectTo,
  })

  return {
    ok: true,
    code: 200,
  }
}
```

주요 사항:

- 서버 액션 구문은 간단합니다. 파일 상단에 "use server" 지시어를 추가하고 addData, authenticate 등의 함수를 작성하면 됩니다.
- 서버 액션은 엔드 투 엔드 타입 안전성을 가지고 있습니다. 이는 서버 액션이 보내고 받는 데이터의 형태를 알 수 있어 개발자 경험에 큰 도움이 됩니다.
- 서버 액션은 캐시되지 않습니다. 이는 서버 액션이 하단에 POST HTTP 엔드포인트를 만들기 때문에 Next.js에서 캐시되지 않습니다. 이는 서버 액션이 폼 제출에서 사용될 때 CSRF 공격을 방지하기 위한 조치입니다.

<div class="content-ad"></div>

# 표 비교

이것은 Next.js에서 서버 작업과 API 라우트에 대한 간략한 개요였습니다. 각각의 예시를 살펴보았습니다.

이제 언제 API 라우트 또는 서버 작업이 특정 시나리오에서 더 적합한지 결정해야 할 때 참고할 수 있는 테이블로 주요 차이점들을 요약해 보겠습니다:

![표 비교](/assets/img/2024-08-17-ServerActionsvsAPIRoutesinNextjs_1.png)

<div class="content-ad"></div>

아래는 다크 모드 버전입니다 😉

![다크 모드](/assets/img/2024-08-17-ServerActionsvsAPIRoutesinNextjs_2.png)

# 결론

Next.js에서 서버 액션과 API 라우트에 관한 이 기사를 즐겁게 읽어주셨기를 바랍니다.

<div class="content-ad"></div>

이러한 개념들은 이미 오랫동안 존재해 왔지만, 두 가지 사이의 명확한 상세 비교를 찾을 수 없어서 데이터를 가져오는 두 가지 방법에 대해 설명하고 싶었습니다.

# 참고 자료

- API 라우트
- 서버 액션
- 세그먼트 구성