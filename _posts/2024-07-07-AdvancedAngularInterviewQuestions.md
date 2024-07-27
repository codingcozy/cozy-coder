---
title: "고급 Angular 인터뷰 질문들"
description: ""
coverImage: "/assets/img/2024-07-07-AdvancedAngularInterviewQuestions_0.png"
date: 2024-07-07 03:05
ogImage: 
  url: /assets/img/2024-07-07-AdvancedAngularInterviewQuestions_0.png
tag: Tech
originalTitle: "Advanced Angular Interview Questions"
link: "https://medium.com/@tusharghosh09006/advanced-angular-interview-questions-d1573f1b350b"
---



<img src="/assets/img/2024-07-07-AdvancedAngularInterviewQuestions_0.png" />

2024년, 레세션과 해고는 계속되고 있습니다. 많은 개발자들이 취업을 찾고 있습니다. 첫 번째와 두 번째 인터뷰에서 경험을 얻고 세 번째 인터뷰를 합격하는 것은 이상적인 상황이 아닙니다. 지금은 인터뷰 요청을 받기 어려운 상황입니다. 처음 인터뷰를 합격하도록 준비하십시오. 지난 3달 동안, 세 번의 인터뷰를 받았습니다. 모든 인터뷰에서 성공했습니다.

Angular 준비를 위해 Google 드라이브에 Angular에 관한 내용을 기록했습니다. 다른 사람들이 Angular 인터뷰를 준비하는 데 도움을 주기 위해 이를 medium에 기사로 공유하고 있습니다. 몇 가지 질문을 디자인 패턴에 연결하려고 노력하고 시나리오 기반 질문을 토의하려고 노력했습니다.

## Angular 애플리케이션을 개선하기 위해 어떤 단계를 취할 것인가요?


<div class="content-ad"></div>

- 사용하지 않는 라이브러리와 코드를 제거하십시오.
- ngFor 루프에서 trackBy를 사용하십시오.
- 성능을 향상시키기 위해 정적 파일을 캐싱하십시오.
- ng-container는 추가적인 DOM 요소를 생성하지 않는 구조 지시자입니다. ng-container는 ngIf, ngFor 및 ngSwitch와 같은 구조 지시자를 적용하기 위한 그룹화 메커니즘을 제공하여 불필요한 HTML 요소를 도입하지 않습니다.
- 애플리케이션에서 이미지 크기를 압축하십시오.
- 모듈에 지연 로딩을 구현하십시오. 이렇게 하면 필요한 부분만로드하여 초기로드 시간을 줄일 수 있습니다.
- 페이지네이션 또는 무한 스크롤링을 사용하여 페이지에서 데이터 로드를 개선하십시오.
- webpack-bundle-analyzer를 통합하여 컴포넌트 크기와 타사 라이브러리 크기를 조사하십시오.
- OnPush 변경 감지 전략도 Angular 성능을 향상시킬 때 사용됩니다. Angular는 속성의 개별 변경을 감지하기 위해 전체 컴포넌트 트리 구조를 탐색할 필요가 없습니다. 이 전략에서는 필요할 때 컴포넌트를 다시 렌더링할 수 있습니다.

## 컴포넌트 간 데이터를 공유하는 방법은 몇 가지가 있나요?

- 입력/출력 바인딩: 부모 컴포넌트는 입력 속성(@Input 데코레이터)을 통해 데이터를 자식 컴포넌트로 전달하고 출력 속성(@Output 데코레이터)을 통해 자식 컴포넌트로부터 데이터를 수신할 수 있습니다.
- ViewChild/ContentChild: 부모 컴포넌트는 ViewChild 및 ContentChild 데코레이터를 사용하여 자식 컴포넌트 및 그 속성에 액세스할 수 있습니다.
- 서비스: Angular 서비스를 사용하여 공유 데이터를 관리하고 액세스할 수 있습니다. 컴포넌트는 동일한 서비스 인스턴스를 주입할 수 있으며 데이터를 공유하는 데 사용할 수 있습니다.
- 상태 관리: 중앙 집중식 스토어를 사용하여 응용 프로그램 상태를 관리하고 컴포넌트 간 데이터를 공유할 수 있습니다.

## ngOnInit 라이프사이클 후크 대신 Angular 컴포넌트의 생성자에서 API를 호출할 수 있습니까?

<div class="content-ad"></div>

Angular 생성자에서 API를 호출하는 것은 기술적으로 가능하지만, 권장되는 방법은 아닙니다. 생성자는 Angular에서 의존성 주입이나 입력 속성과 같은 기능이 완전히 초기화되기 전에 컴포넌트 클래스가 초기화될 때 호출됩니다. 이는 이러한 기능이 완전히 이용 가능하지 않은 상태에서 API를 호출할 경우 예상치 못한 동작이나 오류가 발생할 수 있다는 것을 의미합니다.

## 표준 Angular 컴포넌트와 독립형 컴포넌트의 차이점은 무엇인가요?

- 표준 컴포넌트는 Angular 응용 프로그램 내에서 사용하기 위해 NgModule에 포함되어야합니다. 독립형 컴포넌트는 이를 필요로하지 않으며 NgModule에 포함되지 않고도 독립적으로 사용할 수 있습니다.
- 표준 컴포넌트는 Angular 또는 서드 파티 기능을 사용하기 위해서 NgModule에서 가져와야합니다. 예를 들어 *ngFor 지시문을 사용하려면 NgModule에서 @angular/common에서 CommonModule을 가져와야합니다. 반면에 독립형 컴포넌트는 자체 파일 내에서 직접 종속성을 가져올 수 있습니다.

## Angular에서 크로스사이트 스크립팅(XSS)을 방지하는 방법

<div class="content-ad"></div>

- 보간(interpolation) 및 프로퍼티 바인딩(Property Binding): Angular에는 보간('' '') 및 프로퍼티 바인딩([ ])과 같은 데이터 바인딩을 위한 내장 메커니즘이 있습니다. 이러한 메커니즘은 사용자 입력을 자동으로 살균하여 브라우저에서 안전하게 렌더링할 수 있도록 합니다. 동적 콘텐츠를 렌더링할 때는 XSS를 방지하기 위해 항상 이러한 메커니즘을 사용해야 합니다.
- 사용자 입력 살균화: HTML, CSS 또는 JavaScript를 포함할 수 있는 동적 콘텐츠를 처리해야할 때는 Angular의 내장 DomSanitizer 서비스를 사용하여 입력을 살균화한 후 DOM에 렌더링해야 합니다.

```js
import { DomSanitizer } from '@angular/platform-browser';

constructor(private sanitizer: DomSanitizer) {}

// 사용자 입력 살균화
sanitizedHtml = this.sanitizer.sanitize(SecurityContext.HTML, userInput);
```

- 안전하게 Angular 템플릿 사용: innerHTML 또는 outerHTML을 사용하여 HTML 콘텐츠를 동적으로 생성하지 마세요. 대신 Angular의 템플릿 구문을 활용하여 안전하게 동적 콘텐츠를 렌더링하세요.

## Angular에서 Host Binding과 Host Listening이란 무엇인가요?

<div class="content-ad"></div>

Angular에서는 @HostBinding 및 @HostListener 데코레이터를 사용하여 지시문이나 컴포넌트의 호스트 요소와 상호 작용합니다.

@HostBinding: @HostBinding 데코레이터는 호스트 요소의 속성을 바인딩하는 데 사용됩니다. 이를 통해 지시문이나 컴포넌트의 속성에 따라 호스트 요소의 속성을 설정할 수 있습니다.

```js
import { Directive, HostBinding } from '@angular/core';

@Directive({
 selector: '[appHighlight]'
})
export class HighlightDirective {
  @HostBinding('style.backgroundColor') 
  backgroundColor: string = 'yellow';
}
```

@HostListener: @HostListener 데코레이터는 호스트 요소에서 이벤트를 수신하기 위해 사용됩니다. 이를 통해 호스트 요소에서 특정 이벤트가 발생할 때 호출될 메서드를 정의할 수 있습니다.

<div class="content-ad"></div>

```js
import { 지시자, 호스트리스너 } from '@angular/core';

@Directive({
 selector: '[appClickLogger]'
})
export class ClickLoggerDirective {

 @HostListener('click', ['$event'])
 onClick(event: Event): void {
  console.log('호스트 요소가 클릭되었습니다!', event);
 }

}
```

## 나에게 "입력 필드가 하나 있고, 제출 후 입력한 내용의 길이로 응답을 받는다면 Angular에서 이를 어떻게 구현하겠습니까?" 라는 질문을 받았어요.

이것은 간단한 질문이고, 몇 분 만에 완료할 수 있다는 것을 알고 있어요. 그러나 인터뷰의 목적은 Angular를 얼마나 깊이 이해하고 이 작업을 완료하기 위해 어떤 단계를 취할지 이해하는 것이에요.

이러한 종류의 질문에 직면했을 때 보안, 에러 처리, 테스트, 상태 관리, 폼 유효성 검사, API 호출, 인터셉터와 같은 다양한 부분을 다루려고 노력해보세요.

<div class="content-ad"></div>

여기 몇 가지 기본 아이디어가 있어요.

- 기본 구현: 반응형 폼과 템플릿 기반 폼을 논의해 보세요. 각각의 장단점을 살펴보고 최종적으로 하나를 선택하세요. 저는 다양한 유효성 검사, 동적 컨트롤, 테스트에 용이한 점으로 인해 반응형 폼을 선택할 것입니다.
- 보안: 보안에 큰 관심을 가지고 있습니다. XSS(Cross-site Scripting)를 방지하기 위해 입력값을 정제하는 데 Dom Sanitizer를 사용할 것입니다.
- 오류 처리: Angular은 ErrorHandler 인터페이스를 제공하여 전역 오류를 처리합니다. Angular 애플리케이션에서 오류를 처리하기 위해 사용할 것입니다. 또한 HttpClientInterceptor를 사용하여 서버에서 반환된 오류를 가로채고 처리할 것입니다.
- 테스트: 테스트는 응용 프로그램이 올바르게 작동하고 지정된 요구 사항을 충족하는지 확인합니다. 안정적인 응용 프로그램을 만들기 위해 Karma와 jasmine을 사용한 단위 테스트 및 e2e 테스트를 작성할 것입니다.
- 상태 관리: 컴포넌트 수준, 서비스 수준 또는 Ngrx와 같은 전역 상태에서 상태를 관리할 수 있습니다. 이 경우에는 컴포넌트 수준 상태 액세스를 선택할 것입니다.
- API 호출: Angular은 HttpClient 모듈을 제공하여 서버로부터 HTTP 요청을 보내고 응답을 받을 수 있습니다. Angular 2와 4에서는 Http 모듈을 제공했지만 Angular 5부터 HttpClient 모듈로 변경되었습니다. 여기에서 Rxjs 연산자를 사용하여 쉽게 오류를 처리하고 응답 데이터를 수정할 수 있습니다. 이 문제에서는 get 및 post 호출을 모두 사용할 수 있지만 post 호출을 선택할 것입니다. 보안상 post 호출이 더 안전하기 때문입니다.
- 인터셉터: HTTP 인터셉터를 사용하여 나가는 요청 및 들어오는 응답을 가로챌 수 있습니다. 예를 들어 헤더에 토큰을 추가하기 위해 인터셉터를 사용할 수 있습니다.
- 로깅 서비스: 모니터링 및 디버깅을 위해 중앙 집중식 로깅 서비스나 플랫폼에 오류를 기록할 것입니다.

이 유형의 질문에는 정답이나 오답이 없습니다. 여러분을 준비하는 데 도움이 될만한 샘플 포인트들이에요.

## Angular 라이프사이클 훅에 대해 이야기해 주세요.

<div class="content-ad"></div>

이 질문을 받을 확률이 90%입니다. 재미있는 사실은 초보자부터 고급 개발자까지 모두 이 답을 알고 있다는 것입니다. 중요한 점은 답변을 전달하는 방식이 초보자와의 차이를 만들어줍니다. 저는 항상 이전 직무 경험을 활용하려고 노력합니다.

여기 제 답변입니다:

이 질문은 중요한 문제입니다. Angular에는 8개의 라이프사이클 훅이 있습니다. 이전 직장 환경에서는 주로 ngOnInit와 ngOnDestroy를 많이 사용했습니다. ngOnInit은 컴포넌트 초기화 시 호출됩니다. 한 번만 호출됩니다. 주로 변수 초기화와 API 호출에 사용했습니다. ngOnDestroy는 컴포넌트가 파괴되기 전에 호출됩니다. 메모리 누수를 방지하기 위해 구독을 해제하는 데 많이 활용했습니다.

이러한 메서드를 경력에서 여러 번 사용했습니다. ngOnChanges 메서드는 컴포넌트 생성 시 한 번 호출되고 그 후 입력 속성 중 하나에서 변경이 감지될 때마다 호출됩니다. SimpleChanges 객체를 매개변수로 받습니다. ngAfterViewInit는 컴포넌트 뷰 및 해당 하위 뷰가 초기화된 후 호출됩니다. ngAfterContentInit는 외부 컨텐츠(또는 부모로부터)가 초기화된 후에 호출됩니다.

<div class="content-ad"></div>

그 밖에도 ngDoCheck, ngAfterContentChecked, ngAfterViewChecked 같은 훅이 있지만, 잘 사용하지 않았어요.

## 의존성 주입이란 무엇인가요?

디자인 패턴과 연결지을 기회가 있다면, 꼭 활용해보세요. 답변에 자신이 시니어 또는 경험 많은 개발자임을 암시하는 추가 정보를 더해보세요.

여기 제 답변입니다:

<div class="content-ad"></div>

의존성 주입(Dependency Injection, DI)은 디자인 패턴입니다. Angular에서 DI는 활발하게 사용됩니다. DI는 컴포넌트 의존성을 외부 소스에서 주입하여 컴포넌트 자체에서 생성하는 대신 관리하는 디자인 패턴입니다. 이는 테스트 용이성을 향상시키고 컴포넌트 간 결합을 느슨하게 합니다.

기본적으로 Angular의 DI 시스템은 서비스의 싱글톤 인스턴스를 생성하고 응용 프로그램 전반에 걸쳐 공유합니다. AngularJs에서는 함수 매개변수가 DI에 사용되었지만, Angular 2부터는 생성자 매개변수가 DI에 사용됩니다.

## Angular에 대한 경험이 어떻게 되세요?

이 질문을 몇 차례 받았습니다. 7년 정도로 답변한 적은 없지만 면접관의 주의를 끌기 위해 정보를 제공하려고 노력했습니다.

<div class="content-ad"></div>

여기 있습니다:

안녕하세요! 저는 2017년부터 Angular 2를 사용하기 시작했습니다. Angular 2는 2016년 9월에 출시되었어요. 그 이후로 Angular 팀은 매 6개월마다 새로운 버전을 발표하기 시작했어요. Angular 5에서 httpClient를 소개했고, Angular 8에서는 번들 크기를 줄이기 위해 Ivy 엔진을 도입했어요. Ivy 엔진은 Angular 9에서 기본으로 사용되었습니다. 그리고 Angular 15에서는 독립 컴포넌트를 소개했고, Angular 17에서는 이를 기본으로 만들었어요. Angular 17에서는 내장 제어 흐름, 보류 가능한 뷰, Angular 시그널 등을 소개했답니다.

**유럽 및 미국 클라이언트에 대한 서로 다른 디자인 요구 사항이 있을 때, 디자인과 컴포넌트의 70%가 공통으로 사용될 때, 각각 유럽과 미국에 별도의 서버가 존재하고 따로 배포한다면 이 상황을 어떻게 처리할 것인가요?**

이 상황을 다양한 방법으로 해결할 수 있어요. 유럽과 미국을 처리하기 위해 if else를 사용할 수 있습니다.

<div class="content-ad"></div>

제 제안은 별도의 env 파일을 사용하는 것입니다. 미국을 위해 environment.usa.ts를, 유럽을 위해 environment.european.ts를 만들겠습니다. 앵귤러 애플리케이션을 프로덕션용으로 빌드할 때 빌드 시간에 환경 이름을 선택할 것입니다.

```js
ng build --prod --configuration=usa
```

또는

```js
ng build --prod --configuration=european
```

<div class="content-ad"></div>

해당 서버에 빌드 파일을 배포할 예정입니다.

인터뷰를 준비하기 전에 스스로 준비해야 할 몇 가지 일반적인 질문들이 있습니다.

- Angular란 무엇이며, Angular가 TypeScript를 사용하는 이유는 무엇인가요?
- Angular Material이란 무엇인가요?
- 지시자(directive)란 무엇이며, 지시자의 종류는 무엇이 있나요?
- Angular의 구성 요소는 무엇인가요?
- 의존성 주입(Dependency Injection, DI)이란 무엇인가요?
- 데이터 바인딩이란 무엇이며, 이를 구현할 수 있는 방법은 몇 가지인가요?
- Angular에서 다양한 필터의 종류를 설명해 주실 수 있나요?
- ViewEncapsulation이란 무엇이며, Angular에서는 이를 구현할 수 있는 방법이 몇 가지인가요?
- Angular에서 JavaScript 대신 TypeScript를 우선시하는 이유는 무엇인가요?
- RouterOutlet과 RouterLink에 대해 설명해 주세요.
- 템플릿 내에서 스크립트 태그를 사용할 때 무엇이 발생하나요?
- ViewChild가 무엇이며, 'static: false'를 사용하려면 왜 필요한가요?
- 컴포넌트 간에 데이터를 공유하는 방법은 몇 가지인가요?
- Angular 라이프사이클 훅은 무엇인가요?
- AOT 컴파일이란 무엇이며, AOT의 장점은 무엇인가요?
- Angular에서 "sourceMap": true는 무엇을 의미하는가?
- RxJS는 무엇인가요?
- Promise와 Obserable의 차이는 무엇인가요?
- 템플릿과 반응형 폼은 무엇인가요?
- Angular에서 Forroot와 Childroot는 무엇인가요?
- Angular에서 여러 개의 http 요청을 처리하는 방법은 무엇인가요?
- Map vs mergeMap vs switchMap vs concatMap의 차이는 무엇인가요?
- 클래스 데코레이터(Class decorators)는 무엇인가요?
- Angular에서 Component Decorator는 무엇인가요?
- 번들 분석(Bundle Analysis)이란 무엇인가요?
- Put와 Patch를 언제 사용해야 하나요?
- Angular.json 파일의 목적은 무엇인가요?
- Angular 17의 새로운 기능들은 무엇인가요?
- 표준 Angular 컴포넌트와 독립형 컴포넌트(standalone component)의 주요 차이점은 무엇인가요?
- Angular에서 bootstrapModule이란 무엇인가요?
- Angular 테스팅 프레임워크는 무엇인가요?
- Resolver를 사용하여 데이터 사전로드(pre-fetch)하는 방법은 무엇인가요?
- Angular에서의 가드(Guard)는 무엇인가요?
- Host binding과 Host listening은 무엇인가요?
- Angular에서의 폴리필(Polyfill)은 무엇인가요?
- Angular에서의 Router Outlet은 무엇인가요?
- 여러 개의 라우터 아웃렛을 사용할 수 있나요?
- 생성자 없이 컴포넌트를 작성할 수 있나요?
- 순수 파이프와 불순 파이프의 차이는 무엇인가요?
- Angular에서의 Formbuilder와 Formgroup의 차이는 무엇인가요?
- Angular에서의 뷰 캡슐화(View encapsulation)는 무엇인가요?

요약하면, 답변을 설명하려고 노력했으니 참고하여 답변을 준비해 보세요. 행운을 빕니다.