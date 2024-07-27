---
title: "Angular 애플리케이션에서 파일과 폴더를 체계적으로 정리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_0.png"
date: 2024-07-10 00:46
ogImage: 
  url: /assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_0.png
tag: Tech
originalTitle: "How to organize files and folders in Angular applications?"
link: "https://medium.com/angular-training/how-to-organize-files-and-folders-in-angular-applications-21ab9bfb237b"
---


<img src="/assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_0.png" />

지난 몇 주 동안 몇 번 이 질문을 들었어요. 그래서 관련 블로그 포스트를 작성하려고 합니다. 처음에는 이 질문을 "당신이 좋아하는 색은 무엇인가요?"와 비슷하게 생각했어요. 왜냐하면 저는 모든 사람과 개발팀마다 각자의 의견과 선호도가 있을 것이라고 믿기 때문이에요.

하지만 그 질문이 나오는 이유는 Angular가 어떻게 작동하는지에 대한 오해 때문일 수도 있겠다는 것을 깨달았어요. 게다가 Angular 스타일 가이드가 있어서 우리가 그에 대해 정보를 받아 결정을 내릴 수 있다는 것은 운이 좋은 일이에요.

여기 Q&A 형식으로 나의 의견이 있습니다:

<div class="content-ad"></div>

# Angular 프로젝트의 파일 및 폴더 수는 중요할까요?

Angular 컴파일러에게는 중요하지 않습니다. 열 개의 파일이 있든 10,000개의 파일이 있든, 컴파일된 Angular 프로젝트의 출력물은 동일합니다. HTML/CSS 파일이나 폴더 수가 늘어나지 않습니다. 대신, 다음과 같은 파일들이 생깁니다:

![파일 및 폴더 구성](/assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_1.png)

index.html 하나, main.js 하나, polyfills.js, 그리고 styles.css가 있을 뿐입니다. 파일 및 폴더의 수를 최적화해도 브라우저에서 애플리케이션이 더 빨리 실행되지는 않습니다. 왜냐하면 브라우저는 이러한 파일 및 폴더를 볼 수 없기 때문이죠.

<div class="content-ad"></div>

동료 코드 유지보수자 및 개발자 여러분들께, 1,000개의 파일이 담긴 단일 폴더는 관리하기 어렵습니다. 그리고 깊게 중첩된 폴더 구조 또한 마찬가지입니다. 이 문제를 해결하는 방법을 계속 읽어보세요.

![파일 및 폴더를 구성하는 방법](/assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_2.png)

# 파일을 폴더에 어떻게 그룹화해야 하나요?

Angular 스타일 가이드의 주요 아이디어는 기능별로 파일을 그룹화하는 것입니다. LoginComponent, LoginService, LoginHttpInterceptor 또는 guard가 있나요? 이들을 login 폴더에 넣어보세요.

<div class="content-ad"></div>

Angular 팀은 이 방법을 다음과 같이 정당화합니다:

만약 하나의 폴더에 너무 많은 파일이 생기면 어떻게 될까요? 이로 인해 우리는 다음 질문에 이르게 됩니다:

# 언제 새 폴더를 만들어야 하나요?

Angular 스타일 가이드에서 제공하는 이 규칙을 정말 좋아합니다:

<div class="content-ad"></div>

파일이 몇 개뿐인 폴더는 쉽게 검사하고 관리할 수 있습니다. 스타일 가이드는 다음과 같은 이유로 이 접근 방식을 정당화합니다:

그래서, 파일이 너무 많아 하나의 폴더를 넘치게 만들지 않기 위해 일정 숫자인 7, 9 또는 10을 넘어가면 하위 폴더를 만드는 것이 합리적입니다.

![이미지](/assets/img/2024-07-10-HowtoorganizefilesandfoldersinAngularapplications_3.png)

# 동일한 파일에 여러 클래스를 가져야 할까요?

<div class="content-ad"></div>

짧은 답은 "아니요"입니다. 하나의 "것"만 파일 당에 있는 것이 최선이라는 규칙이 있습니다. 예를 들어, 하나의 컴포넌트, 하나의 디렉티브 또는 하나의 서비스와 같이요. 그렇다고 해서, 그 파일에서만 사용되는 타입 또는 인터페이스를 정의한다면, 별도 파일을 만들기보다 같은 위치에서 정의하는 것이 합리적입니다.

그 주제로, 인터페이스가 타입 정보를 설명하는 데 클래스보다 더 나은 것이라는 것을 기억하세요. 왜냐하면 인터페이스는 (연합 타입과 같이) 어떠한 것으로 컴파일 되지 않기 때문에 코드를 더 크게 만들지 않으면서 코드에 타입 안정성을 제공합니다.

# ng-modules에 관해서는 어떻게 생각하세요?

모듈에 대해 물어볼 것이라는 걸 알고 있었어요. 대부분의 주요 라이브러리와 프레임워크(Angular, Angular Material, NgRx 등)들은 더 이상 NgModule을 사용하지 않고 독립된 것으로 전환 중이기 때문에, 같은 방향을 추천합니다.

<div class="content-ad"></div>

개발자 관점에서 모듈의 주요 목적은 지연로딩이었지만, 혼자서 컴포넌트가 더 세밀하게 (전체 모듈이 아닌 컴포넌트) 지연로딩되고 즉시 사용되지 않는 블록에서 사용될 수 있기 때문에 이 목적은 대부분 사라졌습니다.

결과적으로, 기능별로 것들을 모듈로 구성하는 경우, 기본 기능 폴더만 있으면 어플리케이션을 더 많은 보일러플레이트 코드와 문제 없이 구성할 수 있습니다 (누가 순환 모듈 의존성을 좋아합니까?). 모듈을 다른 사람들과 코드를 공유하기 위해 사용하는 경우, 독립적인 기능을 사용하면 더 세밀한 가져오기를 가능하게 하여 성능을 향상시킬 수 있습니다. 그리고 모듈을 지연로딩에 사용하는 경우, 독립된 컴포넌트나 미래 블록을 사용하는 것 보다 복잡하고 덜 세밀합니다.