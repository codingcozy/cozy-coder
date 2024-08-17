---
title: "콘솔에 중요한 데이터를 기록하지 않는 방법 5가지"
description: ""
coverImage: "/assets/img/2024-08-17-TechniquestoAvoidLoggingSensitiveDataonConsole_0.png"
date: 2024-08-17 00:50
ogImage: 
  url: /assets/img/2024-08-17-TechniquestoAvoidLoggingSensitiveDataonConsole_0.png
tag: Tech
originalTitle: "Techniques to Avoid Logging Sensitive Data on Console"
link: "https://medium.com/@cu.16bcs5007/avoid-logging-sensitive-data-in-your-console-4605e3944cf8"
isUpdated: true
updatedAt: 1723863826965
---



![이미지](/assets/img/2024-08-17-TechniquestoAvoidLoggingSensitiveDataonConsole_0.png)

로그는 응용 프로그램 개발의 중요한 측면이지만, 올바르게 처리되지 않으면 민감한 데이터가 노출될 수 있습니다. 이 기사에서는 TypeScript와 React를 사용할 때 콘솔에 민감한 데이터를 로깅하지 않는 여러 기술에 대해 살펴보겠습니다.

# 1. 환경별 로깅 사용

개발 환경과 프로덕션 환경을 구분하여 로깅 동작을 제어하세요. 예를 들어, 환경 변수를 사용하여 개발 중에 자세한 로깅을 활성화하고, 프로덕션에서는 최소화하거나 사용하지 않도록 할 수 있습니다.


<div class="content-ad"></div>

```js
if (process.env.NODE_ENV === 'development') {
    console.log('Debug information');
}
```

# 2. 로그 산화

민감한 정보를 제거하거나 마스킹하여 출력하기 전에 로그를 산화합니다. 이를 통해 로그된 데이터가 비밀번호, 토큰 또는 기타 개인 정보를 노출하지 않도록 합니다.

```js
function sanitizeLog(data: any): any {
    if (typeof data === 'object') {
        const sanitized = { ...data };
        if (sanitized.password) sanitized.password = '****';
        if (sanitized.token) sanitized.token = '****';
        return sanitized;
    }
    return data;
}

console.log(sanitizeLog(userData));
```

<div class="content-ad"></div>

# 3. 사용자 정의 로거 유틸리티

로그 메커니즘을 추상화하고 무엇을 로깅할지 제어하기 위한 사용자 정의 로거 유틸리티를 만듭니다. 이 접근 방식은 로깅 로직을 중앙 집중화하고 일관된 살균화 및 포매팅을 구현하기 쉽게 만듭니다.

```js
class Logger {
    static log(message: string, data: any) {
        if (process.env.NODE_ENV === 'development') {
            console.log(message, sanitizeLog(data));
        }
    }
}

Logger.log('사용자 데이터:', userData);
```

# 4. TypeScript를 사용한 타입 안전성

<div class="content-ad"></div>

타입스크립트를 사용하여 민감한 데이터를 제외한 유형을 정의하고 강제하세요. 이렇게 하면 유형 시스템을 활용하여 민감한 정보가 실수로 로그에 남지 않도록 도와줍니다.

```js
interface User {
    id: number;
    name: string;
    email: string;
}

interface SensitiveUser extends User {
    password: string;
    token: string;
}

function logUser(user: User) {
    console.log(user);
}
const user: SensitiveUser = { id: 1, name: 'John', email: 'john@example.com', password: 'secret', token: 'abc123' };
logUser(user); // 이것은 적절하게 세척되지 않으면 TypeScript 오류를 발생시킵니다.
```

# 5. 서드파티 로깅 라이브러리

Winston 또는 Pino과 같은 서드파티 라이브러리를 활용하여 고급 로깅 기능을 활용하세요. 이러한 라이브러리는 로깅 수준 및 형식에 대한 더 나은 제어를 제공하여 민감한 데이터를 관리하기 쉽게 만들어줍니다.

<div class="content-ad"></div>

```js
import * as winston from 'winston';

const logger = winston.createLogger({
    level: process.env.NODE_ENV === 'development' ? 'debug' : 'info',
    format: winston.format.json(),
    transports: [new winston.transports.Console()],
});

logger.info('사용자 데이터:', sanitizeLog(userData));
```

# 6. 민감한 작업 로깅 피하기

민감한 작업에서 불필요한 세부 정보가 로깅되지 않도록 데이터를 조건부로 로깅합니다. 이 접근 방식은 민감한 프로세스 중 개인 정보를 노출하는 위험을 줄입니다.

```js
if (operation !== 'sensitive') {
    console.log(`작업: ${operation}, 데이터:`, data);
}
```

<div class="content-ad"></div>

# 7. 정적 코드 분석과 린팅

린팅 도구를 사용하여 로깅 규칙을 강제하고 민감한 데이터가 기록되는 것을 방지하세요. ESLint 또는 TSLint를 구성하여 특정 유형의 데이터를 기록하는 것에 대해 경고하면 안전한 로깅 관행을 유지할 수 있습니다.

```js
// .eslintrc.json
{
    "rules": {
        "no-console": ["error", { "allow": ["warn", "error"] }],
        "security/detect-object-injection": "error"
    }
}
```

# 8. 정기적으로 로그를 검토하고 감사하세요.

<div class="content-ad"></div>

정기적으로 로그를 검토하고 감사하여 민감한 데이터가 노출되지 않도록 확인하세요. 자동화된 도구를 사용하여 로그를 스캔하고 분석하여 민감한 정보가 있는지 확인함으로써 추가적인 보안 방어층을 제공할 수 있습니다.

이러한 기술을 따르면 TypeScript 및 React 애플리케이션의 콘솔 로그를 통해 민감한 정보가 노출될 위험을 크게 줄일 수 있습니다. 적절한 로깅 관행은 애플리케이션 데이터의 보안과 개인 정보 보호를 유지하는 데 중요합니다.