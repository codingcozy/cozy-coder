---
title: "성능 좋은 Angular 애플리케이션 만들기 및 효율적인 유지 관리 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_0.png"
date: 2024-07-07 13:33
ogImage: 
  url: /assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_0.png
tag: Tech
originalTitle: "Guidelines for creating performant Angular applications and their efficient maintenance"
link: "https://medium.com/@patrik.horva90/guidelines-for-creating-performant-angular-applications-and-their-efficient-maintenance-6c7537bd56cf"
---


## 한 문서에서 모든 정보를 얻어보세요!

<img src="/assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_0.png" />

이 글에 관심을 가져 주셔서 감사합니다. 제 이름은 패트릭이고 중급 Angular 개발자입니다. 중형 및 대형 Angular 프로젝트를 작업하고 적극적으로 유지 보수하고 있습니다. 이 글의 첫 부분에서는 대형 Angular 프로젝트의 효율적인 유지 보수에 대해 논의하겠습니다. 이 글의 두 번째 부분에서는 성능에 중점을 두어 이러한 접근 방식이 최상인 이유를 데이터를 바탕으로 제시하겠습니다. 함께 알아봅시다!

## 도구를 알아봅시다

<div class="content-ad"></div>

많은 개발자가 최적화 도구를 무시하거나 알아보지 않는다는 점을 강조할 수 없습니다. 이는 그만큼 자신의 발을 쏘고 있는 것과도 같습니다. 효율적인 앱을 만들고 싶다면 액티브하게 성능을 프로파일링해야 합니다. 그것이 어떻게 동작하는지 테스트하는 것만으로는 충분하지 않습니다. 병목 현상과 문제점을 발견하기 위해 숫자와 통계 데이터가 필요합니다.

Angular 팀은 Angular 애플리케이션을 위한 디버깅 및 프로파일링 기능을 제공하는 놀라운 도구인 Angular DevTools를 제공했습니다. 이에 대해 문서에서 자세히 읽는 것을 권장합니다. 대형 Angular 애플리케이션을 만들고 유지할 때 DevTools를 알고 사용하는 것이 필수적입니다. 다수의 컴포넌트, 비동기 작업 및 복잡한 비즈니스 로직을 가지고 있다면 디버깅 및 유지 보수 과정이 훨씬 더 어려워질 것입니다.

## Typescript와 JSDoc — 둘 다 얻어보세요

TypeScript는 놀라운 기능을 제공하고 개발 경험을 향상시킵니다. 대형 Angular 애플리케이션에서는 코드나 외부 API에서 비롯된 많은 유형 및 인터페이스를 기대할 수 있습니다. 애플리케이션에서 사용할 모든 유형의 데이터를 선언하는 것이 중요합니다. 유형과 인터페이스 이름은 의미 있고 자명해야 합니다.

<div class="content-ad"></div>

코드는 종종 장기적인 자산이며 팀 구성원이 시간이 지남에 따라 바뀔 수 있습니다. 문서화를 통해 개발자가 떠나거나 다른 프로젝트로 이동할 때 코드베이스에 대한 지식이 손실되지 않도록 보장할 수 있습니다. JSDoc를 사용하여 종류, 사용 목적, 존재 이유 및 속성을 설명할 수 있습니다. 이렇게 함으로써 코드에서가 아닌 설명에서 기능과 의미를 탐색할 필요가 없게 됩니다. 예를 들어,

```js
/**
 * 인증된 사용자의 구독 유형
 * 사용자는 구독을 가질 수 있지만 필수는 아닙니다.
 */
export type Subscriptions = 'basic' | 'advanced' | 'premium' | null

/** 인증된 사용자에 대한 기본 데이터 */
export type User = {
 firstName: string;
 lastName: string;
 email: string;
 phone: string | null;
 avatar: string | null;
 subscription: Subscriptions
};

/**
 * 성공적인 인증 후 수신된 모든 데이터를 포함합니다.
 */
export type UserAuthentication = {
 /** 민감한 데이터에 액세스하는 데 필요한 JWT */
 accessToken: string;

 /** 인증된 사용자에 대한 기본 데이터 */
 userData: User;

 /** 
  * 만료 후 새로운 accessToken을 가져오는 데 필요합니다.
  */
 refreshToken: string;
}
```

코드에 JSDoc를 추가하면 IDE에서 설명이 준비됩니다. 마우스를 갖다 대면 준비된 문서를 읽을 수 있습니다. 아래 이미지에서 확인할 수 있습니다:

<div class="content-ad"></div>

한 가지 주의할 점은 TypeScript가 절대적인 유형 안전성을 보장하지 않는다는 것입니다. 변수에 할당된 유형은 외부 API에서 해당 데이터 유형이 수신될 것을 보장하지 않습니다. TypeScript를 매우 지능적인 린터로 생각할 수 있습니다. 유형이 일치하지 않거나 누락되었거나 구성에 따라 수행되지 않은 인스턴스를 잡아주는 데 도움이 됩니다.

## 외부 데이터 유효성 검사

애플리케이션이 손상되지 않도록 받은 데이터를 유효성 검사하는 것이 필수적입니다. 데이터 유형 유효성 검사를 위한 라이브러리 중 하나는 Zod.js입니다. Zod는 TypeScript 우선 스키마 선언 및 유효성 검사 라이브러리입니다. 여기서 "스키마"라는 용어는 간단한 문자열부터 복잡한 중첩된 객체까지 모든 데이터 유형을 널리 지칭하는 것으로 사용합니다.

내 프로젝트에서는 항상 API 응답을 철저히 확인하고 유효성을 검사합니다. 백엔드의 검증되지 않은 변경 사항은 앱의 안정성에 심각한 영향을 미칠 수 있으므로 이 부분에 특별히 주의를 기울입니다. 백엔드 서비스는 사용 가능한 모든 엔드포인트를 명시하고 사용 방법에 대한 명확한 지침을 제공하는 API 문서와 함께 제공됩니다. 이 문서에는 클라이언트로 전송된 데이터를 보여주는 응답 예제도 포함되어 있습니다.

<div class="content-ad"></div>

저는 이러한 응답 예제들이 전송된 데이터와 일치하는 유형과 인터페이스를 생성하는 데 매우 도움이 된다고 생각해요. 이를 통해 일관성을 유지하는 데 도움이 되는 것뿐만 아니라 앱을 더 견고하게 만들어 줍니다. 아래의 흐름 다이어그램에서 이 프로세스를 요약해 봤어요:

![flow diagram](/assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_2.png)

## 디렉토리 구조 및 명명 표준

조직은 앱을 유지하는 데 중요한 요소입니다. 앱에서 구성 요소를 찾는 데 어려움을 겪는다면 코드 구조를 재고해야 합니다. 디렉토리, 컴포넌트, 서비스, 리졸버, 클래스, 인터페이스, 함수 및 변수는 논리적으로 위치해 있고 의미 있는 이름이어야 합니다. 이름은 자명해야 하며, 해당 요소가 무엇인지, 무엇을 나타내는지, 무슨 목적인지를 나타내야 합니다.

<div class="content-ad"></div>

예를 들어, 4개 이상의 페이지/컴포넌트에서 사용되는 다중 입력 구성 요소가 포함된 등록 양식을 생각해보십시오. 이러한 구성 요소를 어떻게 명명해야합니까? 어디에서 선언해야합니까? 그들은 얼마나 복잡합니까? 그들의 목적은 무엇인가요?

```js
<app-form-container>
  <app-form-label value="Username" />
  <app-form-text [(username)]="username" />

  <app-form-label value="Email" />
  <app-form-email [(email)]="email"/>

  <app-form-label value="Birthday" />
  <app-form-date [(date)]="birthday"/>

  <app-form-label value="Password" />
  <app-form-password [(password)]="password"/>

  <app-form-label value="Confirm password" />
  <app-form-password [(password)]="passwordConfirm"/>

  <app-form-submit (submitClicked)="validateAndRegisterIfPossible()" />
</app-form-container>
```

이 문제를 살펴보는 한 가지 방법을 살펴봅시다:

- 이들은 컴포넌트입니다. "Components" 디렉토리에 속합니다.
- 태그에 "form"이 포함되어 있습니다. 모두 양식과 관련이 있으므로 이들을 "Components" 디렉토리 내에 있는 "Form" 디렉토리에 넣습니다.
- 이러한 컴포넌트는 모두 양식과 관련이 있으므로 "FormComponentsModule"이라는 모듈에 함께 넣을 수 있습니다. 양식 구성 요소가 필요한 곳에서 해당 모듈을 가져올 수 있습니다.
- 각 구성 요소에는 목적이 있습니다. 다른 구성 요소를 랩하고 응답성을 보장하는 "app-form-container"가 있습니다. "app-form-label"은 레이블을 표시합니다. "app-form-email"은 적절한 형식과 휴대폰의 키보드 구성에서 이메일 주소를 입력하도록 설계되었습니다. "app-form-password"는 비밀번호를 입력받고 휴대폰의 키보드 구성과 적절한 형식으로 비밀번호의 가시성을 전환할 수 있습니다.

<div class="content-ad"></div>

또 다른 예시: 데이터를 그래프로 표시해야 하는 여러 페이지가 있습니다. 다양한 종류의 그래프가 있습니다. 다음 이미지는 좋은 컴포넌트 명명과 구조를 보여줍니다:

![그래프 구조](/assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_3.png)

## 메소드 데코레이터

대규모 Angular 프로젝트에서 작업하거나 구축할 때 버그는 피할 수 없습니다. 각 변경, 버그 수정 또는 업데이트 후에는 앱을 테스트해야 합니다. 언제든지 버그나 오류가 발견되지 않거나 잡힐 수 없다는 가능성이 항상 존재합니다.

<div class="content-ad"></div>

메소드 데코레이터는 기능적으로 아주 창의적일 수 있어요. 함수 실행을 로깅하는 가장 간단한 데코레이터 중 하나를 사용하면 됩니다.

```js
export class ExampleComponent { 
  @logFunctionExecution
  protected wrappedFunction() {
    // 코드
  }
}

function logFunctionExecution(target: any, key: string, descriptor: PropertyDescriptor) {
  console.log(`${key}() 실행 중`);
}
```

에러 처리와 로깅을 위한 데코레이터 함수를 작성해보겠습니다. 메소드 실행 중에 발생하는 에러를 잡을 수 있도록 해야 해요. 에러가 발생할 때마다 사용자 인터페이스에 표시하고 콘솔에 기록해야 합니다.

```js
export function logError(methodName: string, page: string) {
    return function (_target: any, _propertyKey: string, descriptor: PropertyDescriptor) {
        const originalMethod = descriptor.value;
        descriptor.value = function (...args: any[]) {
            try {
                const result = originalMethod.apply(this, args);
                return result;
            } catch (error) {
                try {
                    console.log(error)
                    displayError(error);
                    sendErrorToServer(serializeError(error, { maxDepth: 5, useToJSON: true }), methodName, page);
                } catch (error) {
                    console.log(error);
                }
            }
        };
        return descriptor;
    }
}
```

<div class="content-ad"></div>

이제 데코레이터 함수를 클래스 메서드에 적용하세요.

```js
export class ExampleComponent {
@logFunctionExecution("wrappedFunctionOne", "ExampleComponent")
  protected wrappedFunctionOne() {
    // code
  }
  @logFunctionExecution("wrappedFunctionTwo", "ExampleComponent")
  protected wrappedFunctionTwo() {
    // code
  }
  @logFunctionExecution("wrappedFunctionThree", "ExampleComponent")
  protected wrappedFunctionThree() {
    // code
  }
}
```

이제 오류가 발생할 때마다 오류를 기록하고 UI에 표시하세요.

참고: "sendErrorToServer" 함수를 발견했다면 알아두세요. 이것은 예를 들어, Ionic + Angular 및 Capacitor를 사용하여 웹 앱을 모바일 장치에 래핑할 때 필요합니다. 테스트 중에는 콘솔 로그에 액세스할 수 없으므로 오류를 serialize-error 패키지로 직렬화하고 백엔드로 보내서 추적할 수 있도록 하는 것이 솔루션 중 하나입니다.

<div class="content-ad"></div>

메서드 데코레이터는 유용하지만 과용하지 마세요. 하나의 클래스 메서드에 8개의 데코레이터를 적용하는 것은 안 좋은 습관이에요. 이곳에는 Typescript 데코레이터에 대한 훌륭한 실용적인 안내서가 있어요.

# Angular 앱 성능

<img src="https://miro.medium.com/v2/resize:fit:960/1*8LGyrnsnX3jxYmP8TgZA_Q.gif" />

## 변경 감지(change detection)가 어떻게 작동하는지 알고 계신가요?

<div class="content-ad"></div>

변경 감지는 Angular가 응용 프로그램 상태가 변경되었는지 확인하고 DOM을 업데이트해야 하는지 확인하는 프로세스입니다. 성능과 관련하여 매우 중요합니다. 고수준에서 Angular는 구성 요소를 위에서 아래로 이동하면서 변경 사항을 찾습니다. Angular는 주기적으로 변경 감지 메커니즘을 실행하여 데이터 모델의 변경 사항이 응용 프로그램 뷰에 반영되도록 합니다. 변경 감지는 수동으로 트리거하거나 비동기 이벤트(예: 사용자 상호 작용 또는 XMLHttpRequest 완료)를 통해 트리거할 수 있습니다. 변경 감지는 매우 최적화되어 있고 성능이 우수하지만 응용 프로그램이 빈번하게 실행한다면 여전히 지연이 발생할 수 있습니다.

## OnPush 변경 감지 전략

"OnPush" 변경 감지 전략은 구성 요소를 효율적으로 만들기 위해 변경 감지 확인 빈도를 줄이는 방법입니다. 구성 요소를 "OnPush"로 구성하면 Angular는 다음 시나리오에서만 변경 사항을 확인합니다.

- 구성 요소의 입력 속성이 변경되었을 때.
- 구성 요소가 명시적으로 ChangeDetectorRef 서비스를 통해 확인을 요청했을 때.
- 구성 요소나 그 자식 중 하나에서 이벤트가 발생하고 처리됐을 때(예: 버튼 클릭).

<div class="content-ad"></div>

다시 말해, “OnPush”는 컴포넌트의 출력이 입력과 자체 상태에만 의존한다고 가정합니다. 이러한 값이 변경되지 않으면 Angular은 해당 컴포넌트와 해당 자식들에 대한 변경 감지를 건너뛰어 성능을 향상시킵니다.

```js
import { Component, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: 'example.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class ExampleComponent {

}
```

## for 루프에서의 “trackBy”

“trackBy” 함수는 *ngFor 구조 지시자와 @for 블록과 함께 사용됩니다. *ngFor 구조 지시자를 사용할 때는 trackBy 함수가 필수가 아니지만, @for 블록에서는 필수입니다.

<div class="content-ad"></div>

for loop을 사용하여 항목 목록을 렌더링 할 때, Angular는 기본 추적 메커니즘을 사용합니다. 이 메커니즘은 객체 식별을 기반으로 합니다. 그러나 동적 목록이나 추가, 삭제 또는 재정렬될 수 있는 항목과 같은 경우에는 이 기본 메커니즘이 최적의 성능 제공에 부족할 수 있습니다. trackBy 함수를 사용하면 목록의 개별 항목을 식별하고 추적하는 사용자 정의 방법을 제공할 수 있습니다.

## 예제로 살펴보기

네 가지 경우에서 변경 감지 속도를 비교해보겠습니다.

- 10,000개의 항목을 준비하는 NewsService를 작성할 것입니다. 비동기로 해당 항목을 반환할 함수들이 있을 것입니다.

<div class="content-ad"></div>

```js
import { Injectable } from '@angular/core';
import { NewsMinimalData } from '../Types/news-types';

@Injectable({
  providedIn: 'root'
})
export class NewsService {

  private newsList!: Array<NewsMinimalData>;

  constructor() { }

  public returnAllNews(): Promise<Array<NewsMinimalData>> {
    return new Promise((resolve) => {
      this.createNewsList();
      setTimeout(() => {
        resolve(this.newsList);
      }, 2000);
    });
  }

  private createNewsList() {
    this.newsList = [];
    for (let i = 1; i < 10001; i++) {
      this.newsList.push({
        id: i,
        title: "News title #" + i,
        description: "Short description for News title #" + i,
      });
    }
  }

}
```

2. Create simple NewsItem component

```js
import { Component, Input } from '@angular/core';
import { NewsMinimalData } from '../../Types/news-types';

@Component({
  selector: 'app-news-item',
  template: `
  <div>
   {news.id}, {news.title}
  </div>
  <div>
   {news.description}
  </div>
  `,
  styles: `
    :host {
   display: flex;
   align-items: center;
   justify-content: space-between;
   width: 100%;
   height: 30px;
   margin-bottom: 12px;
  }
`,
  standalone: true
})
export class NewsItemComponent {
  @Input({ required: true }) news!: NewsMinimalData;
}
```

3. Import FormsModule and NewsItem in AppModule. If you are using it standalone, add these to the imports array.

<div class="content-ad"></div>

4. 간단한 기능이 있는 기본 템플릿을 만듭니다.

```js
<button (click)="addElement()">요소 추가</button>
<button (click)="removeElement()">요소 삭제</button>
<br>
<br>
<label>데이터 변경</label>
<br>
<input placeholder="변경할 뉴스 ID" type="number" name="id" id="" [(ngModel)]="idToChange">
<br>
<input placeholder="선택한 뉴스의 새 제목" type="text" name="text" id="" [(ngModel)]="newText">
<br>
<button (click)="changeData()">데이터 변경</button>
<br>
<br>

<app-news-item *ngFor="let news of newsList" [news]="news" />

<!-- @for (news of newsList; track news.id) {
<app-news-item [news]="news" />
} -->
```

5. AppComponent에 메서드와 속성 추가

```js
import { Component, OnInit } from '@angular/core';
import { NewsService } from './Services/news.service';
import { NewsMinimalData } from './Types/news-types';

@Component({
  selector: 'app-root',
  templateUrl: './app-component.html',
  styles: []
})
export class AppComponent implements OnInit {

  protected newsList!: Array<NewsMinimalData>;
  protected idToChange!: number | null;
  protected newText!: string | null;

  constructor(private newsService: NewsService) {
  }

  ngOnInit() {
    this.idToChange = null;
    this.newText = null;
    this.newsList = [];
    this.prepareNews();
  }

  protected changeData() {
    let index = this.newsList.findIndex(item => item.id === this.idToChange);
    this.newsList[index] = { ...this.newsList[index], title: this.newText! };
  }

  protected addElement() {
    this.newsList.unshift({
      id: this.newsList.length + 1,
      title: '새 항목',
      description: '새 항목에 대한 설명'
    })
  }

  protected trackNews(i: number, e: NewsMinimalData) {
    return e.id;
  }

  protected removeElement() {
    this.newsList.shift();
  }

  private async prepareNews() {
    try {
      this.newsList = await this.newsService.returnAllNews();
    } catch (error) {
      console.log(error);
    }
  }

}
```

<div class="content-ad"></div>

이제 모든 것이 준비되었습니다. 우리는 Angular DevTools를 사용하여 데이터를 추출할 것입니다. 각 케이스에는 해당하는 비디오가 첨부될 것입니다.

장치 정보:

```js
.-----------.---------------------------------------------.
| OS        | Ubuntu 23.10 64-bit, Linux 6.5.0-14-generic |
:-----------+---------------------------------------------:
| Processor | AMD Ryzen™ 7 5700U                          |
:-----------+---------------------------------------------:
| RAM       | 16GB DDR4 SDRAM                             |
:-----------+---------------------------------------------:
| Graphics  | NVIDIA GeForce GTX 1650                     |
'-----------'---------------------------------------------'
```

테스트 과정:

<div class="content-ad"></div>

- 10000개의 요소가 있는 것을 증명하십시오.
- 요소 추가 시 변경 감지에 소요된 시간 확인
- 요소 제거 시 변경 감지에 소요된 시간 확인
- 5번째 요소의 데이터 변경 시 변경 감지에 소요된 시간 확인

CASE 1: ngFor를 OnPush 없이 사용하고 trackBy 함수를 사용하지 않는 경우

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*_QP-9HznHXFYlsAnUkm-Ow.gif)

CASE 2: ngFor를 OnPush로 사용하고 trackBy 함수를 사용하지 않는 경우

<div class="content-ad"></div>


![Image](https://miro.medium.com/v2/resize:fit:1400/1*SViy9zvMVg-XNiSeP8F7PA.gif)

CASE 3: ngFor with OnPush and with trackBy function

![Image](https://miro.medium.com/v2/resize:fit:1400/1*is0kc6ght1mb3EVvB9-fOA.gif)

CASE 4: @for with OnPush and with trackBy function


<div class="content-ad"></div>


![Extracted data](https://miro.medium.com/v2/resize:fit:1400/1*SeZRfYFg_Grl0L5iP1nu_A.gif)

Let's extract the data. The GIFs above show the recorded worst-case results, which are also presented in the table below:

![Table](/assets/img/2024-07-07-GuidelinesforcreatingperformantAngularapplicationsandtheirefficientmaintenance_4.png)

Conclusion from the data: Using @for with OnPush is the fastest way to detect changes.


<div class="content-ad"></div>

**참고:** trackBy 함수가 없을 때 목록에서 요소를 제거하거나 업데이트하는 작업이 느릴 수 있다는 가능성이 있습니다. 이것은 매우 흥미로운 사실이라고 생각했는데, 많은 테스트를 거친 결과 이는 매우 드문 경우였습니다. 요소를 제거하거나 업데이트하는 작업이 이전 케이스보다 약간 느렸습니다.

## runOutsideAngular

runOutsideAngular 메서드는 Angular 존 밖에서 함수를 실행하는 방법입니다. Angular 존은 응용 프로그램 상태의 변경 사항을 추적하고 필요할 때 변경 감지를 트리거하는 메커니즘입니다. Angular 존 바깥에서 함수를 실행함으로써 해당 함수 내에서 수행된 작업으로 인해 변경 감지가 트리거되는 것을 방지할 수 있습니다.

```js
import { Component, NgZone } from '@angular/core';

@Component({
  selector: 'app-example',
  template: '<button (click)="runOutsideAngularExample()">Run Outside Angular</button>',
})
export class ExampleComponent {
  constructor(private zone: NgZone) {}

  runOutsideAngularExample() {
    // Angular 존 바깥에서 작업 수행
    this.zone.runOutsideAngular(() => {
      // 여기서 수행되는 작업은 Angular 변경 감지를 트리거하지 않습니다
      console.log('Angular 존 바깥에서 실행 중');
    });
  }
}
```

<div class="content-ad"></div>

위 예시에서 버튼이 클릭되면 runOutsideAngularExample 메서드가 호출되며, zone.runOutsideAngular 함수 내부의 작업은 Angular의 변경 감지를 트리거하지 않습니다. 이겢 일은 Angular 애플리케이션 상태나 UI를 업데이트할 필요가 없는 작업에 유용하며, 특정 시나리오에서 성능을 향상시키는 데 도움이 될 수 있습니다.

## 지연 로딩

Angular에서의 지연 로딩은 애플리케이션의 특정 부분을 실제로 필요할 때까지 로드를 지연시키는 기술입니다. 사용자가 사이트를 방문할 때 전체 애플리케이션을 로드하는 대신 초기 뷰를 위한 필수 구성 요소만 로드하고, 추가 모듈이나 구성 요소는 일반적으로 사용자의 행동(특정 경로로 이동하는 등)에 의해 필요할 때 로드됩니다.

지연 로딩은 Angular의 라우터와 함께 자주 사용됩니다. 애플리케이션의 다른 부분에 대한 라우트를 정의하고, 사용자가 특정 경로로 이동할 때 관련 모듈이 지연로드되도록 설정합니다. 이는 라우트 정의에서 loadChildren 속성을 구성하여 달성됩니다.

<div class="content-ad"></div>

```js
상성: 루트 = [
  { 경로: '느긋한', loadChildren: () => import('./lazy/lazy.module').then(m => m.LazyModule) },
  // 다른 루트...
];
```

느긋한 로딩은 현재 화면에 필요한 기본 코드만로 초기 로딩 시간을 개선하여 대규모 애플리케이션의 성능을 향상시킵니다. 사용자가 앱과 상호 작용할 때 애플리케이션의 후속 부분이 비동기적으로 로드됩니다.

## 템플릿 표현식에서 함수 사용 피하기

일반적으로 템플릿 표현식에서 함수를 직접 사용하는 것을 피하는 것이 좋습니다. 이는 템플릿 표현식 안의 함수가 응용 프로그램의 성능과 유지 보수에 부정적인 영향을 미칠 수 있기 때문입니다.

<div class="content-ad"></div>

```js
@Component({
    selector: 'app-test',
    template: `
    {function()}
    `,
})
class TestComponent {

    protected function() {
        //logic here...
    }

}
```

각 변경 감지가 발생할 때마다 `function`이 실행됩니다. 이렇게 하면 변경 감지가 너무 자주 일어나면 성능에 영향을 미칠 수 있습니다. 동일한 컴포넌트의 개수가 많아지면 사용자 인터페이스가 느려질 수 있습니다. 템플릿에서 함수를 사용해야 한다면 성능을 최적화하기 위해 OnPush 변경 감지 전략을 사용하세요.

## Pipes

파이프를 사용하면 템플릿 내에서 데이터 변환을 선언적이고 가독성 있는 방식으로 표현할 수 있습니다. 이는 템플릿 코드의 가독성을 향상시키고 유지 관리하기 쉽게 만듭니다.


<div class="content-ad"></div>

프라이플들은 다른 컴포넌트와 템플릿 간에 재사용될 수 있습니다. 사용자 정의 파이프를 만든 후에는 필요한 곳 어디에서든 쉽게 적용할 수 있어 코드의 재사용성을 높일 수 있습니다. 그들은 변화 감지에 매우 효율적이며 입력 값이 변경될 때에만 값을 계산합니다. 

## RxJS를 피하지 마세요

RxJS 또는 JavaScript용 반응형 확장(Reactive Extensions)은 Observable을 사용하여 반응형 프로그래밍을 위한 라이브러리로서 비동기적이거나 콜백 기반의 코드를 조립하는 것을 더 쉽게 만듭니다. 반응형 프로그래밍은 변경 사항의 전파와 데이터 의존성의 선언에 초점을 맞춘 프로그래밍 접근 방식으로, 더 선언적이고 명령적이지 않은 방식으로 데이터를 다룹니다. RxJS는 비동기 데이터 스트림 작업에 작업하기 위한 API를 제공합니다. 핵심 개념은 Observable로, 데이터나 이벤트의 시간에 따라 방출될 수 있는 데이터 스트림을 나타냅니다. Observables은 RxJS가 제공하는 다양한 연산자를 사용하여 조작, 결합, 변환할 수 있습니다.

RxJS는 Angular와 함께 흔히 사용됩니다. 이는 이벤트를 관리하고 반응하며, 비동기 작업을 처리하고 응용 프로그램에서 데이터의 흐름을 간단화하는 데 도움이 됩니다. 반응형 프로그래밍의 원리를 활용하면 RxJS는 복잡한 비동기 시나리오를 처리하는 더 간결하고 모듈식이며 유지보수가 쉬운 코드를 작성할 수 있습니다.

<div class="content-ad"></div>

## 메모리 누수 방지

앵귤러 애플리케이션에서 메모리 누수는 객체에 대한 참조가 제대로 해제되지 않을 때 발생할 수 있으며, 메모리에 사용되지 않는 객체들이 누적되게 됩니다. 메모리 누수의 가장 흔한 방법은 Observables입니다. Observables 구독을 해제하는 것이 반드시 필요합니다!

```js
@Component({...})
export class VideoUploadComponent implements OnInit {
  protected videoSubject = new Subject();

  ngOnInit() {
    this.videoSubject.subscribe(videoData=> {
      this.updateInterface(videoData);
    });
  }

  uploadVideoFile(videoData: any) {
    // 비디오 데이터를 videoSubject로 전달
    this.videoSubject.next(videoData);
  }

  updateInterface(videoData: any) {
    // ...
  }

  ngOnDestroy() {
    this.videoSubject.unsubscribe();
  }
}
```

ngOnDestroy는 여기 있는 이유가 있습니다! 컴포넌트가 파괴되기 직전에 리소스를 정리하고 메모리를 해제하는 것은 완벽한 시기입니다. 메모리 누수는 성능 문제를 야기할 수 있습니다. 앱을 오래 사용할수록 가비지 콜렉터에 의해 할당 해제되지 않아 메모리가 부족해지는 가능성이 있습니다.

<div class="content-ad"></div>

## 서버 측 렌더링

앵귤러의 서버 측 렌더링(SSR)은 브라우저가 아니라 서버에서 앵귤러 애플리케이션을 렌더링하는 프로세스를 의미합니다. 앵귈러 애플리케이션의 전통적인 접근 방식은 클라이언트 측 렌더링(CSR)으로, 여기서 렌더링과 렌더링 로직이 브라우저에서 실행됩니다. SSR을 사용하면 일부 또는 전체 렌더링 프로세스가 콘텐츠가 클라이언트로 전송되기 전에 서버로 이동됩니다.

SSR의 주요 이점 중 하나는 초기 페이지 로드 성능이 향상된다는 것입니다. 서버에서 초기 HTML을 렌더링하면 클라이언트가 미리 렌더링된 페이지를 받아 볼 수 있어 사용자에게 더 빠르게 보일 수 있습니다.

## 프로덕션 구성

<div class="content-ad"></div>

앱을 배포하거나 업데이트하기 전에 angular.json 구성이 빌드할 때 production 옵션을 갖고 있는지 확인해 주세요.

“@angular-devkit/build-angular:application” 빌더를 위한 production 구성의 좋은 예시를 살펴봅시다:

```js
{
 "budgets": [{
 "type": "initial",
 "maximumWarning": "500kb",
 "maximumError": "1mb"
 },
 {
 "type": "anyComponentStyle",
 "maximumWarning": "2kb",
 "maximumError": "4kb"
 }
 ],
 "polyfills": ["src/polyfills.ts"],
 "outputHashing": "media",
 "optimization": true,
 "extractLicenses": false,
 "sourceMap": false,
 "namedChunks": false,
 "progress": true,
 "statsJson": true,
 "prerender": true,
 "ssr": true,
 "server": "src/main.server.ts",
 "serviceWorker": "ngsw-config.json",
 "aot": true
}
```

Angular의 AOT(Ahead-of-Time) 컴파일은 빌드 프로세스로, 빌드 단계 중에 Angular 애플리케이션 코드, 템플릿 및 컴포넌트를 효율적인 JavaScript 코드로 변환하는 과정입니다. AOT의 장점으로는 더 빠른 시작 시간, 더 작은 번들 크기 및 템플릿 오류 확인이 있습니다. AOT 컴파일을 사용함으로써, Angular 애플리케이션은 더 나은 성능, 더 빠른 로딩 시간 및 전체적인 효율성 향상을 얻을 수 있어, 제품 배포에는 권장되는 방법입니다.

<div class="content-ad"></div>

"angular.json" 파일의 "optimization" 키는 Angular CLI가 빌드 프로세스 중에 다양한 최적화를 수행할지를 제어하는 구성 옵션입니다. "true"로 설정되면, "optimization" 키는 스크립트와 스타일의 최소화, 트리 쉐이킹, 데드 코드 제거, 중요한 CSS 및 폰트를 인라인하는 등의 다양한 최적화를 유발합니다.

"sourceMap" 키는 디버깅 및 개발에만 필요하기 때문에 false로 설정해야 합니다.

구성 옵션은 여기에서 찾을 수 있습니다.

## 외부 라이브러리를 최소화하고 신중하게 선택해보세요

<div class="content-ad"></div>

Angular에는 다양한 컴포넌트 및 함수 라이브러리/패키지가 있지만 모두가 성능이 우수한 것은 아닙니다! 외부 라이브러리를 사용하려면 꼭 연구를 하고 필요에 맞는 최상의 것을 선택해야 합니다. 더 많은 외부 라이브러리는 애플리케이션의 크기를 증가시키는데, 이는 바람직하지 않습니다.

## 웹 워커의 힘을 활용하세요

웹 워커(Web Workers)는 웹 브라우저의 기능으로, 메인 브라우저 스레드와 별도로 JavaScript 코드를 백그라운드에서 병렬로 실행할 수 있게 합니다. 이를 통해 개발자들은 복잡한 계산이나 데이터 처리와 같은 시간이 오래 걸리는 작업을 사용자 인터페이스의 응답성에 영향을 주지 않고 실행할 수 있습니다. 웹 워커는 메시지 전달 메커니즘을 통해 메인 스레드와 통신하여 웹 애플리케이션에서 동시성과 성능을 향상시킵니다.

웹 브라우저의 메인 스레드는 사용자 인터페이스와 웹 페이지의 전반적인 기능과 관련된 여러 중요한 작업을 처리하는 역할을 합니다. 주요 책임은 UI 렌더링, JS 실행, 이벤트 처리, 레이아웃 및 스타일 계산, 네트워크 요청 등을 포함합니다.

<div class="content-ad"></div>

이건 오로지 한 스레드에 대해 많은 작업처럼 들립니다! Web Worker로 무거운 계산을 옮겨볼까요? 이 기사에서는 Web Worker의 장점과 사용 방법을 자세하게 설명하고 있습니다. Angular 앱의 구성 단계만 제공해 드리겠습니다:

- tsconfig.worker.json이라는 구성 파일을 생성해주세요. 이 파일은 tsconfig.json 파일과 동일한 디렉토리에 있어야 합니다. 제 경우에는 모든 워커 파일을 src/app/Workers 디렉토리에 추가했습니다.

```js
{
 "extends": "./tsconfig.json",
 "compilerOptions": {
  "outDir": "./out-tsc/worker",
  "lib": [
   "es2022",
   "webworker"
  ],
  "types": []
 },
 "exclude": [],
 "include": [
  "src/app/Workers/*.worker.ts"
 ]
}
```

2. angular.json 파일에 생성된 워커 구성 파일을 추가해주세요.

<div class="content-ad"></div>

```js
{
...
"projects": {
  "app": {
    ...
    "architect": {
      ...
      "build": {
        "options": {
           ...
           "webWorkerTsConfig": "tsconfig.worker.json",
           ...
        }
        ...
      }
      ...
    }
    ...
  }
}
...
}
```

3. app.component.ts 파일에서 워커를 초기화하세요.

```js
@Component({
...
})
export class AppComponent implements OnInit {
  protected worker: Worker;
  
  constructor() {}
  
  ngOnInit() {
      this.worker = new Worker(new URL(relative_path_to_worker_file, import.meta.url));
  
      this.worker.onmessage = (data) => {
        // 받은 데이터/결과를 처리하세요
      }
  
      this.worker.onerror = (err) => {
        console.lor(err);
      }
  
      this.worker.postMessage(379560249782563);
  }

}
```

## 페이지 애니메이션하기

<div class="content-ad"></div>

너도 프레임 드랍을 싫어해? 움직임이 부자연스럽게 보이는 걸 싫어해? 나도 그래. 함께 애니메이션을 최적화하는 방법을 알아보자!

브라우저 페인팅, 즉 렌더링 또는 리플로우는 웹 브라우저가 HTML, CSS 및 기타 자원을 사용자 화면에 시각적으로 변환하는 과정입니다. 이 프로세스에는 레이아웃, 페인트, 합성 등 여러 단계가 포함되어 있으며, 이는 브라우저 렌더링 파이프라인의 중요한 부분입니다. 레이아웃과 페인트라는 두 가지 주요 단계가 있습니다.

레이아웃 단계: 브라우저는 계산된 스타일을 기반으로 페이지의 각 요소의 위치와 크기를 계산합니다. 이 단계는 리플로우로도 알려져 있습니다. 레이아웃 프로세스는 화면에 요소가 어떻게 배치되는지를 결정합니다.

페인팅 단계: 레이아웃이 결정되면, 브라우저는 페인팅 단계를 거칩니다. 이 단계에서는 화면에 실제로 표시될 픽셀을 생성합니다. 각 가시적인 요소에 대해 픽셀을 채우는 작업이 필요하며, 색상, 배경, 테두리, 이미지 등과 같은 스타일을 고려합니다.

<div class="content-ad"></div>

복합 단계: 그린 요소들이 함께 병합되어 웹 페이지의 최종 시각적 표현물이 생성됩니다. 이는 적절한 순서로 레이어를 쌓고, 원하는 출력물을 만들기 위해 블렌딩하는 과정을 포함합니다.

스타일의 비효율적인 사용, 레이아웃 변경의 빈도, 또는 크고 복잡한 웹 페이지는 성능 문제와 렌더링 시간이 느려지는 원인이 될 수 있습니다. 브라우저 그림 그리기와 성능 최적화 관점에서, 일부 CSS 스타일은 "컴포지터 전용" 또는 "저렴한"으로 간주됩니다. 이 스타일은 일반적으로 화면 재배열이나 다시 그리기를 유발하지 않고 브라우저의 컴포지터에서 적용되며, 애니메이션과 전환에 보다 효율적입니다.

- 투명도
- 변형
- 필터
- 뒷면 가시성
- will-change

will-change 속성은 특정 속성이 애니메이션화되거나 이후에 변경될 가능성이 높음을 브라우저에 알려주는 표시입니다. 이를 통해 브라우저는 렌더링 파이프라인을 최적화할 수 있습니다. 과용하면 부정적인 성능 영향을 미칠 수 있습니다.

<div class="content-ad"></div>

절대 위치와 페이지 변환의 차이를 확인해보겠습니다. 설정 부분은 건너뛰고 애니메이션 코드만 추가하겠습니다. Google Chrome 브라우저의 렌더링 옵션에서 페인트 플래싱을 활성화할 것입니다. 플래싱이 적을수록 성능이 좋습니다.

절대 위치를 활용한 애니메이션:

```js
import {
 trigger,
 transition,
 style,
 query,
 group,
 animate,
 AnimationQueryOptions,
} from '@angular/animations';


export const slideToRight = () => {
 const optional: AnimationQueryOptions = { optional: true };
 return [
  style({ position: 'relative' }),
  query(':enter, :leave', [
   style({
    position: 'absolute',
    top: 0,
    left: 0,
    width: '100vw',
    height: '100vh'
   })
  ], optional),
  query(':enter', [
   style({
    position: 'absolute',
    top: 0,
    left: '100%',
    width: '100vw',
    height: '100vh'
   })
  ], optional),
  group([
   query(':leave', [
    animate('500ms ease', style({ left: '-100%' }))
   ], optional),
   query(':enter', [
    animate('500ms ease', style({ left: '0%' }))
   ], optional)
  ]),
 ];
};
```

<img src="https://miro.medium.com/v2/resize:fit:1400/1*ES_xImef-hwl2ki8UH7mLQ.gif" />

<div class="content-ad"></div>

이제 transform CSS 속성을 활용한 애니메이션을 확인해 보겠습니다:

```js
import {
 trigger,
 transition,
 style,
 query,
 group,
 animate,
 AnimationQueryOptions,
} from '@angular/animations';


export const slideToRight = () => {
 const optional: AnimationQueryOptions = { optional: true };
 return [
  style({ position: 'relative' }),
  query(':enter, :leave', [
   style({
    position: 'absolute',
    top: 0,
    left: 0,
    width: '100vw',
    height: '100vh'
   })
  ], optional),
  query(':enter', [
   style({ transform: 'translateX(100vw)' })
  ], optional),
  group([
   query(':leave', [
    animate('500ms ease', style({ transform: 'translateX(-100vw)', opacity: '0.75' }))
   ], optional),
   query(':enter', [
    animate('500ms ease', style({ transform: 'translateX(0)', opacity: '1' }))
   ], optional)
  ]),
 ];
};
```

<img src="https://miro.medium.com/v2/resize:fit:1400/1*XKitHvY2LX4cDjyBXKC3LA.gif" />

이들은 매우 간단한 애니메이션입니다. 이 페이지들에 많은 콘텐츠가 없습니다. 콘텐츠가 더 많고 애니메이션 복잡도가 증가할수록, 화면 페인팅 프로세스가 사용자 경험에 해를 끼칠 수 있습니다. 부드러운 애니메이션은 UX에 중요하므로 애니메이션을 "컴포지터 전용" 스타일로 구현해 보세요!

<div class="content-ad"></div>

여기까지입니다! 이 기사에서 새로운 것을 배우셨기를 바랍니다. 읽어 주셔서 감사합니다.