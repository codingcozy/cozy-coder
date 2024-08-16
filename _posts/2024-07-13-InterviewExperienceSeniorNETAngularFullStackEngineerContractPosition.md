---
title: "시니어 NET Angular 풀스택 엔지니어 계약직 인터뷰 경험 공유"
description: ""
coverImage: "/assets/img/2024-07-13-InterviewExperienceSeniorNETAngularFullStackEngineerContractPosition_0.png"
date: 2024-07-13 00:07
ogImage: 
  url: /assets/img/2024-07-13-InterviewExperienceSeniorNETAngularFullStackEngineerContractPosition_0.png
tag: Tech
originalTitle: "Interview Experience Senior .NET Angular Full Stack Engineer Contract Position"
link: "https://medium.com/@vemurivi/interview-experience-senior-net-angular-full-stack-engineer-contract-position-248057739755"
isUpdated: true
---





미국 배송 회사에서 계약 직위에 지원한 면접을 최근에 받았어요. 면접관은 Java 및 Angular 전문가인 개발자였어요. 아래는 몇 가지 물음과 제 답변입니다:

![면접 경험](/assets/img/2024-07-13-InterviewExperienceSeniorNETAngularFullStackEngineerContractPosition_0.png)

Q: Angular에서 forkJoin이 무엇인가요?

A: forkJoin은 RxJS (JavaScript를 위한 반응형 확장 라이브러리)의 연산자로, Angular 애플리케이션에서 자주 사용됩니다. 이 연산자는 여러 개의 옵저버블을 병렬로 실행하고 모든 옵저버블이 완료될 때까지 기다립니다. 모든 옵저버블이 완료되면 forkJoin은 각 옵저버블에서 마지막으로 내보낸 값을 포함하는 단일 배열을 방출합니다.

<div class="content-ad"></div>

Q: 앵귤러에서 map과 forEach는 무엇인가요? 두 가지 중 어느 것이 mutable인가요?

A: 앵귤러에서 map과 forEach는 배열 및 RxJS observables에서 사용되는 메소드입니다.

- map: 배열 또는 observable의 각 요소를 변환하고 새로운 배열 또는 observable을 반환합니다. 이는 non-mutable이며, 즉 원래 배열이나 observable을 변경하지 않고 변환된 값을 가진 새로운 배열 또는 observable을 생성합니다.
- forEach: 제공된 함수를 각 배열 요소마다 한 번 실행합니다. 이는 forEach 루프 내에서 원래 배열 요소를 수정할 수 있으므로 mutable합니다.

Q: 앵귤러에서 두 가지 종류의 바인딩을 말해주세요.

A: 앵귤러는 데이터 및 뷰를 동기화하여 유지하기 위해 여러 종류의 데이터 바인딩을 지원합니다.

<div class="content-ad"></div>

- 컴포넌트에서 DOM으로: 프로퍼티 바인딩: [property]=”value”: 값이 컴포넌트에서 지정된 프로퍼티 또는 간단한 HTML 속성에 전달됩니다

2. DOM에서 컴포넌트로: 이벤트 바인딩: (event)=”function”: 특정 DOM 이벤트가 발생하면 (예: 클릭, 변경, 키 입력), 컴포넌트에서 지정된 메서드를 호출합니다

3. 양방향 바인딩 ([(ngModel)]):

- 뷰와 컴포넌트 프로퍼티를 동기화하기 위해 프로퍼티와 이벤트 바인딩을 결합합니다.
- 예: `input [(ngModel)]="username"`

<div class="content-ad"></div>

Q: RxJS에서 mergeMap, switchMap 및 concatMap의 차이점은 무엇인가요?

A: 이들은 모두 평탄화 연산자이지만 내부 observable을 다르게 처리합니다:

- mergeMap: 각 소스 값을 observable로 프로젝트하고 결과 observable을 병합합니다. 모든 내부 observable은 동시에 구독됩니다.

```js
source$.pipe(mergeMap(value => innerObservable(value))).subscribe();
```

<div class="content-ad"></div>

- switchMap: 각 소스 값을 옵저버블에 매핑하지만 새로운 옵저버블을 구독하기 전에 이전 내부 옵저버블의 구독을 취소합니다. 한 번에 하나의 내부 옵저버블 구독만 유지합니다.

```js
source$.pipe(switchMap(value => innerObservable(value))).subscribe();
```

- concatMap: 각 소스 값을 옵저버블에 매핑하고 순차적으로 각 옵저버블에 구독합니다. 각 내부 옵저버블이 완료될 때까지 다음 옵저버블을 구독하기를 기다립니다.

```js
source$.pipe(concatMap(value => innerObservable(value))).subscribe();
```

<div class="content-ad"></div>

Q: .NET에서 Java Streams를 사용해 보신 적이 있나요?(약간의 힌트를 주셨군요)

.NET에서는 Java Streams의 직접적인 동등물이 없지만, LINQ (Language Integrated Query)이 .NET 생태계에서 유사한 기능을 제공한다고 언급했습니다.

## Java Streams

Java Streams는 Java 8에서 소개된 Java Stream API의 일부입니다. 이들은 요소(컬렉션) 시퀀스를 처리하는 함수형 접근 방식을 제공하며, 필터링, 매핑, 축소 등의 작업을 지원합니다.

<div class="content-ad"></div>

자바 스트림의 주요 특징:

- 함수형 프로그래밍: 람다 표현식과 메서드 참조 사용.
- 파이프라인 작업: 스트림을 연결하여 작업 파이프라인을 구성할 수 있음.
- 지연 평가: 중간 작업은 최종 작업이 호출될 때까지 실행되지 않음.
- 병렬 처리: 병렬 스트림을 사용하여 작업을 쉽게 병렬화할 수 있음.

## .NET의 LINQ

LINQ(언어 통합 쿼리)는 .NET의 기능으로, 컬렉션을 조회할 수 있게 해주는 기능으로, 컬렉션, 데이터베이스, XML과 같은 다양한 유형의 데이터 원본에서도 일관된 구문을 사용할 수 있음. LINQ는 메서드 구문과 쿼리 구문을 모두 제공함.

<div class="content-ad"></div>

LINQ의 주요 기능:

- 통합된 쿼리: C#과 VB.NET에 직접 내장되어 있습니다.
- 통일된 구문: 다양한 데이터 소스에서 일관된 쿼리 경험을 제공합니다.
- 지연 실행: Java 스트림과 유사하게, LINQ 쿼리는 결과가 나열될 때까지 실행되지 않습니다.
- 강력한 형식 지정: 컴파일 시간 확인을 통해 타입 안전한 쿼리를 제공합니다.

Java에서는 LINQ을 직접 사용할 수 없으며, .NET에서는 Java 스트림을 직접 사용할 수 없습니다. 이는 각각의 생태계 (Java 및 .NET)에 특정하기 때문입니다. 그러나 두 생태계 모두 LINQ에 대한 유사한 기능을 제공하는 라이브러리와 도구가 있으며, .NET에서는 Java 스트림에 대한 유사한 기능을 제공하는 라이브러리와 도구도 있습니다.

Q: REST API에서의 계약 우선 접근 방식이란 무엇인가요?

<div class="content-ad"></div>

A: REST API 개발에서의 계약 우선 접근 방식은 실제 API를 구현하기 전에 API 계약을 정의하는 것을 포함합니다(일반적으로 OpenAPI/Swagger 사양을 사용). 이 계약에는 엔드포인트, 요청/응답 형식 및 데이터 유형에 대한 세부 정보가 포함됩니다. 이는 모든 이해관계자가 개발이 시작되기 전에 API 설계에 동의하도록하며, 더 나은 협력과 오해를 줄일 수 있도록 도와줍니다.

Q: ".NET에 강력한 백그라운드가 있지만, 이 역할은 Java Spring Boot와 함께 작업하는 것을 필요로 합니다. 이 갭을 어떻게 메꾸려고 계획하십니까?"

A: ".NET에 대한 광범위한 경험은 Java Spring Boot와 함께 작업하는 데 강력한 기반을 제공한다고 믿습니다. 두 플랫폼은 객체 지향 프로그래밍, 의존성 주입 및 RESTful API 설계와 같은 공통 원리를 공유합니다. 예를 들어, ASP.NET Core의 의존성 주입 작업은 Spring Boot의 Spring Framework를 사용하는 방식으로 전환 가능합니다.

.NET, Node.js, Java 및 Python과 같은 다른 프로그래밍 언어 간에는 REST API 표준과 원칙이 일관되게 유지됩니다. 언어에 관계없이 모든 REST 서비스는 동일한 핵심 원칙을 따릅니다. 이들은 상태를 유지하지 않고, 즉 각 클라이언트 요청이 서버 측 세션에 의존하지 않고 필요한 모든 정보를 포함하는 것을 의미합니다. 또한 가벼우며 보통 HTTP 위에서 작동하기 때문에 사용 및 통합이 쉽습니다. REST API는 일관성과 예측 가능성을 보장하기 위해 CRUD 작업을 위해 GET, PUT, POST, DELETE 및 PATCH와 같은 표준 HTTP 메서드를 사용하는 균일한 인터페이스를 사용합니다. 게다가 데이터 반복 검색을 줄여 성능을 향상시키기 위해 캐시할 수 있도록 설계되어 있습니다."

<div class="content-ad"></div>

게다가, 새로운 기술을 빠르게 학습하는 능력을 지속적으로 증명했습니다. 또한 온라인 자료를 활용하여 Spring Boot로 샘플 GitHub 프로젝트를 진행했습니다."

Q: API에서 대량의 데이터나 파일 처리를 어떻게 다루겠습니까?

A: API에서 대량의 데이터나 파일 처리는 여러 가지 방법으로 접근할 수 있습니다:

- 페이지네이션: 대규모 데이터를 더 작고 관리 가능한 청크로 분할합니다.
- 스트리밍: 클라이언트로 데이터를 스트리밍하여 한꺼번에 모든 것을 메모리에 로드하는 것을 피합니다.
- 배치 처리: 대용량 파일을 배치로 처리하여 자원 사용을 효과적으로 관리합니다.
- 비동기 처리: 백그라운드 처리와 비동기 API를 사용하여 대규모 작업을 메인 애플리케이션 흐름을 블로킹하지 않고 처리합니다.

<div class="content-ad"></div>

나는 .NET에 Hangfire 라이브러리를 추가하여 클라이언트에게 응답 대기 없이 백그라운드에서 작업을 실행하는 기능을 추가했어. 또한 이미지와 비디오와 같은 미디어 파일에 대한 콘텐츠 전송 네트워크의 사용도 추가했어.

Q: 애자일의 다섯 가지 의식은 무엇인가요?
A: 애자일의 다섯 가지 의식은 다음과 같아:
- 스프린트 계획: 팀이 다가오는 스프린트에서 완료할 작업을 계획해.
- 매일 아침 회의: 팀원이 서로 진행상황과 장애물을 최신 정보로 업데이트하는 짧은 일일 회의.
- 스프린트 검토: 팀이 이해관계자에게 스프린트 동안 완료된 작업을 시연해.
- 스프린트 회고: 팀이 이전 스프린트를 반성하여 다음 스프린트를 위한 개선점을 확인해.
- 백로그 정제 (그루밍): 팀이 제품 백로그 항목을 검토하고 우선순위를 정하여 향후 스프린트에 준비되어 있는지 확인하는 것이야.

<div class="content-ad"></div>

번역: 전체적으로 면접은 Angular와 Java 개념부터 REST API 디자인 및 Agile 방법론에 이르기까지 다양한 주제를 다루었습니다. 제 기술과 경험에 대해 논의할 수 있는 좋은 기회였으며, 회사의 기술 스택과 개발 관행에 대해 더 많이 알아볼 수 있는 기회였습니다.