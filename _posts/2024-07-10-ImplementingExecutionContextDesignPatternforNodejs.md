---
title: "Nodejs를 위한 실행 컨텍스트 디자인 패턴 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-10-ImplementingExecutionContextDesignPatternforNodejs_0.png"
date: 2024-07-10 01:02
ogImage: 
  url: /assets/img/2024-07-10-ImplementingExecutionContextDesignPatternforNodejs_0.png
tag: Tech
originalTitle: "Implementing Execution Context Design Pattern for Node.js"
link: "https://medium.com/intangles-engineering/implementing-execution-context-design-pattern-for-node-js-dbe757714251"
isUpdated: true
---





작년에 Intangles에서 프로젝트 중 하나에서 흥미로운 도전 과제를 만났었습니다. 저희의 API 서버는 Node.JS로 구축되었고 서버 프레임워크로 Restify를 사용하며 수백 개의 API가 있습니다.

저희의 서버 아키텍처는 다음과 같은 구조를 따릅니다: 라우트 - 미들웨어 - 컨트롤러 - 스토어 - DB. 우리는 API에 접근 제어를 추가하려고 했습니다. 이미 중간에 일부 접근 제어 로직이 있었지만, 사용자를 위해 DB 쿼리를 최적화하기 위해 컨트롤러와 스토어에도 추가하고 싶었습니다. 그러나 이를 달성하기 위해 Context를 통합해야 했습니다.

# Context란 무엇인가요?

<div class="content-ad"></div>

만약 안드로이드 배경이 있다면, 이미 Context라는 개념에 익숙할 수도 있습니다. 안드로이드에서 Context는 전체 애플리케이션에서 접근 가능한 글로벌 개체로, 애플리케이션 수준의 리소스에 접근하는 데 사용됩니다.

우리의 시나리오에서는 전체 요청 수명 주기 동안 이용 가능한 유사한 글로벌 개체를 생성하는 데 목표를 두었습니다. 이 "Context"라는 개체에는 미들웨어에서 의사 결정에 필요한 모든 정보인 사용자 및 계정 정보 등이 포함되어야 합니다.

많은 시스템과 프로그래밍 언어가 안드로이드와 같이 Context 디자인 패턴을 활용합니다. 자바스크립트는 자체 실행 컨텍스트가 있지만, 각 함수 실행 시 변경됩니다. 우리는 전체 실행 수명 주기 동안 접근 가능한 것이 필요했습니다.

# 아마 궁금해할 수도 있어요. req 객체를 사용하지 않는 이유는 무엇일까요?

<div class="content-ad"></div>

req 객체는 중간 단계 사이에서 정보를 전달하는 편리한 방법이죠. 하지만 도전 과제는 이 객체를 모든 함수를 통해 전달해야 한다는 점에 있습니다. 모든 이러한 함수에 새 매개변수를 추가하는 것은 지루하고 실수를 범하기 쉬운 작업일 수 있어요.

게다가, 우리 서버는 사용자 API 요청뿐만 아니라 다른 것도 처리해요. 디지털 트윈 프레임워크는 동일한 API 세트를 통해 데이터를 읽고 쓰기 위해 사용해요. req 객체를 사용하면 HTTP 서버를 비즈니스 로직과 긴밀하게 결합하게 되는데, 이는 피하고자 했던 것이죠. 각 함수를 통해 명시적으로 전달하지 않고도 현재 실행 컨텍스트에 접근할 수 있는 솔루션이 필요했어요. 그렇게 하면 역할을 더 깔끔하게 분리할 수 있어요.

# 왜 전역 객체를 사용하지 않을까요?

전역 객체를 사용하는 것은 간단한 해결책처럼 보일 수 있어요. 그러나 전역 객체는 모든 요청에서 공유되므로 요청별 데이터에는 적합하지 않아요. 각 요청에 고유한 객체가 필요했던 것이죠.

<div class="content-ad"></div>

한 가지 방법은 각 요청마다 UUID를 할당하여 새 객체를 생성하고 UUID를 키로 하는 맵에 컨텍스트 객체를 저장하는 것입니다. 그러나 이 방법은 여전히 각 함수에 UUID를 전달해야 하므로 요청 수명 주기 전체에서 요청별 데이터에 신속하게 액세스할 수 있는 방법을 제공하지 않는 문제를 해결하지 못합니다.

# 현재 요청 객체를 전역 변수로 저장하는 것은 왜 좋지 않을까요?

현재 요청 객체를 전역 변수로 저장하는 것은 간단한 해결책처럼 보일 수 있습니다. 그러나 하나의 서버가 동시에 여러 요청을 처리합니다. 전역 변수로 현재 요청 객체를 저장하면 각 새 요청마다 덮어씌워질 것입니다. 우리는 각각의 요청에 특정한 방식으로 요청 객체를 저장할 수 있는 솔루션이 필요했습니다.

# 왜 "async_hooks"을 사용하지 않는 것일까요?

<div class="content-ad"></div>

async_hooks를 사용하는 것을 고려했었는데, 이 모듈은 Node.js 애플리케이션에서 비동기 리소스를 추적하는 API를 제공합니다. async_hooks를 사용하면 각 요청마다 새로운 컨텍스트 객체를 만들 수 있었습니다. 그러나 async_hooks는 수준이 낮은 API로 복잡하고 사용하기 어렵습니다. 게다가 문서가 상세하지 않아 모든 Node.js 버전에서 지원되지 않는다는 것도 단점이었습니다. 우리는 간단하고 사용자 친화적인 솔루션이 필요했습니다.

여러분은 다른 방법을 고려하고 있을 것이지만, 대부분의 방법은 각 함수에 매개변수를 전달해야 합니다. 우리는 각 함수에 전달하지 않고도 전체 요청 수명 주기 동안 컨텍스트 객체에 액세스하는 방법을 찾았습니다.

핵심 팀 간의 방대한 토론 끝에, 우리는 스스로에게 이 질문을 던졌습니다:

# JavaScript의 모든 함수 호출과 함께 존재하는 한 가지 요소는 무엇인가요?

<div class="content-ad"></div>

드래그롤 해주세요...

스텍 트레이스입니다.

# 자바스크립트에서 스택 트레이스란 무엇인가요?

스택 트레이스는 프로그램 실행 중 특정 시점에 활성 스택 프레임들의 보고서입니다. 오류가 발생했을 때 활성화되었던 함수 호출의 스냅샷을 제공하여 디버깅에 유용한 도구입니다. 스택 트레이스를 조사함으로써 개발자는 오류로 이어진 함수 호출 순서를 추적하여 코드에서 문제를 식별하고 수정할 수 있습니다.

<div class="content-ad"></div>

# 자바스크립트에서 스택 추적 접근하는 방법

자바스크립트에서 스택 추적에 접근하려면 Error 객체를 사용할 수 있습니다.

Error 객체에는 stack라는 속성이 있으며, 이 속성에는 스택 추적이 포함되어 있습니다. 이 속성은 오류가 발생한 시점에서 활성 상태였던 함수 호출의 스냅샷을 제공하여 디버깅에 도움을 줍니다.

이 도구를 통합하거나 기능을 탐색하는 데 관심이 있는 분들을 위해, 더 많은 정보는 해당 GitHub 리포지토리인 stacktrace.js GitHub 페이지에서 확인할 수 있습니다.

<div class="content-ad"></div>

## 스택 트레이스에 컨텍스트를 추가하는 방법

자바스크립트의 스택 트레이스는 여러 개의 스택 프레임으로 구성됩니다. 각 스택 프레임에는 함수 호출과 관련된 정보가 포함되어 있습니다. 함수 이름, 파일 이름 및 줄 번호가 포함됩니다. 우리는 스택에 컨텍스트 정보를 스택 프레임으로 추가할 수 있습니다.

하지만 직접적으로 스택 트레이스를 변경할 수는 없습니다.

예를 들어:

<div class="content-ad"></div>

```js
const error = Error("뭔가 잘못 되었어요");
console.log(error.stack);
//'Error: 뭔가 잘못 되었어요\n at <익명>:1:18'
```

보시는 것처럼, 스택 추적은 문자열로 표시됩니다. 이 문자열을 변경할 수는 있지만, 이 수정은 이 Error 인스턴스에만 적용됩니다.

# 스택 추적에 영구적인 변경을 어떻게 적용하나요?

JavaScript에서의 스택 추적은 JavaScript 런타임 엔진에서 관리되므로 직접 수정하려면 엔진의 코드를 변경해야 합니다. 그러나 함수 이름은 우리가 정의하므로 이러한 이름에 컨텍스트 정보를 포함하여 간접적으로 스택 추적에 컨텍스트를 추가할 수 있습니다.

<div class="content-ad"></div>

# 구현

이 프로바이더는 이 프로바이더 내부에서 호출된 함수 및 이 프로바이더의 모든 하위 함수에 대해 스택에 컨텍스트 정보를 추가하는 프로바이더를 생성할 것입니다. 이를 통해 이러한 함수들의 실행 수명 주기 전반에 걸쳐 컨텍스트 정보를 지속시킬 수 있습니다.

다음은 이를 구현하는 방법입니다:

```js
const crypto = require("crypto");
const globalExecutionContext = {};
const KEY_PREFIX = "$$exec_context$$";
```  

<div class="content-ad"></div>

```js
function provideExecutionContext(handler, contextValue) {
    const uniqueId = "f-" + crypto.randomBytes(16).toString("hex") + "-" + Date.now();
    const that = this;
    const functionName = `Object.${KEY_PREFIX}${uniqueId}`;
    const contextProvider = {
        [functionName]: async function() {
            try {
                const returnValue = await handler.apply(that, arguments);
                // destroy the execution context after the execution is completed
                delete globalExecutionContext[uniqueId];
                return returnValue;
            } catch (err) {
                // destroy the execution context in case of an error
                delete globalExecutionContext[uniqueId];
                throw err;
            }
        },
    };
    globalExecutionContext[uniqueId] = contextValue;
    contextProvider[functionName].name = functionName;
    return contextProvider[functionName];
}
```

함수 이름은 해당 함수 라이프사이클의 식별자로 작동합니다. 이제 이 식별자를 사용하여 이 컨텍스트 값을 액세스할 수 있습니다.

이 함수 이름을 사용하여 스택 트레이스에서 이 컨텍스트 값을 검색할 수 있습니다.

# 사용법

<div class="content-ad"></div>

이 구현을 NPM 모듈로 릴리스했습니다. 소스 코드는 여기에서 찾을 수 있습니다.

다음 코드를 고려해봅시다:

```js
const firstFunction = (param) => {
    const userDetails = {
        name: "John",
        hasAccessTo4thFunction: true
    }
    console.log("first function", param) // first function, 10
    secondFunction(20)
}
const secondFunction = (param) => {
    console.log("second function", param) // second function, 20
    thirdFunction(30)
}
const thirdFunction = (param) => {
    console.log("third function", param, contextValue)
    // Call fourth function if user has access
    // fourthFunction(40);
}
const fourthFunction = (param) => {
    console.log("fourth function", param);
}
```

우리는 네 개의 함수가 있습니다. firstFunction은 진입 함수입니다. thirdFunction에서는 사용자가 액세스할 수 있는 경우에만 fourthFunction을 호출하려고 합니다. 다음 함수들에 사용자 컨텍스트를 제공하는 방법은 다음과 같습니다.

<div class="content-ad"></div>

단계 1: 실행 컨텍스트 패키지 설치하기

다음 명령어를 실행하여 패키지를 설치하세요:

```js
npm i @intangles-lab/execution-context
```

단계 2: `provideExecutionContext`로 함수 감싸기

<div class="content-ad"></div>

우리는 두 번째 함수를 provideExecutionContext로 감쌀 것입니다. 첫 번째 함수를 수정하여 실행 컨텍스트를 제공하도록 변경합시다.

```js
const {
    provideExecutionContext
} = require("@intangles-lab/execution-context");
...
const firstFunction = (param) => {
    const userDetails = {
        name: "John",
        hasAccessTo4thFunction: true
    }
    console.log("first function", param) // first function, 10
    const secondFunctionWithContext = provideExecutionContext(secondFunction, userDetails);
    secondFunctionWithContext(20);
}
```

단계 3: 세 번째 함수에서 컨텍스트에 접근

세 번째 함수를 수정하여 getExecutionContext를 사용하여 컨텍스트를 검색합니다.

<div class="content-ad"></div>

```js
const {
    provideExecutionContext,
    getExecutionContext
} = require("@intangles-lab/execution-context");
...
const thirdFunction = (param) => {
    console.log("third function", param) // third function, 30
    const userDetails = getExecutionContext();
    if (userDetails?.hasAccessTo4thFunction) {
        fourthFunction(40); // this is called
    }
}
...
```

이러한 단계를 따르면 @intangles-lab/execution-context 모듈을 사용하여 응용 프로그램의 다른 함수들 간에 context 정보를 제공하고 액세스할 수 있습니다.

# 라이프사이클 내에서 컨텍스트 변경하는 방법?

함수 라이프사이클을 위한 컨텍스트를 변경하려면 실행 중에 컨텍스트 정보를 업데이트할 수 있습니다. 예를 들어 사용자의 액세스를 취소해야 하는 경우 컨텍스트를 수정할 수 있습니다.


<div class="content-ad"></div>

여기에 보여드릴게요:

```js
const {
    provideExecutionContext,
    getExecutionContext,
    setExecutionContext
} = require("@intangles-lab/execution-context-js");
...
const secondFunction = (param) => {
        console.log("second function", param) // second function, 20
        // 사용자의 접근 제거
        const userDetails = getExecutionContext();
        const userDetailsWOAccess = {
            …
            userDetails,
            hasAccessTo4thFunction: false
        };
        setExecutionContext(userDetailsWOAccess);
        thirdFunction(30); // 이제 네 번째 기능이 호출되지 않습니다.
    }
...
```

getExecutionContext 및 setExecutionContext을 사용하여 함수 실행 라이프사이클 중에 컨텍스트를 동적으로 수정할 수 있습니다.

# 우리의 HTTP 서버에 이것을 어떻게 활용했는지 요약

<div class="content-ad"></div>

라이브러리에서 provideExecutionContext 함수를 사용하여 Restify 서버의 get, post 및 기타 함수를 재정의했습니다. 이를 통해 요청 수명주기 전체에서 req 객체를 context로 제공할 수 있습니다.

다음은 구현입니다:

```js
const {
    provideExecutionContext
} = require("@intangles-lab/execution-context-js");
/**
 * Override the server methods to add the request context
 *
 * @param {import("restify").Server} server
 * @param {string} methodString
 * @returns
 **/
function overrideServerRequestMethod(server, methodString) {
    const methodFunction = server[methodString].bind(server);
    server[methodString] = function(path, requestHandler) {
        return methodFunction(path, (req, res, next) => {
            const contextProvider = provideExecutionContext(requestHandler, req);
            return contextProvider(req, res, next);
        });
    };
}
/**
 * Initialize the request context for all the server methods
 *
 * @param {import("restify").Server} server - restify server
 * @returns {undefined} no return value
 */
exports.initializeRequestContext = function initializeRequestContext(server) {
    overrideServerRequestMethod(server, "get");
    overrideServerRequestMethod(server, "post");
    overrideServerRequestMethod(server, "put");
    overrideServerRequestMethod(server, "del");
};
```

서버 파일인 server.js에서 서버를 시작하기 전에 initializeRequestContext를 호출합니다:

<div class="content-ad"></div>

```js
...
const server = restify.createServer(…options);
initializeRequestContext(server);
server.listen(port, function() {
    log("%s listening at %s", server.name, server.url);
});
...
```

이제 요청 라이프사이클의 거의 모든 곳에서 이 컨텍스트에 접근할 수 있습니다:

```js
// requestController.js
const {
    getExecutionContext
} = require("@intangles-lab/execution-context-js");
```

```js
function requestController() {
    const req = getExecutionContext();
    // req 객체를 사용할 수 있습니다. 매개변수로 넘겨 줄 필요가 없습니다.
    return {
        results: […]
    }
}
```

<div class="content-ad"></div>

위의 단계를 따라하면 req 객체를 매개변수로 전달하지 않고도 요청 수명 주기 동안 맥락 정보를 원할 때 제공하고 액세스할 수 있습니다.

# 주의사항

컨텍스트 값은 동일한 호출 스택 내에서 제공된 함수와 함께 있는 함수에만 사용할 수 있습니다. JavaScript의 특성으로 인해 콜백과 프로미스는 컨텍스트 값을 액세스할 수 없습니다. 그러나 이 접근 방식은 async/await 함수와 잘 작동합니다. 컨텍스트가 보존되도록 하려면 async 함수는 await를 사용하여 호출되어야 하며 이 때 컨텍스트 값을 제공된 함수와 동일한 호출 스택에 유지해야 합니다.

콜백에서 사용하려면 콜백 외부에서 컨텍스트를 가져와 다시 콜백 내에서 설정해야 합니다:

<div class="content-ad"></div>

```js
const {
    getExecutionContext,
    setExecutionContext
} = require("@intangles-lab/execution-context-js");
```

```js
function callbackExample() {
    const req = getExecutionContext();
    setTimeout(() => {
        setExecutionContext(req);
        func();
    }, 1000);
}
```

이렇게 하면 콜백 내에서 컨텍스트 값에 접근할 수 있어요! 이렇게 하면 요청 수명주기의 다른 부분에서도 일관된 컨텍스트를 유지할 수 있어요. 

# 마무리하며


<div class="content-ad"></div>

실행 콘텍스트 모듈을 통합함으로써 우리의 요청 라이프사이클 관리가 크게 향상되었습니다. 이 방식은 콘텍스트별 정보에 간편하게 접근하고 코드의 유지보수성과 가독성을 향상시킵니다. 이 설정을 통해 필수적인 콘텍스트가 언제나 필요한 곳에 제공되어 응용 프로그램이 보다 견고하고 디버깅이 쉽도록 보장합니다.