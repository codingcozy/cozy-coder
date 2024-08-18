---
title: "Angular 15 Material에서 Date Range Picker를 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowToAddDateRangePickerInAngular15Material_0.png"
date: 2024-07-09 10:35
ogImage:
  url: /assets/img/2024-07-09-HowToAddDateRangePickerInAngular15Material_0.png
tag: Tech
originalTitle: "How To Add Date Range Picker In Angular 15 Material"
link: "https://medium.com/@websolutionstuff/how-to-add-date-range-picker-in-angular-15-material-3596ff0f9033"
isUpdated: true
---

이 튜토리얼에서는 Angular Material을 사용하여 Angular 15 애플리케이션에 날짜 범위 선택기 구성 요소를 추가하는 과정을 안내해 드릴 겁니다.

날짜 범위 선택기는 사용자가 편리하게 날짜 범위를 선택할 수 있는 강력한 도구로, 이벤트 일정, 예약 시스템 및 데이터 분석 도구와 같이 날짜 기반 작업이 포함된 애플리케이션에 이상적입니다.

Angular Material의 기능을 활용하여 날짜 범위 선택기를 애플리케이션에 신속하게 통합하고 사용자 경험을 향상시킬 수 있습니다.

사용자가 단일 날짜 대신 날짜 범위를 선택할 수 있기를 원한다면 mat-date-range-input 및 mat-date-range-picker 구성 요소를 사용할 수 있습니다.

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

이 튜토리얼을 통해 Angular 15 애플리케이션에 날짜 범위 선택기 컴포넌트를 점진적으로 통합하는 방법을 알아보겠습니다. 설치, 모듈 임포트, 컴포넌트 구현, 사용자 정의 옵션, 이벤트 처리 및 스타일링 기술에 대해 다룰 것입니다.

목차:

- 필요 사항
- Angular Material 설치
- Angular Material 모듈 임포트
- 날짜 범위 선택기 컴포넌트 추가
- 날짜 범위 선택기 옵션 사용자 정의
- 날짜 범위 선택 처리
- Angular 애플리케이션 실행
- 결론

이 튜토리얼을 완료하면 Angular 15 애플리케이션에서 Angular Material을 사용하여 날짜 범위 선택기를 통합하는 방법에 대해 충분히 이해하게 될 것입니다.

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

자, 이제 우리는 Angular 15에서 Angular Material을 사용하여 기능이 풍부하고 직관적인 날짜 범위 선택기 구성 요소로 애플리케이션을 향상시키는 방법을 탐색해 보겠습니다.

단계 1: Prerequisites

시작하기 전에, 컴퓨터에 작동하는 Angular 15 애플리케이션이 설정되어 있는지 확인하세요. 또한, Angular Material 및 해당 CSS 스타일을 포함한 필요한 종속성이 설치되어 있는지 확인하세요.

단계 2: Angular Material 설치

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

프로젝트에 Angular Material을 추가하려면 터미널에서 다음 명령을 실행하세요:

```js
npm install @angular/material@15 @angular/cdk@15
```

참고: Angular 15 Material에서 DatePicker를 추가하는 방법

단계 3: Angular Material 모듈 가져오기

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

지금부터는 애플리케이션의 모듈 파일인 app.module.ts에서 필요한 Angular Material 모듈을 가져오겠습니다:

```js
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { AppComponent } from "./app.component";
import { BrowserAnimationsModule } from "@angular/platform-browser/animations";
import { MatDatepickerModule } from "@angular/material/datepicker";
import { MatNativeDateModule } from "@angular/material/core";
import { MatFormFieldModule } from "@angular/material/form-field";
import { ReactiveFormsModule } from "@angular/forms";

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    MatDatepickerModule,
    MatNativeDateModule,
    MatFormFieldModule,
    ReactiveFormsModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

또한 필요한 Angular Material CDK 모듈도 가져온 것을 확인하세요.

Step 4: Datepicker 컴포넌트 추가하기

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

원하는 컴포넌트 템플릿에 날짜 범위 선택기 컴포넌트를 추가해보세요.

src/app/app.component.html

```js
<div class="container">
<h1>Angular 15 Material에 날짜 범위 선택기 추가하는 방법 - Websolutionstuff</h1>

<mat-form-field appearance="fill">
 <mat-label>날짜 범위를 입력하세요</mat-label>
 <mat-date-range-input [rangePicker]="picker">
  <input matStartDate placeholder="시작 날짜">
  <input matEndDate placeholder="종료 날짜">
 </mat-date-range-input>
 <mat-hint>MM/DD/YYYY – MM/DD/YYYY</mat-hint>
 <mat-datepicker-toggle matIconSuffix [for]="picker"></mat-datepicker-toggle>
 <mat-date-range-picker #picker></mat-date-range-picker>
</mat-form-field>
</div>
```

결과:

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

<img src="/assets/img/2024-07-09-HowToAddDateRangePickerInAngular15Material_0.png" />

Step 5: 수정 가능한 날짜 범위 선택기 옵션

날짜 범위 선택기를 사용자 정의하려면 옵션을 구성하면 됩니다. 예를 들어, 최소 및 최대 선택 가능한 날짜를 설정할 수 있습니다:

```js
<mat-date-range-picker [startAt]="startDate" [endAt]="endDate"></mat-date-range-picker>
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

여러분의 컴포넌트 파일에서 시작 날짜와 종료 날짜를 정의하세요:

```js
startDate: Date;
endDate: Date;
```

```js
constructor() {
  this.startDate = new Date(2023, 0, 1);
  this.endDate = new Date(2023, 11, 31);
}
```

단계 6: 날짜 범위 선택 처리

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

선택된 날짜 범위를 캡처하려면 (dateChange) 이벤트에 바인딩할 수 있습니다:

```js
<mat-date-range-picker (dateChange)="onDateRangeSelected($event)"></mat-date-range-picker>
```

7단계: Angular 애플리케이션 실행

ng serve 명령을 사용하여 Angular 개발 서버를 시작하고 브라우저에서 앱을 엽니다.

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
ng serve
```

단계 8: 결론

이 단계별 안내를 따라하면 Angular 15 애플리케이션에 Angular Material을 사용하여 날짜 범위 선택기 구성 요소를 추가하는 방법을 배울 수 있습니다.

날짜 범위 선택기의 사용자 정의 옵션, 이벤트 처리 및 스타일링 기술을 탐색했습니다. 이 지식을 바탕으로 사용자가 날짜 범위를 손쉽게 선택할 수 있도록 응용 프로그램을 향상시킬 수 있습니다.

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

날짜 범위 선택 컴포넌트는 예약 시스템, 이벤트 캘린더, 데이터 분석 도구와 같이 날짜 범위 선택이 필요한 애플리케이션에 상당한 가치를 더합니다.

또한 다음 내용도 확인해보세요:

- 관련 글: Angular 13에서 Bootstrap 모달
