---
title: "Angular 인터셉터 완전 정복 친절한 가이드"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:50
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Understanding Angular Interceptors: A Friendly Guide"
link: "https://medium.com/@paul-chesa/understanding-angular-interceptors-a-friendly-guide-ed505b32211c"
---



![Image](https://miro.medium.com/v2/resize:fit:996/1*fh_tznUFpL1ODdMWrpjkwQ.gif)

Angular interceptors are a powerful feature that can help you manage HTTP requests and responses in your Angular applications. But what exactly are they, and how do they work? Let’s dive into the world of Angular interceptors.

## What Are Angular Interceptors?

Imagine you’re a teacher in a big school. Every day, you handle lots of students and activities. Now, let’s say you have some helpers who assist you with specific tasks like checking if students have their homework, ensuring they wear their uniforms, or even reminding them to stay quiet in the library.


<div class="content-ad"></div>

앵귤러에서는 이러한 도우미들을 인터셉터라고 합니다. 이들은 각 HTTP 요청과 응답마다 실행할 수 있는 함수 또는 클래스로, 인증, 로깅 및 캐싱과 같은 일반적인 패턴을 구현할 수 있게 해줍니다.

## 도우미 유형 (인터셉터)

- 기능적 인터셉터: 이들은 매번 특정 루틴을 따르는 것을 정확히 아는 도우미와 같습니다. 많은 활동이 있는 바쁜 학교에서도 신뢰할 수 있고 예측할 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1280/1*PTn51ZaAI_S1ILLjJ2JVlw.gif)

<div class="content-ad"></div>

2. DI 기반 Interceptors: 이들 도우미들은 상황 및 계층에 따라 작업이 할당됩니다. 때로는 이들의 작업이 겹치기도 하여, 누가 먼저 무엇을 하는지 예측하기 어려울 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1276/1*TE4eaViBdzw8kjNDUOy_OA.gif)

## 동작 원리

학생(=HTTP 요청)이 학교(=귀하의 애플리케이션)에 들어오면, 각 체크포인트(=Interceptors)를 거칩니다. 이 체크포인트의 각 도우미(=Interceptor)는:

<div class="content-ad"></div>

- 학생이 허가서(인증)를 갖고 있는지 확인합니다.
- 학생의 도착 시간을 기록합니다(로그).
- 학생에게 숙제를 상기해 줍니다(요청 수정).
- 특별한 지시사항을 추적합니다(요청 및 응답 메타데이터).

## 도우미 작업 예시

학교 출입구에 "Logger"라는 도우미가 있어 학생들의 이름을 기록하는 상황을 상상해 보세요. Angular에서는 다음과 같이 나타납니다:

```js
export function loggingInterceptor(req: HttpRequest<unknown>, next: HttpHandlerFn): Observable<HttpEvent<unknown>> {
  console.log(req.url);  // 도우미가 학생의 이름을 기록합니다
  return next(req);      // 학생이 다음 체크포인트로 이동합니다
}
```

<div class="content-ad"></div>

## 도우미 설정하기

당신은 학교 행정부(Angular 앱)에 당신의 도우미(인터셉터)들을 소개하여 어떤 체크포인트(인터셉터)를 사용할지 알려줍니다. 이들을 원하는 순서대로 나열합니다:

```js
bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient(
      withInterceptors([loggingInterceptor, cachingInterceptor]),
    )
  ]
});
```

여기서 "Logger" 도우미가 가장 먼저 작동하고, 다음으로 다른 도우미인 "Cacher"가 나와 학생이 최근에 과제를 제출했는지 확인합니다.

<div class="content-ad"></div>

![Image](https://miro.medium.com/v2/resize:fit:996/1*ulxO2FV_nBrw0gRnOnjYxg.gif)

## 요청 수정하기

가끔, 도우미는 학생에 대해 어떤 것을 바꿔야 할 수도 있어요. 예를 들어, 학생이 배지를 깜빡했다면, 도우미는 그들에게 임시 배지를 제공합니다:

```js
const reqWithHeader = req.clone({
  headers: req.headers.set('X-New-Header', 'new header value'),
});
```

<div class="content-ad"></div>

학생(요청)이 전진하기 전에 필요한 모든 것을 갖추고 있는지 확인하는 것이 중요합니다.

## 고급 작업

도우미는 더 복잡한 작업도 수행할 수 있습니다. 예를 들어, 새로운 응답을 생성하거나(예: 일지에 새 항목 추가) 또는 문제가 발생할 경우 재시도를 처리할 수도 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:996/1*7pROxim0KjsWdbSk2uGE-Q.gif)

<div class="content-ad"></div>

## 의존성 주입 (DI)

어떤 도우미들이 도구나 정보 (예: 학생 목록)가 필요하듯이, 인터셉터는 Angular의 DI 시스템에서 제공하는 서비스를 사용하여 필요한 것을 얻을 수 있습니다.

## 컨텍스트 토큰

가끔, 도우미에게 특별한 지시를 전달해야 할 때가 있습니다. 이는 학생에게 다음 체크포인트에서 도우미에게 보여줄 쪽지를 주는 것과 비슷합니다. 이러한 쪽지 (컨텍스트 토큰)은 도우미에게 어떤 특별한 조치를 취할지 알려줍니다.

<div class="content-ad"></div>

앵귤러에서 인터셉터는 앱을 통해 전달되는 각 HTTP 요청에 대해 다양한 작업을 처리하는 전용 도우미 역할을 합니다. 로깅, 수정, 재시도, 심지어 응답 생성을 담당하여 각 요청이 원활하고 효율적으로 처리되도록 합니다. 세심하게 조직된 학교와 도움이 되는 어시스턴트들처럼, 인터셉터는 앵귤러 앱이 효율적이고 안전하게 실행되도록 유지합니다.

앵귤러 인터셉터를 이해하고 활용함으로써 응용 프로그램의 기능성과 신뢰성을 크게 향상시킬 수 있습니다. 그러니 가서 인터셉터를 설정하고, 귀하의 앱의 HTTP 요청과 응답을 이러한 유용한 "학교 도우미들"에 의해 원활하게 관리하도록 해보세요!

![image](https://miro.medium.com/v2/resize:fit:960/1*nk8_ci1BSvxhNSjdEBiL0g.gif)