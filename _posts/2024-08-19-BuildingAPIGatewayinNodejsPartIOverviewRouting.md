---
title: "Nodejs로 API 게이트웨이 구축하기 제 1부 - 개요, 라우팅"
description: ""
coverImage: "/assets/img/2024-08-19-BuildingAPIGatewayinNodejsPartIOverviewRouting_0.png"
date: 2024-08-19 02:31
ogImage: 
  url: /assets/img/2024-08-19-BuildingAPIGatewayinNodejsPartIOverviewRouting_0.png
tag: Tech
originalTitle: "Building API Gateway in Nodejs Part I  Overview , Routing"
link: "https://medium.com/@dmytro.misik/building-api-gateway-in-node-js-part-i-overview-routing-d684c8963938"
isUpdated: false
---


## API Gateway 아키텍쳐 기본 개념 이해하기

![이미지](/assets/img/2024-08-19-BuildingAPIGatewayinNodejsPartIOverviewRouting_0.png)

API Gateway는 현대적인 마이크로서비스 아키텍처의 중요한 부분입니다. 이는 모든 클라이언트 요청의 진입점 역할을 하며, 이들을 관리, 유효성 검사, 방지 및 적절한 서비스로 라우팅하는 역할을 합니다. 이 시리즈에서는 API Gateway의 중요한 부분에 초점을 맞추어 Node.js로 구현하는 방법을 살펴볼 것입니다. 이번 파트에서는 API Gateway의 개요를 제공하고, 이점과 단점을 논의하며, 나만의 API Gateway 솔루션에서 라우팅 구성요소를 구현하는 데 초점을 맞출 것입니다.

# 개요

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

API Gateway는 모든 클라이언트 요청의 진입점으로 작동하는 서버입니다. 다음과 같은 역할을 수행할 수 있습니다:

- 라우팅: 요청 경로, 메소드, 헤더, 그리고 기타 매개변수에 기반하여 들어오는 클라이언트 요청을 올바른 마이크로서비스로 라우팅합니다.
- 보안: 인증 및 권한 부여와 같은 특정 보안 정책을 시행할 수 있어 허가된 클라이언트만 특정 서비스나 엔드포인트에 액세스할 수 있도록 보장합니다.
- 속도 제한: 클라이언트가 주어진 기간에 할 수 있는 요청 수를 제한하여 마이크로서비스가 너무 많은 요청으로 과부하되는 것을 방지합니다.
- 캐싱: 마이크로서비스로부터의 응답을 캐시하여 반복 요청을 처리할 필요가 줄어들게 합니다.
- 모니터링 및 로깅: 만들어진 API 호출에 대한 상세한 모니터링 및 로깅을 제공하여 사용 패턴, 성능 및 마이크로서비스의 건강 상태에 대한 귀중한 통찰력을 제공합니다.

API Gateway를 사용하는 몇 가지 이점을 소개하자면:

- 간소화된 클라이언트 통신: 여러 서비스와 통신하는 대신 클라이언트는 API Gateway와만 통신하므로 통합 복잡성이 줄어듭니다.
- 중앙화된 보안: 인증 및 권한이 하나의 곳에 구현되므로 새로운 정책을 관리하기가 더 쉬워집니다.
- 서비스 추상화: API Gateway는 백그라운드 서비스의 복잡성을 숨기며 개별 서비스의 수정 및 업그레이드를 더 쉽게 만듭니다.
- 프로토콜에 중립적: 백그라운드 서비스는 다양한 프로토콜(예: GraphQL, gRPC)을 사용하여 통신할 수 있지만 API Gateway는 통합 프로세스를 간소화하는 통일된 HTTP API를 제공합니다.

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

하지만 API 게이트웨이에는 몇 가지 단점이 있습니다:

- 단일 장애 지점: API 게이트웨이가 실패하면 전체 API 서비스가 중단될 수 있습니다.
- 지연 시간 증가: 요청-응답 경로에 추가 계층을 추가하면 지연 시간이 발생할 수 있습니다.
- 개발 부담: 사용자 정의 라우팅, 보안 및 기타 정책을 개발하는 데 상당한 노력과 전문 지식이 필요할 수 있습니다.

그러므로 고가용성, 확장성 및 API 게이트웨이의 탄성을 보장하기 위해 특별한 아키텍처적 방법을 적용해야 합니다. 이는 여러분이 노출하는 API의 중심 구성 요소로써 중요합니다.

자체 API 게이트웨이를 개발하는 개발 부담을 처리할 준비가 되지 않았다면 다음 제품 중 하나를 사용하는 것을 고려해 볼 수 있습니다:

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

- AWS API Gateway: 사용자가 어떤 규모에서도 API Gateway를 생성, 유지 및 게시할 수 있는 관리형 아마존 웹 서비스;
- Kong: 균형 조정, 모니터링, 로깅 및 인증을 제공하는 오픈 소스 API Gateway 및 서비스 관리 도구;
- Nginx: API Gateway로 사용될 수 있는 고성능 오픈 소스 웹 서버;
- Traefik: API Gateway로 사용될 수도 있는 HTTP 역방향 프록시 및 로드 밸런서.

API Gateway가 어떻게 작동하는지 보여주기 위해 다양한 기능을 보여주는 간단한 구현을 만들 것입니다. 먼저 API Gateway의 주요 책임에 집중하여 시작하겠습니다: 라우팅.

# 라우팅

API Gateway로 들어오는 트래픽은 다양한 마이크로서비스로 라우팅되어야 하며, 이것이 API Gateway의 주요 책임입니다. 들어오는 트래픽을 올바르게 라우팅하는 방법을 파악하려면 API Gateway에 구성이 필요합니다. API Gateway를 구성하는 두 가지 옵션이 있습니다:

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

- 정적 구성: JSON 또는 YAML과 같은 형식의 파일로 구성을 제공합니다.
- 동적 구성: 이 방식은 API 게이트웨이가 데이터베이스나 일부 중앙 집중형 서비스(예: 서비스 검색)에서 실시간으로 구성을 가져올 수 있도록 합니다.

동적 구성은 자주 변경되는 환경에서 유용합니다. 그러나 API 게이트웨이 개발에서 추가 복잡성과 성능 오버헤드가 발생할 수 있습니다. API 게이트웨이는 정기적으로 구성 변경을 가져와 적용해야 하기 때문입니다.

간단하게 하기 위해 API 게이트웨이 라우팅 구성에는 간단한 JSON 파일을 사용할 것입니다.

Webpack의 프록시 구성에 영감을 받아 다음과 같은 API 게이트웨이 구성을 정의했습니다:

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
[
    {
        "name": "user-service",
        "context": ["/users"],
        "target": "http://localhost:3001",
        "pathRewrite": {}
    }
]
```

구성은 라우트 배열입니다. 각 라우트에는 다음과 같은 속성이 있습니다:

- name: 추적 목적으로 사용될 수 있는 라우트의 고유 식별자;
- context: API 게이트웨이가 청취할 특정 라우트의 비어 있지 않은 배열. URL이 /users로 시작하는 요청이 수신되면 API 게이트웨이는 해당 대상 서비스로 라우팅합니다;
- target: 지정된 context와 일치하는 요청을 API 게이트웨이가 전달해야 하는 백엔드 서비스 URL을 정의합니다;
- pathRewrite: 속성을 사용하여 요청 URL 경로를 대상 서비스로 전달하기 전 수정할 수 있습니다. 구성이 빈 객체('')로 정의되면 변환이 필요하지 않음을 의미합니다. 그러나 구성 '‘^/test’: ‘/api’,'는 경로 /test를 /api로 대체한다는 것을 의미합니다.

Node.js로 API 게이트웨이를 구현하기 위해 Express 웹 서버를 사용할 것입니다. Express 웹 서버는 유연하고 간단하며 강력한 웹 서버로서 API 게이트웨이를 구축하는 완벽한 선택입니다.

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

설정에 따라 API Gateway는 구성 파일에 정의된 특정 경로만 수신할 수 있습니다. 그러나 모든 수신 트래픽을 수신하도록 구성할 것입니다. 구성 파일에 정의되지 않은 경로의 경우, Gateway는 설명과 함께 404 Not Found 응답을 반환할 것입니다.

들어오는 요청 핸들러를 개발해보겠습니다:

```js
const fetch = require('node-fetch')

const routes = require('../routes/routes.json')

const errors = {
    ROUTE_NOT_FOUND: 'ROUTE_NOT_FOUND',
}

const defaultTimeout = parseInt(process.env.HTTP_DEFAULT_TIMEOUT) || 5000
const nonBodyMethods = ['GET', 'HEAD']

/**
 * @typedef {import('node-fetch').Response} Response
 * @typedef Result
 * @property {string} error
 * @property {string} message
 * @property {Response} response
 */

/**
 * 요청을 해당 서비스로 프록시하는 메서드
 * @typedef {import('express').Request} Request
 * @param {Request} req
 * @returns {Promise<Result>}
 */
async function handler(req) {
    const { path, method, headers, body, ip } = req

    const route = routes.find((route) =>
        route.context.some((c) => path.startsWith(c))
    )
    if (!route) {
        return {
            error: errors.ROUTE_NOT_FOUND,
            message: '경로를 찾을 수 없음',
        }
    }

    const servicePath = Object.entries(route.pathRewrite).reduce(
        (acc, [key, value]) => acc.replace(new RegExp(key), value),
        path
    )

    const url = `${route.target}${servicePath}`
    const reqHeaders = {
        ...headers,
        'X-Forwarded-For': ip,
        'X-Forwarded-Proto': req.protocol,
        'X-Forwarded-Port': req.socket.localPort,
        'X-Forwarded-Host': req.hostname,
        'X-Forwarded-Path': req.baseUrl,
        'X-Forwarded-Method': method,
        'X-Forwarded-Url': req.originalUrl,

        'X-Forfarded-By': 'api-gateway',
        'X-Forwarded-Name': route.name,
        'X-Request-Id': req.id,
    }

    const reqBody = nonBodyMethods.includes(method) ? undefined : body

    const response = await fetch(url, {
        method,
        headers: reqHeaders,
        body: reqBody,
        follow: 0,
        timeout: route?.timeout || defaultTimeout,
    })

    return {
        response,
    }
}

module.exports = handler
module.exports.errors = errors
```

핸들러의 첫 단계는 들어오는 경로와 일치하는 구성을 찾는 것입니다. 구성에서 경로가 정의되지 않은 경우 오류가 반환됩니다. 그 후에 다른 핸들러(나중에 보여드리겠습니다)가 사용자에게 404 Not Found 응답을 전송할 것입니다.

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

API Gateway는 다음으로 패스 리라이트 규칙을 적용하고 HTTP 요청을 대상 서비스로 전달하기 위해 준비합니다. 이 과정에서 API Gateway는 또한 프록시 컨텍스트를 정의하는 사용자 정의 헤더를 추가합니다. 이러한 헤더에는 클라이언트의 IP 주소와 대상 서비스에서 필요한 기타 메타데이터와 같은 원본 요청에 대한 정보가 포함될 수 있습니다.

대상 서비스가 반환한 HTTP 응답은 Express 요청 핸들러로 반환됩니다:

```js
const service = require('../services/proxy')

/**
 * 프록시 핸들러
 * @typedef {import('express').Request} Request
 * @typedef {import('express').Response} Response
 * @typedef {import('express').NextFunction}
 * @param {Request} req
 * @param {Response} res
 * @returns {void}
 */
module.exports = async (req, res, next) => {
    try {
        const { error, message, response } = await service(req)

        if (error) {
            if (error === service.errors.ROUTE_NOT_FOUND) {
                res.status(404).json({ error, message }).send()
            } else {
                res.status(500).json({ error, message }).send()
            }

            return
        }

        res.status(response.status)
        response.headers.forEach((value, key) => res.setHeader(key, value))

        const content = await response.buffer()
        res.send(content)
    } catch (err) {
        next(err)
    }
}
```

에러가 반환되면 핸들러가 분석하여 적절한 응답을 최종 사용자에게 보내고, 그렇지 않으면 대상 서비스로부터의 응답을 최종 사용자에게 전달합니다.

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

처음에는 다음과 같은 중간 처리기들도 구현했습니다:

- 에러 핸들러: 이 처리기는 요청 처리 중에 발생하는 모든 오류를 로그에 기록합니다.
- 로거: 이 처리기는 들어오는 트래픽에 대한 정보를 로그에 남깁니다.
- 요청 ID: 이 처리기는 각 들어오는 요청에 추적을 위한 고유 식별자를 추가합니다.

API 게이트웨이는 JSON만 처리하는 것으로 제한되지 않습니다. XML, HTML, 일반 텍스트 및 이진 데이터와 같은 여러 데이터 형식을 수락하고 반환할 수도 있습니다. API 게이트웨이가 모든 콘텐츠 유형과 작동하도록 하기 위해 Express의 raw 처리기를 구성했습니다.

게다가, 프록시 핸들러에 유닛 테스트를 적용하여 신뢰성과 정확성을 보장했습니다. 또한 GitHub Actions를 사용한 CI 프로세스를 구현하여 테스트를 자동화했습니다. 이 설정을 통해 코드베이스에 대한 모든 변경 사항이 자동으로 테스트되어 코드 품질을 유지할 수 있습니다.

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

API Gateway를 테스트하기 위해 간단한 사용자 서비스를 구현했습니다:

```js
const express = require('express');

const app = express();

const users = []

app.use(express.json());

app.get('/users/:id', (req, res) => {
    const id = req.params.id;
    const user = users.find(user => user.id === id);
    res.json(user);
});

app.post('/users', (req, res) => {
    const user = req.body;
    users.push(user);
    res.json(user);
})

app.listen(3001, () => {
    console.log('Server is running on port 3001');
})
```

다음 요청을 실행하면 API Gateway를 통해 사용자 서비스에 접속할 수 있습니다:

```js
curl --location 'localhost:3000/users' \
--header 'Content-Type: application/json' \
--data '{
    "id": "42",
    "name": "Dmytro"
}'

curl --location 'localhost:3000/users/42'
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

# 어플리케이션 코드

다음 저장소에서 어플리케이션 코드를 찾을 수 있어요:

좋아하시면 저장소를 스타(star)해 주세요!

# 저를 지원하기

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

만약 제 글에서 기쁨이나 가치를 찾았고 감사의 표시를 하고 싶다면, 이제 'Buy Me A Coffee'로 기부할 수 있습니다! 여러분의 지원은 더 많은 이야기, 통찰력, 그리고 여러분과 공감하는 콘텐츠를 계속 공유할 수 있도록 돕는 데 큰 도움이 될 것입니다.

# 결론

API Gateway는 모던 마이크로서비스 개발에서 중요한 역할을 합니다. 모든 클라이언트의 들어오는 트래픽을 단일 진입점으로 작용하여 적절한 서비스로 라우팅하고 보안 및 비율 제한과 같은 여러 가지 중요한 측면들을 관리합니다.

이 글에서는 Node.js와 Express를 사용하여 API Gateway를 개발하기 시작했습니다. 미들웨어와 적절한 구성을 활용하여, 클라이언트가 백엔드 서비스와 상호작용을 단순화하는 유연하고 확장 가능한 게이트웨이를 만들었습니다. 또한 단위 테스트와 CI 프로세스를 구현함으로써 API Gateway가 신뢰할 수 있고 높은 코드 품질을 유지할 수 있도록 보장했습니다.

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

향후 게시물에서는 보안, 모니터링, 속도 제한 등과 같은 API 게이트웨이의 다른 측면을 탐구할 예정입니다. 노드.js를 활용한 API 게이트웨이 개발을 숙달하는 데 도움이 되는 통찰력과 실용적인 예제를 더 알아보세요.