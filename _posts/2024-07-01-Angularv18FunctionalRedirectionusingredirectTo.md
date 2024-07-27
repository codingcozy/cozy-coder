---
title: "Angular v18  redirectTo 기능을 사용한 URL 리다이렉션 방법 "
description: ""
coverImage: "/assets/img/2024-07-01-Angularv18FunctionalRedirectionusingredirectTo_0.png"
date: 2024-07-01 20:38
ogImage: 
  url: /assets/img/2024-07-01-Angularv18FunctionalRedirectionusingredirectTo_0.png
tag: Tech
originalTitle: "Angular v18 — Functional Redirection using redirectTo 🔥🚀"
link: "https://medium.com/@giorgio.galassi/angular-v18-functional-redirection-using-redirectto-10c853d9d837"
---



![image](/assets/img/2024-07-01-Angularv18FunctionalRedirectionusingredirectTo_0.png)

앵귤러의 부활기 가운데, 신호, 신호 NgRx, Zoneless 및 기타 많은 새로운 기능에 대해 모두가 이야기하고 있는 것을 알 수 있습니다. 이러한 것들은 앵귤러 생태계를 활성화시킬 것이라고 약속하는 여러 흥미로운 업데이트 중 하나일 뿐입니다.

하지만 이것들이 유일한 흥미로운 업데이트는 아닙니다. 앵귤러 v18에서는 여러 주목할만한 향상이 소개되었으며, 이 중 하나는 리다이렉션에 함수를 사용할 수 있는 능력인 'redirectTo'입니다. 이 기능은 개발자가 정적 문자열의 제약을 넘어서 더 복잡하고 동적인 리다이렉션 로직을 구현할 수 있도록 합니다.

## 함수형 리다이렉션의 사용 시기 및 장소


<div class="content-ad"></div>

새로운 기능이 도착했을 때 생길 수 있는 질문 중 하나는: 언제 그리고 어디에 사용해야 할까요?

다음은 일부 예시입니다:

- 역할 기반 리다이렉션: 사용자의 역할에 따라 응용 프로그램의 다른 부분으로 사용자를 리디렉션합니다. 예를 들어, 관리자는 대시보드로 보내지고, 일반 사용자는 홈페이지로 보내집니다.
- 조건부 네비게이션: 구독 상태나 프로필 완성도와 같은 특정 조건에 따라 사용자를 리디렉션합니다.
- 동적 경로: 사용자 데이터나 다른 응용 프로그램 상태를 기반으로 동적 경로를 만들어 사용자가 정확히 도착해야 하는 곳에 도착하도록 합니다.

이러한 시나리오를 모두 다루는 예시를 살펴보겠습니다.

<div class="content-ad"></div>

## 사용 사례

사용자가 일반 사용자 또는 관리자로 어떻게 로그인할지를 결정할 수 있는 사용자가 있다고 상상해보세요. 그들의 역할에 따라 적절한 페이지로 리디렉션하고 싶습니다.

우선 아주 간단한 AuthService로 시작하여 사용자 객체를 포함하는 신호가 포함된 것을 작성할 것입니다. 간이로 이 객체는 프론트엔드 자체에서 placeholder로 대체됩니다. 실제 시나리오에서는 외부 서비스에서 가져올 수 있습니다.

```js
// auth.service.ts
import { Injectable, inject, signal } from '@angular/core';
import { Router } from '@angular/router';

export const Roles = {
  User: 'user',
  Admin: 'admin',
} as const;

export type RolesType = (typeof Roles)[keyof typeof Roles];

type User = {
  isAdmin: boolean;
  name: string;
  surname: string;
  username: string;
};

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  user = signal<User | null>(null);

  #router = inject(Router);

  get role(): RolesType {
    return this.user()?.isAdmin ? Roles.Admin : Roles.User;
  }

  get username(): string {
    return this.user()?.username || '';
  }

  isAuth(): boolean {
    return !!this.user();
  }

  isAuthGuard(): boolean {
    const isAuth = this.isAuth();

    if (isAuth) return true;

    this.#router.navigate(['login']);
    return false;
  }

  authAdmin() {
    this.user.set({
      isAdmin: true,
      name: 'Giorgio',
      surname: 'Galassi',
      username: 'ggadmin',
    });
  }

  authUser() {
    this.user.set({
      isAdmin: false,
      name: 'Giorgio',
      surname: 'Galassi',
      username: 'gguser',
    });
  }
}
```

<div class="content-ad"></div>

이제 우리 서비스가 준비되었으니 routes.ts 내에서 라우트 리다이렉션을 사용자 정의하고 관리하는 방법을 살펴보겠습니다.

```js
// routes.ts
import { inject } from '@angular/core';
import { Routes } from '@angular/router';

import { AuthService, Roles, RolesType } from './providers/auth.service';

import { UserComponent } from './pages/user.component';
import { AdminComponent } from './pages/admin.component';
import { LoginComponent } from './pages/login.component';

const Routes = {
  User: 'user',
  Admin: 'admin',
  Login: 'login',
} as const;

export type RoutesType = (typeof Routes)[keyof typeof Routes];

const routesMap = new Map<RolesType, RoutesType>([
  [Roles.Admin, Routes.Admin],
  [Roles.User, Routes.User],
]);

export const routes: Routes = [
  {
    path: '',
    pathMatch: 'full',
    redirectTo: () => {
      const auth = inject(AuthService);
      const { role } = auth;

      if (!auth.isAuth()) return Routes.Login;
      return routesMap.get(role) || Routes.User;
    },
  },
  {
    path: Routes.Login,
    component: LoginComponent,
  },
  {
    path: Routes.User,
    component: UserComponent,
    canActivate: [() => inject(AuthService).isAuthGuard()],
  },
  {
    path: Routes.Admin,
    component: AdminComponent,
    canActivate: [() => inject(AuthService).isAuthGuard()],
  },
];
```

로그인되어 있는 사용자를 리디렉션할 위치를 선택하기 위해 AuthService를 인젝트하고 사용하는 redirectTo 함수에 초점을 맞추세요.

사용자가 로그인되어 있지 않을 경우 로그인 페이지로 리디렉션하는 방법을 주목해 보세요. 또 하나 중요한 점은 일반 문자열이 아니라 특정 로직에서 검색한 문자열을 반환한다는 점입니다.

<div class="content-ad"></div>

이상으로 마치겠습니다!

이 간단한 기사에서는 redirectTo 함수가 얼마나 유용하며 무엇을 할 수 있는지 살펴보았습니다.

이것의 확장은 무한할 수 있습니다.

여러분은 내부 상태를 활용하거나 서로 다른 서비스를 결합할 수도 있습니다. 좋은 실천이 한계입니다.

<div class="content-ad"></div>

아래에서 함께 놀 수 있는 StackBlitz가 있어요.

지금까지 함께해줘서 고마워요. 모든 것이 지금까지 명확했기를 바래요. 
더 많은 글을 더 즐겁게 살펴볼 수 있다면 저는 기쁠 거예요.

다음 글에서 봐요,
G.