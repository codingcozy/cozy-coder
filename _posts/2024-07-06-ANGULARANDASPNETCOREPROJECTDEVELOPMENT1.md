---
title: "앵귤러Angular와 ASPNET Core 프로젝트 개발 첫걸음부터"
description: ""
coverImage: "/assets/img/2024-07-06-ANGULARANDASPNETCOREPROJECTDEVELOPMENT1_0.png"
date: 2024-07-06 11:04
ogImage:
  url: /assets/img/2024-07-06-ANGULARANDASPNETCOREPROJECTDEVELOPMENT1_0.png
tag: Tech
originalTitle: "ANGULAR AND ASP.NET CORE PROJECT DEVELOPMENT 1"
link: "https://medium.com/@ahmetbilgic81/angular-and-asp-net-core-project-development-1-44cbf3911708"
---

![](/assets/img/2024-07-06-ANGULARANDASPNETCOREPROJECTDEVELOPMENT1_0.png)

이 글에서는 Angular 프로젝트 개발에 대해 배우게 될 거에요.

제 새로운 Udemy 강좌가 개설되었다는 것을 기쁘게 알려드립니다!🎥🌟🚀 Angular 개발 플랫폼을 탐색하고 지식을 더욱 향상시켜보세요. 지금 바로 확인해보세요: https://www.udemy.com/course/angular-18-and-aspnet-80-project-development-for-beginners/

![](/assets/img/2024-07-06-ANGULARANDASPNETCOREPROJECTDEVELOPMENT1_1.png)

<div class="content-ad"></div>

Angular은 typescripts, html 및 css로 클라이언트 애플리케이션을 구축하는 프레임워크입니다.

Angular 프레임워크는 개발자들이 UI 요소를 걱정하기보다는 기능을 구현하는 데 더 많은 초점을 맞출 수 있도록 개발 프로세스를 가속화합니다.

또한 Angular는 웹 앱을 구축하는 구조화된 정리된 방법을 제공합니다. Angular를 사용하면 애플리케이션을 서로 다른 Model View Controller 구성 요소로 분리할 필요가 없습니다.

처음에는 Angular 폴더 구조를 살펴보겠습니다.

<div class="content-ad"></div>

앵귤러의 앱 폴더는 애플리케이션의 컴포넌트, 서비스, 모듈, 그리고 라우팅 구성을 담당하는 주요 컨테이너 역할을 합니다.

앱 폴더의 전형적인 구조

app/
├── app.module.ts

<div class="content-ad"></div>

- app.component.ts
- app.component.html
- app.component.css
- ...

<div class="content-ad"></div>

앵귤러 컴포넌트는 UI의 구성 요소입니다. 이들은 TypeScript 파일, 템플릿을 위한 HTML 파일 및 CSS(또는 다른 스타일) 파일로 구성됩니다. 이러한 컴포넌트는 보통 자신의 폴더에 정리되거나 기능이나 기능별로 그룹화됩니다.

app.component.ts, app.component.html 및 app.component.css 파일은 일반적으로 최상위 레이아웃 및 탐색을 유지하는 애플리케이션의 주요 컴포넌트를 정의합니다.

# app.module.ts

앵귤러의 app.module.ts 파일은 애플리케이션의 루트 모듈로 기능하며, 애플리케이션을 어떻게 조립할지에 대한 정보를 제공합니다. 이 파일은 모듈에 속하는 컴포넌트, 지시자, 파이프 및 서비스를 지정하고, 다른 모듈에 대한 종속성을 정의합니다.

<div class="content-ad"></div>

Imports: `imports` 배열은 이 모듈이 의존하는 다른 Angular 모듈을 지정합니다.

Declarations: `declarations` 배열은 이 모듈에 속하는 모든 컴포넌트, 디렉티브 및 파이프를 나열합니다. Angular는 이러한 구성요소에 대해 알아야 하며 애플리케이션에서 사용할 수 있도록 해야 합니다.

Providers: `providers` 배열에는 모듈이 Angular의 의존성 주입 시스템을 통해 다른 부분에 제공해야 하는 서비스가 포함됩니다.

Bootstrap: `bootstrap` 배열은 Angular가 어플리케이션을 시작할 때 부트스트랩해야 하는 루트 컴포넌트를 지정합니다. 이 컴포넌트는 일반적으로 애플리케이션의 주 레이아웃과 구조를 정의합니다.

<div class="content-ad"></div>

# app.component.ts

앵귤러 애플리케이션에서 app.component.ts는 일반적으로 AppComponent로 알려진 애플리케이션의 주요 컴포넌트를 정의하는 TypeScript 파일입니다. 이 컴포넌트는 앵귤러 애플리케이션의 루트 컴포넌트로 작동하며, 전체 애플리케이션의 구조와 동작의 시작점입니다. 일반적으로 앱의 주요 레이아웃, 내비게이션 및 다른 전역 요소를 보유합니다. 다른 컴포넌트의 렌더링을 조정하고 응용 프로그램의 라우팅 및 구조에 기반하여 기타 컴포넌트 간의 데이터 통신을 용이하게 할 수도 있습니다.

app.component.ts는 @angular/core에서 제공되는 @Component 데코레이터를 사용하여 AppComponent 클래스를 선언합니다. 이 데코레이터를 사용하면 컴포넌트에 대한 메타데이터를 지정할 수 있습니다.

메타데이터: @Component 데코레이터는 앵귤러가 컴포넌트를 생성하고 렌더링하기 위해 필요한 메타데이터를 제공합니다. 다음 속성을 포함합니다:

<div class="content-ad"></div>

- selector: 컴포넌트의 CSS 선택자를 지정합니다. 이 경우 'app-root'라는 것은 Angular가 HTML에서 `app-root`를 찾으면 AppComponent를 렌더링합니다.
- templateUrl(또는 template): 컴포넌트와 연결된 HTML 템플릿 파일을 지정합니다. 이 파일은 Angular의 템플릿 구문을 사용하여 컴포넌트의 UI 구조를 정의합니다.
- styleUrls(또는 styles): 컴포넌트의 템플릿에 적용할 하나 이상의 CSS 파일을 지정합니다.

또한, 컴포넌트의 동작과 상태를 정의하는 로직과 속성이 포함되어 있습니다. 일반적으로 다음과 같은 기능을 포함합니다:

- Properties: 서비스나 다른 컴포넌트에서 자주 가져오는 데이터를 사용하는 컴포넌트 속성입니다.
- Methods: 컴포넌트 내에서 작업을 수행하는 함수입니다.
- Lifecycle hooks: Angular이 컴포넌트의 라이프사이클의 특정 시점에 호출하는 ngOnInit, ngOnChanges 등의 메소드입니다.

# HTTP GET 방법

<div class="content-ad"></div>

Angular에서는 http.get 메소드를 사용하여 서버로 HTTP GET 요청을 보내 데이터를 가져옵니다.

다음과 같이 http get 메소드를 호출합니다:

```js
this.http.get < any > "https://api.example.com/data";
```

http.get 메소드를 사용하려면 다음 단계를 따라야 합니다:

<div class="content-ad"></div>

- 모듈 파일 (예: app.module.ts)에서 HttpClientModule를 가져왔는지 확인하세요. 이 모듈은 HTTP 요청을 보내는 데 사용되는 HttpClient 서비스를 제공합니다.
- HTTP 요청을 보내려는 컴포넌트나 서비스에 HttpClient 서비스를 주입하세요. 일반적으로 생성자를 통해 이 작업을 수행합니다.
- HttpClient의 get 메서드를 사용하여 지정된 URL로 GET 요청을 보냅니다. get 메서드는 HTTP 응답의 Observable을 반환하며, 이를 구독하여 데이터를 처리할 수 있습니다.
- subscribe 메서드 내부에서는 필요한대로 응답 데이터를 처리합니다. 이는 컴포넌트 속성 업데이트, 템플릿에서 데이터 렌더링 또는 검색된 데이터에 기반한 기타 작업 등을 포함할 수 있습니다.
- HTTP 요청 중 발생할 수 있는 오류를 처리합니다. catchError 연산자를 사용하여 오류 처리를 추가할 수 있습니다.

# HTTP POST 메서드

Angular에서는 HTTP POST 메서드를 사용하여 지정된 URL을 통해 서버로 데이터를 보냅니다.

우리는 다음과 같이 http post 메서드를 호출합니다:

<div class="content-ad"></div>

```js
// 데이터를 보낼 POST 요청을 보내려면 HttpClient의 post 메서드를 사용하세요.
this.http.post < any > ("https://api.example.com/post-endpoint", data);
```

지정된 URL로 데이터를 보내기 위해 HttpClient의 post 메서드를 사용하세요. post 메서드는 HTTP 응답의 Observable을 반환하며, 응답을 처리하기 위해 구독할 수 있습니다.

# HTTP PUT 방법

Angular에서는 기존 데이터를 업데이트하기 위해 서버로 HTTP PUT 요청을 보내기 위해 this.http.put을 사용합니다.

<div class="content-ad"></div>

다음과 같이 PUT 메서드를 호출합니다:

```js
this.http.put < any > ("https://api.example.com/update-endpoint", data);
```

put 메서드를 사용하여 HttpClient의 PUT 요청을 지정된 URL로 보내고 업데이트된 데이터와 함께 전송합니다. put 메서드는 HTTP 응답의 Observable을 반환하며, 응답을 처리하기 위해 구독할 수 있습니다.

# HTTP DELETE 메서드

<div class="content-ad"></div>

Angular에서는 이.http.delete을 사용하여 데이터 또는 리소스를 삭제하기 위해 서버로 HTTP DELETE 요청을 보냅니다.

아래와 같이 http delete 메서드를 호출합니다:

```js
this.http.delete < any > `https://api.example.com/delete-endpoint/${id}`;
```

HttpClient의 delete 메서드를 사용하여 지정된 URL로 DELETE 요청을 보냅니다. delete 메서드는 HTTP 응답의 Observable을 반환하며, 이를 구독하여 응답을 처리할 수 있습니다.

<div class="content-ad"></div>

요약하자면, Angular는 개발자가 쉽게 동적이고 확장 가능한 웹 애플리케이션을 구축할 수 있도록 하는 견고한 프레임워크로 빛을 발합니다. 이 글에서는 Angular 웹 애플리케이션을 개발하는 데 도움이 되는 여러 가지 주요 기능을 살펴보았습니다.

다음 글에서는 단계별로 Angular 애플리케이션을 개발해보겠습니다.
