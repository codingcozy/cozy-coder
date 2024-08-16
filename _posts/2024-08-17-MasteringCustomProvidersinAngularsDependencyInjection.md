---
title: "Angular의 의존성 주입Custom Providers 마스터하기 "
description: ""
coverImage: "/assets/img/2024-08-17-MasteringCustomProvidersinAngularsDependencyInjection_0.png"
date: 2024-08-17 00:55
ogImage: 
  url: /assets/img/2024-08-17-MasteringCustomProvidersinAngularsDependencyInjection_0.png
tag: Tech
originalTitle: "Mastering Custom Providers in Angulars Dependency Injection "
link: "https://medium.com/gitconnected/mastering-custom-providers-in-angulars-dependency-injection-8fa7dfa12d4f"
isUpdated: false
---



<img src="/assets/img/2024-08-17-MasteringCustomProvidersinAngularsDependencyInjection_0.png" />

# 소개 📝:

Angular의 종속성 주입에서 사용자 정의 프로바이더를 숙달하는 블로그에 오신 것을 환영합니다! 오늘은 Angular에서 사용자 정의 프로바이더의 개념을 간단하게 설명하여 프로젝트에서 쉽게 이해하고 활용할 수 있도록 도와드리겠습니다. Angular의 종속성 주입 시스템에서 사용자 정의 프로바이더의 힘을 발휘해 보세요.

# Angular에서 프로바이더 이해하기 💡💡

<div class="content-ad"></div>

앵귤러에서는 프로바이더를 사용하여 의존성이 어떻게 인스턴스화될지 정의합니다. 프로바이더는 어떤 객체나 값이든 될 수 있지만 대부분의 경우, 서비스나 기능을 제공하는 클래스입니다.

## 코드 예시:

```js
import { Injectable } from '@angular/core';
// 일반 서비스 정의
@Injectable()
export class MyService {
  // 서비스 로직은 여기에 작성
}
// 모듈 프로바이더 배열에서 사용
@NgModule({
  providers: [MyService]
})
export class AppModule {
```

# 커스텀 프로바이더 필요성 🔧

<div class="content-ad"></div>

Angular의 사용자 정의 제공자는 개발자가 서비스를 제공하고 인스턴스화하는 방법을 정의할 수 있어 유연성을 제공합니다. 이를 통해 모듈성, 고급 의존성 주입 패턴 및 최적화된 서비스 인스턴스화가 가능해져 더 깨끗하고 효과적인 Angular 애플리케이션을 개발할 수 있습니다.

# 사용자 정의 제공자 생성

1- Factory 제공자 📕

팩토리 제공자를 사용하면 팩토리 함수를 사용하여 의존성의 인스턴스를 생성하고 구성할 수 있습니다.

<div class="content-ad"></div>

```js
// logger.service.ts
export class LoggerService {
  constructor(private prefix: string) {}
  
  log(message: string): void {
    console.log(`${this.prefix}: ${message}`);
  }
}

// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { LoggerService } from './logger.service';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [
    {
      provide: LoggerService,
      useFactory: () => {
        const prefix = 'MyApp'; // 환경 또는 다른 조건에 따라 이 값을 설정할 수 있습니다
        return new LoggerService(prefix);
      }
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }


// app.component.html 
<h1>Logger Service Test</h1>
<p>로그 출력을 확인하세요.</p>

// app.component.ts 

....
  ngOnInit() {
    this.logger.log('AppComponent에서 안녕하세요!');
  }
```

## 설명 💻:

- 이 예시에서는 factory 함수를 사용하여 LoggerService를 생성하고 접두사를 설정합니다. 이 접두사는 환경 변수, 사용자 설정 또는 다른 실행 시 데이터에 따라 달라질 수 있습니다

결과:

<div class="content-ad"></div>


![Image](/assets/img/2024-08-17-MasteringCustomProvidersinAngularsDependencyInjection_1.png)

## 2-Value Providers 📕:

Value providers allow you to provide a specific value or object that can be injected into your components or services.

```js
// app.config.ts

// 애플리케이션 구성을 위한 인터페이스 정의
// 이를 통해 구성 객체가 특정 구조를 준수하도록 보장합니다.
export interface AppConfig {
  apiUrl: string;
  featureFlag: boolean;
}

// AppConfig를 위한 InjectionToken 생성
// InjectionToken은 값을 또는 객체를 주입하는 데 사용할 수 있는 고유 토큰을 만드는 방법입니다.
// 'app.config' 문자열은 DI 시스템에서 이 특정 토큰을 식별하는 데 사용됩니다.
export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');

// AppConfig 인터페이스를 준수하는 구성 객체 정의
// 이 객체는 구성 요소나 서비스에 주입하려는 실제 구성 값을 보유합니다.
const appConfig: AppConfig = {
  apiUrl: 'https://api.example.com',  // API 요청을 위한 기본 URL
  featureFlag: true                   // 기능 토글 플래그 (기능 활성화/비활성화)
};

// app.module.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { APP_CONFIG, appConfig } from './app.config'; // InjectionToken 및 구성 객체 가져오기

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [
    // APP_CONFIG 토큰에 appConfig 값을 제공합니다.
    // 이는 APP_CONFIG 토큰이 주입될 때마다 Angular가 appConfig 객체를 제공할 것을 의미합니다.
    { provide: APP_CONFIG, useValue: appConfig }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }


@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'angular_custom_providers';

  constructor(@Inject(APP_CONFIG) public config: AppConfig, private logger: LoggingService) {}

}

// app.component.html 
<h1>Application Configuration</h1>
<p>API URL: { config.apiUrl }</p>
<p>Feature Flag: { config.featureFlag }</p>
```


<div class="content-ad"></div>

코드-설명 💻:

이 예시에서, APP_CONFIG은 구성 객체를 제공하는 데 사용된 InjectionToken입니다. useValue 옵션은 주입할 값입니다.

결과:

<img src="/assets/img/2024-08-17-MasteringCustomProvidersinAngularsDependencyInjection_2.png" />

<div class="content-ad"></div>

# 참고 자료 📗:

- [Angular Providers](https://dev.to/angular/a-practical-guide-to-providers-in-angular-3c96)
- [Inject custom provider in Angular 2+](https://stackoverflow.com/questions/39721884/inject-custom-provider-in-angular-2-0-1)
- [NestJS Custom Providers](https://docs.nestjs.com/fundamentals/custom-providers)
- [Angular Architecture Services](https://v17.angular.io/guide/architecture-services)

# 추가 자료 📗:

Angular의 고급 주제에 대해 더 알아보고 싶다면, 다음 블로그를 확인해보세요.

<div class="content-ad"></div>

- Angular 단위 테스트를 더 빨리 실행하는 방법
- 다음 단계로 Angular 번들 크기 최적화하기
- Angular을 사용할 때 피해야 할 나쁜 관행
- Ngrx와 Angular로 상태 관리 소개
- Angular과 Cypress로 CRUD E2E 테스트 작성

# 결론:

마지막으로, 일반 프로바이더는 서비스를 직접 제공하는 데 간단함을 제공하지만, 커스텀 프로바이더는 Angular 애플리케이션에서 서비스 인스턴스화에 대한 유연성과 제어를 제공합니다.

이 블로그의 소스 코드는 GitHub에서 찾을 수 있습니다.

<div class="content-ad"></div>

# 마지막으로 ✌️:

만약 이 포스트가 마음에 드시면 👏을 남겨 주시고, 트위터에서 제 계정을 팔로우해 주세요.

또한 제게 커피 한 잔 ☕️도 사주세요.

이 링크로 가입하면 수수료 중 일부가 직접적으로 저를 지원해줄 것입니다. 여러분에겐 추가 비용이 발생하지 않아요. 도움 주시면 감사하겠습니다!