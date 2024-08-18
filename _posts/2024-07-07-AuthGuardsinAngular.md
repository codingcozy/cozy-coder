---
title: "Angular에서 Auth Guards를 사용 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-AuthGuardsinAngular_0.png"
date: 2024-07-07 03:07
ogImage:
  url: /assets/img/2024-07-07-AuthGuardsinAngular_0.png
tag: Tech
originalTitle: "Auth Guards in Angular"
link: "https://medium.com/@jaydeepvpatil225/auth-guards-in-angular-6960950b3c6c"
isUpdated: true
---

<img src="/assets/img/2024-07-07-AuthGuardsinAngular_0.png" />

이 글에서는 인증 가드의 기본 사항과 다양한 사용 사례를 통해 단계별 구현을 살펴볼 예정입니다.

## 안건:

- Angular란 무엇인가요?

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

- 앵귤러의 Auth Guards
- 앵귤러의 Auth Guards 종류
- Auth Guards의 장점
- Auth Guards의 예시

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

## 준비물:

- VS Code
- Angular CLI
- NodeJS

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

- TypeScript 및 Angular에 대한 기본적인 이해

## Angular이란?

Angular는 웹 애플리케이션을 구축하기 위한 인기있는 오픈 소스 JavaScript 프레임워크입니다. 구글에서 개발되었으며 현재는 구글의 Angular 팀에 의해 유지되고 있습니다. Angular는 개발자들이 동적인 단일 페이지 애플리케이션(SPA)을 만들 수 있게 하며 복잡한 웹 애플리케이션을 구축하기 위한 구조적인 접근 방식을 제공합니다.

## Angular에서의 인증 가드(Auth Guards):

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

- Angular에서는 Auth Guards라는 기술이 사용되어 사용자의 인증 상태에 따라 라우트를 보호하는 기술입니다.
- 인증된 사용자와 미인증 사용자 사이에 다른 액세스 레벨을 관리합니다.
- 사용자가 응용 프로그램에서 특정 경로로 이동할 때, Auth Guard는 사용자가 해당 경로에 대해 인증되었고 허가되었는지 확인할 수 있습니다.
- 사용자가 인증되면, Auth Guard는 네비게이션을 계속 진행하도록 허용합니다. 그렇지 않으면 사용자를 로그인 페이지나 다른 적절한 경로로 리디렉션하여 보호된 콘텐츠에 액세스하는 것을 방지합니다.

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

## Angular의 인증 가드 유형:

Angular에서는 사용자의 인증 상태에 따라 라우트를 보호하는 데 사용할 수 있는 다양한 유형의 Auth Guards가 있습니다.

- `CanActivate`
  - 대부분 사용되는 Angular 애플리케이션에서 사용되는 일반적인 Auth Guard 유형입니다.

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

- 특정 경로를 탐색할 수 있는 사용자를 결정하고 불리언 값 (참 또는 거짓) 또는 불리언으로 해결되는 observable/promise을 반환합니다.

- 반환된 값이 true이면 경로가 활성화됩니다. false이면 경로가 차단되고 사용자는 일반적으로 로그인 페이지로 리디렉션되거나 적절한 경로로 이동합니다.

**CanActivateChild**

- CanActivateChild Auth Guard는 CanActivate와 유사하지만 특정 경로의 자식 경로를 보호하는 데 특히 사용됩니다.

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

· 중첩 된 루트가있고 요구 사항에 따라 모든 하위 루트를 동일한 인증 기준으로 보호해야 할 때 사용됩니다.

### CanDeactivate

· CanDeactivate는 Angular의 또 다른 유형의 가드로, 특정 경로를 떠나는 것이 허용되는지 사용자가 제어 할 수있게합니다. 예를 들어, 저장되지 않은 변경 사항이 있는 경우입니다.

· 사용자에게 확인을 요청하도록하는 또는 저장되지 않은 변경 사항이있는 경로를 떠날 때 다른 확인을 수행하거나 사용자가 떠나도록 허용하기 전에 다른 확인을 수행하도록 사용됩니다.

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

CanLoad

- 인증되지 않은 사용자의 경우 지연로드된 모듈을 로드하지 못하게 하려면 CanLoad Auth Guard를 사용합니다.

- 사용자가 모듈을 비동기적으로 로드할 수 있는지 여부를 확인합니다.

### Auth Guards의 이점:

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

앵귤러의 Auth Guards는 안전한 웹 애플리케이션을 구축하기 위한 중요한 혜택을 제공합니다:

유연성:

Auth Guards는 매우 사용자 정의가 가능합니다. 사용자 역할, 권한 또는 기타 조건을 확인해야 할 경우에도 Auth Guards를 사용자의 요구에 맞게 조정할 수 있습니다.

유지보수 및 테스트:

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

인증 및 권한 부여 로직을 Auth Guards로 분리하면 이 기능을 독립적으로 유지하고 테스트하는 것이 더 쉬워집니다.

Route Protection:

Auth Guards를 사용하면 애플리케이션에서 특정 라우트나 기능을 보호할 수 있어서 권한이 있는 사용자만 특정 페이지에 액세스하거나 특정 작업을 수행할 수 있도록 할 수 있습니다.

사용자 경험:

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

인증 가드를 사용하면 사용자가 인증되지 않은 경우를 처리할 수 있습니다. 페이지 액세스를 거부하는 대신 사용자를 로그인 페이지나 다른 적절한 경로로 리디렉션하여 더 사용자 친화적인 경험을 제공할 수 있습니다.

보안 강화:

경로 및 기능에 대한 액세스 제어로, 인증 가드는 애플리케이션을 안전하게 보호하는 데 도움이 됩니다. 인가되지 않은 액세스를 방지하고 잠재적인 공격에 대비하며 민감한 데이터를 안전하게 지키는 데 중요한 역할을 합니다.

코드 재사용성:

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

인증 가드는 Angular 서비스로 구현되어 있어서 애플리케이션 전반에 걸쳐 재사용할 수 있습니다. 한 번 인증 가드를 정의하면 여러 경로에 쉽게 적용하여 코드를 더 깨끗하고 유지보수가 쉽도록 할 수 있습니다.

보호 기능이 있는 지연 로딩:

인증 가드를 사용하면 사용자의 인증 상태에 따라 지연 로딩된 모듈을 제어할 수 있습니다. 이는 미인증 사용자에게 특정 모듈을 로딩하는 것을 방지하여 애플리케이션 성능을 향상시키고 불필요한 데이터 전송을 줄일 수 있음을 의미합니다.

권한 부여의 일관성:

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

인증 가드는 사용자 인가 로직을 중앙 집중식으로 처리하는 방법을 제공합니다. 이 일관성은 응용 프로그램 전반에 걸쳐 인가 확인이 일관되게 적용되어, 무단으로 사용자에게 액세스 권한을 부여하는 실수 가능성을 줄입니다.

# Auth Guards의 예시:

단계 1

새로운 Angular 애플리케이션을 생성합니다.

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

단계 2

다음 명령어를 사용하여 부트스트랩 모듈을 설치하세요:

Angular JSON 파일에서 부트스트랩을 구성하세요.

```js
"styles": [
"src/styles.css",
"./node_modules/bootstrap/dist/css/bootstrap.min.css"
],
"scripts": [
"./node_modules/bootstrap/dist/js/bootstrap.min.js"
]
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

### 단계 3

새로 생성된 애플리케이션에 다음 구성 요소를 추가하십시오.

login.component.html

```js
<div class="container">
    <div class="row justify-content-center mt-5">
      <div class="col-md-6">
        <form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
          <div class="form-group">
            <label for="username">Username</label>
            <input type="text" class="form-control" id="username" formControlName="username" placeholder="Enter username">
          </div>
          <div class="form-group">
            <label for="password">Password</label>
            <input type="password" class="form-control" id="password" formControlName="password" placeholder="Password">
          </div>
          <button type="submit" class="btn btn-primary" style="margin-top: 6px;">Login</button>
        </form>
      </div>
    </div>
  </div>
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

login.component.ts

```js
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { AuthService } from 'src/app/services/auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  loginForm?: any;

  constructor(private fb: FormBuilder, private authService : AuthService, private router: Router) {}

  ngOnInit(): void {
    this.loginForm = this.fb.group({
      username: ['', Validators.required],
      password: ['', Validators.required]
    });
  }

  onSubmit(): void {
    if (this.loginForm.valid) {
      const username = this.loginForm.get('username').value;
      const password = this.loginForm.get('password').value;

       // 호출( Call) 인증(Authentication) 서비스의 로그인 메서드(login method)
       if (this.authService.login(username, password)) {
        // 성공적인 로그인 시 ProductListComponent로 이동(Navigate to the ProductListComponent upon successful login)
        this.router.navigate(['/product-list']);
      } else {
        // 인증 오류 처리(Authentication error handling) (오류 메시지 표시, 등.)
      }

    }
  }
}
```

product-list.component.html

```js
Markdown 형식으로 변환하는 에박(Markdown) 테이블:
| Id | Title | Description | Price | Category |
| --- | --- | --- | --- | --- |
{% if productData != null %}
{% for product in productData %}
| <a [routerLink]="['/product', product.id]">{{product.id}}</a> | {{product.title}} | {{product.description}} | {{product.price | currency}} | {{product.category}} |
{% endfor %}
{% endif %}
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

product-list.component.ts

```js
import { Component } from '@angular/core';
import { ProductService } from 'src/app/services/product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css']
})
export class ProductListComponent {
  constructor(private productService : ProductService){}

  productData : any;

  ngOnInit(): void {
    this.productService.getProducts().subscribe(
      (data: any[]) => {
        this.productData = data;
      },
      (error) => {
        console.error('상품을 불러오는 동안 오류가 발생했습니다:', error);
      }
    );
  }
}
```

proudct-details.component.html

```js
## 테이블 태그를 Markdown 형식으로 변경하였습니다.

| Id | Title | Description | Price | Category |
|----|-------|-------------|-------|---------|
| {productDetail?.id} | {productDetail?.title} | {productDetail?.description} | {productDetail?.price | currency} | {productDetail?.category} |

<div class="btn-group" role="group" aria-label="Basic mixed styles example">
    <button type="button" class="btn btn-danger"><a class="nav-link" routerLink="offers">제품 할인 정보</a></button>
    <button type="button" class="btn btn-warning"><a class="nav-link" routerLink="rating">제품 평가</a></button>
</div>

<router-outlet></router-outlet>
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

프라우드덕츠 디테일 컴포넌트(proudct-details.component.ts)

```js
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { ProductService } from 'src/app/services/product.service';

@Component({
  selector: 'app-proudct-details',
  templateUrl: './proudct-details.component.html',
  styleUrls: ['./proudct-details.component.css']
})
export class ProudctDetailsComponent {
  productDetail? : any;

  constructor(private route : ActivatedRoute,private productService : ProductService){}

  ngOnInit(): void {
    let productId = this.route.snapshot.params['id'];
    this.getProductDetailById(productId)
  }

  getProductDetailById(id: number){
    this.productService.getProductDetailById(id).subscribe(res => {
      this.productDetail = res
      console.log(res)
    })
  }
}
```

프라우드덕츠 오퍼 컴포넌트(proudct-offers.component.html)

```js
<p>proudct-offers works!</p>
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

운ful_producto-offers.component.ts

```js
import { Component } from "@angular/core";

@Component({
  selector: "app-proudct-offers",
  templateUrl: "./proudct-offers.component.html",
  styleUrls: ["./proudct-offers.component.css"],
})
export class ProudctOffersComponent {}
```

proudct-rating.component.html

```js
<div class="product-rating container">
  <form (ngSubmit)="rateProduct()" #ratingForm="ngForm" class="mb-3" style="margin-top: 2%;">
    <div class="form-group">
      <label for="rating">Rating:</label>
      <input type="number" id="rating" name="rating" min="1" max="5" required
             [(ngModel)]="selectedRating" class="form-control">
    </div>

    <button type="submit" [disabled]="ratingForm.invalid" class="btn btn-primary" style="margin-top: 2%;">Rate Product</button>
  </form>
  <button (click)="saveChanges()" [disabled]="!hasUnsavedChanges()" class="btn btn-success">Save Changes</button>
</div>
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

proudct-rating.component.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-proudct-rating',
  templateUrl: './proudct-rating.component.html',
  styleUrls: ['./proudct-rating.component.css']
})
export class ProudctRatingComponent {

  private unsavedChanges = false;
  selectedRating: number | null = null;

  rateProduct() {
    if (this.selectedRating !== null) {
      console.log('Rating:', this.selectedRating);
      this.unsavedChanges = true;
    }
  }

  saveChanges() {
    console.log('Saving changes...');
    this.unsavedChanges = false;
  }

  hasUnsavedChanges(): boolean {
    return this.unsavedChanges;
  }
}
```

Step 4

아래 명령어를 사용하여 auth 및 product 서비스를 생성하세요:

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

product.service.ts

```js
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { AuthService } from './auth.service';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ProductService {

  private apiUrl = 'https://fakestoreapi.com';
  private authSecretKey = 'Bearer Token';

  constructor(private http: HttpClient) {}

  private getHeaders(): HttpHeaders {
    const authToken = localStorage.getItem(this.authSecretKey);
    return new HttpHeaders({
      'Content-Type': 'application/json',
      Authorization: `Bearer ${authToken}`
    });
  }

  getProducts() : Observable<any[]>{
    const headers = this.getHeaders();
    return this.http.get<any[]>(`${this.apiUrl}/products`, { headers });
  }

  getProductDetailById(id : number){
    const headers = this.getHeaders();
    return this.http.get(`${this.apiUrl}/products/` + id, { headers })
  }
}
```

auth.service.ts

```js
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {

  private isAuthenticated = false;
  private authSecretKey = 'Bearer Token';

  constructor() {
    this.isAuthenticated = !!localStorage.getItem(this.authSecretKey);
  }

  login(username: string, password: string): boolean {
    if (username === 'Jaydeep Patil' && password === 'Pass@123') {
      const authToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpheWRlZXAgUGF0aWwiLCJpYXQiOjE1MTYyMzkwMjJ9.yt3EOXf60R62Mef2oFpbFh2ihkP5qZ4fM8bjVnF8YhA'; // Generate or receive the token from your server
      localStorage.setItem(this.authSecretKey, authToken);
      this.isAuthenticated = true;
      return true;
    } else {
      return false;
    }
  }

  isAuthenticatedUser(): boolean {
    return this.isAuthenticated;
  }

  logout(): void {
    localStorage.removeItem(this.authSecretKey);
    this.isAuthenticated = false;
  }
}
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

Step 5

다음으로, 다음과 같이 표시된 기능 모듈을 추가하세요. 이 모듈은 CanLoad 인증 가드와 함께 사용했습니다.

product-service-routing.module.ts

```js
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { ProductServiceComponent } from "./product-service-component/product-service-component.component";

const routes: Routes = [{ path: "", component: ProductServiceComponent }];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class ProductServiceRoutingModule {}
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

product-service.module.ts

```typescript
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { ProductServiceComponent } from "./product-service-component/product-service-component.component";
import { ProductServiceRoutingModule } from "./product-service-routing.module";

@NgModule({
  declarations: [ProductServiceComponent],
  imports: [ProductServiceRoutingModule, CommonModule],
})
export class ProductServiceModule {}
```

product-service-component.component.html

```html
<p>product-service-component works!</p>
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

```typescript
product-service-component.component.ts

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-product-service-component',
  templateUrl: './product-service-component.component.html',
  styleUrls: ['./product-service-component.component.css']
})
export class ProductServiceComponent implements OnInit {

  constructor(){
    console.log('service component')
  }

  ngOnInit(): void {
    console.log('service component')
  }

}

Step 6

Add the following code related to navbar tabs inside the app component for navigation purposes:
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

app.component.html

```js
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Auth Guards Demo</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" routerLink="/login">Login</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" routerLink="/products">Products</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" routerLink="/service">Products Service</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" (click)="logout()">Logout</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<router-outlet></router-outlet>
```

app.component.ts

```js
import { Component } from '@angular/core';
import { AuthService } from './services/auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'angular-guards';

  constructor(private authService: AuthService, private router : Router) {}

  logout(): void {
    this.authService.logout();
    this.router.navigate(['/login']);
  }
}
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

7단계

다음 명령어를 사용하여 새 인증 가드를 생성하고 아래와 같이 코드를 추가하세요.

auth.guard.ts

```js
import { Injectable } from '@angular/core';
import { CanActivate, CanActivateChild, CanDeactivate, CanLoad, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';
import { ProudctRatingComponent } from '../components/proudct-rating/proudct-rating.component';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate, CanActivateChild, CanDeactivate<ProudctRatingComponent>, CanLoad  {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    return this.checkAuth();
  }

  canActivateChild(): boolean {
    return this.checkAuth();
  }

  canDeactivate(component: ProudctRatingComponent): boolean {
    if (component.hasUnsavedChanges()) {
      return window.confirm('저장되지 않은 변경 사항이 있습니다. 정말 떠나시겠습니까?');
    }
    return true;
  }

  canLoad(): boolean {
    return this.checkAuth();
  }

  private checkAuth(): boolean {
    if (this.authService.isAuthenticatedUser()) {
      return true;
    } else {
      // 사용자가 인증되지 않은 경우 로그인 페이지로 리디렉트
      this.router.navigate(['/login']);
      return false;
    }
  }

}
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

8단계

다음으로 앱 라우팅 파일을 열고 인증 가드를 사용하여 다른 경로를 구성합니다.

app-routing.module.ts

```js
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { LoginComponent } from "./components/login/login.component";
import { ProductListComponent } from "./components/product-list/product-list.component";
import { AuthGuard } from "./guards/auth.guard";
import { ProudctDetailsComponent } from "./components/proudct-details/proudct-details.component";
import { ProudctRatingComponent } from "./components/proudct-rating/proudct-rating.component";
import { ProudctOffersComponent } from "./components/proudct-offers/proudct-offers.component";

const routes: Routes = [
  { path: "", component: ProductListComponent, pathMatch: "full", canActivate: [AuthGuard] },
  { path: "login", component: LoginComponent },
  { path: "products", component: ProductListComponent, canActivate: [AuthGuard] },
  { path: "products", component: ProductListComponent, canActivate: [AuthGuard] },
  {
    path: "product/:id",
    component: ProudctDetailsComponent,
    canActivateChild: [AuthGuard],
    children: [
      { path: "rating", component: ProudctRatingComponent, canDeactivate: [AuthGuard] },
      { path: "offers", component: ProudctOffersComponent },
    ],
  },
  {
    path: "service",
    loadChildren: () => import("./modules/product-service/product-service.module").then((m) => m.ProductServiceModule),
    canLoad: [AuthGuard],
  },
  { path: "**", component: ProductListComponent, pathMatch: "full", canActivate: [AuthGuard] },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
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

9단계

아래와 같이 루트 모듈 내에서 인증 가드를 구성하고 애플리케이션 관련 모듈을 가져오십시오.

app.module.ts

```js
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";

import { AppRoutingModule } from "./app-routing.module";
import { AppComponent } from "./app.component";
import { LoginComponent } from "./components/login/login.component";
import { ProductListComponent } from "./components/product-list/product-list.component";

import { HttpClientModule } from "@angular/common/http";
import { ReactiveFormsModule } from "@angular/forms";
import { AuthGuard } from "./guards/auth.guard";
import { ProudctDetailsComponent } from "./components/proudct-details/proudct-details.component";
import { ProudctRatingComponent } from "./components/proudct-rating/proudct-rating.component";
import { ProudctOffersComponent } from "./components/proudct-offers/proudct-offers.component";

import { FormsModule } from "@angular/forms";
import { ProductServiceModule } from "./modules/product-service/product-service.module";

@NgModule({
  declarations: [
    AppComponent,
    LoginComponent,
    ProductListComponent,
    ProudctDetailsComponent,
    ProudctRatingComponent,
    ProudctOffersComponent,
  ],
  imports: [BrowserModule, AppRoutingModule, HttpClientModule, ReactiveFormsModule, FormsModule, ProductServiceModule],
  providers: [AuthGuard],
  bootstrap: [AppComponent],
})
export class AppModule {}
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

10단계

애플리케이션을 실행해보세요.

## 샘플 스크린샷

사용자 이름: Jaydeep Patil

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

패스워드: Pass@123

![이미지1](/assets/img/2024-07-07-AuthGuardsinAngular_1.png)

![이미지2](/assets/img/2024-07-07-AuthGuardsinAngular_2.png)

![이미지3](/assets/img/2024-07-07-AuthGuardsinAngular_3.png)

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

<img src="/assets/img/2024-07-07-AuthGuardsinAngular_4.png" />

<img src="/assets/img/2024-07-07-AuthGuardsinAngular_5.png" />

<img src="/assets/img/2024-07-07-AuthGuardsinAngular_6.png" />

## GitHub:

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

https://github.com/Jaydeep-007/angular-auth-guards

## 결론:

이 글에서는 Auth Guards의 기본, 각종 가드의 종류, 다양한 예시를 통해 그들의 장점에 대해 논의했습니다.

즐거운 코딩하세요!
