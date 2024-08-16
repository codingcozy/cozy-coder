---
title: "Angular와 GCP를 활용한 오픈 소스 PDF 서비스 구축 - 긴 처리 작업 효과적으로 처리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-BuildingPDFOpenSourceServiceswithAngularGCPHandlinglongprocessingtasks_0.png"
date: 2024-07-07 13:32
ogImage: 
  url: /assets/img/2024-07-07-BuildingPDFOpenSourceServiceswithAngularGCPHandlinglongprocessingtasks_0.png
tag: Tech
originalTitle: "Building PDF Open Source Services with Angular , GCP — Handling long processing tasks"
link: "https://medium.com/itnext/building-pdf-open-source-services-with-angular-gcp-handling-long-processing-tasks-e15cb4e511d3"
isUpdated: true
---





<img src="/assets/img/2024-07-07-BuildingPDFOpenSourceServiceswithAngularGCPHandlinglongprocessingtasks_0.png" />

안녕하세요! Angular(Analogjs), Firestore, Cloud Storage 및 CloudRun을 사용하여 오픈 소스 PDF 서비스를 구축하는 여정에 오신 것을 환영합니다. 이 프로젝트는 제 지식을 공유하고 최상의 실천법을 계속 배우며 동시에 커뮤니티에 기여하는 플랫폼으로 작용합니다.

부분 1: 아키텍처 개요
부분 2: Cloud Run에 서비스 배포
부분 3: 긴 처리 작업 처리

데모: https://pdfun.xyz
Angular Tiny Conf의 YouTube 토크를 확인해보세요!

<div class="content-ad"></div>

해결책은 GCP 생태계를 기반으로 구축되어 있으므로 프로젝트를 GCP에 배포하는 것이 좋습니다. 이렇게 하면 GCP의 서비스에 액세스할 수 있습니다. 해결책은 두 부분으로 나뉩니다:

- 웹 UI (Analogjs — Angular): 사용자 상호작용 처리
- 백엔드 (Node — Express): PDF 파일 처리

PDF 서비스를 구축하는 과정에서 PDF 파일을 업로드, 다운로드 및 처리해야 하는데, 이는 상당한 시간이 소요될 수 있습니다. 이 기사에서는 이러한 긴 처리 작업을 효율적으로 처리하는 방법을 탐색합니다.

# 일반 API 요청과 그 함정들

<div class="content-ad"></div>

일반적으로 클라이언트가 API 요청을 보내면 서버가 요청을 처리하고 응답을 보냅니다. 이 동기식 접근 방식은 짧은 작업에는 잘 작동합니다. 그러나 긴 처리 작업에 대해선 자신의 함정이 있습니다.

주된 문제는 클라이언트가 서버가 작업을 완료할 때까지 기다려야 하고 응답을 받기 전까지 기다려야 한다는 것입니다. 이러한 점은 특히 작업이 오래 걸릴 경우에 사용자 경험을 저해할 수 있습니다.

게다가 대부분의 브라우저와 클라이언트 측 라이브러리는 API 요청에 대한 시간 초과 제한이 있습니다. 서버가 이 제한 내에 응답하지 않으면 요청이 자동으로 취소됩니다.

# 클라이언트 측에서의 최대 시간 초과

<div class="content-ad"></div>

API 요청의 최대 타임아웃은 클라이언트 측 라이브러리나 브라우저에 따라 다릅니다. 예를 들어, Angular의 HttpClient의 기본 타임아웃은 0으로, 응답이 올 때까지 무한정 기다립니다. 그러나 Chrome이나 Firefox 같은 브라우저는 대략 300초(5분)로 최대 타임아웃을 가지고 있습니다. 이 시간 내에 서버가 응답하지 않으면 요청이 종료됩니다.

# 클라이언트 측에서 긴 요청을 처리하는 일반적인 방법

클라이언트 측에서 긴 요청을 처리하는 여러 방법이 있습니다:

- 폴링(Polling): 클라이언트가 서버로 요청을 보내고 주기적으로 작업이 완료되었는지 확인하기 위해 후속 요청을 보냅니다.
- 롱 폴링(Long Polling): 클라이언트가 서버로 요청을 보내고, 서버는 작업이 완료되거나 타임아웃이 발생할 때까지 요청을 유지합니다.
- 웹소켓(WebSockets): 클라이언트와 서버 사이에 지속적이고 양방향 통신 채널을 설정하여, 서버가 작업이 완료되면 응답을 보낼 수 있습니다.
- 서버-보내는-이벤트(Server-Sent Events): 서버가 클라이언트에 업데이트를 보내는 단일, 장기적인 연결을 통해 통신합니다.

<div class="content-ad"></div>

이 방법들은 효과적일 수 있지만, 더 복잡해지고 자원의 비효율성이 증가하는 등의 단점도 있습니다.

## 폴링 예시

Angular에서 폴링을 구현하는 예시입니다.

```js
// polling.component.ts

INTERVAL = 2000; // 2초
data = signal({});

timer(0, this.INTERVAL)
  .pipe(
    // API 요청을 중지
    takeUntil(this.stopTimer$),
    delay(1000),
    // 서버에서 데이터 가져오는 함수
    switchMap(() => this.getData()),
    // 오류 발생 시 재시도
    retry()
  )
  .subscribe({
    next: (res: any) => {
      if (res.status === 'SUCCEED') {
        this.stopTimer$.next(true);
      }
      this.data.set(res);
    },
    error: (error: Error) => {
      this.errorMessage.set(error.message);
    },
  });
```

<div class="content-ad"></div>

rxjs를 사용하여 타이머 연산자를 활용한 함수를 만들어, getData()를 사용하여 서버에서 데이터를 가져오는 간격(2초)을 실행합니다. 컴포넌트가 소멸되거나 원하는 데이터가 검색되면 stopTimer$를 emit하여 멈출 수 있습니다.

## SSE(Server-sent events)의 예제

다음은 서버에서 SSE를 구현한 예제입니다:

```js
// server (Analogjs - Nitro server)

export default defineEventHandler(async (event) => {
    const eventStream = createEventStream(event);

    const interval = setInterval(async () => {
        await eventStream.push(`Message @ ${new Date().toLocaleTimeString()}`);
    }, 1000);

    eventStream.onClosed(async () => {
        clearInterval(interval);
        await eventStream.close();
    });

    return eventStream.send();
});
```

<div class="content-ad"></div>

우리는 createEventStream을 활용하여 스트림을 생성하고 간격(1초)을 설정하여 서버에서 클라이언트로 문자열 데이터를 스트리밍하는 예시입니다.

클라이언트 구현을 살펴보겠습니다:

```js
  constructor() {
    // afterNextRender를 사용하여 코드가 브라우저에서 실행되는 것을 보장합니다.
    afterNextRender(() => {
      this.eventSource = new EventSource('/api/v1/sse');

      this.eventSource.onmessage = (event) => {
        this.data.set(event.data);
      };
    });
  }

  ngOnDestroy() {
    this.eventSource?.close();
  }
```

EventSource를 사용하여 서버를 가리키는 이벤트 소스를 만듭니다. 그 후 this.eventSource.onmessage 콜백 메서드를 사용하여 서버로부터 데이터를 수신할 수 있습니다.

<div class="content-ad"></div>

# GCP 클라우드런과 Firestore를 활용하여 긴 요청 처리하기

Google Cloud Platform (GCP)은 오랜 처리 작업을 처리하는 강력한 도구를 제공합니다. 특히, 클라우드 런과 Firestore를 활용할 수 있습니다.

PDF 리사이즈 서비스의 아키텍처 흐름은 다음과 같습니다:

![PDF 리사이즈 서비스의 아키텍처 흐름](/assets/img/2024-07-07-BuildingPDFOpenSourceServiceswithAngularGCPHandlinglongprocessingtasks_1.png)

<div class="content-ad"></div>

PDF 크기 조정 서비스에 대해 더 자세히 이해하기 위해 첫 번째 기사를 읽었습니다.

Firestore를 활용하면 폴링이나 서버 전송 이벤트를 직접 구현할 필요가 없어서 훨씬 편리합니다. 이것이 Backend as a service의 아름다움이죠😉.

프론트엔드에서 Firestore의 데이터 변경을 관찰하고, 사용자가 준비되면 크기가 조정된 파일을 다운로드할 수 있도록 허용할 수 있습니다. 예시 코드를 살펴보겠습니다:

```js
// 코드가 이해하기 쉽도록 단순화되었습니다.

docRef = doc(this.firestore, `${this.generateFilePath()}/${this.currentID()}`)

pdf = computed(() => {
  // 데이터의 observable을 반환합니다.
  return docData(this.docRef()) as Observable<UploadedFile>
})

downloadUrl$ = this.pdf().pipe(
    // taskResponse 객체를 포함한 데이터만 가져옵니다.
    filter((doc) => Object.keys(doc?.taskResponse ?? {}).length > 0),
    switchMap((doc) => {
      // 응답 데이터 처리 및 유효성 검사

      return this.getDownloadLink(
        `${doc.filePath}/${doc.taskResponse?.fileName}`,
      )
    }),
  )
```

<div class="content-ad"></div>

우리가 해야 할 일은 프런트엔드 코드에서 리스너를 생성하면, 서버가 데이터를 준비되면 다시 돌려줄 것이에요! https://pdfun.xyz에서 테스트해 볼 수 있어요. 

결론적으로, 웹 개발에서 긴 처리 작업을 다루는 것은 어려울 수 있지만, 올바른 도구와 전략을 활용하면 분명히 관리할 수 있어요. Angular와 GCP의 힘을 활용하여 긴 처리 작업을 효과적이고 효율적으로 처리하는 견고한 PDF 오픈 소스 서비스를 구축할 수 있어요. 즐거운 코딩되세요!