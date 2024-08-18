---
title: "Angular 17 HTTP 인터셉터 가이드"
description: ""
coverImage: "/assets/img/2024-07-06-Angular17HTTPInterceptorsguide_0.png"
date: 2024-07-06 11:03
ogImage:
  url: /assets/img/2024-07-06-Angular17HTTPInterceptorsguide_0.png
tag: Tech
originalTitle: "Angular 17: HTTP Interceptors guide"
link: "https://medium.com/@mohsinogen/angular-17-http-interceptors-guide-417e7c8ffada"
isUpdated: true
---

/assets/img/2024-07-06-Angular17HTTPInterceptorsguide_0.png

자신의 Angular 애플리케이션에 HTTP 요청 또는 응답에 대한 작업을 수행해야하는 경우(예 : 인증 헤더 추가), HTTP Interceptors는 정확하고 중앙 집중식 방법을 제공합니다. 이렇게 함으로써 컴포넌트들이 핵심 로직에 집중할 수 있게 합니다. 또한 인터셉터는 응용 프로그램 코드를 혼란스럽게하지 않고 오류 처리, 로깅, 심지어 요청/응답 데이터를 실시간으로 수정하는 것을 간소화할 수 있습니다.

/assets/img/2024-07-06-Angular17HTTPInterceptorsguide_1.png

# 왜 Interceptors가 필요한가요?

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

## 1) 중앙 집중식 로직:

인터셉터는 HTTP 요청이 이루어지거나 응답이 수신되기 전에 로직을 실행하는 중앙 메커니즘으로 작용합니다. 이를 통해 로직을 여러 구성 요소나 서비스에 흩어지게 하는 필요성을 없애며 코드 재사용성과 유지보수성을 증진시킵니다.

## 2) 인증과 권한:

인터셉터는 인증 및 권한 관련 문제를 처리하는 뛰어난 해결책을 제공합니다. 개발자는 발신 요청을 가로채어 헤더, 토큰을 손쉽게 추가하거나 인증 검사를 수행할 수 있어서 애플리케이션 코드베이스를 지저분하게 만들지 않습니다.

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

## 3) 에러 처리:

HTTP 응답을 가로채는 것은 개발자가 튼튼한 에러 처리 메커니즘을 구현할 수 있도록 합니다. 인터셉터는 에러 응답을 가로채고 이를 의미 있는 메시지로 변환하여 애플리케이션 전반에 걸쳐 에러 전파를 최적화할 수 있습니다.

## 4) 요청 및 응답 변환:

인터셉터를 통해 개발자는 쉽게 요청 페이로드 또는 응답 데이터를 변환할 수 있습니다. 이를 통해 요청/응답 로깅, 데이터 직렬화/역직렬화 또는 응답을 도메인 모델로 매핑하는 시나리오를 구현할 수 있습니다.

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

# Angular에서의 구현

Angular의 HTTP Interceptor는 Angular 프레임워크에 완벽하게 통합되어 HTTP 트래픽을 가로채는 간소화된 접근 방식을 제공합니다. 아래는 Angular 17 단독 프로젝트에서 HTTP Interceptor의 구현 방법입니다.

이 Interceptor에서는 요청에 인증 헤더를 추가하고 오류를 처리할 것입니다.

- 새로운 Angular 프로젝트를 생성합니다

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
ng new test-app
```

2. 첫 번째로 HTTP 클라이언트를 설정하세요

```js
// app.config.ts

import { ApplicationConfig } from "@angular/core";
import { provideRouter } from "@angular/router";

import { routes } from "./app.routes";
import { provideHttpClient } from "@angular/common/http";

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes), provideHttpClient()],
};
```

3. HTTP 요청을 해봅시다

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
// app.component.ts

import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterOutlet } from '@angular/router';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {
  title = 'test-app';

  constructor(private http: HttpClient){}

  callApiHandler(){
    this.http.get('https://www.example.com').subscribe((res)=>{},)
  }
}
```

```js
// app.component.html

<div>
  <button (click)="callApiHandler()">Test</button>
</div>
```

4. 이제 아래의 Angular CLI 명령을 사용하여 인터셉터를 생성합니다.

```js
ng generate interceptor demo
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

아래 파일들이 생성됩니다

/assets/img/2024-07-06-Angular17HTTPInterceptorsguide_2.png

5. 아래 코드를 demo.interceptor.ts에 추가하여 모든 HTTP 요청에 인증 헤더를 추가하는 인터셉터를 구현합니다.

js
// demo.interceptor.ts

import { HttpInterceptorFn } from '@angular/common/http';

export const demoInterceptor: HttpInterceptorFn = (req, next) => {
const authToken = 'YOUR_AUTH_TOKEN_HERE';

// 요청을 복제하고 인증 헤더를 추가합니다
const authReq = req.clone({
setHeaders: {
Authorization: `Bearer ${authToken}`
}
});

// 업데이트된 헤더가 포함된 복제된 요청을 다음 핸들러에 전달합니다
return next(authReq);
};

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

인터셉터를 설정하는 방법은 withInterceptors를 호출하고 인터셉터를 전달하는 것입니다.

```js
// app.config.ts

...
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { demoInterceptor } from './demo.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    ...
    provideHttpClient(withInterceptors([demoInterceptor])),
  ],
};
```

이제 모든 HTTP 요청에서 권한 부여 헤더가 전송됩니다. 브라우저에서 확인하고 요청 헤더에서 확인할 수 있습니다.

/assets/img/2024-07-06-Angular17HTTPInterceptorsguide_3.png

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

6. 오류 처리

컴포넌트 내부에서 오류를 처리하는 대신 인터셉터 자체에 로직을 작성하여 오류를 처리할 수 있습니다.

```js
// demo.interceptor.ts

import { HttpErrorResponse, HttpInterceptorFn } from '@angular/common/http';
import { catchError, throwError } from 'rxjs';

export const demoInterceptor: HttpInterceptorFn = (req, next) => {
  ...
  ...
  return next(authReq).pipe(
    catchError((err: any) => {
      if (err instanceof HttpErrorResponse) {
        // HTTP 오류 처리
        if (err.status === 401) {
          // 권한이 없는 오류에 대한 특정 처리
          console.error('권한 없는 요청:', err);
          // 여기서 다시 인증 흐름을 트리거하거나 사용자를 리디렉션할 수 있습니다
        } else {
          // 다른 HTTP 오류 코드 처리
          console.error('HTTP 오류:', err);
        }
      } else {
        // HTTP가 아닌 오류 처리
        console.error('오류 발생:', err);
      }

      // 발생한 오류를 다시 전파하기 위해 에러를 다시 던집니다
      return throwError(() => err);
    })
  );;
};
```

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

- 인터셉터는 앱 전반에 걸친 일반적인 작업을 간편하게 해줍니다. 이는 재미없는 반복 코드를 줄이고 더 나은 구성을 의미합니다.
- 애플리케이션을 더 신뢰할 만들고 효율적으로 만들어줍니다.
- 보안, 오류 처리 및 다양한 기능 향상을 위해 사용하세요.

아래 레포지토리에서 소스 코드를 받아보세요.
