---
title: "Nodejs를 사용한 Angular 인증 방법"
description: ""
coverImage: "/assets/img/2024-07-09-AuthenticationinAngularwithNodejs_0.png"
date: 2024-07-09 10:36
ogImage:
  url: /assets/img/2024-07-09-AuthenticationinAngularwithNodejs_0.png
tag: Tech
originalTitle: "Authentication in Angular with Node.js"
link: "https://medium.com/@meetspatel92/authentication-in-angular-with-node-js-8d97fb90a3d4"
isUpdated: true
---

이 글에서는 백엔드로 Node.js를 사용하여 Angular 애플리케이션에서 인증(가입 및 로그인)을 구현하는 과정을 안내할 것입니다. 또한 인증 가드를 사용하여 특정 경로를 보호하는 프라이빗 라우팅 설정 방법도 다룰 것입니다. 이 튜토리얼에는 데이터 플로우 다이어그램(DFD) 및 UML 다이어그램이 포함되어 아키텍처를 설명할 것입니다.

# 필수 준비 사항

- Angular와 Node.js의 기본 지식
- 로컬 머신에 MongoDB 설정이나 클라우드 MongoDB 인스턴스가 있어야 합니다.
- Node.js와 npm이 설치되어 있어야 합니다.
- Angular CLI

# Angular CLI를 위한 기본 명령어

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
Angular CLI 설치 방법 - npm install -g @angular/cli
기본 Angular 프로젝트 생성 방법 - ng new "프로젝트명"
프로젝트 빌드 및 실행(프로젝트 디렉토리 내에서 실행) - ng serve
컴포넌트 생성 방법 - ng generate component '컴포넌트명'| ng g c '컴포넌트명'
서비스 생성 방법 - ng generate service "서비스명" | ng g s '서비스명'
가드 생성 방법 - ng generate guard "가드명"
```

# 프로젝트 구조

우리 프로젝트 구조에 대한 간략한 개요입니다:

angular-authentication

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

![Authentication in Angular with Node.js](/assets/img/2024-07-09-AuthenticationinAngularwithNodejs_0.png)

# Angular Components and Services

## Seller Authentication Component (seller-auth.component.ts)

```js
import { Component, OnInit } from '@angular/core';
import { SellerService } from '../services/seller.service';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';
import { Login, Signup } from '../data-type';
import { CommonModule } from '@angular/common';
@Component({
  selector: 'app-seller-auth',
  standalone: true,
  imports: [FormsModule, CommonModule],
  templateUrl: './seller-auth.component.html',
  styleUrls: ['./seller-auth.component.css'],
})
export class SellerAuthComponent implements OnInit {
  showLogin = false
  authError:String = "";
  constructor(private sellerService: SellerService, private router: Router, private seller: SellerService) {}
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

```js
  ngOnInit(): void {
    this.seller.reloadSeller();
  }

  signUp(data: Signup): void {
    this.sellerService.signUp(data);
  }

  login(data: Login): void {
    this.seller.Login(data);
    this.seller.isLoginError.subscribe((isError) => {
      if (isError) {
        this.authError = "Email or password is not correct";
      } else {
        this.authError = "";
      }
    });
  }

  openLogin() {
    this.showLogin = true;
  }

  openSignUp() {
    this.showLogin = false;
  }
```

설명:

컴포넌트 초기화(ngOnInit): 컴포넌트가 처음으로 로드될 때 초기화 로직을 실행합니다.

회원가입(signUp()): 판매자 회원가입 데이터를 SellerService로 전송합니다.

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

Login (login() ): 판매자 로그인 데이터를 SellerService로 보내 성공 시 /seller-home 으로 이동하거나 실패 시 authError를 설정합니다.

Form Toggle (openLogin() and openSignUp() ): showLogin 변수에 따라 로그인 및 가입 양식을 표시하는 메소드입니다.

# NOTE : — 사용자 인증 컴포넌트에도 동일한 로직이 적용됩니다.

## Seller Template (seller-auth.component.html)

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

```json
<div class="seller-auth">
  <div *ngIf="!showLogin" class="sign-up">
    <h1>Seller Sign-Up</h1>
    <form
      #sellerSignUp="ngForm"
      (ngSubmit)="signUp(sellerSignUp.value)"
      class="common-form"
    >
      <input
        class="form-input"
        type="text"
        name="name"
        placeholder="Enter Name"
        ngModel
      />
      <input
        class="form-input"
        type="email"
        name="email"
        placeholder="Enter Email"
        ngModel
      />
      <input
        class="form-input"
        type="password"
        name="password"
        placeholder="Enter Password"
        ngModel
      />
      <button class="form-button">SignUp</button>
      <a class="auth-toggle-link" (click)="openLogin()">Already have an account?SignIn</a>
    </form>
  </div>
```

```json
  <!-- Login Area -->
  <div *ngIf="showLogin" class="login">
    <h1>Seller login</h1>
    <p class="error-p">{ authError }</p>
    <form
      #sellerLogin="ngForm"
      (ngSubmit)="login(sellerLogin.value)"
      class="common-form"
    >
      <input
        class="form-input"
        type="email"
        name="email"
        placeholder="Enter Email"
        ngModel
      />
      <input
        class="form-input"
        type="password"
        name="password"
        placeholder="Enter Password"
        ngModel
      />
      <button class="form-button">Login</button>
      <a class="auth-toggle-link" (click)="openSignUp()">Don't have an account?SignUp</a>
    </form>
  </div>
</div>
```

Seller Sign-Up 섹션 (!showLogin):

- 판매자가 가입할 수 있는 양식이 표시됩니다.
- Angular 템플릿 구문 (ngModel)을 사용하여 사용자 입력 (name, email, password) 을 캡처하기 위한 양방향 데이터 바인딩을 수행합니다.
- required, email 및 minlength 지시문을 사용하여 입력을 유효성 검사합니다.
- 양식 제출시 signUp() 메서드를 호출합니다.

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

판매자 로그인 섹션 (showLogin):

- 로그인을 위한 양식을 표시합니다.
- 로그인에 실패하면 인증 오류 (authError)를 표시합니다.
- 사용자 입력 (이메일, 비밀번호)을 캡쳐하기 위해 양방향 데이터 바인딩을 위해 Angular 템플릿 구문 (ngModel)을 사용합니다.
- 필수 및 최소 길이 지시문을 사용해서 입력값을 유효성 검사합니다.
- 양식 제출시 login() 메서드를 호출합니다.

토글 링크:

- 사용자가 회원가입 및 로그인 양식을 전환할 수 있도록 합니다 (auth-toggle-link).
- 양식간 전환을 위해 openLogin() 및 openSignUp() 메서드를 호출합니다.

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

## 판매자 인증 컴포넌트 (seller.service.ts)

```js
import { EventEmitter, Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Login, Signup } from "../data-type";
import { BehaviorSubject } from "rxjs";
import { Router } from "@angular/router";
```

```js
@Injectable({
  providedIn: 'root',
})
export class SellerService {
  isSellerLoggedIn = new BehaviorSubject<boolean>(false);
  isLoginError = new EventEmitter<boolean>(false);
  constructor(private http: HttpClient, private router: Router) {}
  signUp(data: Signup) {
    this.http
      .post('http://localhost:8080/users', data, {
        observe: 'response',
      })
      .subscribe((result) => {
        this.isSellerLoggedIn.next(true);
        localStorage.setItem('seller', JSON.stringify(result.body));
        this.router.navigate(['/seller-home']);
      });
  }
  Login(data: Login) {
    this.http
      .get<any>(
        `http://localhost:8080/users/login?email=${data.email}&password=${data.password}`,
        { observe: 'response' }
      )
      .subscribe((result: any) => {
        if (result && result.body) {
          this.isLoginError.emit(false);
          localStorage.setItem('seller', JSON.stringify(result.body));
          this.router.navigate(['seller-home']);
        } else {
          this.isLoginError.emit(true);
        }
      });
  }
}
```

# 설명:

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

- isSellerLoggedIn (BehaviorSubject`boolean` ): BehaviorSubject를 사용하여 판매자의 로그인 상태를 추적합니다. 이를 통해 구성 요소들이 로그인 상태의 변화를 구독할 수 있습니다.
- isLoginError (EventEmitter`boolean` ): 로그인 오류를 표시하기 위해 이벤트를 발생시키며, 구성 요소들이 인증 실패에 반응할 수 있도록 합니다.
- signUp(data: Signup) : 새로운 판매자를 등록하기 위해 /users 엔드포인트로 POST 요청을 보냅니다. 성공적으로 등록되면 isSellerLoggedIn을 true로 설정하고 판매자 데이터를 localStorage에 저장한 뒤 /seller-home으로 이동합니다.
- Login(data: Login) : 사용자가 제공한 이메일과 비밀번호로 /users/login 엔드포인트로 GET 요청을 보냅니다. 성공시 seller 데이터를 localStorage에 업데이트하고 isSellerLoggedIn을 true로 설정한 후 /seller-home으로 이동합니다. 실패할 경우 isLoginError를 발생시켜 로그인 실패를 처리합니다.

# NOTE : — 사용자 서비스에도 동일한 논리가 적용됩니다.

## Guard(auth.guard.ts)

```js
import { CanActivateFn } from "@angular/router";
import { SellerService } from "./services/seller.service";
import { inject } from "@angular/core";
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

```js
export const authGuard: CanActivateFn = (route, state) => {
  const sellerService = inject(SellerService);
  if (localStorage.getItem("seller")) {
    return true;
  }
  return sellerService.isSellerLoggedIn;
};
```

설명:

- 판매자별 루트를 보호하기 위한 루트 가드를 구현합니다.
- localStorage 또는 SellerService를 사용하여 판매자 인증 상태를 확인합니다.
- 인가되지 않은 사용자를 로그인 페이지로 리디렉션합니다.

Routes(app.routes.ts)

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
import { Routes } from "@angular/router";
import { HomeComponent } from "./home/home.component";
import { SellerAuthComponent } from "./seller-auth/seller-auth.component";
import { SellerHomeComponent } from "./seller-home/seller-home.component";
import { authGuard } from "./auth.guard";
```

const routes: Routes = [
' path: ``, component: HomeComponent ',
' path: `seller-auth`, component: SellerAuthComponent ',
' path: `seller-home`, component: SellerHomeComponent, canActivate: [authGuard] ',
];

설명:

- 판매자 홈 컴포넌트 라우팅에 [authGuard]를 구현하여 판매자가 로그인 한 경우에만 판매자 홈 컴포넌트 라우팅이 적용되도록합니다.

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

---

![2024-07-09-AuthenticationinAngularwithNodejs_1](/assets/img/2024-07-09-AuthenticationinAngularwithNodejs_1.png)

![2024-07-09-AuthenticationinAngularwithNodejs_2](/assets/img/2024-07-09-AuthenticationinAngularwithNodejs_2.png)

# 결론

## 이 튜토리얼에서는 백엔드로 Node.js를 사용하여 Angular 애플리케이션에서 인증을 구현하는 방법을 다루었습니다. 이 과정은 회원 가입 및 로그인 기능 설정, 백엔드 서비스와 통합, 인증 가드로 라우트 보호 등을 포함합니다.

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

# 주요 포인트:

- 컴포넌트 및 서비스 설계: SellerAuthComponent 및 SellerService와 같이 Angular 컴포넌트 및 서비스를 올바르게 구조화하면 관심사의 명확한 분리를 보장합니다.
- 폼 처리: Angular의 템플릿 기반 폼을 사용하여 사용자 입력을 캡처하고 유효성을 검사함으로써 사용자 경험을 개선합니다.
- HTTP 상호작용: Angular의 HttpClient를 사용하여 HTTP 요청과 응답을 효과적으로 처리하면 백엔드 통신이 원활해집니다.
- 상태 관리: BehaviorSubject 및 EventEmitter를 사용하여 인증 상태를 관리하면 반응형 UI 업데이트가 가능해집니다.
- 라우트 보호: 인증 가드를 구현하여 민감한 라우트에는 인증된 사용자만 접근할 수 있도록 보장합니다.

이러한 실천 방법을 따라 Angular 애플리케이션에서 견고한 인증 시스템을 만들어 사용자에게 안전하고 사용자 친화적인 환경을 제공할 수 있습니다. 제공된 다이어그램은 구조와 데이터 흐름을 명확하게 보여주어 구현에 대한 철저한 이해를 도와줍니다.
